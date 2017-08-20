---
layout: post
title:  "How to Request for Permissions in Google Assistant Actions with API.AI"
date:   2017-08-18 12:20:10 
---


## Introduction

Are you building a Google Assistant Action with API.AI? If you are you may need to access your device's location in order to use the application.  Today I will discuss how to properly request for permission to access the user's location and follow it up in API.AI.

## Before the Request

In order to launch a request permission flow, you first need an intent that would need the location of the user.  When you are in this previous intent, you would first check if the permissions are granted using API.AI actions on google sdk in your webhook.  If the permissions were already granted, then you do not need the rest of the tutorial. (Why are you even here then?)  If you find out your permissions are not granted then you would need to call `app.askForPermission()` in the webhook for this intent.  Here is what you would do:

```
if (app.isPermissionGranted()) {
	// continue with application processing with  location
} else {
   // ask for permissions
   app.askForPermission(context, permission dialogState)
}
```

The documentation for `askForPermission()` is [here][askForPerm] and for `isPermissionGranted()` is [here][isPermGrant]

## During the Request 

When the code executes `app.askForPermission()` it automatically prompts the Assistant to say: 'Can I access your location?'.  The user should respond with yes if they would like to continue using your action.  


## After the Request

Now, you must handle what happens now that you have asked the user for permission.  So we must create an intent to handle the follow up.  You must set up this intent to have an *event* of `actions_intent_PERMISSION`.  This tells API.AI that when the event actions_intent_PERMISSION is called, then automatically launch this intent.  

Now API.AI knows to execute this intent every time you ask for permissions.  In the webhook for this new intent, you must check again if permissions were actually granted (because the user can say no).  You would check it with `app.isPermissionGranted()`.  This will verify if we actually have permissions and now you can continue on with your application's execution!

## Conclusion

It is that simple! You now have an action that asks for the users location properly!

If you enjoyed this post join our newsletter to always get free content sent straight to your inbox!

Reach out and follow me on [twitter][twitter]!  Check out my [GitHub][github] for my open source projects! Take a look at my [YouTube Channel][youtube].


[github]: https://github.com/acucciniello
[twitter]: https://twitter.com/antocucciniello
[youtube]: https://www.youtube.com/channel/UC8icMMql5SjCaXXMvILGIUA
[gistLink]: https://gist.github.com/acucciniello/faff332c5c401402d698ca5fd2af9dba
[askForPerm]: https://developers.google.com/actions/reference/nodejs/AssistantApp#askForPermission
[isPermGrant]: https://developers.google.com/actions/reference/nodejs/AssistantApp#isPermissionGranted