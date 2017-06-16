---
layout: post
title:  "Using alexa-app for Simpler and Faster Alexa Skill Development"
date:   2017-06-16 12:20:10 
---


## Introduction

Have you built an Amazon Echo skill previously and wanted to find a way to speed up development? I think I may have found the package for you.  It is called [alexa-app][alexaApp]. This package allows for us to get a functioning skill up and running much faster than we could previously with traditional methods.

For today's tutorial, we are going to use a simple skill I created for you to follow along with on development.  It is called [alphabet-teacher][alphaTeach]. To improve your learning of how it all works I suggest that you retype all the code in order to help your mind make the connections necessary.

First well need to set up ourselves for this.

## Setup

Git Clone the repo:

```
$ git clone https://github.com/acucciniello/alphabet-teacher.git
```
Install all the dependencies:

```
$ cd alphabet-teacher
$ npm install
```

Run the tests to see if everything is working:

```
$ npm test
```

Now that we are all set up let's take a look at the code.

## Code
 
 We will be editing two files for the main functionality of this application.  The first one is index.js
 
### index.js

If you are taking a look at `index.js`, you can see it is pretty simple.  It is requiring our `alphabet-teacher.js` module.  Once it has that it uses that to create lambda function from it.  So, when creating this skill on lambda, this file (index.js) is the entry point. Pretty simple, now let's actually look at what is happening with `alexa-app` in `alphabet-teacher.js`.
 
### alphabet-teacher.js

The first thing we do here, is require our `alexa-app` module this way we can use it.  Then we create a new app instance giving it a name (alphabet-teacher).

From there, we will want to create a launch intent as our first intent.  In order to do this, we use the structure: 

```
app.launch(function(request, response) {
	response.say()
})
```

Let's quickly break it down. `app.launch` specifies a launch intent.  Then we have a callback that has access to the lambda request and response. Then we simply can end the launch request by saying a phrase and sending it over with `response.say()`.

Next let's take a look at how our [Amazon specific intents][intents] are created.  These are intents like Help, Stop and Cancel intent.

```
app.intent('AMAZON.HelpIntent',{
  'slots': {},
  'utterances': []
}, function (request, response) {
  response.say()
})
```

The cool part here is that these intents are automatically built in and you do not need to do anything extra to create them.  You might also notice here that you must add two empty objects as the parameters. One for the slot types and the one for the utterances. But in this case we do not need to fill these objects with anything.

The last type of intent here:

```
app.intent('AlphabetIntent', {
  'slots': {},
  'utterances': ['to say the alphabet', 'to tell me the alphabet', 'to recite the alphabet']
}, function (request, response) {
  response.say()
})
```

Here we created a custom intent called *Alphabet Intent*.  This intent is something I created.  And all we had to do to create this intent is give it this name, along with it's sample utterances.  If you do not know what an intent is or a sample utterance is check out my blog post on that [here][intentBP]. See how easy it is to develop any type of intent in seconds with `alexa-app`?

## Test

Without going too far out of the scope of this tutorial let's install one more package: 

```
$ npm install -g alexa-skill-test
```

This spins up a local server an allows us to test our alexa skill's intents very easily.

![AlexaSkillTestImage](/assets/alexa-app/skill_test_image.png)

Test your intents here to see the phrases we hardcoded come out as a response!

## Conclusion

If you have done alexa development before you can see how easy this makes development.  Besides the little time it takes to get used to this form of developing, it makes all your processes easier! I hope you decide to check it out for yourself.  Get in touch with me if you enjoyed this!

Reach out and follow me on [twitter][twitter]!  Check out my [GitHub][github] for my open source projects! Take a look at my [YouTube Channel][youtube].


[github]: https://github.com/acucciniello
[twitter]: https://twitter.com/antocucciniello
[youtube]: https://www.youtube.com/channel/UC8icMMql5SjCaXXMvILGIUA
[alphaTeach]: https://github.com/acucciniello/alphabet-teacher
[alexaApp]: https://www.npmjs.com/package/alexa-app
[intentBP]: https://www.packtpub.com/books/content/how-add-custom-slot-types-intents/?utm_source=twitter&utm_medium=social&utm_campaign=blog
[intents]: https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/built-in-intent-ref/standard-intents