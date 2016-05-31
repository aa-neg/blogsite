---
layout: post
title:  "Initial musings in functional programming"
date:   2016-05-10 08:40:09 +1100
categories: Javascript, promises, nodeJS, async
---

The title could be misleading however during my time in javascript I have grown much appreciation to the benefits of writing javascript with a functional programming mindset. Some of the nice paradigms that functional brings is the concept of composing your codebase into many functions and applying functions on these functions. In addition the concept of immutability which in my mind helps make the code predictable due to the fact that you will mutating limited parts of your application. 

In addition to this is introduction I have begun to see what everyone else who was programming in this paradigm was mentioning about different 'types' of functions and the concept of 'pure' functions atleast in my understanding (which will evolve I hope).

So heres an example of a function that will iterate over a list then sum the values of such list. I will show what I have learned to watchout for when constructing various different kinds of functions. I will show various implementations of the same idea. I think this is useful to be aware of when programming functions as since you will usually breakdown your problem into many sub functions its important to consider what affect these subfunctions will have upon various data structures as this will affect your decision in composing these functions together. 

{% highlight javascript %}

SumList = (myList) => {
    var total = 0;
    for (i = 0; i < myList.length; i++) {
        total = total + myList[i];
    }
    return total;
}

var myList = [1,2,3];
console.log(SumList(myList))

{% endhighlight %}

In this first example we introduce a variable contained in the functions scope and mutate it over and over again until we are done and spit it out as our result. It does not change or mutate its input of myList.

Here is another implementation of the same concept.


{% highlight javascript %}

SumList = (myList) => {
    var total = 0;
    for (i = 0; i < myList.length; i++) {
        total = total + myList.pop();
    }
    return total;
}

var myList = [1,2,3];
console.log(SumList(myList))

{% endhighlight %}

So here we in addition to mutating our local scope variable will mutate the list by popping off the numbers until there is no list left to consume. This is the first interaction this function has to the outside input as it will have an affect to data structures outside its scope.

Now heres a similar method however using recursion

{% highlight javascript %}

SumList = (myList) => {
    if (myList.length == 0) {
        return 0;
    } else {
        return myList.pop() + SumList(myList)
    }
}

var myList = [1,2,3];
console.log(SumList(myList))

{% endhighlight %}

So here we still mutate our list however we have removed the inner scope mutation. Then to take this one step further lets create copies of the list and make each copy 'immutable' before sending it off into the next step.

{% highlight javascript %}

SumList = (myList) => {
    if (myList.length == 0) {
        return 0;
    } else {
        return myList.pop() + SumList(new Immutable.List(myList))
    }
}

var myList = Immutable.List([1,2,3]);
console.log(SumList(myList))

{% endhighlight %}

This is just sudo-code for this immutability concept but now the advantage here is that we didn't change the initial input and now we have this added ability of having a path of states in which we have walked. This was one of the larger advantages I saw when exploring these functional concepts was the idea in which recursion gave the possibility to walk back through the states. So you are able to see along each step what is exactly happening with the concept of immutability. Where with a mutable implementation you then have to add the 'saving' and snapshotting of the states as you arn't sure exactly how you got from a to b. There are disadvantages however which do include performance (recursion vs loops, additional memory for the states etc) although one of my biggest lessons has been to code with the idea of correctness, maintainability and readability in mind first then tweaking for performance later and in my mind this paradigm and concept greatly adheres to the former. 

Currently at work I have been working the front-end side of things with anuglarJS, however through this exploration of different function types I have been more inclined to try ReactJS + Redux instead due to these advantages and some of their claims about their developer debugging tools.

Which will probably be one of my next blog posts...
