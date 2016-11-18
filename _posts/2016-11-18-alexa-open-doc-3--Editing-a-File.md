---
layout: post
title:  "alexa-open-doc #3: Editing a File"
date:   2016-11-18 13:50:10 
---


## Background


If you have read my [second post][secondBlogPostLink], you know that the this skill can now create a file in your Google Drive using your voice.  The next step in functionality is the ability to add text to the end of a file in your Google Drive.  To implement this, we will need to do some work on the Amazon side and then some work with Google Drive API.  Let's discuss the Amazon work first!

## Amazon Work

In order to add new functionality to the skill, we needed to create a brand new Intent.  I discussed this in more detail in my last [post][secondBlogPostLink] if you would like a more detailed explanation of how that process is done.  This time I created an intent called *EditFileIntent*.  the purpose of this intent is to take in a file name and the text you would like to add to the end of the file.  This is what the intent looked like in the intent schema:

![EditFileIntentImage](/assets/alexa-open-doc/3/edit_file_intent.png)]

The next step was adding a new slot type in order to handle the text input.  I created a slot type called *INPUT_TEXT*.  The example phrases are just random sets of texts to make alexa understand when I am trying to add text to an end of a file.  Here is what the slot looked like:

![INPUT_TEXTImage](/assets/alexa-open-doc/3/input_text_slot.png)

The last step for getting this properly working, is to add sample utterances so Alexa knows that when I say something similar to these to classify them as EditFileIntents and execute the code associated with it.  Here is how it looked when all done:

![SampleUtterancesImage](/assets/alexa-open-doc/3/edit_file_sample_uttterances.png)

Next up get the proper functionality working with Google!

## Google Work


Now, time to do the actual work of adding text to the end of a file in Google Drive. Unlike creating a file, this process takes the use of multiple functions.  The first step here is to take the file name that the user wants to edit and find it in Google Drive.  In order to do this, I need to use *files.list* function in a different way then used in the first post.  I needed to add a parameter to the function to query the files: *q: name contains = fileName*.  This is the only way to search for a file name while being case insensitive.  One drawback of this is that it is prefix matching.  Meaning if I was looking for a file name called "Ten Laws", and I searched "Laws Ten" it would not find the file. Once the file was found, I needed to obtain the file id in order to do more processing.  That is where the next step comes in.

Once I had access to the id of the file the user wanted to edit, I could now use the Google Drive function *files.export* in order to download the text in the file and store it locally.  For documentation on that function check it out [here][fileExportFunction].  I used mimeType for exporting as 'text/plain'.  Now that I had the text locally, the next logical step would be adding the text that user specified to the end of the file.

In order to add text to the end of the file, I created a simple function that took the input the user wanted to add (to do this you access *intent.slots.inputText.value*) and concatenate that to the end of the text exported in the last step.  After this step, we can move onto the last step for completing this functionality, which is to re-upload the file back up to Google Drive with the added text.  

To upload a local file to Google Drive I used the function *files.update*.  For documentation on this function check it out [here][fileUdateFunction].  This takes the file id, the text of the file, and the mimeType which is 'text/plain'.  This updates the file in your Google Drive with the newly added text on a new line!

Now that you have Google working, it's time to combine the two and get to adding to files using your voice!


## All Together

Similar to the last post you need to create a new function that Amazon recognizes and add it to your main file.  This is the function that will go search for the file name, obtain the file id, export the text from the document, add the text to the end of the file, then update the file with the new text.  Exciting stuff!


Now the skill works just as I expected.  You say *"Alexa, ask Google Drive to edit the {fileName} with {inputText}."*  Here is a video of it all working:


<iframe width="560" height="315" src="https://www.youtube.com/embed/YR8YMkR6JKo" frameborder="0" allowfullscreen></iframe>


## Next Steps


The next step is publising and testing the skill! What would you like to see added to the functionality of this skill? If you have any ideas reach out to my email antonio.cucciniell16@gmail or on twitter @antocucciniello!  Checkout the project on GitHub [here][linkToProject].


[secondBlogPostLink]: http://www.acucciniello.com/2016/11/07/alexa-open-doc-2-Creating-a-File.html
[fileExportFunction]: https://developers.google.com/drive/v3/reference/files/export
[fileUpdateFunction]: https://developers.google.com/drive/v3/reference/files/update
[linkToProject]: https://github.com/acucciniello/alexa-open-doc


