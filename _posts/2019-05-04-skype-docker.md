---
title: "Docker all the things (at least skype)"
categories: Linux
date: 2019-04-08
tags:
  - linux
  - docker
  - skype
---

I hate using Skype these days. It's a mess, the interface is all over the place, it requires you to install a ton of things on the system, it crashes, and so on and on. But, by some punishment doled out by destiny, I have to use it. So instead of installing on my system, I took a page from Jessie Fraz and decided to run in inside a Docker container. What, you say, am I crazy? Maybe. But I though it would be interesting enough to try.

First, one think that I also dislike about Docker: you have to keep pulling tons of stuff from the internet, and it's not clear when you're going to do that, and from where you are downloading stuff. This makes me uncomfortable, so I've tracked from where Docker is getting its images, for example. 