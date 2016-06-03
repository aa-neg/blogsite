---
layout: post
title:  "Docker development environment for windows."
date:   2016-06-03 08:40:09 +1100
categories: Javascript, Docker, nodeJS, Windows
---
So at work I invested in some R&D on the Docker technologies. At work everything is a windows environment and we are currently writing essentially small micro services / local web apps for at most 12ish users and for our deployment theres just a machine that runs these services. My current project was using node 0.12 which meant that the our 'production' machine was using an older npm and node to house the project. This is obviously not ideal as future projects may conflict and cause issues with varying versions. In addition this required the node oracledb to be installed which required various environment variables set up with the correct C++ compilers which has not been the most trivial task to install on some cluttered machines.

<strong>Our current solution Docker!</strong>

So docker runs on a VM (cause of our windows environment) but allows for very easily configurable images (very easy to setup with detailed docks and active community) to solve this problem. Now although Docker does not recommend this for production given our current requirements these aren't such an issue as our applications will not be public facing. 

First you can easily setup any terminal (powershell , cmd, console2, cmder etc) to link the shell to the vm. The command will list a command to copy and paste to create this link.

{% highlight javascript %}
docker-machine env
{% endhighlight %}

Next I had setup a stash repo with a bunch of directories containing a docker file and oracle all packaged ready to be built. It was as easy as just running in the directory with the relative Dockerfile and requirements (oracle).

{% highlight javascript %}
docker build -t arnolda/oracle-node-image .
{% endhighlight %}

The beauty of this is that now everything the developer will need will be packaged within this container. All the npm globals setup (grunt, gulp, bower etc) oracledb setup with the environment variables, the same bash terminal as this is through a linux box and all within 500mb of space. 

To setup the development environment I simply shared a folder through the virtual box then mounted it into a container based on the above image. (you can go through the virtual box manager and add these shared folders)

{% highlight javascript %}
docker run -p 8080:8080 --volume /c/Code:/Code -it arnolda/oracle-node-image
{% endhighlight %}

So theres quite a bit happening in the above command firstly the -p tag will expose the port (which has been exposed in the Dockerfile) so that our webserver can be accessed out of the VM the --volume tag will mount the shared folder of called /c/Code into the Code directory in the root of the container and -it runs an interactive terminal with the above image.

Thats it now you can simply use all the your fancy IDE's/editors in your windows environment however all terminal interaction and testing is done through the containers bash entry-point. Now when you run and test your websrver you know that if it is working in this container environment it will work in production as you will be deploying it with the same image. In addition this works similar to a python virtual env where all requirements will not pollute your machine with any environment variables (oracledb . . .) but instead allow you to have the simplicity of a linux development environment rather than the windows option which does take time to setup.

So now during our bamboo deployment I have setup docker to be built and run with the server all in a single build file. This now has the additional advantage of being able to bash into the production container to either debug view logs (although probably not ideal but it does offer this for when the situation is required). 

I'm sure this has been done with Vagrant boxes etc in the past however I do just love how easy it is to setup a dockerfile with everything pre-built and working. In addition the fact that you can continue to build the image as you add to the environment (possibly add an environment dependency) you can then commit your changes as the next iteration of the image. This allows the ability to simply revert not only the codebase through git but the actual environment in which you are developing (maybe you added removed some environment variables and things aren't going as planned). Its as simple as destroying the container process and restarting a new container shell.

Its just nice having your development environment (terminal interaction) with a reliable terminal with commands and env variables with the easy of use of a windows OS in terms of browsing and user interface. You can run all your other programs not part of development such as your browsers / editors / skype etc in windows but the development systems that matter will just work. I think this is development system allows for the best of both worlds and I do hope to improve this development cycle after some time seeing some of its weakness.