---
layout: post
title:  "How to Get Device Address from Alexa Device - Device Address API"
date:   2017-08-25 12:20:10 
---


## Introduction

Have you ever wanted to build an Amazon Alexa Skill that uses your device's address or location? If so you are in the right place.  Amazon recently came out with the Device Address API that allows you to get the device's postal code or address information.  Today I will show you how to have a skill that uses the device address API.

## Developer Portal

When you have made your skill, you can head over to the section *Configuration->Permissions*.  Make sure to check the box to get the full address or just postal code depending on what you need from the user.

![Developer_Portal](/assets/deviceAddress/devportal.png)


Now it is time to head over to your code.


## Code 

In the intent that gets launched when you say 'do something near me' or 'find something nearby' have a function that will contain the following flow:

Using the alexa SDK, you need to access two objects from the request. The first is `deviceId`, which is the specific identifier for the device you want an address of.  The second is `consentToken	`, which proves that the user has given consent to read the address. 

Device Id is accessible by: `this.event.context.System.device.deviceId`.

Consent Token can be retrieved with:
`this.event.context.System.user.permissions.consentToken`.

Before you can do that, you must check to see that the user has permissions granted.  If they do not you must prompt the user with a card to grant permissions.  You could do this with:

```
const ALL_ADDRESS_PERMISSION = 'read::alexa:device:all:address'
const PERMISSIONS = [ALL_ADDRESS_PERMISSION]
var askForPermissions = 'We need permission to your location'

// inside the if statement where you check if permissions were given

this.emit(':tellWithPermissionCard', askForPermissions, PERMISSIONS)
``` 

Once we have a valid `deviceId` and `consentToken`, we must hit the device address api endpoint.  It takes the following form:

```
https://api.amazonalexa.com/v1/devices/{deviceId}/settings/address
```

Here you would insert the deviceId that you retrieved from your intent request.  Next, you would send a GET Request to the url with the authorization header of `Bearer + consentToken`.  Like the following:

```
var urlOptions = {
	headers: {
		Authorization: 'Bearer ' + consentToken
	}
}
```

You can now send it with a GET Request using `got`.  

```
got(url, urlOptions)
```

This will return a response of something like this:

```
{
  "stateOrRegion" : "WA",
  "city" : "Seattle",
  "countryCode" : "US",
  "postalCode" : "98109",
  "addressLine1" : "410 Terry Ave North",
  "addressLine2" : "",
  "addressLine3" : "aeiou",
  "districtOrCounty" : ""
}
```

Now all that is left is to parse the data that was received.  Once that is done you now have plenty of information about the device's address in order to finsih your skill's processing!

## Conclusion

That is how you can successfully get the device address from your Amazon Echo in your skill. It is pretty straight forward!

One tip, the responses will come back as `null` if you have not set the addrses in your device settings in the Alexa companion mobile app.  Make sure you have set your address in the application otherwise the code will have a bunch of null values that are returned.

If you enjoyed this post join our newsletter to always get free content sent straight to your inbox!

Reach out and follow me on [twitter][twitter]!  Check out my [GitHub][github] for my open source projects! Take a look at my [YouTube Channel][youtube].


[github]: https://github.com/acucciniello
[twitter]: https://twitter.com/antocucciniello
[youtube]: https://www.youtube.com/channel/UC8icMMql5SjCaXXMvILGIUA
