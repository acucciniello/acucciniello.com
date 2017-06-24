---
layout: post
title:  "Bespoken-tools to Mock Alexa Functionality in Tests"
date:   2017-06-30 12:20:10 
---


## Introduction

Hey there! If you are like me, you wanted to add some unit tests to your alexa skills.  The problem that always limited me was it seemed like such a daunting task to have to mock so many different things on your own.  Luckily I have found an awesome package that allows for this: [bespoken-tools][bst]

For this tutorial I am going to show you how I set up my testing in my [alexa-skill-boilerplate][ASB]. We are going to use this as an example. As a side note we are using [mocha][mocha] as a testing framework and [chai][chai] as an assertion library.

## Test Setup

1.Clone the repository (with your new skill name):

```
$ git clone https://github.com/acucciniello/alexa-skill-boilerplate.git
```
2.Install all packages:

```
$ yarn
```

Now that you are all set up let's dive into the code

## test/test.js

Enter that directory^:

```
$ cd test
```

and open the `test.js` file.

This is where all of our tests are.

The first thing we need to do here is import our libraries and initialize some global variables:

```
var chai = require('chai')
var bst = require('bespoken-tools')
var assert = chai.assert
var server = null
var alexa = null
```

This should be pretty straightforward if you have used node.js before.

Next, we need to set up two functions: `beforeEach()` and `afterEach()`.  As you can probably guess by the name, these are functions that allow you to do things before or after tests are ran.

### beforeEach()

```
beforeEach(function (done) {
  // first param - the location of lambda file to be return
  // second - port
  // third - verbose mode res/req sent to console
  server = new bst.LambdaServer('../index.js', 10000, true)
  // 1st - url of server
  // 2 - IntentSchema
  // utterances
  alexa = new bst.BSTAlexa('http://localhost:10000',
                './speechAssets/IntentSchema.json',
                './speechAssets/SampleUtterances.txt')
  server.start(function () {
    alexa.start(function (error) {
      if (error !== undefined) {
        console.log('Error: ' + error)
      } else {
        done()
      }
    })
  })
```

The purpose of our `beforeEach()` function is to allow us to create a server that will host our lambda code.  This is the first parameter to the function. Next we want to set up an alexa emulator with `new bst.BSTAlexa()`. In order to get this working we need an `IntentSchema.json` and `SampleUtterances.txt`.  They respectively hold you intent schema and the utterances you want to use for your intents.  

### afterEach()

```
afterEach(function (done) {
  alexa.stop(function () {
    server.stop(function () {
      done()
    })
  })
})
```

This one is pretty straight forward.  We simply want to stop the alexa emulator we created in `beforeEach()` and we want to end the Lambda server we initialized.

### Launch Intent Testing

```
it('Launches the skill', function (done) {
  // Launch the skill via launch request
  alexa.launched(function (error, payload) {
    if (error) {
      console.log(error)
      done()
    }
    assert.equal(payload.response.outputSpeech.ssml, '<speak>Welcome to Your Skill. The purpose of this skill is...  To start using the skill, say Alexa, ask ....</speak>')
    assert.equal(payload.response.shouldEndSession, false)
    done()
  })
})
```

So this is an example of testing a launch intent.  You can mock the launch intent with `alexa.launched()`. And if you look in our callback, you can witness how easy `bespoken-tools` makes it to test the output of the skill.  I am testing the speech output and that the response does not end upon invoking the launch intent.

### Amazon General Intent Testing

```
it('Launches the Help intent and doesnt end session', function (done) {
  alexa.intended('AMAZON.HelpIntent', null, function (error, payload) {
    if (error) {
      console.log(error)
      done()
    }
    assert.equal(payload.response.outputSpeech.ssml, '<speak>Welcome to Your Skill. The purpose of this skill is...  To start using the skill, say Alexa, ask .... What would you like to do?</speak>')
    assert.equal(payload.response.shouldEndSession, false)
    done()
  })
})

it('Stops and Exits Skill upon calling StopIntent', function (done) {
  alexa.intended('AMAZON.StopIntent', null, function (error, payload) {
    if (error) {
      console.log(error)
      done()
    }
    assert.equal(payload.response.outputSpeech.ssml, '<speak>Stopping your Request and Exiting Skill</speak>')
    done()
  })
})

it('Cancels and Exits Skill upon calling CancelIntent', function (done) {
  alexa.intended('AMAZON.CancelIntent', null, function (error, payload) {
    if (error) {
      console.log(error)
      done()
    }
    assert.equal(payload.response.outputSpeech.ssml, '<speak>Canceling your Request and Exiting Skill</speak>')
    done()
  })
})
```

Here we simply testing the functionality of an `AMAZON.HelpIntent`, `AMAZON.CancelIntent`, and `AMAZON.StopIntent`. Like the last example we are simply checking to the see the output matches what we want the skill to say.  The only difference here is we are using `alexa.intended()`.  This takes the intent name as the first parameter, the second one is the slot for that intent, and the callback.

### Any Other Intent Testing

```
it('Launches SampleIntent with Utterance', function (done) {
  alexa.spoken('to say the skill', function (error, response, request) {
    if (error) {
      console.log(error)
    }
    assert.equal(request.request.intent.name, 'SampleIntent')
  })
  alexa.spoken('to tell me the name', function (error, response, request) {
    if (error) {
      console.log(error)
    }
    assert.equal(request.request.intent.name, 'SampleIntent')
  })
  alexa.spoken('to recite the time', function (error, response, request) {
    if (error) {
      console.log(error)
    }
    assert.equal(request.request.intent.name, 'SampleIntent')
  })
  done()
})

it('Says Hi there with SampleIntent', function (done) {
  alexa.intended('SampleIntent', null, function (error, payload) {
    if (error) {
      console.log(error)
      done()
    }
    assert.equal(payload.response.outputSpeech.ssml, '<speak>Hi there</speak>')
    done()
  })
})
```

So this intent we are testing here is the `SampleIntent`. Besides testing the output, we want to test to see that the intent actually gets called upon using the Sample Utterances we defined in `SampleUtterances.txt`.  For that we use `alexa.spoken()`.  


## Conclusion

I created this for any developer attempting to start building Amazon Alexa skills and does not know where to start when it comes to creating tests for their skill!

Thank you for reading! It would help me greatly if could take two seconds and share this on social media!

Reach out and follow me on [twitter][twitter]!  Check out my [GitHub][github] for my open source projects! Take a look at my [YouTube Channel][youtube].


[github]: https://github.com/acucciniello
[twitter]: https://twitter.com/antocucciniello
[youtube]: https://www.youtube.com/channel/UC8icMMql5SjCaXXMvILGIUA
[ASB]: https://github.com/acucciniello/alexa-skill-boilerplate
[mocha]: https://mochajs.org/
[chai]: http://chaijs.com/
[bst]: https://bespoken.tools/
