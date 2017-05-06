---
layout: post
title:  "Storing private keys linux"
date: 2017-05-06 08:40:09 +1100
categories: Linux, Security
---


I am currently writing a simple slack aplication which will be deployed through several aws lambda functions. All the various platforms invovle several sets of private keys this is just my current solution in storing and setting the environment variables.

One issue I found was that you are unable to 'sudo source' a shell script since it is a built in command and not a program. So here is my work-around to have a 'secure' file storing my keys and the ability to source it into the my development environment.

The idea is as follows:
* Store keys in files that only root can even read
* create shell scripts which source environment variables from the text files

So for example you would create the following
{% highlight javascript %}
	vim ~/Keys/aws_lambda.txt
{% endhighlight %}

Add your keys (depending how you format will change your script)

{% highlight javascript %}
	aws_id{}secretasldkjid
	aws_secret{}veryverysecret
{% endhighlight %}

Next change the permissions and ownership of the file such that only root can even read it

{% highlight javascript %}
	//Should be able to see your settings
	cat ~/Keys/aws_lambda.txt

	sudo chown root:root ~/Keys/aws_lambda.txt
	sudo chmod 700 ~/Keys/aws_lambda.txt

	//Now you have been deined
	cat ~/Keys/aws_lambda.txt
{% endhighlight %}

Now set-up a script to source your environment variables


{% highlight javascript %}
	vim ~/EnvConfig/aws/aws_lambda.sh
{% endhighlight %}

Add the following to extract the keys and set the variables

{% highlight javascript %}
	export AWS_ID=`sudo grep -i aws_id ~/Keys/aws_lambda.txt | awk -F'}' '{print $2}'`
	export AWS_SECRET_KEY=`sudo  grep -i aws_secret ~/Keys/aws_lambda.txt | awk -F'}' '{print $2}'`
{% endhighlight %}

Note you require the backticks to evaluate the expressions. Next source the script to set-up your environment.

{% highlight javascript %}
	source ~/EnvConfig/aws/aws_lmabda.sh
	echo $AWS_ID
{% endhighlight %}

Now your environment is set-up for local development. So far it has been working for me and theres probably many flaws in this method and better methods out there but atleast so far all i've found with security is that there are always going to be ways around them.

