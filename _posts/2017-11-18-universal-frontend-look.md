---
layout: post
title:  "Universal frontend look"
date: 2017-10-18 08:40:09 +1100
categories: vue, react, frontend 
---

At work to attempt to improve delivery times the 'frontend lead' has wrapped a simple component library and made this the defacto standard moving forward. Meaning if you want to use a primary button it has to be this component, if you want a pink button and it isn't available well no pink button.

In the past I would have loved this as all it would required for frontend work is to simply monkey glue shit together and if anything breaks you can just blame the library. However ironically as my role is moving more towards writing systems and infrastructure i've grown an increased appriciation for ui development. 

Their overall goal I agree with. Currently with each product team doing their own shit everything looks very different (and to be honest not very good). Having a unified front for design makes sense. This problem has been delved into by other companies. For example google with their material design and apple with well apple. However I disagree with their execution of this plan.

The ideal world would have a set of well thought out guidelines (obviously open to iteration) about overall design philosophies. For example cancel should be on the left of two options, modals are only used for prompts, navigation is always on the left etc. Then as a helper feature a company supported component library should be made just as a baseline which can be improved upon as it gains adoption. It would include simply things such as branding and just plug and play forms (which is what the majority of the UI will be, forms) as a base similar to a bootstrap form.

In this case however it has become the case where a team had spent 6 months producing nothing (literally nothing) so someone decided to make a stand and just get something out there. Now it has become the case where they want to heavily police all the changes. There are quotes like 'you should not be modifying your own css'. I can empathize with where they have come from and why they have taken this approach. However it brews the following poor mentality:

1. Why think about and take responsibility for the UI when you can glue shit together and blame someone else if it looks shit

2. Any bugs? Just throw your hands up and  "o things are blocked i can't do anything" rather than fix it and PR

There are positives however from a company point of view:

1. Developers are more expendable as expertise from a few good developers can be felt across the company 

2. Consistency across products will be guaranteed

3. Easier to manage the UI/UX experience as you just have to manage a single team which governs all products

In practice however I feel the types of employees who would be happy with this aren't the ones you want developing public facing interfaces. If you just want to glue things together and not think about why you aren't going to make the best product just due to the fact you don't really give a shit. If you do care the iteration process can be very fun and one of the advantages of UI development is there can easily be data driven feedback. It also really encourages innovation and outside the box thinking to provide a simple yet memorable experience which is what I think a great front-end developer achieves. An amazing UI experience that under the hood is surprisingly simple and maintainable. 

Guess my entire gripe with the entire situation comes down to the fact you shouldn't tyrant innovation. 

If you are gonna bother hiring good engineers then you can't tell them how to do their job. You give them a goal you want to work towards and let them figure the shit out along the way with open discussion and feedback (some of the positives of agile methodology). So I don't agree with not allowing people to tweak and innovate the base library or even write their own because at the end of the day if it adheres with the overall guidelines and has solid reasoning behind the decision then it should make it to production. 





