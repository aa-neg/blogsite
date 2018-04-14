---
layout: post
title:  "Project workflow integration"
date: 2018-04-14 08:40:09 +1100
categories: neovim, tmux, startup
---

So throughout work and side projects I've come across some common patterns and small inefficiencies I have noticed. I am always trying to be concious of patterns which could be improved upon in my everyday workflow.

So i'll go over some of the issues i've noticed before discussing some of the potential solutions.

* Firstly I have many different project types happening at work. There are various tools being built and depending if its a microservice a backend for a frontend or simply just frontend development my setup changes,

* Each project type has a particular window set-up required. For example frontend development needs chrome developer tools while working on an API service will look quite different.

* There is a disjoint and set-up time between swapping between workspace types and usually a single feature of business requirement will span multiple aspects.


So I guess this isn't really a new problem but something that I have noticed only more recently bouncing across a full stack. This is the same for my own projects mixing between playing around with flutter working on Todemy or just playing around writing rust cli tooling. Previously my editor and set-up of vscode with vim keys just was not enough.

I have recently swapped into utlizing tmux and neovim mainly to use the [tmuxp](https://github.com/tmux-python/tmuxp) sessions settings.

I'll probably go over my set-up in a future post as it matures but the ability to easily swap between and load sessions while also running arbitrary commands before hand was quite a game changer. In addition with bash completion plus a range of aliases my workflow has improved immensely.

Also I have been writing each month on our Todemy [blog](https://todemy.com/blog/)  :)



