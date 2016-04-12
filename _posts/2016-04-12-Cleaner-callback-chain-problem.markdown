---
layout: post
title:  "Cleaner version of callback async error catching problem"
date:   2016-04-11 08:40:09 +1100
categories: Javascript, promises, nodeJS, async
---

After discussing with <a href="https://gist.github.com/bryanwood/">bryan</a> he's made a cleaner version of the problem from the previous post.

{% highlight javascript %}
var async = require("async");

function doWorkSync(throwError, callback){
    try{
        if(throwError) throw new Error('I Threw');
        callback(null, 'OK');
    }catch(err){
        callback(err);
    }
}

function doWorkAsync(throwError, callback){
    try{
    setTimeout(function(){
        if(throwError) throw new Error('I Threw');
        callback(null, 'OK');
    });
    }catch(err){
        callback(err);
    }
}

async.series([
   function(callback){
       doWorkSync(false, callback);
   },
   function(callback){
       doWorkSync(false, callback);
   }
], function(err, results){
    console.log('First Series');
    console.log('Errors', err);
    console.log('Results', results);
});

async.series([
   function(callback){
       doWorkAsync(true, callback);
   },
   function(callback){
       doWorkAsync(false, callback);
   }
], function(err, results){
    console.log('Second Series');
    console.log('Errors', err);
    console.log('Results', results);
});
{% endhighlight %}

<h3>How to run it</h3>

Just change the first arguments in the async series functions to either true or false (if they should throw an error or not) to see the interactions. If you see the first implementation above we run through the first series without throwing an error then throw an error in the second series. Now you would expect that the second series would catch the error however when this is actually run, you'll find that the first series is catching the error instead. This is essentially the simplified problem from my previous post. 


Hope this opens to some discussions on exactly why this is happening. But regardless is an interesting nuance of javascript. 