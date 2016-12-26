---
layout: post
title:  "Refactoring out relation database reliance"
date:   2016-10-30 08:40:09 +1100
categories: developer, knowledge, sharing, sevens, simulation
---

One of the larger projects at work had very strong tie to a relational database. This proved to be a very good example of 'pick the right tool for the job'. Currently I have refactored a very core part of the project the 'save and load to use a single atomic data structure and managed to literally delete just under two thousand lines of code. Now albeit most of this was due to the fact that most of this was inside the legacy code base.

One of the larger lessons learned here was the notion of storing data. We previously had a tendency to just blindly use an oracle database mainly because it was core in regards to the business logic. There were several projects where we decided to store metadata inside these relation oracle database tables.

Firstly there was a trend of storing all of this metadata inside the tables due to the fact we have already paid for the storage and most of the core business logic was tied with these oracle tables. However this lead to several unnecessary complications attempting to force this metadata into these oracle tables. The scope of what needed to be stored was constantly changing, so from a developer point of view there needed to be changes to both the staging schema in addition to the production schema. 
