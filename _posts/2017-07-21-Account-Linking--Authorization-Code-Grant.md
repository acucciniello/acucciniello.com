---
layout: post
title:  "Account Linking: Authorization Code Grant"
date:   2017-07-21 12:20:10 
---


## Introduction

Have you ever wanted to link your application to a third party application? This is where you can use **Oauth 2.0**.  It is a form of authentication that allows you to link third party applications to your application very easily and securely.  Today I will talk to you about the flow of Authorization Code Grant.  It is one of the forms of Oauth that can be used. 

## Step 1: Sign In

In order to start the process, there must be an *Authorization Url* that you try to access from your application.  This usually looks something along the lines of this:

![SignIn](https://github.com/acucciniello/acucciniello.com/blob/gh-pages/assets/oauth-auth-code/signin.png)

It presents you with what the application is trying to use or read and if you accept the terms and permissions it is being asked to grant.

From a development perspective there are something's that this Authorization URL must contain. 

First is `client_id`.  This is a id that is specific to your application that is used by the third party to verify which application is asking for permissions. 

The next parameter is `response_type=code`. This tells the application to use the Authorization Code Grant form of authentication and tells the third party it is looking for an auth code in return.    

The third parameter is `scope`.  This is application specific and is usually not always necessary.  This determines what the application is actually trying to get permission to.

Next, is the `redirect_uri`. This is the url to redirect the user to once the user signed in.  This is usually where the authorization code is received.

Last, is the `state` parameter.  It is a string that does not get changed throughout the entire authentication process.  It should be randomly generated in most cases.

Now you usually have an authorization code. 

## Step 2: Exchange Code for Token

Once you have an authorization code, you must exchange that within ten minutes of receiving it for an `access token`.  The exchanging happens when the redirect_uri is hit.  The redirect_uri receives your code and now hits a *Token URL* to get a token for the url.  For this you must also have some parameters.

From the Authorization URL, you must use the same client_id, state, and redirect_uri.

The new things added here are a `client_secret` and `code`.  The client secret is secret key specific to your application that should not be shared with others.  The code is the authorization code that is received from the authorization url in the last step.


This will trade the authorization code for the access token.  Now it is up to how you would like to store the access token received from the user.  

## Step 3: Authenticate User

This last step here is to authenticate the user on each request.  This is done by asking your application for the user's access token.  If it is there and it is valid, the user can access the third party functionality.  If it is expired, then you may use a refresh token to get a new acccess token.  If it is not valid and there is no refresh token, you must get permission all over again.

## Conclusion

In the beginning Oauth was a little confusing to me.  I hope that I made the process clearer as I have tried to authenticate a few applications now to my Alexa and Google Home applications.

Reach out and follow me on [twitter][twitter]!  Check out my [GitHub][github] for my open source projects! Take a look at my [YouTube Channel][youtube].


[github]: https://github.com/acucciniello
[twitter]: https://twitter.com/antocucciniello
[youtube]: https://www.youtube.com/channel/UC8icMMql5SjCaXXMvILGIUA