---
title: "Basic workflow"
excerpt: "A critical step in creating your conversational application is to provide the language knowledge required to your app's specific domain. This is done by providing training data."
---
## Adding and testing with training conversations

1. Add Training Conversations with the Training Conversation Editor illustrating how the conversation should flow.
2. Train the Language Model to update with the new Training Conversations
3. Update your Conversation Logic to interact with the Intents and Entities provided in your Training Conversations (For more: see Logic guide)
4. Test your changes using the Chat interface within the Console
  1. If it does not behave as expected:
    1. Check for Javascript errors, update your Conversation Logic and test
    2. Check for NLP errors, update Training Conversations and re-train the Language Model


## Adding data from conversation logs `Coming Soon!` 

You can view all incoming conversations that have occurred with end users. This allows you to check how your applications are performing in the real world, and diagnose issues you’re seeing with conversations.

Soon, you will be able to import any conversation log into the training editor to improve your language model.

![Conversation logs](https://files.readme.io/b3b38d9-screencapture-localhost-8000-1493140854166.png)