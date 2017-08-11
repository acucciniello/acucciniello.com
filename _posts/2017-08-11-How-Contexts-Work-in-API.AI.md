---
layout: post
title:  "How Contexts Work in API.AI"
date:   2017-08-11 12:20:10 
---


## Introduction

If you read my last post API.AI has a more in-depth conversational flow then normal Alexa Intents and Utterances have.  API.AI introduced the idea of *contexts* in conversations.  It allows API.AI to know what the user is saying and in what context.  So today I am going to explain how this works!

## API.AI

So in order to use contexts, you must set them up inside of an intent in API.AI.  Here is what they look like:

![Context](/assets/contexts/contexts.png)

You have the option of adding an input context(s) or an output context(s) to an intent.  Let's take a look at both.

### Output Context

This is added on the second line in the context section.  When an output context is added to an intent it gets a lifespan.  The lifespan of the output context determines *how long* the context will last in terms of intents.  So an output context with a lifespan of 2 will be a context for the next two intents.  The output context is used to determine what are the possible follow-up intents to the current intent that was called. It determines this by matching a follow-up intent's input context with this intents output context.

### Input Context

This is added to the first line of the context section in intents.  If you state that an intent has an input context then it can only be invoked when the output context of the current intent has the same context.  When the user responds to the previous intent with a *User Says* that matches what the intent has, and a matching input context with output context. 

## Code

Now that you know what intents look like in API.AI, if you are using contexts, you can now take entities that you get from the user in previous intents and use them in future intents that are still in the same context.  This becomes useful when you need API.AI to use information that was previously said by the user in a later intent.  This makes API.AI smarter in the sense of almost having a memory for the conversation between it and the user.  

You can access the entities in your code using the API.AI actions on google SDK like so:

```
app.getArgument('number')
```
 
This gets the value of the entity with a type of 'number'.

## Conclusion

The concept of using contexts in conversations confused me for a while.  I am now a little more versed with it.  I hope that this blog post was able to shed some light on the topic and aid you in your development of it. 

If you enjoyed this post join our newsletter to always get free content sent straight to your inbox!

Reach out and follow me on [twitter][twitter]!  Check out my [GitHub][github] for my open source projects! Take a look at my [YouTube Channel][youtube].


[github]: https://github.com/acucciniello
[twitter]: https://twitter.com/antocucciniello
[youtube]: https://www.youtube.com/channel/UC8icMMql5SjCaXXMvILGIUA
[gistLink]: https://gist.github.com/acucciniello/faff332c5c401402d698ca5fd2af9dba
[firebaseTut]: https://github.com/actions-on-google/apiai-webhook-template-nodejs