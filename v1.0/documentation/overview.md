---
title: "Overview"
excerpt: "Init.ai is a hosted platform for building intelligent conversational apps."
---
We provide all of the components needed to build these apps including Natural Language Processing, machine learning, integrations with messaging platforms, and a client API to control the conversation.
[block:api-header]
{
  "type": "basic",
  "title": "Platform at-a-glance"
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/f4f9dd7-guide_parts.png",
        "guide_parts.png",
        1976,
        782,
        "#7e92a1"
      ]
    }
  ]
}
[/block]
When a message comes through our system it gets processed by the NLP system, run through any client API code that has been deployed. Then, a response is selected, populated with any dynamic data, and sent back to the user. The apps can also be configured to send notifications and other messages which don’t rely on user input or action.
[block:api-header]
{
  "type": "basic",
  "title": "Natural Language Processing"
}
[/block]
In order to have an engaging conversation with one of these applications you need a pretty good Natural Language Processing (NLP) layer. Our NLP was built from the ground up to specifically understand input from conversational level. This means that instead of only understanding single input message and returning a single output, our NLP is trained and built specifically around conversation.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/3ffc996-getting_started_nlp.png",
        "getting_started_nlp.png",
        1780,
        1510,
        "#e5e1d1"
      ]
    }
  ]
}
[/block]
The benefits of handling NLP this way are numerous, but the most significant are the ability to be completely contextually aware, handle vague follow-up questions, and even predict subsequent messages in a conversation.

Out-of-the-box, your every project comes with a basic language model that understands English and basic parts of speech. This means you’ll only need to focus on teaching it the terms and knowledge specific to what you’re building.

[block:api-header]
{
  "type": "basic",
  "title": "Teaching your language model"
}
[/block]
Teaching is an integral part of what makes Init so powerful. Your app knows very little about its world when it is first created, so you have to teach it the **key terms**, **conversations**, and **definitions** it should know. We do this by giving your language model “Conversation data” through an interface that we provide inside our web app. This is the basis for teaching your language model what it should understand.

## What the heck is a language model?
Our language model is a component of the platform that understands natural language and conversation. This model resembles a computer program, it takes input (in this case text) and provides an output (meaning, response, etc.), and **the more input you provide the smarter it gets.**

[block:callout]
{
  "type": "info",
  "title": "Dive deep on language",
  "body": "Our tools provide a way to train these language models and then control them with code. We'll dive into more training in our [Training & Language](doc:training-language) guide."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Conversation logic & controlling conversation flow"
}
[/block]
Once you've inputted some training data we can start customizing your conversation Flow with our logic system. Our logic system talks directly to our NLP and lets you choose how to execute pieces of conversation based on the training data you've provided.

Though your app has an understanding of the language **it does not have any concept of how it should respond to a user**. So, we will need to help it select responses (which you have given to it via the training data) and populate those responses with any dynamic data before sending it to the user.

## The conversation flow

A conversation is a collection of **Flows** that contain individual **Streams** which are made up of atomic **Steps** to manage discrete portions of the conversations state. Starting from the outside and working our way down: 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/2ead0cf-conversation-engine.png",
        "conversation-engine.png",
        1590,
        696,
        "#3696c1"
      ],
      "caption": "What a generic 'Flow' might look like",
      "border": true
    }
  ]
}
[/block]
**Flow: **A Flow is a representation of the path a conversation can take at a high level. It can contain multiple Streams.

**Stream:** A Stream is a component of a conversation that represents a user trying to accomplish a certain goal. Streams can contain multiple Steps.

**Step:** A Step is an individual stop or checkpoint in a conversation.

**Auto Response:** Based on a particular classification you can have your application respond automatically. We recommend using this for simpler tasks that don't require any data collection.
[block:callout]
{
  "type": "info",
  "title": "Conversation Logic Guide",
  "body": "Check out our [Adding logic](doc:adding-logic) guide for a more in-depth understanding of the logic system"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Messaging platforms"
}
[/block]
We support a handful of the most popular messaging platforms including Facebook Messenger, Twilio SMS, Twilio IP Messaging, and Telegram. Each platform is nuanced in different ways and we adjust how these outbound messages are translated across messaging platforms for you.
[block:callout]
{
  "type": "info",
  "title": "More on messaging",
  "body": "Learn more about our [Messaging Connections](doc:messaging-connections)"
}
[/block]