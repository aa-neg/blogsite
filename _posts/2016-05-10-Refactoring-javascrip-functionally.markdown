---
layout: post
title:  "Refactoring javascript"
date:   2016-05-10 08:40:09 +1100
categories: Javascript, promises, nodeJS, async
---

Recently I've been refactoring a large amount of javascript and I thought i'd share some of the concepts I have been employing during this process.

<strong>Why did I decide to refactor?</strong>

Given the fact that I have taken over this project i've been trying to limit the amount of refactoring previously as I was focusing on pushing out the product. However reliability of the product has come into question since previously there was no tests for any of the features or the code base. This has led me to refactor the code using a functional programming concept of breaking things down into reuseable simple functions. One of the main reasons was the simplicity of testing a function but doing so has shown light on some of the benefits of programming javascript with a functional mindset. 

This gave me insight into structuring the projects API. Before each API function contained all the logic within that function. Now i wrap all the logic within another function exposed by a promise. 

{% highlight javascript %}
exports.SomeAPIFunc = function (req, res) {
	var responseObject = {
		errors: [],
		data: []
	}
	oracleDB.connect(someConnectionInformation, function(err, connection){
		if (err) {
			throw err
		} else {
			connection.execute(
				someQuery,
				someParams,
				function(err, results){
					if (err) {
						throw err
					} else {
						results.forEach(function(result){
							result += someFormatt;

						})
						res.JSON(responseObject);
					}
				}
			)
		}
	})
}
{% endhighlight %}

So the above is the standard API sure its simple to just copy pasta the above and scatter it throughout the project however by wrapping the function below in a promise I can reuse the API call So lets wrap this logic into a promise

{% highlight javascript %}
var someFunc = exports.someFunc = function(responseObject) {
	return new Promise(function( resolve , reject){
		//Code in here but we resolve(responseOBject) and reject(errors)
	});
}

exports.SomeAPIFunc = function(req, res) {
	var responseObject = {
		errors: [],
		data: []
	}

	someFunc(responseObject)
	.then(function(results){
		res.JSON(results)
	})
	.then(function(err){
		logger.error("Shit happened damn.");
		res.JSON(err);
	})
}
{% endhighlight %}

So now the API just exposes our function that returns a promise and this function can be used else where. However it is still messy with a connect and then an execute. One way to simplify this is to use a connection pool but currently our application will connect , execute/query/commit and then disconnect. So we can seperate this into their own functions.

{% highlight javascript %}

function connect(connectionInfo) {
	return new Promise(function(resolve , reject){
		oracleDB.connect(connectionInfo, function(err, connection){
			if (err){
				reject(err)
			} else {
				resolve(connection);
			}
		})
	})
}

function query(connection, query, params) {
	return new Promise(function(resolve ,reject){
		connection.execute(query, params, function(err, result){
			if (err) {
				reject(err)
			} else {
				resolve(result);
			}
		})
	})
}

function runQuery (connectionInfo, query, params, formatter) {
	return new Promise(function(resolve, reject){
		connect(connectionInfo)
		.then(query(connection, query, params))
		.then(formatter(results))
		.then(function(formattedResults){
			resolve(formattedResults);
		})
		.catch(function(err){
			reject(err);
		})
	})
}

{% endhighlight %}

Now we have split the executing function into various stages then used another function to combine these steps together. Note with the use of these promises we now have the option to wrap this again in a generator. (but that will be for another post)

<h3>This seems like we are adding complexity...</h3>

While at first we are increaseing complexity when the project grows this actually helps simplify the entire project. Since we are reusing these functions we can now test each component (unit tests) and also run integration tests (the whole api or runQuery function). This also allows for a higher level overview of the logic and it also includes error handling for us. This is the main advantage I have seen with this refactoring which is splitting the complexity into function and then combining these functions to form simple steps. With each of these steps having the ability to be simply tested and mocked easily. 

Now when we want to add additional logic we simply encapsualte that logic in a function in insert it into the higher level overview pipeline of the logic. 

{% highlight javascript %}

exports.someFuncApi = function(req , res) {
	var responseObject = {
		errors: [],
		data: []
	}

	someFunc(responseObject)
	.then(additionalFormatting(results))
	.then(CommunicateToOtherFunctions(results))
	.then(function(finalResults) {
		res.JSON(finalResults);
	})
	.catch(function(err){
		res.JSON(errors);
	})

}

{% endhighlight %}

Although the above code will not work the overview of the logic is quite clear and simple to understand and follow. Also it allows for future development to insert logic into the pipline and for other API's and functions reuse these options.

I really do think embracing the initial complexity of functional javascript will reap great rewards further down the road (as most people writing with this mindset will tell you). Just a disclaimer I am still really new to all of this so if any information is incorrect it is due to my lack of understand which I'm more than happy to be corrected!

