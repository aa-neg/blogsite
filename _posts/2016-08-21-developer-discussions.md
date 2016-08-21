---
layout: post
title:  "Sharing knowledge amongst a developer team"
date:   2016-08-20 08:40:09 +1100
categories: developer, knowledge, sharing
---

So currently within my current team in the company there are only 3 developers. One is located in a different office so currently there is only 1 developer for discussions. One of the weaknesses which I wanted to address within the team was the lack of a code review or discussion on what we were currently working on. The problem is however we do have 3 meetings each week (relatively short < 30 mins) to discuss what we are currently working on. However the problem which I am facing is that new knowledge or ways to solve problems or what problems we are currently facing are not discussed or brought up. 

Now in all honesty the problems we are solving are not the most difficult in the world however there have been interesting methodologies or techniques I have introduced which seem to be a learning experience only for me. Ideally i'd like some way to share the knowledge or to have discussions on how to solve various tasks and problems. I can see several problems and reasons for why this has been the case. Firstly the team all works on different projects and the company has a a history of 1 developer 1 project. This in itself makes it difficult as the time it takes to articulate the current problem for a reasonable discussion usually takes a while or the problem requires quite a bit of background knowledge about the project and the current state of where things sit. Secondly not everyone has the same skillset and knowledge so for example sometimes I will have a javascript specific question and unfortunately the only place i can turn to is my own group of developers outside of work for discussion. 

To attempt to address this problem I usually will ask the other developer what he is currently working on in hopes to spark a conversation and see if he has learned anything new or interesting which I could piggy back off his knowledge. Sadly for the past 2 months though I haven't been able to mine anything. Recently however we did have a discussion on an idea which he had. Currently nearly all of our projects interact with an Oracle database and we interact with the database through a library and construct our query/execute strings. He suggested creating all these call strings as procedures inside the schemas instead for 'performance gains' and for a common place for where all the database actions live. Currently there is no real standard and sometimes the queries are coded within the backend or read from a directory of sql files. 

I voiced several problems which I thought about from his idea. Firstly it would be difficult to source control the procedures inside the schema. There could be conflicts with multiple developers (currently there already could be as table data is shared). In addition I didn't see how this standard would be better than what was currently set nor did I see why we would be getting any performance benefits. 

The point i'm trying to convey here is that the discussions within our team are not about how we are solving problems but I noticed instead its more individual and all my discussions have been quite hypothetical and not very practical about how we would be implementing various features or tasks. There is an additional developer coming on board in the future (next month) and I hope that the skill set which she brings will be enough to invoke discussions and hopefully some kind of code review can be taken place as I think that sharing knowledge and working as a team can really improve the atmosphere and productivity of a team.





