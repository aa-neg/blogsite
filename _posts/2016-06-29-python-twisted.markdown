---
layout: post
title:  "My python twisted introduction"
date:   2016-06-29 08:40:09 +1100
categories: python, twisted
---

So at work I've been moving towards more back-end centric work which is a change from my javascriptness (I have grown to really like the language especially writing the back-end server side with nodeJS). This existing project I have moved towards previously only ran synchronous code. To achieve performance is spawns multiple processes which then runs synchronous code. Now the challenge is that there is a new messaging system that needs to be integrated that uses python's twisted. I guess its just nodeJS for python it has callbacks deferreds all the jazz. One problem is the twisted library has a 'reactor' which is the event loop so once you have set up your business you need to start the event loop.

{% highlight python %}
from twisted.internet import defer, reactor

def asyncTaskOne():
    print "Hello I'm async task one!"

def asyncTaskTwo():
    print "Hello I'm async task two!"

reactor.calllater(0, asyncTaskone)
reactor.callLater(1, asyncTasktwo)

reactor.run()

print "After the reactor has run."

{% endhighlight %}

So this is a very basic example that runs two functions. Now the problem I ran into was that reactor.run() will now 'block' and not run the print statement until we add a reactor.stop() function somewhere to continue. This leads to the problem that ideally I would like to just connect leave the connection open to my messaging things (wait for new messages) then close if I really want to. Now this wouldn't be a problem if all the previous code was written this way however having the reactor block future executions is a problem.

So here is my solution I spawn a thread that contains that reactor event loop then call react.callFromThread(func, args) to send messages.

{% highlight python %}
from twisted.internet import defer, reactor
import threading
import time

def asyncTaskOne():
    print "Hello I'm async task one!"

def asyncTaskTwo():
    print "Hello I'm async task two!"

def reactorClient(): 
    reactor.calllater(0, asyncTaskone)
    reactor.run()

def stopReactorRunning():
    reactor.stop()

def callProcess():
    time.sleep(5)
    reactor.callFromThread(lambda ignore: asyncTaskTwo)

reactorThread = threading.thread(name = 'reactor_client' , target = reactorClient)
reactorThread.join()
callProcess()
reactorThread.start()

print "After the reactor has run."

{% endhighlight %}

So now when you run the above code the reactor client will start up then we will be able to call after 5 seconds asyncTaskTwo with the reactor.callFromThread() from the main thread. However I ran into this error message with the 'signal only works in main thread'. With this issue you also are unable to crtl+C out due to the fact python can't send the signal out to the process. The lambda ignore is used as the function does not take any arguments. 

So to solve this I simply added the installSignalHandlers=0 argument for reactor.run() and there will be no error messages however you will still be unable to ctrl+c the program as you will need to inject the reactor.stop().

So why is this helpful to spawn a different thread? Well the plan is to create an emitter that will wait until the react thread is finished processing the message then change the flag of this emitter such that the main thread can continue. So in essence I will achieve a synchronous wrapper and solution to this asynchronous client. 

Twisted has some nice syntax tweaks such as the @defer.inlinecallbacks combined with yield generators. However that will probably be for another blog post.

