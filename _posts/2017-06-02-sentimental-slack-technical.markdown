---
layout: post
title:  "Overview intergrating slack with aws lambda"
date: 2017-06-03 08:40:09 +1100
categories: aws, lambda, slack, nodejs
---

I recently create a slack 'bot' which was scheduled to run 3 times a week asking a series of 3 questions then producing a report at the end. This was all done through a series of aws lambda functions.

Firstly the slack api documentation was pretty good and i found i didn't have any issues simply just curling the endpoint with my token to test out. Aws was also fairly straight forward to set-up and to just get running, I could just in web-browser hack up a nodejs lambda function then click test to run it. Very easy and straight away I was able to create an end to end horizontal from lambda function to message recieved in slack. The next piece was the response event from slack to aws lambda. This was slightly more tedious to set-up as there was the additional component of the api gateway and linking / deploying the slack app into the team channel. However the initial proof of concept was rather simple and i was able to invoke a lambda function to post a message into then hitting the callback event and recieving a response in my cloudwatch logs.

### Constraints/issues

So there were already some constraints using this technology stack integration. 

1) From a slack event callback the endpoint must respond within 3 seconds (slack constriant)

2) The slack auth token is inside the body of the response rather than the header. (minor but would have been nice to have it in the header)

3) Aws lmabda despite all their advertising can have slow boot times which can cause gateway timeouts from the user pov (having to resend their response)

### Sharing and maintaining state

Now as each lambda function is stateless to complete my chain of 3 questions each callback contained a callback key. There was essentially a state table i held in s3 which would return (if it exists) the next callback key. After the function would process the response the state was stored in s3 and a response was generated and sent back to slack. If the next callback key existed it simply just reinvoked the chain of post message with the new state to perform.

### devops

There are many tools to handle these deployments but since this was my first project i usually find value in attempting to make my own (allows me to understnd what issues these tools are addressing). I firstly for each function I had created a seperate folder with an index file. There were several common functions such as the slack / s3 api wrappers (probably should have used someones library but meh) and my simple http method wrapper. I wrote seperate shell scripts for each function which would zip all the required files from the project directory and deploy them into lambda land. 

There were quite a few issues with my method. Firstly changing any common files would also propagate across each deployment (hence why during build a build process these would be updated). In addition since this wasn't a larger project the differentiation between development and production was just a single line switch within the code rather than an environment change. 

Also each new deployment never left a trail of changes (master branch for life) so if anything did go wrong it would require a manual roll back assuming i actually commited some form of working state. This area could easily be improved and as the porject feautre set is growing i'll probably be writing another post on this evolution. The solution would have to involve multiple deployments (should be able to deploy entire project with 1 button to the various environments) in addition to the monitoring requirements such as how many messages were sent, and how long it took for the entire process to run. These can help ensure that everything is actually working as intended as well probably the next step is to maybe add some actual test coverage.

### production problems

Since after a weekend of hacking this together i decided to just launch it across the company slack there were a range of issues that occured. Firstly during updates I had accidently forgot to redo the development change so for one week I was the only one reciving messages and since I was reciving them I had thought everything was working as intended. Also initially people were having issues with slack not rendering emojis on their mobile view (i developed and tested only on a desktop environment) so I had to change how some of the question answers were written (this was later amended by slack as their fonts seem to render properly now). 

### Data storage structures

To store the responses I have used only a stagnant s3 which after a while will slowly move to the glacier level storage to save money (just for solution sake as realistically the entire application's cost is negligable). Further down the line for state related queries I might move this into a faster storage option such as dynamoDB ('faster') or an in memory cache like redis. 

The bucket contains several levels. 

At the first level it contains the dates (in the format of day month year eg 01012017) or the 'metadata' tag which will contain various questions and state. The second level for the date responses is the user which is treated as the slack id. This then moves to the question answered then finially this key will contain the entire json response just serialized. 

There are several disadvantages to this method firstly since i am storing by date if two exact same questions were to be answered then the previous would be overwritten. This could be avoided by possibly storing it as a epoch time. In addition it is difficult to just query a single user as the parent level contains all the dates. So to retrieve the data for a user i would have to walk across the entire date range and then check if the user key exists or not in the child level. 

The advantages however is that since the first purpose of the report is to generate a weekly summary all i would care about is the timestamp. This also allows me to easily query a subset of the data by range then iterate through to retrieve all the answers.


### Complete stack solution

So from end to end the process is set-up as so.

1) Each job is initiated from a scheduled event that triggers the lambda function with the desired starting state from inputs.

2) This job will go through attempt to open channels to every user and send out a question. 

3) Each question will contain a callback id and once the event is triggered it will be sent to a 'router' function.

4) Before the router function the call will go through an api gateway which will check that it is from the slack.bot domain

5) Next it will confirm the slack token then decrypt the slack bot key and invoke the appropriate lambda function dependant on the payload

6) This router will then respond to slack with a generate message replacing the message which invoked with a 'one moment...'

7) The invoked lambda function will save the state in s3 and query from s3 which state is next and repeat the post function which the cron job initiated wif further state exists. 

Generating reports:

1) Lambda cron job which will query and construct the report from s3 and building a google chart url to display in slack. 


### Further features / improvements

Firstly the entire project was quite fun to write even though it I felt it required quite a bit of head space since I was usually considering multiple parts at once. The improvements would be features such as allowing for slash commands to retrieve the information such as '/sam report last week' would show you a report from the last 7 days. This would allow any member to retireve stats data and reports which would hopefully provide more value to the users and hopefully provide more inventive to actually answer these questions. In addition i'll be adding random questions which the users can submit to add to the random question pool. 

Next feature would almost be a sperate project but I wanted the possiblity to ask for feedback across several members. This would tie into performance reviews and the overal sentiment and well being of the employees in the company. I'd like this to incorporate a web view form and then possibly the bot would grow further depending on the needs and requirements. 


