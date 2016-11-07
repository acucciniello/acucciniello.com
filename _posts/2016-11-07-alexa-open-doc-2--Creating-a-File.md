---
layout: post
title:  "alexa-open-doc #2: Creating a File"
date:   2016-11-07 13:50:10 
---


## Background


If you have read my [first post][firstBlogPostLink], you know I successfully was able to achieve listing the last ten files I edited in my Google Drive.  The next step in functionality is the ability to create a file to use in Google Drive.  Once again to implement this, it took work on both the Amazon side and on the Google side.


## Amazon Work


When invoking the skill previously, the user would say something like: **"Alexa, ask Google Drive to list my files please."**  When a user says that phrase or a similar phrase, Alexa understands it and classifies it in what is called an *Intent*.  An intent is basically a way for Alexa to understand what you intend on expecting as an output.  Now that I wanted to add a new function in my skill (intending a different output by creating a file instead of listing a file), I needed to add a new intent to my intent schema in Amazon. For on specifics of intent schemas check out this [resource][intentSchemaLink]. I added this to my intent schema for Amazon to understand what this kind of intent was:




![CreateFileIntentImage](/assets/alexa-open-doc/2/create_file_intent.png)]


In the image, you may see what are called *Slots*.  A slot is basically something that Alexa should expect from the user.  It takes a name and a type.  For more information on slots and slot types check out the above link on intents.  Here I created a *fileName* slot to handle the file name specified by the user, and I created a *fileType* slot to handle the type of file the user would like to create.  The types of these were custom slot types that I made to show exactly what I a *fileName* and *fileType* would look like to help Alexa understand how it is used.  In order to define the custom slot types, I went to the **Amazon Developer Portal** for the skill then **Interaction Model**, on that page, scroll down to Custom Slot Types and Click on **Add Slot Type**. To define custom slot type, create it with the same name as you placed in your intent schema.  Then fill the document with line-separated values of how you expect the input to be.  Below is an example for FILE_NAMES type:


![FILE_TYPESImage](/assets/alexa-open-doc/2/fileName_Slot.png)


Once a custom slot type is created, it is time to create *Sample Utterances* of how you expect the input from the user to be.  The format for an utterance is: NameOfIntent + sample phrase + slotNames.  For example, here is the sample utterance for this intent was: *CreateFileIntent to create a {fileType} called {fileName}*.  


Once you have defined those, now you can start to edit your code to handle the intent.  First step, would be to create a file that is called IntentNameFunction.js. This way when you have plenty of intents, you know what file handles which intent. Now require this file in your entry point file(for me, it was service.js, for others could be index.js, or main.js) and add this line under your *intentHandlers* : **'CreateFileIntent': CreateFileFunction,**.  Now you can save and go ahead to start solving the Google Drive side of the problem. 


## Google Work


Now, time to do the actual work of creating file in Google Drive. Thanks to Google Drive API this is simple.  All it is was using a function called *files.create*.  This authenticates a user, takes in file's metadata(which consists of a name and a mime type), and the return fields.  To read more about the parameters of the files.create function check out this [link][fileCreateFunction] to Google Drive API.


So you are probably thinking: "Yay! We're done here Antonio, right???" Not so fast, in order for the user to specify what kind of file that they would like to create, I needed a way to handle what they wanted to say and how Google understands it.   So, I created a function that takes in words like document or spreadsheet and converts them to their specific mime type.  This way google knows that when a user says document they mean to specify the mime type for a document.  


Now that you have Google working, it's time to combine the two and get to creating files using your voice!


## All Together


Now in your file for your function that handles the new intent, you need to be able to access the user's input by doing: **intent.slots.slotName.value**.  Now you have the value of exactly what the file name should be and what the user wants as the type of document to create.  Pass those into your functions for finding the mime type and actually creating the file.  Now the skill works just as I expected.  Here is a video of it all working:


<iframe width="560" height="315" src="https://www.youtube.com/embed/PEA7v2P6FRU" frameborder="0" align="middle" allowfullscreen></iframe>


## Next Steps


The next step is implementing appending to the end of a file.  Stay tuned for how I complete that! Checkout the project on GitHub [here][linkToProject]


[firstBlogPostLink]: http://www.acucciniello.com/2016/10/18/alexa-open-doc-1-Introduction-and-Background.html
[intentSchemaLink]: https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/alexa-skills-kit-interaction-model-reference
[fileCreateFunction]: https://developers.google.com/drive/v3/reference/files/create
[linkToProject]: https://github.com/acucciniello/alexa-open-doc


