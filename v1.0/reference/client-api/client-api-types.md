---
title: "Types"
excerpt: ""
---
# CarouselList

`Object`

Definition of a carousel-list to be sent to the conversation.
[block:parameters]
{
  "data": {
    "0-0": "`title`",
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Value/Description",
    "0-1": "String",
    "0-2": "The title of the card in the carousel",
    "1-0": "`description`",
    "1-1": "String",
    "1-2": "The subtitle / description of the card in the carousel (max 128 characters).",
    "2-0": "`media_url`",
    "2-1": "String",
    "2-2": "URL to the media in the card",
    "3-0": "`media_type`",
    "3-1": "String",
    "3-2": "The MIME type of the media in the card",
    "4-0": "`actions`",
    "4-1": "Array&lt;Action&gt;",
    "4-2": "See [`action`](#classification-sub-type) definition."
  },
  "cols": 3,
  "rows": 5
}
[/block]
# Action

`Object`
[block:parameters]
{
  "data": {
    "0-0": "`type`",
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Value/Description",
    "0-1": "String",
    "0-2": "Either `link` or `postback`. See [ActionTypes](doc:client-api-statics#section-actiontypes).",
    "1-0": "`text`",
    "1-1": "String",
    "1-2": "Button or link text.",
    "2-0": "`uri`",
    "2-1": "String",
    "2-2": "Required for `type` `link`",
    "3-0": "`payload`",
    "3-1": "Object&lt;[ActionPayload](#action-payload)&gt;",
    "3-2": "Required for `type` `postback`. Must be object in [ActionPayload](#action-payload) format."
  },
  "cols": 3,
  "rows": 4
}
[/block]
# ActionPayload

`Object`
[block:parameters]
{
  "data": {
    "0-0": "`data`",
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Value/Description",
    "0-1": "Anything",
    "0-2": "Your payload.",
    "1-0": "`stream`",
    "1-1": "String",
    "1-2": "Stream name to return to after action. Can be result from `getStreamName()`",
    "2-0": "`version`",
    "2-1": "String",
    "2-2": "Must equal '1'."
  },
  "cols": 3,
  "rows": 3
}
[/block]
**<sub>[Back to top](#section-available-methods)</sub>**
- - -

# Classification

`Object`

The determined intent as inferred from the NLP system.
[block:parameters]
{
  "data": {
    "0-0": "`overall_confidence`",
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Value/Description",
    "0-1": "`Number`",
    "0-2": "A `Number` 0-1 representing the overall confidence of the NLP classification.",
    "1-0": "`base_type`",
    "1-1": "Object",
    "1-2": "See [`base_type`](#classification-base-type) definition.",
    "2-0": "`sub_type`",
    "2-1": "Object",
    "2-2": "See [`sub_type`](#classification-sub-type) definition."
  },
  "cols": 3,
  "rows": 3
}
[/block]
# base_type

`Object`
[block:parameters]
{
  "data": {
    "0-0": "`value`",
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Value/Description",
    "0-1": "String",
    "0-2": "A string definition of the type's value.",
    "1-0": "`confidence`",
    "1-1": "Number",
    "1-2": "A `Number`0-1 representing the type's classification accuracy."
  },
  "cols": 3,
  "rows": 2
}
[/block]
# sub_type

`Object`
[block:parameters]
{
  "data": {
    "0-0": "`value`",
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Value/Description",
    "0-1": "String",
    "0-2": "A string definition of the type's value.",
    "1-0": "`confidence`",
    "1-1": "Number",
    "1-2": "A `Number` 0-1 representing the type's classification accuracy."
  },
  "cols": 3,
  "rows": 2
}
[/block]
**<sub>[Back to top](#section-available-methods)</sub>**
- - -

# ConversationModel

`Object`

An Object representing the current conversation state.
[block:parameters]
{
  "data": {
    "0-0": "`id`",
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Value/Description",
    "0-1": "String",
    "0-2": "The database id of the current conversation.",
    "1-0": "`messages`",
    "1-1": "Array",
    "1-2": "A collection of messages that have occurred in the lifespan of the conversation.<br ><br />\n\nSee [`MessagePart`](#message-part) for the structure of these Objects.",
    "2-0": "`state`",
    "2-1": "Object",
    "2-2": "A representation of \"stateful\" data and values used to contextualize the conversation flow."
  },
  "cols": 3,
  "rows": 3
}
[/block]
**<sub>[Back to top](#section-available-methods)</sub>**
- - -

# FlowDefinition

`Object`

A Flow is an object that defines how the app handles the user's navigation through a conversation. This object is constructed using a combination of the following keys:
[block:parameters]
{
  "data": {
    "0-0": "`classifications`",
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Required",
    "h-3": "Value/Description",
    "0-1": "Object",
    "0-2": "*no*",
    "0-3": "An object mapping classification designations to [Streams](doc:conversation-flow#section-streams).",
    "1-0": "`eventHandlers`",
    "1-1": "Object",
    "1-2": "*no*",
    "1-3": "An object mapping events to specific handlers.<br /><br />Read more about [Event Handlers](doc:events).",
    "2-0": "`streams`",
    "2-1": "Object",
    "2-2": "*no*",
    "2-3": "An object mapping streams to collections of [Steps](doc:conversation-flow#section-steps)."
  },
  "cols": 4,
  "rows": 3
}
[/block]
**Examples**

```js
const flowDefinition = {
  classifications: {
    'check_weather': 'checkWeather'
  },
  eventHandlers: {
    '*': handleEvent,
  },
  streams: {
    checkWeather: [getLocation],
    getLocation: [getState, getCity, getAddress, confirmAddress],
    getPayment: [collectCardNumber, collectExpiration],
    checkout: ['getLocation', 'getPayment', sendReceipt],
    selectProduct: [...],
    placeOrder: [selectProduct, 'checkout'],
    main: 'placeOrder',
    end: [help],
  },
}

client.runFlow(flowDefinition)
```

**<sub>[Back to top](#section-available-methods)</sub>**
- - -

# MessagePart <a id="message-part"></a>

`Object`

A `MessagePart` is composed of the following:
[block:parameters]
{
  "data": {
    "0-0": "`content`",
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Value/Description",
    "0-1": "String",
    "0-2": "The message text/body",
    "1-0": "`content_type`",
    "1-1": "String",
    "1-2": "The type of message. Currently, the only supported type is `'text'`.",
    "2-0": "`classification`",
    "2-1": "Object",
    "2-2": "The resulting classification from the NLP model (See [Classification](#classification)]) .",
    "3-0": "`slots`",
    "3-1": "Object",
    "3-2": "A mapping of individual `entities`.",
    "4-0": "`sender`",
    "4-1": "Object",
    "4-2": "The [`User`](#user) representing the user who sent the current message."
  },
  "cols": 3,
  "rows": 5
}
[/block]
- - -

# Slots

`Object`
[block:parameters]
{
  "data": {
    "0-0": "`base_type`",
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Value/Description",
    "0-1": "String",
    "0-2": "Available values include:<br /><ul><li>`string` (default)</li><li>`number`</li><li>`currency`</li><li>`time`</li><li>`duration`</li><li>`ordinal`</li><li>`temperature`</li><li>`distance`</li><li>`volume`</li><li>`amount-of-money`</li><li>`url`</li><li>`phone-number`</li></ul>",
    "1-0": "`entity`",
    "1-1": "String",
    "1-2": "This is equivalent to the `sub_type` for the classification of the slot. Values are generated through training data added to an application.<br /><br />Examples may include: `city`, `product_name`, `airport`, `recipient`, etc.",
    "2-0": "`roles`",
    "2-1": "Array&lt;String&gt;",
    "2-2": "An array of strings representing keys on the [`values_by_role`](#slot-values-by-role) Object.",
    "3-0": "`values_by_role`",
    "3-1": "Object",
    "3-2": "See [`values_by_role`](#slot-values-by-role)."
  },
  "cols": 3,
  "rows": 4
}
[/block]
# values_by_role

`Object`

An object defining slotted entities keyed by role. Each value in this Object contains an array of [`value role definitions`](#slot-value-role).

## ValueRole: Object <a id="slot-value-role"></a>
[block:parameters]
{
  "data": {
    "0-0": "`end_char`",
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Value/Description",
    "0-1": "Number",
    "0-2": "The character index that ends the slot selection.",
    "1-0": "`end_token`",
    "1-1": "Number",
    "1-2": "The index of the last token in the selection.",
    "2-0": "`raw_value`",
    "2-1": "String",
    "2-2": "The value of the slot.",
    "3-0": "`start_char`",
    "3-1": "Number",
    "3-2": "The character index that begins the slot selection.",
    "4-0": "`start_token`",
    "4-1": "Number",
    "4-2": "The index of the first token in the selection."
  },
  "cols": 3,
  "rows": 5
}
[/block]
**<sub>[Back to top](#section-available-methods)</sub>**
- - -

# SlotValue

`Object`
[block:parameters]
{
  "data": {
    "0-0": "`value`",
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Value/Description",
    "0-1": "String",
    "0-2": "The display value of the current slot.",
    "1-0": "`raw_value`",
    "1-1": "String",
    "1-2": "The raw value of the current slot",
    "2-0": "`parsed`",
    "2-1": "`Object|null`",
    "2-2": "(defaults to `null`)  – This will return an Object for `time`, `date`, `number`, and `currency` values. Otherwise will be `null`."
  },
  "cols": 3,
  "rows": 3
}
[/block]
**<sub>[Back to top](#section-available-methods)</sub>**
- - -

# SlotValuesByRole

`Array`
[block:parameters]
{
  "data": {
    "0-0": "`slotValues`",
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Value/Description",
    "0-1": "[`SlotValues`](#slotvalues)",
    "0-2": "An Array of [`SlotValue`](#slotvalue) Objects"
  },
  "cols": 3,
  "rows": 1
}
[/block]
**<sub>[Back to top](#section-available-methods)</sub>**
- - -

# SlotValues

`Object`
[block:parameters]
{
  "data": {
    "0-0": "`values_by_role`",
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Value/Description",
    "0-1": "[`SlotValuesByRole`](#slotvaluesbyrole)",
    "0-2": "A mapping of [`SlotValue`](#slotvalue) Objects keyed by role"
  },
  "cols": 3,
  "rows": 1
}
[/block]
**<sub>[Back to top](#section-available-methods)</sub>**
- - -


# StepDefinition

`Object`

Overrides for any of the default `step` methods (`extractInfo`, `next`, `satisfied`, and `prompt`).
[block:parameters]
{
  "data": {
    "0-0": "`satisfied`",
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Value/Description",
    "0-1": "Function",
    "0-2": "A function expected to return a `Boolean`",
    "1-0": "`prompt`",
    "1-1": "Function",
    "1-2": "A Function to be executed if `satisfied` returns `false`.<br /><br />If your prompt function needs to be asynchronous, you must define it to accept a callback as the first and only argument. You must either:<br  /><br /><ul><li>call `client.done()` from within your asynchronous code when you are ready to return a message or update conversation state, ending execution.</li><li>invoke the callback Init provides to have processing continue to the next step in the Flow</li></ul>",
    "2-0": "`extractInfo`",
    "2-1": "Function",
    "2-2": "A Function used to extract a `slot` value from a message. This function is provided a [`MessagePart`](#message-part) as an argument.",
    "3-0": "`next`",
    "3-1": "Function",
    "3-2": "A [Step](doc:conversation-flow#section-steps) may redirect the conversation to another [Stream](doc:conversation-flow#section-streams) if desired. It may do so by defining a `next` function function and returning name of the Stream to go to next."
  },
  "cols": 3,
  "rows": 4
}
[/block]
**<sub>[Back to top](#section-available-methods)</sub>**
- - -

# User

`Object`

The default User object provided to your function contains:
[block:parameters]
{
  "data": {
    "0-0": "`app_id`",
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Value/Description",
    "0-1": "String",
    "0-2": "The id of the current project",
    "1-0": "`id`",
    "1-1": "String",
    "1-2": "The user id",
    "2-0": "`platform_user_id`",
    "2-1": "String",
    "2-2": "The user's id in the Init.ai database",
    "3-0": "`remote_id`",
    "3-1": "String",
    "3-2": "The id associating this user with an external service (such as Twilio)",
    "4-0": "`first_name`",
    "4-1": "String",
    "4-2": "The user's first name.",
    "5-0": "`last_name`",
    "5-1": "String",
    "5-2": "The user's last name.",
    "6-0": "`metadata`",
    "6-1": "Object",
    "6-2": "By default, this is provided as an empty Object for you to store arbitrary user data.",
    "7-0": "`created_at`",
    "7-1": "String",
    "7-2": "A timestamp to denote when the user was created.",
    "8-0": "`updated_at`",
    "8-1": "String",
    "8-2": "A timestamp to denote the last update to the user."
  },
  "cols": 3,
  "rows": 9
}
[/block]
**<sub>[Back to top](#section-available-methods)</sub>**