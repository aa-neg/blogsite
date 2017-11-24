---
layout: post
title:  "Overview of chatbot options"
date: 2017-08-14 08:40:09 +1100
categories: api.ai, aws, lex, wit.ai, watson 
---

During the hackathon I was introduced to aws lex. Even performing very simple tasks it added a wow factor and chatbot hype to our submission. For work I created a simple slack bot which the company could self facilitate feedback and questions through a web interface. Out of interest I immediately wanted to play around with the various chatbot nlp engine offerings and maybe integrate that as the web interface instead of a traditional form. 

### Chat interfaces

Now theres been quite a bit of hype over chat bots and chat interfaces however I have yet to find one that I thought was implemented well. Now this in my opinion has nothing to do with the underlying technology it is rather more of a UX issue than anything else. A chat interface is just a glorified CLI. Its difficult to find things unless you know what you want and you can really only do one thing at a time (unless you are boss and pipe many things together, similar to prompting the bot with multiple intents). This is very different to traditional interfaces where what is available can easily be clicked around or guided.

### NLP engines and offerings

There are many options in pre-built NLP engines and chat bot API's. 

#### Do it yourself

As with most things I usually consider the first option. How much work would it take to build it myself? I honestly think people rush too quickly to pre-built solutions and you end up with a 20mb web page using react redux mobx and rxjs that just adds two numbers together -_-. 

Now most available API's implement the following categories. Intent, entities and actions. From input (and sometimes context) determine user intent. From the intent populate if appropriate entities then send these through to what actions need to be taken (or the fulfillment of the request). To accomplish this there are various NLP techniques to tokenize the input or parse the syntax tree appropriately. Implementing the various algorithms there are many libraries and guides (http://www.nltk.org/book/). 

The overall effort isn't so crazy to do it yourself however the results for more complex queries will most likely be lacking compared to other paid offerings (they also have much larger datasets to train against). I do plan to implement this in the future however for now just understanding the UX of implementing a bot I decided to go with a pre-made solution.

#### Facebook's wit.ai

https://wit.ai

The documentation is great and only recently they have removed support for bot building (removing context and stories https://wit.ai/blog/2017/07/27/sunsetting-stories). I understand their prerogative of their choice as I agree that pure text based chat interfaces to forms are not effective to most of the public. 

##### latency
 When running some very sketchy ping tests I get from Sydney around 1.3sec round trip from their APIs which isn't that bad. 

##### API and integration
Well its facebook so its got first class support for messenger. The API is well documented and farily straight forward to use. There is some disadvantage to not being able to send context to their intent queries (deprecated) however you are able to export your apps data set to add some version control.

#### Google's API.ai

Now this is slightly different as google do offer a cloud NLP service which I am placing in a different category to their API.ai. 



