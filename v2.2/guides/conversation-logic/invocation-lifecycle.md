---
title: "Invocation lifecycle"
excerpt: ""
---
The logic system is triggered either by a message from an end user or a programmatic [event](doc:events). These triggers are sent via [webhooks](doc:webhooks) to your server with payloads containing data such as NLP results, conversation state, and participants (users).

The lifecycle of an invocation is generally as follows:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/abad0bc-Invocation_lifecycle-3.jpg",
        "Invocation lifecycle-3.jpg",
        1573,
        863,
        "#5c76ce"
      ]
    }
  ]
}
[/block]
1. Incoming _message_ or _event_ processed for intent and stored
2. A [`LogicInvocation` webhook](webhooks#section-logicinvocation) is sent to your server
3. Your server runs necessary conversation and API logic (with the help of the [Init.ai Node.js SDK](https://github.com/init-ai/initai-node))
4. Your server sends the result of the logic run to Init.ai
5. Init.ai applies any state changes made by your logic. The appropriate outbound message(s) are sent and the [`MessageOutbound` webhook](webhooks#section-messageoutbound) is called