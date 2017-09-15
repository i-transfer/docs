---
title: "Conversation Logic"
excerpt: "Conversational apps can be quite intelligent. But it is up to you, the developer, to bring them to life."
---
Init.ai provides the necessary tools to make a great conversational apps:

* language understanding
* conversation management
* messaging connections
* The ability to connect to external APIs

To tie the pieces together you need to provide your application's conversation logic. Our NLP system takes the intent of end users and transforms it into actionable data. Your custom logic can then use that data to do any number of things. It can reply to simple questions, pull or provide data in your own systems – like ordering a product or updating a reservation, or it can interact with external APIs to do lookup necessary information.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a356fe2-logic-flow.png",
        "logic-flow.png",
        1232,
        614,
        "#fbfbfb"
      ]
    }
  ]
}
[/block]
The Init.ai platform provides tooling to ensure your application's logic is closely connected to other components – like language processing.

As a developer, it is up to you to provide custom logic to manage the logic associated with your application. This custom logic is written in JavaScript and executed in Node.js (more specifically, [AWS Lambda](https://aws.amazon.com/lambda/)). Each project created on the Init.ai platform ships with our [Node.js](https://github.com/init-ai/initai-node) library as a default dependency. This library is used to control conversations programatically. Each time a user sends a message or an [event is triggered](doc:events), your custom logic will be executed with the ability to read and write arbitrary conversation state and compose responses based on intent of the inbound message.