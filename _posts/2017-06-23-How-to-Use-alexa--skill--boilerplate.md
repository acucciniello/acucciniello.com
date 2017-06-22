---
layout: post
title:  "How to use alexa-skill-boilerplate"
date:   2017-06-23 12:20:10 
---


## Introduction

Hey there! I am writing this as a deeper explanation of my open source boilerplate for Amazon Alexa Skill Development called [alexa-skill-boilerplate][ASB].

My motivation for this was to create a quick setup that you could easily clone and use instantly to create your own alexa skills.

Let's go over the packages I am using for this.

## Packages

1. `alexa-app`: This is for faster and easier Alexa skill development with built in functions you can check out [here][alexaapp]

2. `mocha`: This is a [testing frameowrk][mocha] for our unit tests.

3. `chai`: This is an [assertion library][chai] to aid us in testing.

4. `bespoken-tools`: A [library][bst] that aids with mocking alexa functionality.

5. `claudia`: This allows us to deploy our skill to AWS Lambda easily through the command line. Check it out [here][claudia]

Now let's take a look at how to get setup!

## Getting Started

## Getting Started

1. Clone the repository (with your new skill name):

```
$ git clone https://github.com/acucciniello/alexa-skill-boilerplate.git new-skill-name
```

2. Install all packages:

```
$ yarn
```

3. Start Adding Intents to your skill by editing `app.js`
 
 So here is where you can add intents by simply editing the `SampleIntent` I have there for you.  

 Another tip that you would want to follow is at the top of `app.js`, you want to change the app name from app to whatever the skills name will be. This is the line I am referring to:
 
```
var app = new Alexa.app('app') // eslint-disable-line
```

Edit the `'app'` to whatever the skill's name will be.
 
## Test

When it is time for you to add tests, open `test/test.js`.  Follow the format that is laid out with the previous tests as a good start.

Here we are using mocha for a testing framework.  We are using Chai for the assertion library to compare specific values.  We are using `bespoken-tools` in our tests to mock our alexa skill behavior by allowing us to invoke certain intents and access the response's values.

Now to run the tests I have set up an easy command for you: 

```
$ yarn test
```

## Deployment
Before we do any deployment, setup your environment to use claudia (this is taken from my blog post on using claudia [here][claudiaBP]:

Now, you want to create a folder in your user’s home directory on your computer. For me the user’s name is Antonio. For you it may be your name. Enter that directory, and create a folder called .aws and enter it.

```
$ mkdir ~/.aws && cd .aws
```

The last thing to do is to create a file called credentials in that folder we just made.

```
$ touch credentials
```

This is where claudia will know that you have proper AWS permissions that are associated with your account. Your file should look like this:

```
[claudia]
aws_access_key_id = ACCESS_KEY_ID_FROM_AWS_PAGE
aws_secret_access_key = SECRET_KEY_FROM_AWS_PAGE
```

Once we setup up claudia for usage, we need to create a lambda function.  I made this easy with:

```
$ yarn deploy
```

That registers your skill with AWS Lambda.  Now you only need to do a couple of manual things to actually test this skill in AWS Lambda:

1. When in the AWS Lambda homepage, click on the skill you just created

![AWSLambdaFunctionsImage]()

2. Click test 

![TestButtonImage]()

3. This will ask you to bring up a test event.  Click the scroll down menu and select 'Alexa Start Session'.

![StartSessionImage]()

4. Click Save and test 

![SaveTestButton]()

5. It succeeds!
 
## Conclusion

I created this for any developer attempting to start building Amazon Alexa skills and does not know where to start, or an expereinced developer who wants to be more efficient while using the best technologies!

Thank you for reading! It would help me greatly if could take two seconds and share this on social media!

Reach out and follow me on [twitter][twitter]!  Check out my [GitHub][github] for my open source projects! Take a look at my [YouTube Channel][youtube].


[github]: https://github.com/acucciniello
[twitter]: https://twitter.com/antocucciniello
[youtube]: https://www.youtube.com/channel/UC8icMMql5SjCaXXMvILGIUA
[ASB]: https://github.com/acucciniello/alexa-skill-boilerplate
[alexaapp]: https://www.npmjs.com/package/alexa-app
[mocha]: https://mochajs.org/
[chai]: http://chaijs.com/
[bst]: https://bespoken.tools/
[claudia]: https://github.com/claudiajs/claudia
[claudiaBP]: http://www.acucciniello.com/How-to-Setup-Claudia.js-for-faster-AWS-Lambda-Development/