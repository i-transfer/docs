---
title: "Intents, Entities, and Templates"
excerpt: "Designing and classifying messages can be intimidating and tricky for beginners. Here's some of our recommended best practices."
---
## Intents

Intents assign meaning to a particular message by containing the intention of the end-user or the system.

Init supports **base types** and **sub types** for all intent classifications. It is recommended that you name your base types according to general categories of actions, and then consider **sub types** as variants of those. Frequently these variants will be by topic or subject matter, but they might also encode information about whether something was a yes or no question, etc.

**Examples**

| **Message**                                                | **Intent (base_type/sub_type)**        |
| ---------------------------------------------------------- | -------------------------------------- |
| "I'd like them delivered to NYC"                           | `provide_delivery_info/city`           |
| "They're going to Margaret"                                | `provide_delivery_info/recipient_name` |
| "How much will delivery cost me?"                          | `ask_product_question/delivery_cost`   |
| "Thanks so much"                                           | `thanks`                               |
| "Yeah" "Yes" "Cool"                                        | `affirmative`                          |
| "Yes please do that" "That works"                          | `affirmative/confirm`                  |
| "Nope" "No"                                                | `negative`                             |
| "Hello" "Hi" “Hey”                                         | `greeting`                             |
| "Hi am I in the right place?”                              | `greeting/yn`                          |
| "What's the temperature outside?" "How hot is it outside?" | `check_temperature`                    |
| "Is it hot outside right now?"                             | `check_temperature/hot_yn`             |
| "I'd like some shoes delivered to me in Boston"            | `request_item/shoes`                   |


Adopting a naming convention can help you organize inbound message classifications. For example, you can add prefixes like these to your intents:


- `ask_` 
- `request_` 
- `provide_` 
- `check_` 

Init will not treat the classifications differently based on these names, but it will help you understand the meaning as you use it.


## Entities

Entities are important words of phrases within messages that need to be classified as well. Take a look at this sentence:  "I want the red square"

There are two entities in here **red** which would be classified as a `color` and **square** which would be classified as a `shape`.

Similar to message classifications, they are composed of a **base type**, **entity name**, and an optional **role** in the sentence: `base_type/entity_name#role` 

When naming entities, think about two things: the type of data being represented (numbers, strings, time, objects) and the purpose that piece of data has in relation to how people are using your application.

Sometimes you may have the same type of concept represented by more than one entity. But in general, if you find that you have the same concept represented more than once in your application, it may be a candidate for role classification and this is recommended.


## Templates

Templates are automatically created with training data. Any outbound message is turned into a response template which is selected at the appropriate time and sent to the user. When you add entities to a message you're telling our system that this is a place where data needs to be filled in.

These templates often need dynamic data to populate messages when they are sent. For example: "It's **72** degrees and **Sunny**" has two pieces of data that need to come from an external API.

When you're writing your conversation logic and are making calls to APIs, you pass that data to the template to populate it using the entity names.

**Logic Example**

    // ...
    
    const weatherData = {
      temperature: response.temperature,
      condition: response.condition,
    }
    
    client.addResponse('provide_weather', weatherData)
    
    // ...

In this example we know that the classification `provide_weather` expects two pieces of data `temperature` and `condition` because we specified that within our training conversations. The `client.addResponse` method accepts a message classification (aka template) `'provide_weather'` and an optional object with the data to fill any placeholders.

**Important:** The data provided to your templates must have keys that match the entity names provided in your training conversations to work properly.