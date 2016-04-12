---
layout: post
title:  "Hello World!"
date:   2016-04-01 08:40:09 +1100
categories: unit-testing, javascript
---

Unfortunately one of the devs in my team at work is leaving and is handing over his project which he has built over his 9 month time at the company. Just to preface it was my first opportunity to work with another dev on the same project and there were many aspects which I have learned and am very grateful for. I can safely say during this month long handover period has been quite a milestone in my mindset as a dev in addition to a huge change in work flow which in turn has really improved my ability to produce maintainable code. 

The project is written in node with an angularJS front-end. One of the the devs regrets was not writing unit tests throughout the project. We decided to implement them and integrate them into out bamboo system before he left. This was the first time i had wrote any tests even though I had seen the usefulness of such a system. We had used the <a href="http://jasmine.github.io/">jasmine</a> and <a href="https://karma-runner.github.io/0.13/index.html">karma</a> frameworks.

{% highlight javascript &}
describe("some controller", function() {
 	 
});

{& endhighlight &} 

Some of the problems I encountered: 