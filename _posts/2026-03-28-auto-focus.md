---
layout: post
title: Auto-Focus
description: A macOS menu bar app that detects developer flow states and silences notifications automatically—built with Go and AI assistance.
date: 2026-03-28
tags:
  - go
  - macos
  - productivity
  - ai
---
My first AI-assisted project was [Auto-Focus](https://auto-focus.app). A while ago, I read an article from Meta on an [internal research project](https://research.facebook.com/publications/workgraph-personal-focus-vs-interruption-for-engineers-at-meta/) where they discovered how asynchronous communication tools tend to break *the flow* of engineers and how difficult it is for individuals to get back into it, after a seemingly innocent "Hey, do you have a moment?" message.

I've faced this issue too many times myself and replicated their internal tool with the same name and turned it into a macOS menu bar application that runs in the background, detects *flow*, and consequently disables notifications for you.

I started it off as a small and hacky Go binary, before Cursor, Claude Code, and their kind really took off and it worked well. It leveraged AppleScript and a macOS Shortcut to toggle the *Do not disturb* focus mode.

Telling non-tech folks about it and hearing their enthusiasm ~~,~~ really motivated me to make it usable for *everyone*. So, I did, or rather my *clankers*.

I plan to write more about the project, approaches, and lessons in the future. But for now, if you would like to try it, you can download the app on [auto-focus.app](https://auto-focus.app) or check out the code and compile it yourself on [github.com/janschill/auto-focus](https://github.com/janschill/auto-focus).