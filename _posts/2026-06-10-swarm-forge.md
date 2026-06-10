---
layout: post
title: Swarm Forge
description: Looping with swarm-forge to build a web app
date: 2026-06-07
tags:
  - software-engineering
  - ai
  - claude
draft: false
---
The loop. Now that everyone is talking about not writing prompts anymore and just building loops, I had to try it myself. I saw [a post from Uncle Bob](https://x.com/unclebobmartin/status/2062557016086786435?s=20) the other day about his swarm-forge project.

It's a collection of shell scripts and pre-written prompts for different roles in the software engineering process: Architect, QA, Hardener, Coder, Specifier, and Cleaner. The idea is to take raw AI coding speed and wrap it in discipline: each agent reads a shared "constitution" of rules (tests, mutation testing, complexity limits) so the swarm produces something maintainable rather than a fast pile of slop. As Uncle Bob describes the roles: "The specifier writes both Gherkin and user oriented QA procedures. Coder codes. Cleaner refactors. Architect manages modules and dependencies. Hardener run mutation tests. QA runs the specifiers QA procedures."

Under the hood it runs each agent in its own tmux window and passes work between them through handoff files and a notify script. Each agent also gets its own git worktree, and these all get merged at the end. This way the agents can work in parallel without stepping on each other.

I'm currently working on a well-defined web project with one large requirements document. I started off by telling the Specifier to break down the requirements document and slice it into features. So far the six agents have produced a lot of artifacts. Mostly tests and very nice code. But the system had failed to realize it should wrap the project in an HTTP server. After I told it to do that, it started adding HTML and HTTP handlers to connect the features with an actual web interface. Interesting that the discipline is all there, but the obvious "this is a web app" framing wasn't. It built the parts correctly and waited to be told how they fit together.

It was cumbersome to keep telling the Specifier to continue once a feature was finished. This is where I added one more agent. Now I have a Claude Code session monitoring the Specifier and handing it more work when it's done: a loop watching the loop. Let's see what I have tomorrow when I wake up.