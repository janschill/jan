---
layout: post
title: Obsidian Notes and Blog Posts
description: How I use Obsidian with Templater and a shell script to draft and publish Jekyll blog posts without leaving my editor.
date: 2026-03-28
tags:
  - blogging
  - obsidian
  - workflow
---
I've been wanting to blog more for a long time and made multiple attempts at doing so, but could never find a flow that would work well for me. This is another try at it.

What's been the struggle? Frankly, I think it's mostly because writing is hard, especially blogging. It certainly isn't because there's a lack of good static-site generators, editors, or blog hosting options out there. For me, the barrier of writing posts, plus a bit of friction in finding a setup that works for me, is certainly the reason it has never amounted to anything useful.

Recently, I've given Obsidian another try, which I had stopped using for similar reasons. The problem there was that I went all in on plugins and clever processes, when all I needed was a way to keep track of some simple one-on-one and project notes at work. I stopped using it~~,~~ because I spent more time setting up templates, scripts, folder structures than actually writing meaningful notes. As of a week, I use Obsidian with a relatively flat folder structure, not blocking me on: "Where should this note go?"-questions.

Given all notes in Obsidian and on my Jekyll blog are Markdown files, I felt Obsidian would be a natural fit as my blog editor. I keep my Obsidian notes and my blog posts in two separate git repositories and want to keep it that way. Basically: use Obsidian to draft the post and once done, copy it over to the blog repository and push it up to GitHub.

I use the Obsidian Community Plugin "Templater" to create draft posts with Jekyll frontmatter.

```md
<%*
const title = await tp.system.prompt("Blog post title");
const slug = title.toLowerCase().replace(/[^a-z0-9]+/g, '-').replace(/^-|-$/g, '');
const date = tp.date.now("YYYY-MM-DD");
const filename = date + "-" + slug;
await tp.file.rename(filename);
await tp.file.move("blog/" + filename);
-%>
---
layout: post
title: "<% title %>"
description: ""
date: <% tp.date.now("YYYY-MM-DD") %>
tags: []
---

```

With Templater I can create a new post with `CMD + P` -> `Templater: Create new note from template`, which will then prompt me for a title. It slugifies the current date and title for the file name and moves the file into the `blog` directory. A careful reader will observe I am moving on a slippery road towards my eternal struggle of over-engineering the setup. Let's just see how this will go, shall we?

The script to copy the finished post to the blog repository looks like this and can be executed with another Obsidian plugin called "Shell commands":

```sh
#!/usr/bin/env bash
set -euo pipefail

BLOG_REPO="$HOME/code/janschill/jan"
POSTS_DIR="$BLOG_REPO/_posts"
SOURCE="$1"
BASENAME=$(basename "$SOURCE" .md)
VAULT_DIR=$(cd "$(dirname "$0")" && pwd)
DEST_PATH="${POSTS_DIR}/${BASENAME}.md"
# ...
read -rp "Commit and push to blog repo? [y/N] " CONFIRM
if [[ "$CONFIRM" =~ ^[Yy]$ ]]; then
  cd "$BLOG_REPO"
  git add "$DEST_PATH"
  git commit -m "Add new post: $BASENAME"
  git push
  echo "Published! Post will be live shortly."
else
  echo "File copied but not committed. You can commit manually in $BLOG_REPO"
fi
```

Execute the script directly from Obsidian. No need to open the Terminal.

![Pasted image 20260328110828](/assets/images/2026/Pasted image 20260328110828.png)

Consider this a test. See you in a year for another post.