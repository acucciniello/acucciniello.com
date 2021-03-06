---
layout: post
title:  "Modularizing Your Alexa Skill Code"
date:   2017-04-06 17:38:20 
---

## Introduction 

Hello! If you have been reading along you know that I have published an Amazon Echo Skill, but at the time I have posted this I published my third skill on the skill store.  It is called [Fan Muse Jobs][theMuseJobs] and we will be using it as an example of how to modularize your Amazon Alexa Skill Code. Let's get right into the main directory!

## Main Directory

Here is what the main directory looks like:

![MainStructurePage](/assets/modularizing-alexa-code/the-muse-jobs-structure.png)

Let's break this down.  

`docs/`: This is used to create a GitHub projects page for the project. We will not be going into detail about this directory here.

`helpers/`: This is a directory to hold all of the functions that get called within an intent function in order to complete some processing for the intent.

`intents/`: This is a directory to hold all of the intent functions for your skill. 

`.gitignore`: This file was made to prevent certain auto generated files from being committed to source control

`alexa-skill.js`: The file that you get from Amazon when wanting to create a skill.  Your `service.js` uses this file.

`package.json`: This holds all the information about the project and its npm dependencies

`service.js`: This is the main JavaScript file your skill will call when running it in AWS Lambda.

Let's go more in depth.

## helpers

Here is what the `helpers/` directory looks like:

![HelpersPage](/assets/modularizing-alexa-code/helpers.png)

All of these files are called to complete some task that an intent needs in order to give the proper output to the user.  This makes your intent function code look clean.

## intents

Here is what the `intents/` directory looks like:

![IntentsPage](/assets/modularizing-alexa-code/intents.png)

Each of these files represents a function that gets executed when the user says something specific that triggers that intent. You must have at the minimum a launch, stop, cancel, and help intent in order to have a skill.  The rest of the intents were specific to my skill.

## service.js

This is the main JavaScript file that uses `alexa-skill.js` and all of your intents.  It contains an intent handler that knows to route a specific intent to an intent function.

## Conclusion

In conclusion you should make sure that all of your intent functions are separated into their own files.  They should be requiring modules that you created to handle what happens in the intents and those files should be in `helpers`.  All of your intents will then be required in the main file `service.js`.

I hope you enjoyed this post! Share it with your fellow developers on social media.  Follow me on twitter to stay updated with my posts [here][twitter]. Check out my other two Amazon Alexa Skills [Edit Docs][alexaOpenDoc] and [Fink-link][findLinkNYC].  If you are into self-help and self growth check out my YouTube [channel][youtube] where I give you information on the books I read!
 

[theMuseJobs]: https://github.com/acucciniello/the-muse-jobs
[alexaOpenDoc]: https://github.com/acucciniello/alexa-open-doc
[findLinkNYC]: https://github.com/acucciniello/find-link-nyc
[gitHub]: https://github.com/acucciniello
[twitter]: https://twitter.com/antocucciniello/
[youtube]: https://www.youtube.com/channel/UC8icMMql5SjCaXXMvILGIUA

 

