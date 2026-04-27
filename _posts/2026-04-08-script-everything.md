---
layout: post
title: Script Everything
description: "AI tools cut scripting time from hours to minutes, shifting the automation break-even. Three real examples: K8s auth, deployed commit lookup, and browser auto-login."
date: 2026-04-27
tags:
  - automation
  - ai
  - claude
  - kubernetes
---
As developers, we have a tendency to automate everything possible. Historically, writing automation scripts would prove this xkcd right:

![File:automation.png](https://www.explainxkcd.com/wiki/images/a/a9/automation.png)

With AI coding assistants, writing one of these scripts costs minutes, not hours. That moves the break-even line on the other xkcd worth keeping in mind, *Is it worth the time?*, far enough that plenty of things which used to sit firmly in "not worth it" now sit on the worth-it side. The exceptions are still real: an automation that tries to do too much across a multi-service migration, or wrap an internal API with no docs, will spiral the same way it always did.
Here are a few examples where I've experienced great results:

1. Authenticated K8s in-cluster requests
2. Checking latest git commit deployed
3. Browser extension to click buttons for auto-login

I'll go into detail on each below. The **Placement in xkcd** row in each table is where I think the automation lives on this grid:

![Is It Worth the Time?](https://imgs.xkcd.com/comics/is_it_worth_the_time.png "Don't forget the time you spend finding the chart to look up what you save. And the time spent reading this reminder about the time spent. And the time trying to figure out if either of those actually make sense. Remember, every second counts toward your life total, including these right now.")

## Authenticated K8s in-cluster requests

| Problem           | Strict K8s access policies prevent unauthenticated debug curls |
| ----------------- | -------------------------------------------------------------- |
| Placement in xkcd | Monthly and it would take me around ten minutes                |
| Solution          | Borrow the workload identity of an existing pod and run a curl pod under the same service account     |

The trick is that the script doesn't fake auth, it borrows the real workload identity an existing pod already uses, so the call path is identical to a normal in-cluster request.

```bash
# ...
URL="$1"; SCOPE="$2"; NS="$3"; LABEL="$4"
KC="kubectl -n $NS"

# 1. Find a pod that already has the identity we want.
POD=$($KC get pods -l "$LABEL" -o jsonpath='{.items[0].metadata.name}')
SA=$($KC  get pod "$POD" -o jsonpath='{.spec.serviceAccountName}')

# 2. Pull federated credentials from the pod.
TENANT=$($KC exec "$POD" -- printenv AZURE_TENANT_ID)
CLIENT=$($KC exec "$POD" -- printenv AZURE_CLIENT_ID)
FED=$($KC    exec "$POD" -- cat /var/run/secrets/azure/tokens/azure-identity-token)

# 3. Exchange the federated token for an access token (locally).
TOKEN=$(curl -sS -X POST \
  "https://login.microsoftonline.com/$TENANT/oauth2/v2.0/token" \
  -d "client_id=$CLIENT" \
  -d "scope=$SCOPE" \
  -d "grant_type=client_credentials" \
  -d "client_assertion_type=urn:ietf:params:oauth:client-assertion-type:jwt-bearer" \
  --data-urlencode "client_assertion=$FED" | jq -r .access_token)

# 4. Run a curl pod inside the cluster, under the same SA.
$KC run "tmp-curl-$$" --rm -it --restart=Never \
  --image=curlimages/curl \
  --overrides='{"spec":{"serviceAccountName":"'"$SA"'",
    "containers":[{"name":"c","image":"curlimages/curl",
      "command":["curl","-sS","-H","Authorization: Bearer '"$TOKEN"'","'"$URL"'"]}]}}'
```


## Checking the latest git commit deployed
| Problem           | Get context of git SHA from K8s deployed image           |
| ----------------- | -------------------------------------------------------- |
| Placement in xkcd | weekly and it would take me around five minutes.         |
| Solution          | Resolve the deployed image's SHA back to its commit and PR, locally or via gh |

The useful bit is that it resolves a deployed image tag back to its commit *and* its PR in one step, even when the image was built from a different repo than the one I'm in.

```bash
NS="${1:?namespace}"

kubectl -n "$NS" get deploy -o json \
  | jq -r '.items[] | "\(.metadata.name)\t\(.spec.template.spec.containers[0].image)"' \
  | while IFS=$'\t' read -r name image; do
      tag="${image##*:}"          # 20260327-afaa004
      sha="${tag##*-}"            # afaa004 (strip date prefix)

      echo "== $name  $tag =="

      # Try local git, else fall back to gh API.
      if git cat-file -e "$sha" 2>/dev/null; then
        git log -1 --format='  %s%n  %an, %ar%n  %H' "$sha"
      else
        slug=$(echo "$image" | sed -E 's#.*/([^/]+/[^:@]+).*#\1#')
        gh api "repos/$slug/commits/$sha" \
          --jq '"  \(.commit.message | split("\n")[0])\n  \(.commit.author.name), \(.commit.author.date)"'
        pr=$(gh api "repos/$slug/commits/$sha/pulls" --jq '.[0].number // empty')
        [[ -n "$pr" ]] && echo "  PR: https://github.com/$slug/pull/$pr"
      fi
    done
```

## Browser extension for auto-login

| Problem           | Multi-step login in staging environment                |
| ----------------- | ------------------------------------------------------ |
| Placement in xkcd | daily and it would take me around thirty seconds       |
| Solution          | MV3 content script that walks the multi-step BankID flow and simulates real keystrokes so React accepts the input |

Chrome MV3 content-script extension. Matches the auth flow's hosts, runs at `document_end` in all frames, and walks the multi-step OpenID/BankID login: shows a profile picker on the entry page, persists the selected profile in `chrome.storage.local`, then on each subsequent host fills the right field and clicks Next.

The post-worthy detail: React doesn't fire `onChange` when you set `.value` directly, so the typing has to be simulated key-by-key: `keydown`, `input`, `keyup`, repeat. Without that, the form silently rejects everything.

```js
// content.js (condensed) — host-dispatched form filler with simulated typing.
(function () {
  const sleep = ms => new Promise(r => setTimeout(r, ms));
  const PROFILES = [{ name: "Test User", id: "<redacted>", otp: "<redacted>", password: "<redacted>" }];

  async function typeInto(input, text) {
    // .value = "..." doesn't fire React's onChange — we have to dispatch events.
    const setter = Object.getOwnPropertyDescriptor(HTMLInputElement.prototype, "value").set;
    input.focus();
    for (const ch of text) {
      input.dispatchEvent(new KeyboardEvent("keydown", { key: ch, bubbles: true }));
      setter.call(input, input.value + ch);
      input.dispatchEvent(new Event("input", { bubbles: true }));
      input.dispatchEvent(new KeyboardEvent("keyup",   { key: ch, bubbles: true }));
      await sleep(60);
    }
    input.dispatchEvent(new Event("change", { bubbles: true }));
  }

  const waitFor = async (sel, t = 10000) => {
    const start = Date.now();
    while (Date.now() - start < t) {
      const el = document.querySelector(sel);
      if (el && el.offsetParent !== null) return el;
      await sleep(200);
    }
  };

  const STEPS = {
    "portal.example":  async (p) => { /* show picker, store p, click Login */ },
    "id.example":      async (p) => { await typeInto(await waitFor("input[name=id]"),  p.id);  (await waitFor("button[type=submit]")).click(); },
    "otp.example":     async (p) => { await typeInto(await waitFor("input[type=tel]"), p.otp); document.querySelector("button.next").click(); },
    "pw.example":      async (p) => { await typeInto(await waitFor("input[type=password]"), p.password); document.querySelector("button.next").click(); },
  };

  async function run() {
    const handler = Object.entries(STEPS).find(([host]) => location.hostname.endsWith(host))?.[1];
    if (!handler) return;
    const { profile } = (await chrome.storage.local.get("profile")) || {};
    if (profile) await handler(profile);
  }

  // SPA-aware: re-dispatch when the DOM changes.
  let t; new MutationObserver(() => { clearTimeout(t); t = setTimeout(run, 600); })
    .observe(document.documentElement, { childList: true, subtree: true });
  setTimeout(run, 500);
})();
```

The first xkcd is less true when each script costs minutes, not hours. The second one still applies — the grid is real — but the grid has shifted. Things I wouldn't have automated a year ago now pay for themselves in a week.


