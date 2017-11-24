---
layout: post
title:  "Unearthed hackathon image processing"
date: 2017-09-10 08:40:09 +1100
categories: image processing, unearthed, hackathon, python 
---

Unearthed Sydney 2017 being my second hackathon was a very different beast to the govhack previously. There were 4 challenges which were fairly well speced and the focus was more of the idea being 'we have these problems and these data sets, please to brono work for us'. Most of the teams were data science heavy and for us I was our 'data scientist' which was limiting however I did manage to get a sketchy solution out.

The challenged we decided was provided by Boart Longyear (https://www.boartlongyear.com). Currently core samples are drilled and manual measurements are taken by trained geologists. These measurements are then used to produce a 3D model which is used to determine what deposits are available. These methods had fairly standard rulesets but there were subtle discrepancies amongst different geologists. With the increasing demand for copper for electric cars etc (he presented some stat around we have used most of our copper mined in the last 20 years and it is getting harder to find) it presented a good opportunity to improve this workflow and hence why the hackathon was chosen as a cheap way to see if there is a 'better way'. 

So really their current problem and pain points were so:
* Manual process with specific rule sets
* Requires trained expertise to perform
* Difficult to transfer data from core to modeling software (manual process)
* Hard to collaborate and check other geologists work

As a brief solution we made some basic mobile app that took a picture and performed very simple image processing to estimate some various angles.
