---
layout: post
title:  "alexa-open-doc #4: Publishing Process"
date:   2016-12-06 14:38:20 
---

## Introduction 


If you have been following along with my previous posts you know that I finished the core functionality of the skill I just created: *Edit Docs*.  It is a Amazon Echo skill that allows users to edit text in a Google Drive file, and create different types of files using your voice.  When I finished the functionality a couple of weeks ago I was really excited to see my idea come to fruition so I submitted my skill in Amazon Developer Portal so others could use it.  Unfortunately it was not that easy and there were a few more things that Amazon wanted me to change before allowing the public to use it.  I am writing this post hoping that others trying to publish a skill can save time and have their skill published sooner.  The process took two weeks in total, with Amazon failing my certification four separate times.  Let's start with the first submission.


## Submission #1

After completing the functionality, I was ecstatic and was ready for others to use my skill but then I received an email from Amazon looking like this: 

![Email](/assets/alexa-open-doc/4/amazon_email.png)

Now I was pretty sad, and after a little bit of depressed binge eating and crying, I figured I should just try and fix this skill.  After reading the email, Amazon said there were six things wrong with the skill.

#### Trademark Issues

For problems 1 and 2, I had trademarking issues.  Originally the name of the skill was "Google Drive" but since I did not have permission from Google to use this, I was not allowed to publish it with that name. Problem Two had to do with my logo.  If you have seen the code for this project on [GitHub][alexaOpenDoc], the name of the repo is called *alexa-open-doc*.  I created the logo by taking a picture of the name, and Amazon did not like that I used *alexa* in the logo.  

**Takeaway: Watch out for places where you may be illegally using another company's copyrighted material**

#### Example Phrases

The next thing that Amazon had an issue with was the fact that my example phrases were not examples.  Let me explain.  Originally I had my example phrase as: *Alexa, ask Edit Docs to create a file called yourFile*.  But, I needed to have an actual name of a file in this case I used "Ideas" as the name of the file.  So the final correct phrase was: *Alexa, ask Edit Docs to create a folder called ideas*

**Takeaway: Make sure your example phrases can be used by actual users.**

#### Account Linking Card

If you have kept up with my posts thus far, you would know that in order to use Google Drive, the user would need to take advantage of Account Linking with Amazon Echo. When a user enables a skill, they are prompted to link their account with the echo (in this case, the user's Google account).  But if for some reason the user's account is not linked but the skill is still enabled, the skill should prompt the user to open up the app and link their skill.  There is a special way to do that called sending a *Link Account Card*.  When the account is not linked it sends this to the companion app: 

![LinkAccountCard](/assets/alexa-open-doc/4/link_card.png)

This makes it easy for the user to open the app and right away be sent to the linking page with one click.

**Takeaway: Allow for a Linking Account Card in order to help the user link their account in the case that it is not linked.**

#### Numbers 5 + 6

These were not reproducible so I resubmitted the skill, and Amazon came back with a couple of more complaints.


## Submission #2

Unfortunately, the second time around Amazon came back to me saying that there were three different areas they had problems with.

#### Account Linking Screen

When a user links the skill with their Account they are sent to a screen that says *"Your Edit Docs Skill was successfully linked”*.  The problem was that my page was displaying the old name of the skill saying: *"Your Google Drive Skill was successfully linked”*.  Amazon was having issues with the fact that it still said Google Drive.  Amazon was able to fix this on their end because I believe it had something to do with the redirect URL that they generate for you.

**Takeaway: Sometimes Amazon will have to fix something that they are having a problem with.  Just make sure to try to do everything you can before asking them for help.**

#### Handling all Outcomes

The next two problems Amazon had with the skill, was that when trying to invoke the skill using *"Alexa ask edit docs to update ideas with the following i really enjoy this project"*, Alexa would return this: *"There was a problem communicating with the requested skill"*.  After some digging around in the code, I found that source of the problem.  When searching for a file in your drive account, if there was no file found, the function from google would not return an error, instead, it would return a success and continue on.  The issue here was I was not handling the event when no files matched the search name.  

**Takeaway: Make sure to handle all outcomes of your code, otherwise it can cause Alexa to output an error message.**

## Submission #3

At this point, I assumed Amazon had random issue generator that would automatically email me with more issues.  Luckily, at this point there was only one thing wrong this time.  

#### Launching Your Skill

When invoking the skill by simply saying: *"Alexa, open edit docs"*, my skill automatically listed the files you have in your Google Drive.  Instead when invoking the skill through a basic Launch Intent Request (when you say "Alexa, open edit docs") they would almost like a welcome menu that gives a quick explanation of the skill and how it can be used.  Once it is finished responding it maintains the session open for the user to respond with what they like.  To keep the session open after replying with Alexa, make sure to use:


{% highlight ruby %}
response.ask(output)
{% endhighlight %}

Instead of:

{% highlight ruby %}
response.tell(output)
{% endhighlight %}

**Takeaway: Allow the Launch Intent to be almost like a welcome menu for the user to use the skill.**

## Submission #4

Once again Amazon used their random issue generator on me, and came back with two brand new problems.  

#### AMAZON.HelpIntent

There are different types of Intents that Amazon has created for general purposes.  One of them is called the Amazon Help Intent, which allows a user to ask Alexa for help while using a skill.  The Help Intent needs to: describe the purpose of the skill, contain example phrases of how to use the skill, ends with a question to the use, and keeps the session open for a response. 

Implementing a Help Intent is pretty simple.  Simply add *AMAZON.HelpIntent* to the list of intents in your intent Schema, and add it simply as any other intent. No need for sample utterances, Amazon handles that for you.  All you as the programmer needs to do is to handle what Alexa will respond with to the user when asking for help.

**Takeaway: Add a Help intent this way the user can ask for help using the skill.**


#### AMAZON.StopIntent/AMAZON.CancelIntent

Another type of generic intent created by Amazon is the Stop Intent and the Cancel Intent. The Stop and Cancel Intents need to: stop the processing of a skill and end the session.  

Implementing a Stop Intent and Cancel intent is the same as before for the Help Intent.  Simply add *AMAZON.StopIntent* and *AMAZON.CancelIntent* to the list of intents in your intent Schema, and add it simply as any other intent. Once again, no need for sample utterances, Amazon handles that for you.  All you need to do is make sure the skill stops processing when the Stop or Cancel Intents are invoked.

**Takeaway: Add a Stop and Cancel intent this way the user can stop/cancel the skill.**

## Conclusion

After resubmitting my skill on the fifth time, Edit Docs was finally approved and posted on the store.  If you would like to check out videos of the skill functioning look at my project [page][videosPage].  If you want to checkout the actual code to help you build your very own Amazon Echo skill go to my [GitHub][alexaOpenDoc]. If you are interested in downloading the skill you can find the directions to do that in the README.md of the [project][alexaOpenDoc].


I hope you enjoyed reading about my journey while creating this skill.  Check back later for more posts about my future projects!

[videosPage]: https://acucciniello.github.io/alexa-open-doc/
[alexaOpenDoc]: https://github.com/acucciniello/alexa-open-doc
 

