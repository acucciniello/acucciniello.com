---
layout: post
title:  "Comparing Google Home Action and Alexa Skill Development: Take One"
date:   2017-07-14 12:20:10 
---


## Introduction

If you have been following along with my posts you know that I have created four Amazon Alexa Skills up to this point.  Now I finally took the leap over into creating Google Home Actions using API.AI.  

Throughout my first experience, I learned plenty along the way.  I noticed some similarities and differences between the two platforms for development and wanted to point them out.  My goal here is to help those that know one of the forms and would like a quick overview on how they are similar.

## Similarities

Here are two of the similarities that I was able to notice!

### Intents

API.AI and Alexa Skills both use this concept of intents.  This is how the skill or action knows what the user is *intending* to do.

The differences here lie on how they are both established in two platforms.  For Alexa there is an Intent Schema JSON file that displays what the intents are, and what slot types they have.  Then you have a sample file on how the intents are invoked. For API.AI, this is all in the same JSON file (unless you use the UI to do it then they are just on the same page). 

### Starting Action/Skill

In a similar way how you launch a skill on an Echo, n Google Home you must start an action with *"Ok Google, talk to action_name"*.  This opens the `Welcome Intent` for that action and from there, you can move to other intents within the action.

## Differences

Here are two concepts that were different during development.

### Contexts

With API.AI we have this new concept of *Contexts*.  They are either inputs or outputs to intents that are executed or used to pass information to an intent, based off the scenario you are in.  Hence the reason why it is called a context.  It uses the conntext of the situation to help determine what happens.  I will not go into all the parts of contexts here so check out this explanation from [Google][context].

### Webhooks/Fulfillment

With Google Home Actions, in order to get more than hardcoded functionality to your action, you must add what is known as a *fulfillment*.  This is basically a third party API or one that you developed that you call on execution of a specific intent.  

In Alexa skills, you code all the functionality inside the skill's intent function and then host it on Lambda or use your own server.  Here we must code that functionality and have a public URL to access it.

I will be creating a tutorial on how to do this, so stay tuned to my site for that!

## Conclusion

Overall, I really enjoyed developing the Google Home Action with API.AI.  The UI for API.AI is very friendly, and once you get used to the new terminology it should be easy.

Take a look at the [action] I built
Thank you for reading! It would help me greatly if could take two seconds and share this on social media!

Reach out and follow me on [twitter][twitter]!  Check out my [GitHub][github] for my open source projects! Take a look at my [YouTube Channel][youtube].


[github]: https://github.com/acucciniello
[twitter]: https://twitter.com/antocucciniello
[youtube]: https://www.youtube.com/channel/UC8icMMql5SjCaXXMvILGIUA
[hnaction]: https://github.com/acucciniello/hacker-news-action
[context]: https://api.ai/docs/contexts