---
layout: post
title:  "Sentimental Slack initial proof of concept"
date: 2017-06-01 08:40:09 +1100
categories: slack, aws
---

Currently i have been increasingly interested in the cloud infrustructure. As a smaller proof of concept to a larger company sentiment/feedback tool integrated with slack I had made and deployed a 'simple' slack bot. The bot asked a series of 3 multiple choice questions attempting to gauge the users sentiment across various aspects of their work life.

"How do you feel about your current project?"

"How would you rate your team?"

"How do you feel about hypothesis?"

The initial cadence of the bot was 3 times a week which it then would send out a report to the #general channel with pretty graphs plotting the distribution of the answers (google charts api). 

Now from a higher level only considering the value this provided there were some surprising findings. From a simple proof of concept the initial data set showed that there was a trend of people willing to mention that they weren't the most thrilled about their current project. In addition to my surprise the uptake of the application was quite high as nearly everyone answered all 3 questions posted on Monday Wednesday and Friday. 

Now the problem and value changed, this tool now provided insight and data which previously wasn't there. So there have been a few tweaks moving forward with just simply the sentimental portion of this larger feedback tool. 

1) Avoid the case where the tool eventually gets ignored

2) Consider the quality of the data (how do retrieve an unbias sample?)

3) Add further customization

To tackle the initial problems i did recieve feedback that the bot was asking too many questions and became too predictable. In addition the fact that the workflow just sent the 3 questions each time with the same set of responses there was a disconnection with what was being asked and it turned into almost the case of quickly selecting 3 answers to complete the survey. 

The first change was to only ask 1 question (even though technically the idea of rolling state across stateless lambda functions was fun) each day. In addition I have added 2 additional 'random' questions with a binary response. For example "would you rather be 8 hours late for your flight or have to wait at the airport for 8 hours?" or something to that manner. This is my hope to make the surveys feel more prominent and not such a chore. 

There is still the next challenge of ideally creating a random sample of which of these now 5 questions is asked each day as currently since these are all scheduled cron lambda instances I have thought of a clean solution that will maintain the state of which questions have already been asked (seems just messy so far). 

Overall this initial project has been very interesting and I found that building tools for myself or smaller side projects may be cool but more value is gained from simply having users which your software is interacting with. 



