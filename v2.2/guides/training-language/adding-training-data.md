---
title: "Adding Training Data"
excerpt: ""
---
Natural language is incredibly broad, capable of expressing concepts and actions in many domains. This makes it incredibly difficult for a computer to understand and be able to act on natural language. To make effective applications, Init.ai facilitates teaching of specific domains relevant to your business.

## Training conversations

Training data for language knowledge is composed of training conversations. A training conversation is a set of messages between an end-user and the system. These conversations must be annotated to indicate the intent of the sender of each message and highlight any entities within each message.

### Example conversations are created in two ways:

- Using the Training Conversation Editor in the Console
- Using conversation logs from end users `Coming Soon` 

Training data is stored and edited using Conversational Markup Language or CML in your project.


## Adding training data

Init stores all example conversations in CML files. The format is straightforward and is inspired by Markdown style syntax

Init.ai provides an editor within the console to create and edit training conversations. The training conversation editor allows you to create new example conversations by simulating both sides of a conversation -- ie you work as both the end user and the conversational application.

## Viewing your language components

There are four views in the Language section of the Console and they all serve a specific purpose.

**1 Training Conversations**
Displays all of the training conversations you’ve saved and allows you to create new ones.

**2 Intents**
Displays all of the intents that are associated with your training conversations and language model

**3 Entities**
Displays all of the entities present in your training conversations and language model

**4 Models**
Displays all of your current and previous models (as well as the status of any models in training). It is also where you tell our system to begin a training run.