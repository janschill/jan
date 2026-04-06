---
layout: post
title: LLMs for Proofreading
description: How I use a custom LLM proofreader skill to catch mistakes in my writing without losing my voice — a non-native blogger's practical workflow.
date: 2026-03-28
tags:
  - ai
  - writing
  - claude
---
My personal hurdles in writing are: getting started on a blank page and struggling with my non-native language.
For the first struggle, I believe forcing yourself to write down one sentence often results in getting the ball rolling. Just let the thoughts flow and write them down as they come. You can clean up afterwards. But cleaning up afterwards is where the second hurdle for me just makes blogging not enjoyable and I stop. Tools like red or blue, squiggly lines under incorrect spelling or grammar mistakes have existed forever *Grammarly* has also helped us non-native or even native writers for a long time.

Now, with generative AI, everything changes and you can generate blog posts from a few bullet
points and publish them on LinkedIn. That's where it seems everyone does it, at least.
But it should be possible to harness those benefits and just tailor them to your needs without compromising your own writing.

Agentic [skills](https://agentskills.io/home) and precise prompts work wonders here. I've integrated a script that feeds my writing into a small large-language-model (SLLM?) with a custom proofreader skill, which I use *after* finishing the draft to spot all those snuck-in mistakes or inconsistent wording/tone. It should never change much, just use common best practices in writing and suggest those to me.

Here is the [link to the skill](https://gist.github.com/janschill/55b71e7d8fe51479aa9b29748bcd20a6), the prompt and the example output for this post:

```makefile
proofread:
	@claude -p "/proofreader Read $(file) and apply your suggested corrections inline using the Edit tool. For each change, use this format: suggested text. Do not remove or replace text directly, always show both versions so the author can review in Obsidian. Do not ask for confirmation." --allowedTools Read,Edit --model claude-sonnet-4-6
```

>My personal hurdles in writing are: getting started on a blank page and ~~secondly struggling~~ ==struggling== with my non-native language.
For the first struggle, I believe forcing yourself to write down one sentence~~,~~ often results in getting the ball rolling. Just let the thoughts flow and write them ~~done~~ ==down== as they come. You can clean up afterwards. But cleaning up afterwards is where the second hurdle for me just makes blogging not enjoyable and I stop. Tools like red or blue, squiggly lines under incorrect spelling or grammar mistakes have existed forever. *Grammarly* has also helped us ~~non- or even native~~ ==non-native or even native== writers for a long time.
>
>~~Now with generative AI everything changes~~ ==Now, with generative AI, everything changes== and you can generate blog posts from a few bullet
points and publish them on LinkedIn~~, that's where it seems at least everyone does it.~~ ==— that's where it seems everyone does it, at least.==
But it should be possible to harness those benefits and just tailor them to your needs without compromising your own writing.
>
>Agentic [skills](https://agentskills.io/home) and precise prompts work wonders here. I've integrated a script that feeds my ~~writings~~ ==writing== into a small large-language-model (SLLM?) with a custom proofreader skill, which I use *after* finishing the draft to spot all those snuck-in mistakes or inconsistent wording/tone. It should never change much, just use common ~~best-practices~~ ==best practices== in writing and suggest those to me.
>
Here is the [link to the skill](https://gist.github.com/janschill/55b71e7d8fe51479aa9b29748bcd20a6), ~~prompt and the~~ ==the prompt and the== example output for this post: