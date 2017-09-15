---
title: "Adding training data"
excerpt: "A critical step in creating your conversational application is to provide the language knowledge required to your app's specific domain. This is done by providing training data."
---
Conversational apps, are by name and definition, conversational. That means their interface is natural language.

Natural language is incredibly broad, capable of expressing concepts and actions in many domains. This makes it incredibly difficult for a computer to understand and be able to act on natural language. To make effective applications, Init.ai facilitates teaching of specific domains relevant to your business.
[block:api-header]
{
  "type": "basic",
  "title": "Training data"
}
[/block]
Training data for language knowledge is composed of example conversations. An example conversation is a set of messages between a human user and a conversation app that has been annotated to indicate the intent of the sender of each message and highlight any relevant terms and data values within each message.

Example conversations are created via a few routes:

- Directly by a developer or designer using Init's training tools
- Directly by a developer using text files in Init's CML format
- By reviewing conversations with end users and correcting their annotations

Training data created by any of these routes is stored using **Conversational Markup Language** or **CML** in a project's Git repository. The training tools facilitate reading and writing of these files in a simplified manner.
[block:api-header]
{
  "type": "basic",
  "title": "Adding training data"
}
[/block]
Init stores all example conversations in **CML** files. The format is straightforward, but sometimes writing these files directly can be tedious.

Init.ai's conversation training interface  is embedded within its console. It looks like this:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/adcc459-console_csi_save_convo_umbrella_1.png",
        "console_csi_save_convo_umbrella_1.png",
        600,
        1058,
        "#fafbfb"
      ]
    }
  ]
}
[/block]
The training interface allows for teaching your application new language terms interactively.

It has a few views:

- **Chat** – Where you test how your application is responding
- **Teach** – Where add training data, annotate that data, and classify it
- **Language** – Shows all available entities and message classifications

#  Chat view

The chat mode is an interactive component that simulates interacting with your conversational app as an end user. In fact, your Init developer account gets its own special end user account within your conversational app that functions exactly like a normal user account.

In chat mode, you type inputs as an end user would and then wait for your app's response.

For each message, you can get information about how the NLP system classified it, but you cannot make corrections.

# Teach view

The teach mode of the Conversation Trainer allows you to create new example conversations by simulating both sides of a conversation -- ie you work as both the end user and the conversational application.

The teach mode is used for creating an application's core set of language knowledge, as well as for extending its features.

In this mode, outbound messages from the application are directly entered by the developer
New classifications for both messages and slots can be entered using the UI.

After a conversation is annotated, you can then save the conversation to your application's training data.

Since the Conversation Trainer runs in a console but needs to be able to add to your application's training data, it needs to be able to read and save files in your application's Git repo checked out to your computer. It does this by communicating with the Init Dev Server. To save conversations in Teach mode, you must run the Init.ai Dev Server from the checked out source code for your application.

# Language view

The Language mode provides a high-level view of your application's language knowledge.

This mode useful when adding new language features to your application since it serves as a reference for all message and slot classifications that already exist in your application. You can also double-check that existing classifications are assigned correctly, helping to troubleshoot problems in your application's training data.

This mode is also where to begin when you need to change existing features. For example, if you need to change an existing classification to another name because of a feature change in your application, this mode lets you find all relevant conversations.

The Language mode also allows you to view the outbound message templates defined for your application. This is important when developing your application's logic, since when sending a message to a user you should use Init.ai's templating system, which enables more natural conversations and easier logic development.