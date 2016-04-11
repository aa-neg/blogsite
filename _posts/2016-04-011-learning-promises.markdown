---
layout: post
title:  "My taste of implementing promises"
date:   2016-04-11 08:40:09 +1100
categories: Javascript, promises, nodeJS, async
---

Currently at work I'm refactoring some code. I've quickly written a high level overview of some of the implementations to deal with the asynchronous nature of javascript in one of the libraries I'm refactoring. The whole point for the re-factor was to add error handling throughout the library. In addition to order the library such that each component was exported such that to make it easier to unit test (no unit tests are currently there for all components).

I faced this issue which is mocked out below. Essentially I have an async series function which calls several functions with a callback object (from the async library but these are really promises with an err and result component). In the secondFunction I implement a chain of promises with a catch statement. Now I purposefully error the thirdFunction and this is caught within the secondFunction's catch statement! 

{% highlight javascript %}

var async = require("async");
var promise = require("promise");


try {
  async.series({
    componentOne: function(callback) {
      firstFunction(callback);
    },
    componentTwo: function(callback) {
      secondFunction(callback);
    },
    componentThree: function(callback) {
      thirdFunction(callback);
    }
  },
  function(err, components){
    console.log(err);
    console.log("These are the components")
    console.log(components);
  })
} catch(err) {
  console.log("The outer scope error");
  console.log(err);
}
{% endhighlight %}

What is happening above:

This simply iterates over the first object till all callbacks have been invoked. This is then shown in the components (results) of the second argument (the function call). The usage of an object allows the results in an object format i.e {componentOne: results from component one, componentTwo: results from component two}. The error argument of the function does the same thing however holding the errors from the callbacks instead.

The callbacks contain two arguments callback(err, result). Each result or err that is passed back is then appended to the relative objects. If an error is passed back the rest of the async chain will not be called however. For work there are 'soft errors' which I do want to pass back however I still want the chain to continue below is a quick overview of how this is produced. 

Now heres the list of the functions which are invoked in the async series.

{% highlight javascript %}

// This first function is to illustrate the async series
function firstFunction(callback) {
  console.log("this is the first function");
  callback(null, 'firstFunction was called');
}


function secondFunction(callback) {

  var return_object = {
    results: [],
    errors: []
  }

  databaseConnection('some credentails', 'derp')
    .then(executeQuery)
    .then(manipulateResults)
    .then(cleanResults)
    .catch(function(err){
      console.log("This error occured in the promise chain: " + err);
      return_object.errors.push('Second function call broke')
      callback(null, return_object);
      return;
    });

  // connect to some database
  function databaseConnection(credentials, callback) {
    return new Promise(function(resolve, reject) {
      console.log("Started databaseConneciton")
      var connection = {
        name: "This is a databaseConnection object"
      }

      var resolve_object = {
        connection: connection,
        callback: callback
      }
      console.log(resolve_object);
      resolve(resolve_object);
    })
  }

  // invoke some query on the connection
  function executeQuery(connection) {
    return new Promise(function(resolve, reject){
      console.log("executing some queries")
      var  resolve_object = {
        connection: connection,
        callback: callback
      }
      var results = ['row1', 'row2']
      resolve(resolve_object);
    })
  }

  // Do things with the results
  function manipulateResults(results) {
    return new Promise(function(resolve, reject) {
      console.log("manipulating results");
      console.log(results);
      // this should move into the catch statement
      // console.log(breakingCall);
      resolve(results)
    })
  }

  // function won't be called simply comment out the above consolelog for this to resolve.
  function cleanResults(results) {
      console.log(results.connection);
      return_object.results.push('Everything went fine heres some results')
      callback(null, 'some results from the second function');
  }

}

function thirdFunction(callback) {
    console.log("this is the third function");
    console.log(catchThis);
    callback('error from third function', 'thirdFunction was called');
    return;
}


{% endhighlight %}

The interesting part: The error is caught within the second function rather than the async series function!

Now this may seem like quite a large amount of garbage however essentially the second function contains my implementation of a promise chain. Now the third function which would have been called in the outer async series which contains all of these functions gets caught in the catch statement which I've used in the second function. 

You can confirm this by commenting out the 'catchThis' console.log. In addition you can experiment by uncommenting the console.log(breakingCall) in manipulateResults within the second function. This from my understanding is the fact that async.series uses a promise chain and the callback is continuing that chain. So when the chain has broken it will seek the next 'catch' clause to spit out its error messages. Hence interestingly the error appears in my artificial error object in the second function rather than in the async series section.

Below is the full script. I hope this illustrates the problem I faced (and my conclusion which may be wrong) but regardless may help as a learning exercise for others to play with the idea of promises using both the async and promise libraries for nodeJS. Please feel free to fork the script from my github https://github.com/aa-neg/nodeAsyncTest 

FullScript:

{% highlight javascript %}

var async = require("async");
var promise = require("promise");


try {
  async.series({
    componentOne: function(callback) {
      firstFunction(callback);
    },
    componentTwo: function(callback) {
      secondFunction(callback);
    },
    componentThree: function(callback) {
      thirdFunction(callback);
    }
  },
  function(err, components){
    console.log("We have entered the results of the async series");
    console.log(err);
    console.log("These are the components");
    console.log(components);
  })
} catch(err) {
  console.log("The outer scope error");
  console.log(err);
}

// This first function is to illustrate the async series
function firstFunction(callback) {
  console.log("this is the first function");
  callback(null, 'firstFunction was called');
}


function secondFunction(callback) {

  var return_object = {
    results: [],
    errors: []
  }

  databaseConnection('some credentails', 'derp')
    .then(executeQuery)
    .then(manipulateResults)
    .then(cleanResults)
    .catch(function(err){
      console.log("This error occured in the promise chain: " + err);
      return_object.errors.push('Second function call broke')
      callback(null, return_object);
      return;
    });

  // connect to some database
  function databaseConnection(credentials, callback) {
    return new Promise(function(resolve, reject) {
      console.log("Started databaseConneciton")
      var connection = {
        name: "This is a databaseConnection object"
      }

      var resolve_object = {
        connection: connection,
        callback: callback
      }
      console.log(resolve_object);
      resolve(resolve_object);
    })
  }

  // invoke some query on the connection
  function executeQuery(connection) {
    return new Promise(function(resolve, reject){
      console.log("executing some queries")
      var  resolve_object = {
        connection: connection,
        callback: callback
      }
      var results = ['row1', 'row2']
      resolve(resolve_object);
    })
  }

  // Do things with the results
  function manipulateResults(results) {
    return new Promise(function(resolve, reject) {
      console.log("manipulating results");
      console.log(results);
      // this should move into the catch statement
      // console.log(breakingCall);
      resolve(results)
    })
  }

  // function won't be called simply comment out the above consolelog for this to resolve.
  function cleanResults(results) {
      console.log(results.connection);
      return_object.results.push('Everything went fine heres some results')
      callback(null, 'some results from the second function');
  }

}

function thirdFunction(callback) {
    console.log("this is the third function");
    console.log(catchThis);
    callback('error from third function', 'thirdFunction was called');
    return;
}


{% endhighlight %}



<h4>Why did this happen?!</h4>

Well this is because async runs synchronously whole the promise library invokes an asynchronously. Now look at the below changes (added timesout to everything). When this is called the reference error is thrown as expected. So what I managed to do was in the same cycle was chain the error while the 'then' statements were being invoked. 

<h5>Lesson learned?</h5>

Javascript. . . 
But will not put an asynchronous loop inside the synchronous async cycle or just somehow learn to code it better :)





{% highlight %}

ReferenceError: catchThis is not defined
    at thirdFunction (C:\code\nodeAsyncTest\nodeAsyncTest.js:116:17)
    at async.series.componentThree (C:\code\nodeAsyncTest\nodeAsyncTest.js:14:7)
    at C:\code\nodeAsyncTest\node_modules\async\lib\async.js:718:13
    at iterate (C:\code\nodeAsyncTest\node_modules\async\lib\async.js:262:13)
    at C:\code\nodeAsyncTest\node_modules\async\lib\async.js:274:29
    at C:\code\nodeAsyncTest\node_modules\async\lib\async.js:44:16
    at C:\code\nodeAsyncTest\node_modules\async\lib\async.js:723:17
    at C:\code\nodeAsyncTest\node_modules\async\lib\async.js:167:37
    at null._onTimeout (C:\code\nodeAsyncTest\nodeAsyncTest.js:108:9)
    at Timer.listOnTimeout (timers.js:92:15)

{% endhighlight %}

New script:

{% highlight javascript %}

var async = require("async");
var promise = require("promise");


try {
  async.series({
    componentOne: function(callback) {
      firstFunction(callback);
    },
    componentTwo: function(callback) {
      secondFunction(callback);
    },
    componentThree: function(callback) {
      thirdFunction(callback);
    }
  },
  function(err, components){
    console.log("We have entered the results of the async series");
    console.log(err);
    console.log("These are the components");
    console.log(components);
  })
} catch(err) {
  console.log("The outer scope error");
  console.log(err);
}

// This first function is to illustrate the async series
function firstFunction(callback) {
  console.log("this is the first function");
  setTimeout(function() {
    callback(null, 'firstFunction was called');
  }, 0);
}


function secondFunction(callback) {

  var return_object = {
    results: [],
    errors: []
  }

  databaseConnection('some credentails', 'derp')
    .then(executeQuery)
    .then(manipulateResults)
    .then(cleanResults)
    .catch(function(err){
      console.log("This error occured in the promise chain: " + err);
      return_object.errors.push('Second function call broke');
      setTimeout(function() {
        callback(null, return_object);
      }, 0);
    });

  // connect to some database
  function databaseConnection(credentials, callback) {
    return new Promise(function(resolve, reject) {
      console.log("Started databaseConneciton")
      var connection = {
        name: "This is a databaseConnection object"
      }

      var resolve_object = {
        connection: connection,
        callback: callback
      }
      console.log(resolve_object);
      setTimeout(function(){
        resolve(resolve_object);
      }, 0);
    })
  }

  // invoke some query on the connection
  function executeQuery(connection) {
    return new Promise(function(resolve, reject){
      console.log("executing some queries")
      var  resolve_object = {
        connection: connection,
        callback: callback
      }
      var results = ['row1', 'row2']
      setTimeout(function(){
        resolve(resolve_object);
      }, 0);
    })
  }

  // Do things with the results
  function manipulateResults(results) {
    return new Promise(function(resolve, reject) {
      console.log("manipulating results");
      console.log(results);
      // this should move into the catch statement
      // console.log(breakingCall);
      setTimeout(function(){
        resolve(results)
      }, 0);
    });
  }

  // function won't be called simply comment out the above consolelog for this to resolve.
  function cleanResults(results) {
      console.log(results.connection);
      return_object.results.push('Everything went fine heres some results')
      setTimeout(function(){
        callback(null, 'some results from the second function');
      }, 0);
  }

}

function thirdFunction(callback) {
    console.log("this is the third function");
    console.log(catchThis);
    setTimeout(function(){
        callback('error from third function', 'thirdFunction was called');
    }, 0);
}

{% endhighlight %}