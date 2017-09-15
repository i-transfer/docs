---
title: "Classifications & Templates"
excerpt: "Designing classifications for messages can be intimidating and tricky for beginners. Here's our best practices."
---
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
Message classifications embed multiple related but distinct types of information. They generally contain information about the intent of either the end user or the conversation app. But they may also vary according to how a question is asked or phrase.

Init simplified classifications somewhat. If two messages are identical except for their tone of voice, use the style component of the classification to distinguish them.

The taxonomy of classifications you create for your application depends on its specific domain.

Frequently inbound message classifications end up being of high-level types like:

- providing information
- asking a question
- greeting
- thanks
- agreement
- disagreement

Init supports classification **base types** and **sub types**. It is recommended that you name your classification base types according to general categories of user actions, and then consider **sub types** as variants of those. Frequently these variants will be by topic or subject matter, but they might also encode information about whether something was a yes or no question, etc.
[block:api-header]
{
  "type": "basic",
  "title": "Inbound Message Classifications"
}
[/block]
To get started naming classifications, take a look at some example classification names and some example messages:
[block:parameters]
{
  "data": {
    "h-0": "Message Classification",
    "h-1": "Message",
    "0-0": "`provide_delivery_info/city`",
    "0-1": "\"I'd like them delivered to NYC\"",
    "1-0": "`provide_delivery_info/recipient_name`",
    "1-1": "\"They're going to Margaret\"",
    "2-0": "`ask_product_question/delivery_cost`",
    "2-1": "\"How much will delivery cost me?\"",
    "3-0": "`thanks`",
    "3-1": "\"Thanks so much\"",
    "4-0": "`affirmative`",
    "4-1": "\"Yeah\"\n\"Yes\"\n\"Cool\"",
    "5-0": "`affirmative/confirm`",
    "5-1": "\"Yeah\"\n\"That works\"",
    "6-0": "`negative`",
    "6-1": "\"Nope\"\n\"No\"",
    "7-0": "`greeting`",
    "7-1": "\"Hello\"\n\"Hi\"",
    "8-0": "`greeting#slang`",
    "8-1": "\"Sup\"\n\"Yo whats going on\"",
    "9-0": "`greeting/yn`",
    "9-1": "\"Hi am I in the right place?\"",
    "10-0": "`check_temperature`",
    "10-1": "\"What's the temperature outside?\"\n\"How hot is it outside?\"",
    "11-0": "`check_temperature/hot_yn`",
    "11-1": "\"Is it hot outside right now?\"",
    "12-0": "`request_item/shoes`",
    "12-1": "\"I'd like some shoes delivered to me in Boston\""
  },
  "cols": 2,
  "rows": 13
}
[/block]
Adopting a naming convention can help you organize inbound message classifications. For example, you can add prefixes like:

- `ask_`
- `request_`
- `provide_`
- `check_`

to the classification names to help you stay organized. Init will not treat the classifications differently based on these names at this point, but it will help you understand the meaning of the classification as you use it.
[block:api-header]
{
  "type": "basic",
  "title": "Outbound Message classification"
}
[/block]
Outbound classifications should follow similar naming conventions to inbound classifications.
Think about what the goal of your application sending a particular type of message is when deciding on a name.

Like inbound classifications, outbound classifications can have a base type and a sub type to help you stay organized.

Outbound messages also support the style marker, which will assist Init.ai in selecting the best response to send when you are using Init's templating system (see below).
[block:api-header]
{
  "type": "basic",
  "title": "Entities"
}
[/block]
Entities are important words of phrases within messages that need to be classified as well. Take a look at this sentence:

"I want the red square"

There are two entities in here **red** which would be classified as a `color` and **square** which would be classified as a `shape`.

Similar to message classifications, they are composed of a **base type**, **entity name**, and an optional **role** in the sentence: `base_type/entity_name#role`

When naming entities, think about two things: the type of data being represented (numbers, strings, time, objects) and purpose that piece of data has in relation to how people are using your application.

Sometimes you may have the same type of concept represented by more than one entity. But in general, if you find that you have the same concept represented more than once in your application, it may be a candidate for role classification and this is recommended.
[block:api-header]
{
  "type": "basic",
  "title": "Language Style"
}
[/block]
You may add a style tag to any message classification to mark its tone of voice or style.

The NLP system will use the style value to assign incoming message classifications, and it will also predict the appropriate style of responses when sending templated messages.

Style is optional in message classifications. Here are a few examples:

  * `formal`
  * `informal`
  * `slang`
[block:api-header]
{
  "type": "basic",
  "title": "Templates"
}
[/block]
Outbound messages often need dynamic data to populate messages when they are sent.

For example: "It's 72 degrees and Sunny" has two pieces of data that need to come from an external API.

Templates are automatically created with training data. Any outbound message is turned into a response template which is selected at the appropriate time and sent to the user. When you add entities to a message you're telling our system that this is a place where data needs to be filled in.

Using the above example **72** and **Sunny** are treated as placeholder values in the training data. When you're writing your conversation logic and are making calls to APIs, you then pass that data to the template to populate it.
[block:code]
{
  "codes": [
    {
      "code": "// ...\n\nconst weatherData = {\n  temperature: response.temp,\n  condition: response.condition,\n}\n\nclient.addResponse('app:response:name:provide_weather', weatherData)\n\n// ...",
      "language": "javascript"
    }
  ]
}
[/block]
In this example we know that the classification `provide_weather` expects two pieces of data `temperature` and `condition`. The `client.addResponse` method accepts a message classification (aka template) `'app:response:name:provide_weather'` and an optional object with the data to fill any placeholders.