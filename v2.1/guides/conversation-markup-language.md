---
title: "Conversation Markup Language"
excerpt: ""
---
Conversation data is stored in a format we call Conversation Markup Language (CML). It is a flavor of markdown used to design training conversations for automated conversational apps. The goal of CML is to be familiar, easy to read, and collaborative.

# Turns

A single back and forth between two parties in conversation is a “Turn” and is created in CML by full line breaks. Messages from the user are prefixed with `user> ` and replies by the application are prefixed with `service> `.

**Example**

```
user> Hello

service> Hello, human. What should I call you?

user> Amy

service> Hey Amy, would you like to know the weather?
```

# Classifications

Classifications tell the system how to relate a particular message's meaning to an action or desire. Each message (input and output) requires a classification, also known as a "tag".

Tagging a message uses a markdown list item, `*`, syntax underneath the message.

> **Note:** Messages cannot be tagged with multiple intents.

**Example**

```
user> What’s the weather?
* check_weather

service> Today, it is currently [sunny](condition).
* provide_weather
```

In this example the user wants to check the weather, so the message is tagged with the classification `check_weather`

> **Note:** There should not be an empty line between the message and its classification

Classifications are also used to categorize types of messages sent by your conversational app. That will be covered in Outputs.

Classifications are composed of three parts:

`base_type`: The most general refinement of a classification.

`sub_type`: Optional refinement of the base type.

`style`: The most precise component of a classification only to be used to distinguish between messages that have identical meaning. For example, you might define style to be 'formal', 'slang', or 'casual'.

> **Note:** Only `base_type` is required.

Here is an example of how a classification including base_type, sub_type, and style would be structured:

```
base_type/sub_type#style
```

> **Note:** Each component in a classification (`base_type` vs `subtype`) is limited to 255 characters in length and should be ASCII.

# Slots & entities

Slots are single- or multi-word phrases in a message that represent a piece of data of importance or value. The Init NLP system will extract these pieces of data from incoming messages, process or parse the data, and present it to the business logic system.

To enable the NLP system to locate slots, the training data must include examples of those slots appearing in conversations. The NLP system will then use a combination of sentence structure and similar word/phrase meaning to locate and extract slots from messages it has never seen before.

Slots have types, indicating what type of data they contain. These data types are called entities. An entity represents a concept or data type important to your conversational app. Example entities could be "city", "product", "stock", "company", and so on.

An entity can be any arbitrary concept. For many English words, the NLP system has an understanding of words that are related to each other. For example, it has an inherent understanding that "Los Angeles" and "Chicago" are similar concepts.

The NLP system can parse and interpret the value of extracted slots in various ways that make it easier for the business logic system to process and act on. The parsing of an entity is determined by its `base_type`.

Entity Base types can be:

|base_type|Example|
|---|---|
|`string`|"foo", "bar"|
|`time`|"2pm", "Tomorrow", "Wednesday the 12th"|
|`duration`|"3 hour tour", "20 minutes short", "all day"|
|`temperature`|"72 degrees", "zero Fahrenheit"|
|`number`|"42", "sixty seven", "10MM", "0.168"|
|`ordinal`|"sixth", "1st"|
|`distance`|"500 miles", "30m", "20K leagues"|
|`volume`|"10 gallons", "16 oz", "1 liter"|
|`amount-of-money`|"three dollars", "12 bucks", "€13.12"|
|`email`|"foo@bar.com"|
|`url`|"https://Init.ai", "google.com", "npr.org", "foo.bar.bot?ref=url"|
|`phone-number`|"8675309", "+4402012345", "1800-555-5555 ext 123"|
|`location`|"Los Angeles", "Chicago"|

After locating and extracting slot values, Init uses the Duckling library (https://duckling.wit.ai/) to parse slot values into a machine-friendly representation. Values of base type string are passed along unmodified (except in the case of canonicalization, which is discussed separately).

Slots are marked using Markdown link syntax, where the link and URL are of the format:

`[value](base_type/entity#role)`

`base_type` defaults to string, and can be omitted, as in:

`[value](entity#role)`

role is also optional, so these are valid as well:

`[value](entity)`

`[value](base_type/entity)`

**Examples**

```
user> What is the weather in [London](city)?
user> What is the weather in [London](string/city)?

user> I need a flight from [London](city#departure) to
[New York](city#arrival)?
```

The first two examples are identical since `base_type` defaults to string.

In the second example, the entity city is used twice, but have different meanings in the sentence, which is indicated by the differing roles.

`base_type`, entity, and role are case insensitive and will be converted to lowercase by the Init platform.

Note: It is recommended that names for `base_type`, entity, and role are written in `snake_case_with_underscores_separating_words`.

> **Note:** Each component is limited to 255 characters in length and should be ASCII.