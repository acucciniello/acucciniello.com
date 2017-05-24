---
layout: post
title:  "How to Setup up Claudia.js for Faster AWS Lambda Development"
date:   2017-05-26 12:20:10 
---


## Introduction

Have you ever used Amazon Web Services Lambda before to host your application without a server? During development, if you were doing it the manual way, you probably had to make your code changes, then zip up all of your files.  Once you had that you needed to upload the files over in your web browser at your Lambda page, and then you had to test it.  All of that takes about a minute (minus the code changes).  Doing this each code change just to see if a difference was made, adds up in the end.  

That is where **Claudia.js** comes in.  It is a command line tool that allows you to create, update and test Lambda functions easily through the command line.  Today we are going to create a basic application that uses this.  Check out the GitHub [repository][claudiaTut] I created to make this easier for you!

## Prerequisites

Here are the prerequisites for this tutorial:

1. An Amazon Web Services account that has IAM full access and full Lambda Function.  Check out [this][awsSignUp] for how to create an account.
2. Node.js (version 4.3.2 and up) and NPM installed on your machine.  If you need these, check out [this][nodeInstall] link.
 
## Setup

Create your Directory:

```
$ mkdir claudia-tutorial
```

Create a package.json file:

```
$ cd claudia-tutorial
$ npm init
```

Install Claudia.js:

```
$ npm install -g --save claudia
```

Now, you want to create a folder in your user's home directory on your computer.  For me the user's name is `Antonio`.  For you it may be your name.  Enter that directory, and create a folder called .aws and enter it.

```
$ mkdir .aws && cd .aws
```

The last thing to do is to create a file called `credentials` in that folder we just made.  This is where claudia will know that you have proper AWS permissions that are associated with your account.  Your file should look like this:

```
[claudia]
aws_access_key_id = ACCESS_KEY_ID_FROM_AWS_PAGE
aws_secret_access_key = SECRET_KEY_FROM_AWS_PAGE
```

The first line represents the profile name that you would like to use for claudia.  Now to make this work, you simply need to replace the values I have for the access key and secret key with your specific values.

With your environment set up let's start with some code!

## Code

Create two .js files:

```
$ touch lambda.js
$ touch hello-world.js
```

`lambda.js` will serve as the entry point of your Lambda Function.  `hello-world.js` is simply a function that lambda.js is pulling in for usage.

Fill `lambda.js` with the following:

```
var helloWorld = require('./hello-world.js')

exports.handler = function (event, context, callback) {
  helloWorld('Hello, World', function (err, success) {
    if (err) {
      console.log('Error:' + err)
      return
    }
    context.succeed('We succeeded with: ' + success)
    return
  })
}
```

This first pulls in the helloWorld function.  Once that is done, it creates a simple JavaScript lambda function with `exports.handler`.  What we are doing is calling helloWorld and executing a callback once it is done.  This is what gets executed when you test your Lambda Function.

Fill `hello-world.js` with:

```
module.exports = HelloWorld

function HelloWorld (input, callback) {
  if (!input) {
    var err = 'There is no input'
    callback(err)
    return
  }
  callback(null, input)
  return
}
```

This takes in a string, and if there is no string it will throw an error.  If there is a string it will execute the callback.

So in total, this calls HelloWorld, then executes the callback. Pretty simple.

Let's show you how easy it is to get this as a fully functioning Lambda function with Claudia!

## Clauida

First, we need to create our Lambda function with AWS.  Head over to terminal and enter this:

```
$ claudia create --region us-east-1 --handler lambda.handler --profile claudia
```

Let's break that down.  We are using claudia to create a lambda function.  We set the region to `us-east-1` but you may change it depending on your region.  We set what the handler function is with: `--handler lambda.handler`.  This is looking for the file/module name first and the method second.  So since lambda.js is the file name and handler is the method, it is `lambda.handler`.  Then last we set AWS profile we would like to use as `claudia`.  This was established above in our credentials file.  Whatever the profile was named in that file, you want to use that name there and wherever else you add the profile option.

Running that command uploads the code to Lambda and creates the function you would like.  It then creates a `claudia.json` file that stores the function's details for easier calling.

Now, you will want to test your function with:

```
$ claudia test-lambda --profile claudia
```

This should get you a response like the following:

![TestOutputImg](/assets/claudia-tutorial/CLSuccess.png)

Now you can go ahead and change the first parameter to the HelloWorld function in lambda.js.  This should change your output.  But in order to test this, you must update your Lambda function with:

```
$ claudia update --profile claudia
```

So this looks all good and well from the command line how do I know this really works with AWS Lambda though?

## Lambda

Head over to your AWS Console and click on [Lambda][lambdaLink].  You can see this function that I created using claudia is here:

![LambdaFunctionImg](/assets/claudia-tutorial/LambdaFunction.png)

And when I attempt to test it I get the same output as before:

![LambdaOutputImg](/assets/claudia-tutorial/LambdaSuccess.png)

## Conclusion

Congrats you are all done! You have setup Claudia and now can use this as a building block for your real applications!

Overall, using Claudia.js will help with development by making it easier and faster to start a Lambda function and test it.  I used to develop my Lambda functions the manual way and that took forever. I can honestly say this is something you should definitely be using.

Reach out and follow me on [twitter][twitter]!  Check out my [GitHub][github] for my open source projects!

For further information on Clauda.js check out their main [page][claudiaHome].

[github]: https://github.com/acucciniello
[twitter]: https://twitter.com/antocucciniello
[claudiaTut]: https://github.com/acucciniello/claudia-tutorial
[claudiaHome]: https://claudiajs.com/
[awsSignUp]: http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/AboutAWSAccounts.html
[nodeInstall]: https://docs.npmjs.com/getting-started/installing-node


