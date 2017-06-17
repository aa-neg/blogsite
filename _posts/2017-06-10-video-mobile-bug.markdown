---
layout: post
title:  "Videos wouldn't work with our app!"
date: 2017-06-09 08:40:09 +1100
categories: ionic, devops, 
---

So at work they have boarded the ionic hype train and converted wrapped the existing web application into a mobile web view. The concept seemed simple, just make sure all the links work, wrap it up and set-up all the build scripts and you're done. So one dev put his head down and was just hammering it out. 2 weeks in after encountering various issues such as google recaptcha (required domain), all our links were relative and not absolute, a multitude of ui bugs and finially he just could not seem to get videos to play on both android and ios platforms. So to give him a break another dev and I was given a timebox of a week to solve this issue, which we were able to resolve on the first day.

While the solution was rather simple the symptoms were interesting initially. As some background there was a previous app which we were basing our solution on.  This initial video solution used the mediaelement library which had smart bitrate rendering and fallbacks for compatability. This worked fine for desktop and was thought to be neccessary as we were still supporting IE and flash (banks . . ). For ios we did have issues in the past and had implemented a work around to just download and request the entire video as a blob then open another web page to load the video (we even implemented the loading bar based on the range request).

However when porting this new application one of the symptoms was that this existing solution ported just wasn't able to play any of the videos from our backend within the emulators (ios) and simulators. The dev tried reverting to plain html5 and even using the cordova media streaming api to no avail. Of course the classic 'big buck bunny' video was able to play just fine, and the problem seemed to only stem from videos served from our backend. All that was seen from the network tab was the fact that the html5 video player just request the first 2 bytes then stopped. We did implement logic to determine based on the range request which part of the video stream to send through so the dev initially thought there must have been a bug with the code. After much tweaking and searching he had found nothing and the entire problem appeared to be a mystery. 

So here we picked up with a fresh look. Since there were videos that did work we started looking at the differences. Firstly the videos that worked were of both http links and https links. Our backend connection was through https. After viewing what requests the html5 video player was making we noticed that for all other videos there would be a null range check first (the options) then request the first 2 bytes before streaming the rest of the video with subsequent requets. We had a theory, there was probably something wrong with the initial part of the protocol which was the certificates. The only real difference was that for our test environment we were using self-signed certificates. Testing this theory we deployed various links to videos onto both platforms including a link to our production video (with a proper certificate). Boom the production video played as expected.

Now to make sure we found the root cause we wanted to swap the link from https to http and in theory the video should stream as intended. Now this wasn't that easy as we were behind  corporate firewall. We first tried to deploy a new set-up and take out the https requirement from the waf but since it was quite a black-box it quickly turned out to be very fiddly. So to get it working we connected the corporate laptop to our own phones hotspot and ssh tunneled through to the macs which built the apps through ionic. Using a simple http request the videos were streaming to both device as planned!

Afterwards was simply writing a quick overlay for the video and tiding up some ui elements but a week timeboxed task done in just a day. It was quite a fun task working around the various constraints placed infront of us and it always is great when a root cause is found. 




