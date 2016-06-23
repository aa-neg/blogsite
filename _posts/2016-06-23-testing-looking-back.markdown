
---
layout: post
title:  "Testing code, looking back"
date:   2016-04-015 08:40:09 +1100
categories: unit-testing, javascript
---

    Unfortunately one of the devs in my team at work is leaving and is handing over his project which he has built over his 9 month time at the company. Just to preface it was my first opportunity to work with another dev on the same project and there were many aspects which I have learned and am very grateful for. I can safely say during this month long handover period has been quite a milestone in my mindset as a dev in addition to a huge change in work flow which in turn has really improved my ability to produce maintainable code. 

    The project is written in node with an angularJS front-end. One of the the devs regrets was not writing unit tests throughout the project. We decided to implement them and integrate them into out bamboo system before he left. This was the first time i had wrote any tests even though I had seen the usefulness of such a system. We had used the <a href="http://jasmine.github.io/">jasmine</a> and <a href="https://karma-runner.github.io/0.13/index.html">karma</a> frameworks.

    Writing unit tests for code that had many dependencies. So when testing the front-end in angular you have to mock out all the injected modules. You have options to allow them to hit the actual modules /services then intercept the http back-end calls when they are invoked. This posed quite a problem as sometimes writing the test felt like the same amount of code and logic as writing the actual program. I think learning from this however I can see ways in which writing tests really helps improve the quality of your code produced. 


Now a few months later from this event I have been writing tests and I think my understanding of the idea has grown considerably.

Firstly lets say you want to test the following function

{% highlight javascript %}
$scope.theBestFunction = function() {
    $scope.someotherFunction();
    angular.forEach(someGlobalArray, function(arrayItem) {
        arrayItem = arrayItem + "I'm a mutation!";
        someService.callAdditionFunction(arrayItem);
    });

    return someGlobalArray;
}
{% endhighlight %}

Now writing a test is fine but what i want to illustrate here is that although your test will pass doesn't mean that the code is any good. 

You begin writing the test and now you notice you have to mock our several other pieces which arn't obvious when first reading the function. This should be an immediate code smell and I think its worth considering if you should be passing a copy of the object (so you don't mutate outside the function's scope).

So below would be an example of mocking this function

{% highlight javascript %}
describe('theBestController', function() {
    var theBestController;
    var mockSomeGlobalArray = [];

    beforeEach(function() {
        module('theBestApp');

        inject(function(_$controller_, someService){
            controller = _$controller_;
        })
    });

    describe('thebestFunction', function() {
        it('Should obviously work cause its da best!', function () {
            expect(theBestFunction).toBeDefined();
            $scope.theBestFunction();
            expect(mockSomeGlobalArray.length).toBeGreaterThan(0);
        })
    })
})

{% endhighlight %}

So here your test will pass and you will feel like now you have awesome code coverage and that your system is the best and awesome!

Now this is more of sudo code (won't work and syntax is probably wrong) however the point i'm trying to illustrate is that I found that writing tests wasn't enough (might seem like the above is obvious however this was the trap i fell into). You have to write the actual functions to be meaningful and well designed to have meaningful and well designed tests. I think both go hand in hand. If you would just like to see if you application works I think that writing integration tests are a much better use of your time. You just start it up send some things in and see if they come out the other side as you expect. Writing unit tests however I found requires alot of additional work which usually doesn't amount to much other than over-head unless you have constructed your application accordingly. Just my few cents in what i noticed when first implementing these unit tests. As I am now writing this after the initial draft from above (several months after) and what i noticed was that the unit tests weren't finding bugs due to the fact I was not writing meaningful tests nor structuring my actual application such that it allows for meaningful tests, meaning it has a clear input and output and you know what parts it mutates. You can then test input and output and mock and test on what parts the function will interact with. I think this consideration has been a very important lesson which I am still evolving.

This first introduction a few months back of testing however has really helped shape my development as for example during one of my university assignments we had to mock out a queuing service then run some statistical analysis on it. I was able to consider how to write that python script in such a way that I was able to write tests that show that:

1) With a a seed the results were deterministic (tests were repeatable)
2) Different seeds actually gave different results
3) The arrivals were processed throughout each module appropriately

So here I combined the idea of unit tests and integration tests to show that my system did what I designed it to do. Well hopefully I did...

Overall I think the main thing I've learned from this is that writing tests is good however I think It is a two fold process. I can see now why test driven development is popular as considering your tests first means that the code you produce will achieve the goal and be easily testable. 