---
layout: post
title:  "Deploying with docker"
date:   2016-08-29 08:40:09 +1100
categories: developer, knowledge, sharing, docker
---


So at work i've managed to muster up a linux 'production' server (ubuntu server 16.04). Now all my previous docker workflow can be used for production. All these tools are just used internally and currently there isn't any performance constraints on any of the projects. 

So theres been quite a few steps in setting this up but this is how I've currently done it:

1) checkout your project into the /projects root folder of the server

2) Spawn a detached container running bash based on an image sharing the folder

docker run --volume /project/project-name:/project -d --name project-name -p 7777:7777 -m "300M" -it arnolda/your-image-name /bin/bash

So there's quite a bit happening here firstly we share the project folder in with volume, we run it detached with -d, name the project, share the port across the container, allocate memory (can also add a swap if necessary), open an interactive terminal based on the image and run bash.

3) View your container with : docker ps

4) Attach yourself inside the container: docker attach project-name

5) We use screen to run the actual applications so:

installation steps,

screen

run application

ctrl-a 'd'

Now screen will be running in the background and to exit the container without killing the process we run

ctrl-p ctrl-q




<h5>Why bother with docker?</h5>

So docker solves several problems which we previously had:

1) production environment being the same as the development environment

2) All native environment configurations on the 'production' server are separated

3) Easy to restart all applications (restart all containers) when the server is rebooted.

4) Users adjusting their projects environment will not corrupt other installations. e.g someone installing different version of node.js or python



<h5>Final thoughts </h5>

I'll probably expand on my thoughts in the future however so far its been such a great experience working with these changes and I can highly recommend the workflow. Since currently my development machine is a windows box i've been developing all my projects inside docker containers running the VM. Its been nice to use this workflow into my production / staging builds being commandline based theres still more automation which can be achieved here.

