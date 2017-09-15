---
title: "Message Lifecycle"
excerpt: ""
---
The logic system is triggered either by a message from an end user or a programmatic event. Your Conversation Logic is provided with a payload containing data such as NLP results (for textual messages), conversation state, and data regarding participants (users).
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a49741e-logic_image_1.png",
        "logic_image_1.png",
        925,
        378,
        "#dcdad7"
      ]
    }
  ]
}
[/block]
# Lifecycle of an invocation

1. Incoming _message_ or _event_ processed for intent and stored

1. Logic system is invoked with an instantiated instance of the [Init.ai Node.js client](https://github.com/init-ai/initai-node) is provided to your exported `handle` function in your JavaScript. This client provides access to the necessary conversation data. See the [`docs`](doc:client-api) for more

1. Init.ai applies any state changes made by your logic and sends the appropriate outbound message as constructed