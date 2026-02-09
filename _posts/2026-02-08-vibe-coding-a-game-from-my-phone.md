---
layout: post
title: Vibe Coding a Game from my Phone
description: Using "Claude Code" on my phone to build a Wordle-like game.
date: 2026-02-09
tags:
  - "#claude"
---
Last Sunday I had a few hours to myself but not my laptop at hand. Sitting on the couch, I opened the Claude iOS app and noticed the Code section, which lets you chat to Anthropic's models in the context of a GitHub repository. I had been wanting to build something like this for a while.

Over Christmas my family and I played MyCity Legacy and loved it. My partner loved it so much, she bought the game for herself the day we left my family's place. The addictive loop of placing tiles to build your own little MyCity combined with the restrictiveness of Wordle, one unique puzzle per day and that's it, felt like a perfect combination for a daily web game. Like [Josh Wardle created Wordle for his partner](https://www.nytimes.com/2022/01/03/technology/wordle-word-game-creator.html), I wanted to build her something.

I set up a new GitHub repository, created a new session with the repository connected, and gave it a prompt:

> I would like to create a new web game for my partner and I. We love playing the NY Times games like Wordle, Connections etc. We recently played the board game MyCity Legacy and really enjoyed it as well. Now I was thinking of creating a MyCity-type of game with the NY Times aspects, such as: daily challenges, spend around 15 minutes on it, solo play, but then compare with peers. Let's make a plan and think about how we could design this game.

It proposed a daily polyomino puzzle with terrain scoring, a shareable emoji grid, and streak tracking. Exactly what I had in mind. With just a simple prompt, it understood the main mechanics of MyCity Legacy and of course knows about the NYT games and their details.

I told it to go ahead while I enabled GitHub Pages and created a new subdomain in Hetzner. When it reported back on finishing and pushing its work to the repository, I went to <https://bird-city.janschill.de> and there it was. A Wordle looking, MyCity Legacy-type of game. Incredible.

After playing a few rounds and not being fully pleased with how it looked, I gave it a few more prompts, which unsurprisingly broke things here and there, but eventually I had a game I was really pleased with.

We have been playing it every day and sharing our scores over Signal. The game is not perfect, and I have a growing list of things I want to tweak, but that is part of the fun. If you want to try it yourself, you can find it at [bird-city.janschill.de](https://bird-city.janschill.de).

![Bird City Game](/assets/images/birdcity.png)
