---
layout: post
title:  "Terminal based developer workflow in Windows"
date:   2016-07-06 08:40:09 +1100
categories: vim, conemu, windows
---

So with my development work at work currently moving towards creating command line tools and away
from creating web interfaces I have found some improvement in moving my workflow towards a terminal
environment with ConEmu and vim.

So this is inspired from my recent delve into Arch Linux and my set-up is heavily inspired by the
Xmonad tiling window manager. To acheive a similar affect (not the same but close enough for now)
I use ConEmu http://conemu.github.io/en/VimXterm.html . It allows you to set hotkeys for creating
a terminal split in various directions and to move between them. I currently have set-up the ConEmu
terminal to startup with a windows short cut to ctrl+alt+t. If I am running any servers I run
a cmder (http://cmder.net/) terminal in quake mode which will come down with ctrl+` regardless of
which screen I am currently on. 

Conemu shortcuts:

  alt+shift+b : Split current active window below
  alt+shift+r : Split current active window right
  alt+shift+j : Move to next terminal
  alt+shift+k : Move to previous terminal
  alt+shift+c : close active terminal

With this setup I'm able to mimic some of the tiling window workflow (I still have to alt-tab to
move across screens (3 screens at work) i'm still trying to find better solutions for this). Next
I previously used sublime mainly due to the fact its really user friendly and has nice features such
as auto reload to latest version of the file when activated (helps when editing really large files
you can just run multiple copies of it and not run into conflicts) however I have recently moved
towards a terminal vim editor to enhance my workflow. 

Now you can find my crappy vimrc from here (https://github.com/aa-neg/vimfiles) theres a few nice
plugins which help emulate some of sublimes features. It was quite an enjoyable experience setting
up my vimfile from scratch and i can see myself staying with this editor for quite a while (I
usually do move around quite a bit).

<h5>Advantages of this workflow</h5>

Firstly the advantage I saw straight away was the fluidity of writing code and then running it. Next
was the easy to quickly just close the terminal running the process (I was writing some shitty
python code that involved threads so didn't close with a simple ctrl+c command) and reopen to that
location and re-run the program. Next it really forced me to learn vim properly (each time i wanted
to do something with a mouse i had to google how to do it in vim) so was a great learning
experience. In addition I began to improve my skills with the terminal tools such as cat , grep
(obviously I stil have such a long way to go however it is good to start). 

Now I still use sublime to navigate projects i'm not familiar with as if I am not doing any
typing and just reading and understanding the structure sublime has such great tools such as the
right hand bar that shows the general overview of the file and great mouse support. However for
actual development I have found advantages with the vim setup. I think it is important to have
a knowledge of a vast range of tools as such tools will be better suited to the job. For me atleast
there is enjoyment in working with this set-up and I am learning much more than I would be just
staying with sublime although with this I can see the cases where sublime would make more sense to
use.




