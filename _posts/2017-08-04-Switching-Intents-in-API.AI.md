---
layout: post
title:  "Switching Intents in API.AI"
date:   2017-08-04 12:20:10 
---


## Introduction

Over the past week, I was struggling on how to make my conversations work in API.AI.  In Alexa, you simply have intents and they get called with a sample utterance.  In API.AI there are plenty of more concepts that allows conversations to be smarter and more advanced.  Being new to the concepts of *Contexts*, *Events*, and *Actions* switching between intents was difficult for me to understand.  Today I will show you how to setup your intents so they switch automatically!

All the code for this example can be found [here][gistLink]. 

## Code

Here is an example of how your code will be setup:

```
'use strict';

process.env.DEBUG = 'actions-on-google:*';
const App = require('actions-on-google').ApiAiApp;
const functions = require('firebase-functions');

// [START YourAction]
exports.yourAction = functions.https.onRequest((request, response) => {
  const app = new App({request, response});
  console.log('Request headers: ' + JSON.stringify(request.headers));
  console.log('Request body: ' + JSON.stringify(request.body));

  // Fulfill action business logic
  function responseHandler (app) {
    // Complete your fulfillment logic and send a response
    app.ask('Welcome to your action.  What would you like to do next? You can say 'test' or 'time'.);
  }
  function testIntent (app) {
      app.tell('test');
  }
  function timeIntent (app) {
      app.tell('time');
  }
  const actionMap = new Map();
  actionMap.set('input.welcome', responseHandler);
  actionMap.set('test', testIntent);
  actionMap.set('time', timeIntent); 
  app.handleRequest(actionMap);
});
// [END YourAction]
```

To host the action, we are using Google Cloud functions and Firebase. If you would like more information on that check out this repo [here][firebaseTut].  In the full fledged example you would deploy your cloud function to firebase, set up a fullfillment in API.AI.

So we created 3 functions, one for each intent.  The `actionMap` was created to map each action in API.AI to their corresponding function in the code. Pretty straightforward. 

## API.AI

Now head on over to API.AI.  We are going to create three intents for this example. First being our welcome intent, second will be time intent and third will be test intent.  Ignore the names as they are not important.

### Welcome Intent

This intent is made just to have a prompt when the action is launched.  You need to set the action name to `input.welcome`.  The action name allows you to map it to a specific function in your code.  Set two output contexts for the scenario.  This tells API.AI what intents to look out for as a followup to the current intent you are in.  Here is a screenshot of what it should look like.

![WelcomeIntent](/assets/apiapi-intent-switch/WelcomeIntent.png)

### Test + Time Intent

Our next intent will be the test intent.  It is same as the time intent just replace 'time' and 'test'.  These will both have input contexts, that allow it to get called when a function has an output context that matches the input context.  This is what allows API.AI to know what is to be called.  Set the actions to be time and tests respectively.

![TestIntent](/assets/apiapi-intent-switch/testIntent.png)

![TimeIntent](/assets/apiapi-intent-switch/timeIntent.png)

 
## Conclusion

In the end, what you need is to match the outgoing intents output context and the incoming intents input context along with matching the phrase of the user.  That is how you move from one intent to the next. 

I hope this saves you some time as the time I am reading this, there is not much documentation on the topic. 

Reach out and follow me on [twitter][twitter]!  Check out my [GitHub][github] for my open source projects! Take a look at my [YouTube Channel][youtube].


[github]: https://github.com/acucciniello
[twitter]: https://twitter.com/antocucciniello
[youtube]: https://www.youtube.com/channel/UC8icMMql5SjCaXXMvILGIUA
[gistLink]: https://gist.github.com/acucciniello/faff332c5c401402d698ca5fd2af9dba
[firebaseTut]: https://github.com/actions-on-google/apiai-webhook-template-nodejs