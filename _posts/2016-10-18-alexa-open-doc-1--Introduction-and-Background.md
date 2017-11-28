---
layout: post
title:  "alexa-open-doc #1: Introduction and Background"
date:   2016-10-18 14:38:20 
---

## Introduction 


For the past year and a half, I have been playing around with Node.js and created a few side projects to increase my knowledge and skills.  Going from nothing to where I am now was a struggle but is well worth it to look back and see all the hurdles I have overcome to get to the point I am at.  For all those starting to learn, learning by doing is the best way to get the information engrained in you.  It is the hardest, but it forces you to learn and because of the experiences you have associated with that learning you tend to remember what you learned better.  Enough about learning and let's get to the topic of this post.


## Inspiration

Have you ever had a  great idea come to you when you were doing a monotonous task like washing dishes or showering? Have you ever wanted to write that idea down and this way you can make millions of dollars off that idea but your hands were tied in the that monotonous task? That is where alexa-open-doc comes in.  


## Progress Thus Far

For my first functional task, I wanted to be able to access Google Drive and have Alexa list my files back out at me.  This presented itself with a few challenges:

	1. I did not know anything about building an alexa skill

	2. I have never used any of Google's APIs before

	3. I knew even less on how to get the two to communicate

### Alexa Skill Basics
	
To start learning about how to create a basic alexa skill.  I started where everyone starts heading over to our old friend Google.  Luckily I found a extremely detailed and easy to follow tutorial on how to build a basic skill called greeter. This can be found in Amazon's coursebook on GitHub [here][alexaBigNerdRanchTutorial].  Following this tutorial you should get a basic skill that you can add by using your mobile companion app for alexa. Once the skill is added, you can then say:

{% highlight ruby %}
"Alexa, ask greeter to say hello"
{% endhighlight %}

And Alexa will respond with:

{% highlight ruby %}
"Hello"
{% endhighlight %}

After building this and getting some Alexa skill basics under my belt, I felt it was time to tackle a simple google drive problem: Listing my files.

### Google Drive API Basics

With confidence running high after completing the basic Alexa skill, trying to list my google drive files was a little less confusing given this extremely easy to follow to [quickstart tutorial][qucikstartTurorial]. This basic tutorial gives you an understanding on how to create a client_secrets.json file used to hold the credentials for your application in order to link your account with Google Drive.  Once completed this you should be able to run the program, that generates an authorization code, which you copy and paste into your terminal, then it takes the code and generates an access token and a refresh token to access the API.  Once you have access you are able to do list the files you have in your account to the console.

Now that I had gotten the individual pieces done, the next step was to combine them and get the files read out from Alexa.

### Amazon Alexa + Google Drive

After completing the two, I figured it makes sense to just combine the two, by having my exports.handler call a function that calls my Google Drive code and then passes back the list of files and stores it in a variable to then send out the response with response.tell(variable) (where variable is the variable that holds the file names).  But just as expected, it was not right way to do it.  That is where account linking with Alexa skills come into play.

After doing some research this [link][accountLinking] became my greatest resource to solve any account linking issues I had. The first struggle was learning what OAuth 2.0 authentication framework was.  This allows third-party applications to gain access to an HTTP service(in this case allow Alexa to have access to Google Drive).  Now there are two forms of authentication: Implicit Grant and Authorization Code Grant.  The main difference between the two forms are how the access token is received from your system.  

Implicit Grant allows you to get an access token as soon as you ask for one, but the token expires after an hour, and the only way around this is to relink the skill in your Alexa companion application.  This is also less secure than using the second type: Authorization Code Grant.  This type asks Google for an access code, then takes that code and exchanges it for an access token and an optional refresh token.  The benefit of this is once the access token expires, Alexa will use the refresh token it received to automatically get a new access token so the skill be used indefinitely.  Here are a couple of diagrams explaining how the flow works in both types of authorization.

![Implicit Grant](/assets/alexa-open-doc/1/Implicit_grant.png)

{% highlight ruby %}
Figure 1: Implicit Grant
{% endhighlight %}

![Authorization Code Grant](/assets/alexa-open-doc/1/auth_code_grant.png)

{% highlight ruby %}
Figure 2: Authorization Code Grant
{% endhighlight %}

So, in order to use this indefinitely, I implemented this using authorization code grant. Once this is set up using the information in the [account linking page][accountLinking], you should now be able to open up your Alexa companion app, click on the top left corner to bring up the menu, click on skills, then click on your skills in the top right, click on the skill you are creating then, enable the skill. When enabling the skill this should take you to a page where you sign into your gmail account and you give permissions to the skill. Once you allow the skill, it should take you to a page where is says "[Enter Skill Name] is successfully linked!"

![Enable Skill](/assets/alexa-open-doc/1/enable_skill.PNG)

{% highlight ruby %}
Figure 3: Page to Enable Skill
{% endhighlight %}

![Successfully Linked Skill](/assets/alexa-open-doc/1/successfully.PNG)

{% highlight ruby %}
Figure 4: A Successfully Linked Skill
{% endhighlight %}

If you have reached this step.  Your Amazon Alexa should now have an access token when it sends a Lambda Request. Now in your code, you will need access to that token in order to pass it over to Google Drive to authenticate the user.  You can access the token by: 

{% highlight ruby %}
session.user.accessToken
{% endhighlight %}

 Pass that through to where Google authenticates it and then you should get your list of files back.  Check out this [link][twitterLinkToVideo] which is a video of the whole thing working!



[alexaBigNerdRanchTutorial]: https://github.com/bignerdranch/developing-alexa-skills-solutions/blob/master/coursebook/haIntro_Chapter.pdf
[qucikstartTurorial]: https://developers.google.com/drive/v3/web/quickstart/nodejs
[accountLinking]: https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/linking-an-alexa-user-with-a-user-in-your-system
[twitterLinkToVideo]: https://twitter.com/antocucciniello/status/787300186980777985
 

