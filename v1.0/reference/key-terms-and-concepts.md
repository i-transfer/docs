---
title: "Key terms and concepts"
excerpt: ""
---
# Key terms and concepts

## Init.ai platform and general messaging

# Message

A message represents a piece of text or other content such as an image sent by a human or by an application.

A message usually corresponds to one press of "Send" within a messaging application.

A message contains one or more [message parts](#section-message-part).

# Message part

A message part is a piece of a message, generally corresponding to an individual sentence, that represents one part of a message received from a user.

Init.ai parses messages composed of text into multiple parts as needed, generally by splitting sentences, so that each message part can be classified individually and processed accordingly.

A message part can also be an image, video, or other rich content.

An [event](#section-event) is a special type of message part that contains a data payload. This can be processed by the business [logic system](#section-logic-system) to handle changes in the conversation state or the state of a user.

# Event

An event is a set of data the occurs at a certain time to indicate that something about the state of a conversation or a user has changed, or to indicate that an action needs to be taken by an [application](#section-application).

An event is usually sent inbound to an app when an external action has been taken. For example, if a conversation initiates a payment flow, when that payment has been collected by the application's creator (via an external website, etc), then an event could be sent to an application to record that payment in the user's conversation state.

Events are composed of an event type and a payload. An application processes incoming events in a similar way to messages containing text. In fact, an event is actually treated as a special type of message part by the Init system. The payload can contain arbitrary data.

# Project

In the Init.ai platform, a project represents a unique deployment of the code and language data that defines an [application](#section-application).

In this way, the same [application](#section-application) may be deployed to multiple projects.

A project contains data about its end users and has its own connections to [messaging platforms](#section-messaging-platform).

# Application

An an application represents a service, available through a natural language interface, that performs a specific function for its end users and for its creator.

An application is distinct from a project in the application can be thought of as what the end user sees and everything behind it. A project in Init.ai is a single instance of an application. If you are familiar with web development, a similar relationship happens when an API is deployed to multiple environments, or on multiple servers, from the same codebase.

# Conversational app

A conversational app, sometimes referred to as a conversational interface, is a way in which a user can interact with a business or service through natural language. A conversational app can be presented to its users within a messaging app such as Telegram or Facebook Messenger, through SMS, or within a chat window embedded in a mobile application or website.

A conversational app may have a human or a computer on each side of the conversation.

# Messaging platform

A messaging platform is an end-user service that allows people to communicate with each other and with automated services like bots and conversational apps.

Examples include: Facebook Messenger, Google Hangouts, Skype, Telegram, Kik, etc.

Some technologies like SMS are not technically messaging platforms, but may be referred to as such by Init since they connect users with conversational apps.

# Inbound message

An inbound message is a message sent by a human to a conversational app.

# Outbound message

An outbound message is a message sent by a conversational app to a human.

# Logic system

Conversational apps can include some programming to control their behavior and how they respond to incoming messages and events. The system that controls this is the business logic.

Init.ai processes incoming messages to determine the intent and desire of users, and passes this information to the business [logic system](#section-logic-system) to determine how to respond.

Currently, logic refers to the JavaScript code that you have written using our Client API which Init.ai will execute within your [conversational app](#section-conversational-app).

# Console

The Init.ai console ([https://console.init.ai](https://console.init.ai)) is web application used to view information about a project that has been deployed to the Init.ai platform.

This information includes logs of conversations, information about its end users, as well as configuration options relevant to the specific deployment of the application, such as its connections to messaging platforms.

The Console also provides a user interface for creating and editing CML as well as testing your application.

> **Note:** The console currently only supports the latest versions of Chrome and Firefox

# Git repository

Applications built on Init.ai are formed from a collection of files that define the language model, behavior, and certain types of configuration. These files are stored in a Git repository so that changes to these files can be tracked over time and so that developers can easily collaborate.

# Dev Server

The [Init.ai Dev Server](/reference/dev-server.md) provides training server and toolkit for developing Projects on the Init.ai platform. It contains a server which facilitates editing of your CML files via the Console.

# Conversation Trainer

The Conversation Trainer is a UI element that is used for testing and training an Init.ai application.

In test mode, the Conversation Trainer simulates interacting with an application as an end user would.

In training mode, the Conversation Trainer is used to generate example conversations to improve the application language model. These conversations that will be used to improve and expand an application's understanding of the natural language relevant to the application. In training mode, these example conversations are read from and saved to files with in the application's Git repository via a connection to the [Dev Server](/reference/dev-server.md). This view will be disabled while the Dev Server is inactive.

# Training data

Training data is the data used to create an application's language model.

Training data consists of sets of example conversations that are read by Init.ai to create the NLP model.

# Version

Init.ai projects are consistently versioned since they are deterministically defined by files in the project’s training data and logic.

Since Init requires that Git be used to store and track changes to applications, the version of an Init project corresponds to the revision of the Git repository.

# Deployment

A deployment is the act of updating an application running on the Init platform with a new set of files that define its language knowledge and behavior.

Since Init.ai uses Git to handle storage of versioning of files, the act of deploying is done via a Git push to GitHub.

> **Note:** Init.ai may support other Git systems besides GitHub in the future.

# Webhook

A webhook is a notification sent from one server to another, usually over HTTP.

Init.ai supports both inbound and outbound webhooks.

Outbound webhooks can be configured to be send for each outbound message an application should or will send.

Inbound webhooks can be configured to add messages of various types to the conversation a user is having with an application. These inbound messages can contain text, or they can be events containing arbitrary data.

# Human monitoring

Human monitoring is the process of having humans authorized by an application's creators or owners observe the interaction between an application and its end users.

# Structured message

A structured message is a message type supported by some messaging platforms that contains not only text, but UI elements that may be interactive.

# Token

A token is a piece of data used to identify one user of a system in a secure manner. A token usually consists of a random string which should be kept secret, like a password.

Init.ai uses tokens in various ways. See remote token, user token, and manage token.

Init uses JWT for creation of all tokens.

# Remote token

A remote token identifies an external system controlled by the creator or owner of an Init project.

A remote token can access and update all users and conversations for a project, but it cannot change the configuration of a project.

A remote token is signed using a secret value specific to that project.

# User token

A user token identifies an end user of an Init project.

A user token can only access and update the data and messages for one user of a project.

A user token is signed using a secret value specific to that project and can be generated by the project creator's external API.

# Manage token

A manage token identifies a developer, project creator, or project owner.

A manage token can access all data within an Init project as well as change its configuration.

A manage token can only be generated by the Init.ai platform.

# Sentiment

Sentiment refers to how positive or negative a piece of text is, in terms of emotions.

# Frustration

Init.ai attempts to detect when users get frustrated during a conversation, either through explicit statements or through implicit patterns.

## Application concepts

# Flow

A Flow is a representation of the path a conversation can take at a high level.

A Flow is composed of multiple [Streams](#section-stream), and each [Streams](#section-stream) is composed of multiple [Steps](#section-steps).

A Flow can define a conversation in a way that resembles a state machine, with a set of possible transitions implicit in the order of [Streams](#section-stream) and [Steps](#section-step) within. But a Flow also can handle a user taking detours in a conversation and leading it more directly.

# Stream

A Stream is a component of a conversation that represents a user trying to accomplish a certain goal.

A Stream is composed of multiple [Steps](#section-steps), in order, that lead the user towards that goal. A Stream can also include other Streams within.

The goal could be going through a checkout process, providing information, etc.

```
A: [a1, B, a2, a3]
B: [b1, b2]
```

# Step

A Step is an individual stop or checkpoint in a conversation.

A Step corresponds to an actor in the conversation (either a user or the application) taking some type of action. That action could mean collecting information, providing information, initiating an external event, etc.

# Conversation state

Conversation state is the data that programmatically represents values important to the [logic system](#section-logic-system) of an application and that describe the current state of a conversation between a user and an application.

Conversation state can contain arbitrary data stored by an application's [logic system](#section-logic-system). It is persisted across messages through the end of a conversation.

# Environment variables

Environment variables are values accessible to the [logic system](#section-logic-system) of an project for all users and conversations in that project. These values are not specific to any single user or conversation.

Environment variables only change through the configuration of an project. They are suitable for storing data specific to the deployment of an application that should not be stored in code.

# Event handler

An event handler is a piece of code in the [logic system](#section-logic-system) that is invoked when an event is received by an application. An event handler can be defined to handle only a particular type of [event](#section-event), or it can be set to handle any type of [event](#section-event).

# Auto responder

The auto responder is a technique for replying to inbound messages from a user without writing code in the [logic system](#section-logic-system). For every new message in a conversation, the Init.ai NLP system predicts the [[classification](#section-classification)](#section-[classification](#section-classification)) of the next outbound message that the application will send. By enabling the auto response for certain types of outbound messages, Init will automatically send that type of response when it is predicted.

# Continuation

A continuation is a point in a conversation where it is expected that one user will continue with another message and does not expect an immediate response to a message that participant just sent. For example

```
< "What color paint would you like?"

"Um"

"Taupe"
```

In this case "Um" is a state of continuation and the application should not respond to that message, but instead wait for the answer to its question.

## Language concepts

# Slot

A slot is a value extracted from a piece of text. A slot corresponds to data that is relevant to an application, and is extracted according to that relevance.

A slot is a particular instance of an [entity](#section-entity) occurring in a piece of text, and the slot is filled with the value that actually occurred in the text.

A slot can have a role, which is inferred based on its positioning and surrounding context, and can be used to disambiguate between multiple occurrences of the same type of [entity](#section-entity) in a piece of text.

# Model

A model as used by Init.ai means a piece of data used by Init.ai to understand natural language and conversation.

A model resembles a computer program, in that it takes input (generally text) and provides an output (meaning, response, etc.). However, a model is not programmed by a person. Instead it is generated through a training process which takes sample inputs and sample outputs and gradually evolves the model so that it can generate the correct outputs for inputs it sees in the future.

A model belongs to an [project](#section-project), and can have multiple model versions.

# Classification

A [classification](#section-classification) is the membership of a piece of text within a certain category. The [classification](#section-classification) can have different components and uses depending on the application it is part of.

A [classification](#section-classification) can be assigned to an inbound message, an outbound message, or a slot within a message.

For an inbound message, the [classification](#section-classification) generally corresponds to the intent of the person who wrote the message.

Ex:

```
What is the weather today?
* check_weather
```

For an outbound message, the [classification](#section-classification) refers to type of message the application is trying to send. All messages with the same [classification](#section-classification) should have the same meaning.

Ex:

```
< Flight 223 on Virgin America is delayed.
* provide_flight_status/delayed
```

For a slot, the [classification](#section-classification) refers to the [entity](#section-entity) type and role in the message that the slot represents.

Ex:

```
What is the weather in [Chicago](city)?
```

"Chicago" is classified as a "city" slot.

# Base Type

[Classifications](#section-classification) can contain multiple components.

The base type of a [classification](#section-classification) the most general refinement of a [classification](#section-classification).

For [entities](#section-entity), the base type corresponds to a value type that determines how Init.ai will parse the value, for example as a string, a number, an ordinal, etc.

Ex:

```
Is flight 223 on Virgin running late?
* check_flight_status/is_delayed_yn
```

In this case, `check_flight_status` is the base type of the message [classification](#section-classification) `check_flight_status/is_delayed_yn`.

Ex:

```
I'd like to order [three](number/product_count) shirts
```

In this case, `number` is the base type of the slot [classification](#section-classification) `number/product_count`.

# Sub Type

Sub type is an optional refinement of the [base type](#section-base-type).

Ex:

```
Is flight 223 on Virgin running late?
* check_flight_status/is_delayed_yn
```

```
How delayed is Virgin flight 223?
* check_flight_status/delay_amount
```

In this case, `is_delayed_yn` is the sub type of the message [classification](#section-classification) `check_flight_status/is_delayed_yn`.

All messages with the base type `check_flight_status` should correspond to the user requesting the status of a flight. Different sub types would still mean the user is requesting the status of a flight, but in a different way.

# Style

Style is a component of [classification](#section-classification) for entire messages, but not for slots. It is the most precise component of a [classification](#section-classification) and should be used to only distinguish between messages that have identical meaning, including parts of speech, but vary only by the "tone of voice" or formality used in their construction, for example formal vs slang.

Ex:

```
Yo homie
* greeting#slang
```

```
Good day dearest sir
* greeting#stuffy
```

# Role

A role is a component of [classification](#section-classification) for slots within messages. The role helps determine what function within a message a [slot](#section-slot) is playing. The helps disambiguate between two or more instances of the same [entity](#section-entity) type occurring within a sentence.

Roles are optional additions to [slots](#section-slot) tags.

Ex:

```
I need a flight from [New York](city#departure) to [Chicago](city#arrival)
```

New York and Chicago are both cities, but play different part in the message, yielding different roles.

# Entity

An entity represents an individual concept or type of data.

An entity can be relatively general – that is, something that is part of common knowledge like 'city' or 'day of week'. Or an entity can be something specific to an application or the domain of the application – for example a product type, or the name of an account or service.