---
title: "Conversation Markup Language"
excerpt: ""
---
Conversation data is stored in a format we call CML (Conversation Markup Language). It is a flavor of markdown used to design training conversations for automated conversational apps. The goal of CML is to be familiar, easy to read, and collaborative.

# Turns

A single back and forth between two parties in conversation is a “Turn” and are created in CML by full line breaks. Replies by the application are prefixed with `< `.

**Example**

```
Hello

< Hello, human. What should I call you?

Amy

< Hey Amy, would you like to know the weather?
```

# Classifications

Classifications tell the system how to relate a particular message meaning to an action or desire. Each message (input and output) requires a classification, which can be considered a tag.

Tagging a message uses list item like, `*`, syntax underneath the message.

> **Note:** Messages cannot be tagged with multiple intents.

**Example**

```
What’s the weather?
* check_weather
```

In this example the user want’s to check the weather, so the message is tagged with a classification of `check_weather`

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

# Multi-part messages

A message composed of multiple sentences can be split by the NLP system and the resulting message parts can be classified separately. To indicate that multiple sentences belong to the same message, you define each sentence as a message part when manually editing CML. This is useful in situations where users may ask multiple things in the same message and you may want to send more than one response.

```
Is it going to rain?
* get_weather_conditions
^ What's the temperature outside?
* get_temp
```

 A `^` denotes that a message is a continuation of the previous message. A message continuation must not have a blank line between it and the previous message part

> **Note:** Multi-part messages, using the continuation syntax, are only allowed for user inputs, not app outputs.

Incoming messages and messages in the Teach view can be automatically split into multiple message parts on the `.`, `!`, and `?` characters (by sentence).  For example, in the Teach view, the message:

```
Is it going to rain? What's the temperature outside?
```

can be automatically split to appear as such:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/1eb9fb8-Screen_Shot_2016-12-19_at_4.30.13_PM.png",
        "Screen Shot 2016-12-19 at 4.30.13 PM.png",
        696,
        324,
        "#fbfbfa"
      ],
      "sizing": "smart"
    }
  ]
}
[/block]
 To change this setting, add or alter a line in your app repo’s `init.yaml` file to match either:

`split_messages: sentence`: The app will automatically split messages by sentence.

or

`split_messages: none`: The app will not split messages into separate message parts.

and push to Github.  If your file does not contain a line like these, its default behavior is to split by sentence.  After, your file should look like this:
[block:code]
{
  "codes": [
    {
      "code": "init_version: 0.0.x\napp_name: Whats-the-weather\nsplit_messages: sentence",
      "language": "yaml",
      "name": "init.yaml"
    }
  ]
}
[/block]
# Slots & entities

Slots are single or multi word structures in a message that represent a piece of data of importance or value. The Init NLP system will extract these pieces of data from incoming messages, process or parse the data, and present it to the business logic system.

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
What is the weather in [London](city)?
What is the weather in [London](string/city)?

I need a flight from [London](city#departure) to [New York](city#arrival)?
```

The first two examples are identical since `base_type` defaults to string.

In the second example, the entity city is used twice, but have different meanings in the sentence, which is indicated by the differing roles.

`base_type`, entity, and role are case insensitive and will be converted to lowercase by the Init platform.

Note: It is recommended that names for `base_type`, entity, and role are written in `snake_case_with_underscores_separating_words`.

> **Note:** Each component is limited to 255 characters in length and should be ASCII.

# Metadata

Conversations require some form of metadata that the Init.ai system understands. We use front-matter style syntax for storing specific pieces of data related to a conversation file. If you see this in your files try not to modify it since it can mess up your models!

```yaml
--- cml-1.0.0
title: Conversation title
timestamp: 04-28-2016, 13:27:18:7570
source_type: user
source_id: d3b72896-7387-4aa0-aeb3-2c6c881de496
medium: sms
creator: Trevor McNaughton
creator_id: 8fb9410f-a1b6-4fbd-923b-44b96748d20d
tags:
  - onboarding
  - something
language: en-us
---
```

> **Note:** Language is according ISO-3166 standard