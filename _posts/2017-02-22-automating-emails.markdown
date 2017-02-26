---
layout: post
title:  "'Automating emails'"
date:   2017-02-22 08:40:09 +1100
categories: emailing, automating
---

There was a recent task to investigate a solution to send 'automated weekly emails'. At first glance the solution would seem simple but there were several surprising complications.

Firstly there are several parts to consider. There is the email gateway which has many important features to consider, the reputation of the IP's and cost/limit of emails per month. There is argument for the quality of the API's available although I do find this negligible. So examples include

* Sendgrid
* Mailgun
* Amazon SES
* Mandrill

Now there are other options which are layers ontop of these gateways. They include other features such as analytics or guarantees on email reputation. Or additional interfaces to send emails.

* salesForce (marketing cloud)
* Campaign Monitor
* MailChimp

Now initially there were suggestions to use the salesforce marketing cloud due to the fact it was  already used to send SMS confirmation. But after attempting to read through their docs which havn't been updated in literally over 10 years (lasted edited 2005 . . .) it just seems like one huge scam. For the project's needs of a simple API to send email this was the most convoluted interface and documentation I have ever seen which I guess makes some sense as this is how all of those consulting companies make money installing this shit. Regardless most of these other emails have features which we were not going to use and wrapped existing gateways anyway.

So in the end we decided to just use the in-house mailing service as it was the most simple as all the unsubscribe and analytics were not very difficult to implement ourselves. However this is where the interesting problem occurred, as all of our deployments are with amazon sc2 instances how would we set-up the 'trigger' to send these emails. There were several options:

    We could set-up a n additional SC2 instance however this would require additional overhead in maintenance. 

    We could implement a scheduled task inside the current application SC2 instance however there is the core problem of having the pre-production and production services both having this scheduled task and there would be the possibility of two emails being sent through. 

So the main problem of a possible race condition with both the pre-production and production environments. In the end to avoid all the complication of having both containers communicate with eachother or know if they are the main 'rea production' we decided to just have the emails triggered manually. This also simplifies the complexity of sending the emails for different client sets at different timezones. Since currently manual emails are sent a simple interface to send them turned out to be the simplest solution. So in the end it turned out automating would have ironically been more work than doing it manually. 

