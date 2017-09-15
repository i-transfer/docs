---
title: "Basic workflow"
excerpt: "A critical step in creating your conversational application is to provide the language knowledge required to your app's specific domain. This is done by providing training data."
---
[block:api-header]
{
  "type": "basic",
  "title": "Adding new language features"
}
[/block]
- Use the Teach view to write several example conversations illustrating how the new feature will appear to the end user and save these to your training data.
- Update your application's logic to support the new feature, including mapping any newly added message classifications to the appropriate code in your Flow (see Logic guide)
- Push your changes to GitHub, to initiate a deploy to Init.ai
- Test your changes using the Chat mode.
- If your app does not behave as expected:
 - Due to logic system errors, update your app's logic and repeat steps 3 and 4
 - Due to NLP misclassification, copy the incorrect conversations from Chat to Teach, correct and save them, then repeat steps 3 and 4
- Repeat as needed
[block:api-header]
{
  "type": "basic",
  "title": "Conversation logs"
}
[/block]
Conversation logs enables you view conversations that have occurred with end users, so that you make sure everything is working properly, and in the case of problems, diagnose and solve them
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b92d0de-conversation-log.png",
        "conversation-log.png",
        1716,
        1060,
        "#3f6092"
      ]
    }
  ]
}
[/block]
If a user of your application encounters an error, find the conversation in the Conversation logs view. If the NLP system misclassified a message or a slot, you can copy the conversation to Teach (while the Dev Server is connected) so that you may correct the issue and add the conversation to your training data so the experience will be better next time.