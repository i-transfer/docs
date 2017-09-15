---
title: "Methods"
excerpt: ""
---
# addCarouselListResponse

```js
(payload: Object): → {void}
```

Adds an image to the list of message parts to to be sent to the user.

**Arguments**
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "`payload`",
    "0-1": "[CarouselList](doc:client-api-types#section-carousellist-a-id-carousellist-a-)",
    "0-2": "The carousel to send"
  },
  "cols": 3,
  "rows": 1
}
[/block]
See CarouselList definition.

- `payload` must contain an array `items`.
- Each object in `items` must be an Item.
- Each Item must contain a `title` and an (possibly empty) array `actions`. It may optionally contain `description`, `media_url`, and `media_type`.
- Each object in `actions` must be an Action.
- Each Action must contain a string `text`, and a `type`, of either `link` or `postback`.
  - If `type` is `link`, the Action must contain a  `uri`.
  - If `type` is `postback`, the Action must contain `payload`.

**Returns**

N/A

**Example Usage**

```js
client.addCarouselListResponse({
  items: [
    {
      media_url: 'https://c2.staticflickr.com/4/3512/5763418254_e2f42b2224_b.jpg',
      media_type: 'image/jpeg',
      description: 'Yosemite is a really nice place.',
      title: 'Yosemite',
      actions: [
        {
          type: 'postback',
          text: 'Visit',
          payload: {
            data: {
              action: 'visit',
              park: 'yosemite'
            },
            version: '1',
            stream: 'selectPark',
          },
        },
      ],
    },
    {
      media_url: 'https://upload.wikimedia.org/wikipedia/commons/3/36/Morning_Glory_Pool.jpg',
      media_type: 'image/jpeg',
      description: 'Yellowstone showcases geology in its most raw form.',
      title: 'Yellowstone',
      actions: [
        {
          type: 'link',
          text: 'View info',
          uri: 'https://en.wikipedia.org/wiki/Yellowstone_National_Park',
        },
      ],
    },
  ],
})
```

**<sub>[Back to top](#)</sub>**
- - -

# addImageResponse

```js
(imageUrl: String, alternativeText: String): → {void}
```

Adds an image to the list of message parts to to be sent to the user.

**Arguments**

[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "`imageUrl`",
    "0-1": "String",
    "0-2": "The URL of the image to send",
    "1-0": "`alternativeText`",
    "1-1": "String",
    "1-2": "The alternative text for the image"
  },
  "cols": 3,
  "rows": 2
}
[/block]
**Returns**

N/A

**Example Usage**

```js
client.addImageResponse('https://some.url/image.png', 'The product')
```

**<sub>[Back to top](#)</sub>**
- - -

# addResponse

```js
(responseName:String, responseData:Object): → {void}
```

Queues a message to be sent using a prepared example from the application training data. Provide the message classification as the first argument and data with which to populate slots second.

**Arguments**
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "`responseName`",
    "0-1": "String",
    "0-2": "The name of the message classification to send",
    "1-0": "`responseData`",
    "1-1": "Object",
    "1-2": "The data used to populate message slots (if any). Each key maps to a slot name."
  },
  "cols": 3,
  "rows": 2
}
[/block]
**Returns**

`{void}`

**Example Usage**

```js
client.addResponse('provide_weather', {foo: 'bar'})
client.addResponse('provide_weather', {condition: ['bar', 'baz', 'ben']})
client.addResponse('provide_weather', {'string/condition': 'sunny'})
client.addResponse('provide_weather/current', {'string/condition': 'sunny'})
```

**<sub>[Back to top](#)</sub>**
- - -

# addResponseWithReplies

```js
(responseName:String, responseData:Object, replies:Array of Object): → {void}
```

Variant of addResponse that supports sending quick reply buttons as suggestions to the user.

**Arguments**
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "`responseName`",
    "0-1": "String",
    "0-2": "The name of the message classification to send",
    "1-0": "`responseData`",
    "1-1": "Object",
    "1-2": "The data used to populate message slots (if any). Each key maps to a slot name.",
    "2-0": "`replies`",
    "2-1": "Array of Objects",
    "2-2": "List of quick replies to be offered to the user"
  },
  "cols": 3,
  "rows": 3
}
[/block]
**Returns**

`{void}`

**Example Usage**

```js
client.addResponseWithReplies('provide_weather', {foo: 'bar'}, replies)
client.addResponseWithReplies('provide_weather', {condition: ['bar', 'baz', 'ben']}, replies)
client.addResponseWithReplies('provide_weather', {'string/condition': 'sunny'}, replies)
client.addResponseWithReplies('provide_weather/current', {'string/condition': 'sunny'}, replies)
```

Where `replies: [client.makeReplyButton(...), ...]`

**<sub>[Back to top](#)</sub>**
- - -

# addSuggestion

```js
(suggestion: Suggestion): → {void}
```

Queues a suggestion to be included in the next conversation update. The results of adding a suggestion(s) are not applied directly to the conversation, but are made available to connected clients via the Remote API or a monitor client.

There are two types of suggestions:

* A **"templated"** suggestion which allows you to use prepared messages from your training data and populate any slot values with custom data.
* A **"raw"** suggestion which allows you to provide an arbitrary string to be delivered to your service/agent.

**Arguments**

`addSuggestion` takes a configuration Object of the following shape as its only argument:
[block:parameters]
{
  "data": {
    "h-0": "Key",
    "h-1": "Type`",
    "h-2": "Description",
    "0-0": "`text`",
    "0-1": "String",
    "0-2": "A \"raw\" string value that will be delivered to connected clients.\n\n**Note:** When using `text`, you may not use `intent` or `data` values.",
    "1-0": "`intent`",
    "1-1": "String",
    "1-2": "An *agent* intent from your training data (Providing an intent that is only applied to `service` or `user` messages will result in an error)",
    "2-0": "`data`",
    "2-1": "Object",
    "2-2": "An object used to populate entity slots for the given intent.",
    "3-0": "`metadata`",
    "3-1": "Object",
    "3-2": "An (optional) arbitrary Object which you may use to shuttle data related to the suggestion to your connected clients."
  },
  "cols": 3,
  "rows": 4
}
[/block]
**Returns**

`void`

**Example usage**

*"Raw" suggestions* 
[block:code]
{
  "codes": [
    {
      "code": "client.addSuggestion({ text: 'Your order will ship tomorrow!' })",
      "language": "javascript",
      "name": "Without metadata"
    },
    {
      "code": "client.addSuggestedMessage({\n  text: 'Your order will ship tomorrow!',\n  metadata: { orderId: '123-456' }\n})",
      "language": "javascript",
      "name": "With metadata"
    }
  ]
}
[/block]
*"Templated" suggestion* 
[block:code]
{
  "codes": [
    {
      "code": "client.addSuggestion({\n  intent: 'provide_shipping_info',\n  data: { orderNumber: '123', estimatedArrival: 'December 2, 2017' }\n})",
      "language": "javascript",
      "name": "With slot data"
    },
    {
      "code": "client.addSuggestedMessage({\n  intent: 'provide_order_confirmation',\n  metadata: { orderId: '123' }\n})",
      "language": "javascript",
      "name": "Without slot data"
    }
  ]
}
[/block]
**<sub>[Back to top](#)</sub>**
- - -

# makeReplyButton

```js
(text:String, iconUrl:String, stream:String, data:Object): → {Object}
```

Creates a Reply Button object to be passed to addResponseWithReplies

**Arguments**
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "`text`",
    "0-1": "String or null",
    "0-2": "Button text",
    "1-0": "`iconUrl`",
    "1-1": "String or null",
    "1-2": "URL of icon to attach to button",
    "2-0": "`stream`",
    "2-1": "String",
    "2-2": "Stream name to route the reply to",
    "3-0": "`data`",
    "3-1": "Object",
    "3-2": "Arbitrary data that will be returned with postback payload."
  },
  "cols": 3,
  "rows": 4
}
[/block]

**Returns**

`{Object}`

**Example Usage**

```js
client.addResponseWithReplies('provide_weather', {foo: 'bar'}, [client.makeReplyButton('Next day', 'https://../icon.png', 'check_weather', {})])
```

**<sub>[Back to top](#)</sub>**
- - -

# createStep

```js
(StepDefinition): → {Step:Object}
```

Create a [`Step`](doc:conversation-flow#section-steps) for a [Conversation Flow](doc:conversation-flow).

**Arguments**
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "`StepDefinition`",
    "0-1": "Object",
    "0-2": "See [`StepDefinition`](doc:client-api-types#section-stepdefinition-a-id-step-definition-a-)"
  },
  "cols": 3,
  "rows": 1
}
[/block]
**Returns**
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "`Step`",
    "0-1": "Object",
    "0-2": "An Object containing the configured methods for state management"
  },
  "cols": 3,
  "rows": 1
}
[/block]
**Example Usage**
[block:code]
{
  "codes": [
    {
      "code": "client.createStep({\n  extractInfo(messagePart) {\n    // Custom extract info action\n  },\n\n  next() {\n    // Custom return value pointing to next stream\n  },\n\n  satisfied() {},\n\n  prompt() {}\n})",
      "language": "javascript"
    }
  ]
}
[/block]
**<sub>[Back to top](#)</sub>**
- - -

# done

```js
(): → {void}
```
[block:callout]
{
  "type": "danger",
  "body": "As of version **0.0.14**, we recommend using [`sendResult`](#section-sendresult) instead of `done`",
  "title": "Deprecation warning"
}
[/block]
The `done` method is used as a signal that you have completed processing this message and would like to send your reply (or queued replies) the the user.

You should call this *only* when you are ready to immediately terminate your message processing function.

The `done` callback is required unless you provide a callback to the `prompt` method in your [`StepDefinition`](doc:client-api-types#section-stepdefinition-a-id-step-definition-a-), triggering it into asynchronous mode.

**Arguments**

N/A

**Returns**

N/A

**Example Usage**

```js
client.done()
```

**<sub>[Back to top](#)</sub>**
- - -

# expect

```js
(streamName: String, classifications: Array) → {void}
```

Appends the provided stream to the queue of expectations for subsequent inbound messages.

**Arguments**
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "`streamName`",
    "0-1": "String",
    "0-2": "A valid stream name. You may use a String literal or use the `getStreamName()` method.",
    "1-0": "`steps`",
    "1-1": "Array<String>",
    "1-2": "An Array of classification strings that the conversation flow should override routing for"
  },
  "cols": 3,
  "rows": 2
}
[/block]
**Returns**

N/A

**Example Usage**

```js
client.expect(client.getStreamName(), ['provide_weather', 'provide_city'])
client.expect('getWeatherQueryData', ['provide_weather', 'provide_city'])
```

The above example(s) will ensure that if the next message contains has a classification of `provide_weather` or `provide_city` the conversation logic will forego following the path defined via the `classifications` mapping in`runFlow` and enter the provided Stream.

- - -

# getConversation

```js
(): → {ConversationModel:Object}
```

Retrieve an copy of the [`ConversationModel`](doc:client-api-types#section-conversationmodel-a-id-conversation-model-a-l).

This object is a serialized representation of an [Immutable Map](https://facebook.github.io/immutable-js/docs/#/Map) held internally by the client instance. To set or update values on this Object, see [`updateConversationState`](#section-updateconversationstate).

**Arguments**

N/A

**Returns**
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "`ConversationModel`",
    "0-1": "Object",
    "0-2": "See [`ConversationModel`](doc:client-api-types#section-conversationmodel-a-id-conversation-model-a-)"
  },
  "cols": 3,
  "rows": 1
}
[/block]
**Example Usage**

```js
const conversation = client.getConversation()

console.log(conversation)
/**
{
  id: '...',
  messages: [...],
  state: {...},
}
**/
```

**<sub>[Back to top](#)</sub>**
- - -

# getConversationState

```js
(): → {ConversationState:Object}
```

Retrieve a copy of the conversation state object.

As with [`getConversation`](#getConversation), this object is a serialized representation of an [Immutable Map](https://facebook.github.io/immutable-js/docs/#/Map) held internally by the client instance. To set or update values on this Object, see [`updateConversationState`](#section- -updateconversationstate-).

**Arguments**

N/A

**Returns**
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "`ConversationState`",
    "0-1": "Object",
    "0-2": "The conversational state model. This Object is [writable](#section- -updateconversationstate-) via `updateConversationState` and as such can take any shape you would like."
  },
  "cols": 3,
  "rows": 1
}
[/block]
**Example Usage**

```js
const conversationState = client.getConversationState()

// Assuming: client.updateConversationState('foo', 'bar')
console.log(conversationState) // {foo: 'bar'}


// Assuming: client.updateConversationState('foo', {bar: 'baz'})
console.log(conversationState) // {foo: {bar: 'baz'}}
```

**Caveats**

It is important to avoid setting this value as a variable for later lookup in your program. Since the values may be manually `set` in later functions, the result of `getConversationState` will only ever return the latest "copy" of the up-to-date model.

```js
var convo = client.getConversationState() // => {foo: 'bar'}

client.updateConversationState({foo: 'noo', bar: 'baz'})

conosle.log(convo) // => {foo: 'bar'}
console.log(client.getConversationState()) // => {foo: 'noo', bar: 'baz'}
```

**<sub>[Back to top](#)</sub>**
- - -

# getCurrentApplicationEnvironment
[block:callout]
{
  "type": "danger",
  "title": "Deprecation warning",
  "body": "`getCurrentApplicationEnvironment` is deprecated and will be removed in a future version. Use the `getEnvironment` method instead."
}
[/block]
```js
(): → {ApplicationEnvironment:Object}
```

This fetches a copy of the current application environment. This object is a serialized representation of an [Immutable Map](https://facebook.github.io/immutable-js/docs/#/Map) held internally by the client instance. As such, mutating its values will have no affect on your program.

**Arguments**

N/A

**Returns**
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "0-0": "`ApplicationEnvironment`",
    "h-1": "Type",
    "h-2": "Description",
    "0-1": "Object",
    "0-2": "The shape of this Object will vary depending on your app configurations"
  },
  "cols": 3,
  "rows": 1
}
[/block]
**Example Usage**

```js
const applicationEnvironment = getCurrentApplicationEnvironment()
console.log(applicationEnvironment) // {foo: 'bar'}
```

**<sub>[Back to top](#)</sub>**
- - -

# getEnvironment

Retrieve assigned environment variables set for current application. These variables are set via the console in *Settings > General*.

**Note:** (requires version **0.0.9** or later).

**Arguments**

N/A

**Returns**
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "`ApplicationEnvironment`",
    "0-1": "Object",
    "0-2": "The shape of this Object will vary depending on your app configurations"
  },
  "cols": 3,
  "rows": 1
}
[/block]
**Example Usage**

```js
const envVars = client.getEnvironment() // {foo: 'bar', baz: 'quux'}
console.log(envVars.foo) // bar
```

**<sub>[Back to top](#)</sub>**
- - -
# getEntities

Retrieve a map slot values keyed by entity.

**Arguments**
[block:parameters]
{
  "data": {
    "0-0": "`messagePart`",
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-1": "Object",
    "0-2": "An Object containing `slots` for a MessagePart. This is accessed via the [`getMessagePart`](#section-getmessagepart) method.",
    "1-0": "`entity`",
    "1-1": "String",
    "1-2": "A string representing the entity to lookup"
  },
  "cols": 3,
  "rows": 2
}
[/block]
**Returns**
[block:parameters]
{
  "data": {
    "0-0": "`SlotValues`",
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-1": "`Array|null`",
    "0-2": "A mapping of [SlotValuesByRole](doc:client-api-types#section-slotvaluesbyrole-a-id-slotvaluesbyrole-a-) Arrays"
  },
  "cols": 3,
  "rows": 1
}
[/block]
**Example Usage**

```js
client.getEntities(client.getMessagePart(), 'opponent')

// Returns
{
  name: [
    {
      value: 'dodgers',
      raw_value: 'dodgers',
      parsed: null
    },
    {
      value: 'indians',
      raw_value: 'indians',
      parsed: null
    },
  ],
  city: [
    {
      value: 'los angeles',
      raw_value: 'los angeles',
      parsed: null
    },
    {
      value: 'cleveland',
      raw_value: 'cleveland',
      parsed: null
    },
  ],
]
```

**<sub>[Back to top](#)</sub>**
- - -

# getFirstEntityWithRole

Retrieve the first slot value with the given entity and specified role.

**Arguments**
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "`messagePart`",
    "0-1": "Object",
    "0-2": "An object containing `slots` for a MessagePart. This is accessed via the [`getMessagePart`](#section-getmessagepart) method.",
    "1-0": "`entity`",
    "1-1": "String",
    "1-2": "A string representing the entity to lookup",
    "2-0": "`role`",
    "2-1": "String",
    "2-2": "A String representing the role to lookup. By default, this is `\"generic\"`"
  },
  "cols": 3,
  "rows": 3
}
[/block]
**Returns**
[block:parameters]
{
  "data": {
    "0-0": "`SlotValue`",
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-1": "`Object|null`",
    "0-2": "See [`SlotValue`](#slotvalue)"
  },
  "cols": 3,
  "rows": 1
}
[/block]
**Example Usage**

```js
client.getFirstEntityWithRole(client.getMessagePart(), 'opponent', 'name')

// Returns
{
  value: 'dodgers',
  raw_value: 'dodgers',
  parsed: null
}


client.getFirstEntityWithRole(client.getMessagePart(), 'opponent')

// Returns the matching entity with role=generic
{
  value: 'la dodgers',
  raw_value: 'la dodgers',
  parsed: null
}
```

**<sub>[Back to top](#)</sub>**
- - -

# getMessagePart

```js
(): → {MessagePart:Object}
```

Fetches the current [`MessagePart`](doc:client-api-types#section-messagepart-a-id-message-part-a-) on which you are operating.

This object is a serialized representation of an [Immutable Map](https://facebook.github.io/immutable-js/docs/#/Map) held internally by the client instance. As such, mutating its values will have no affect on your program.


**Arguments**

N/A

**Returns**
[block:parameters]
{
  "data": {
    "0-0": "`MessagePart`",
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-1": "Object",
    "0-2": "See [`MessagePart`](doc:client-api-types#section-messagepart-a-id-message-part-a-)"
  },
  "cols": 3,
  "rows": 1
}
[/block]
**Example Usage**

```js
client.getMessagePart()
```

**<sub>[Back to top](#)</sub>**
- - -

# getPostbackData

```js
(): → {Object}
```

Fetches the postback data from the current [`MessagePart`](doc:client-api-types#section-messagepart-a-id-message-part-a-) on which you are operating. If the current message part is not a `postback`, then this function will return null.

**Arguments**

N/A

**Returns**
[block:parameters]
{
  "data": {
    "0-0": "`Data`",
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-1": "Object",
    "0-2": "See [`ActionPayload`](doc:client-api-types#section-actionpayload-a-id-action-payload-a-)."
  },
  "cols": 3,
  "rows": 1
}
[/block]
**Example Usage**

```js
const postbackData = client.getPostbackData()
```

**<sub>[Back to top](#)</sub>**

- - -

# getUsers

```js
(): → {Users:Array<User>}
```

Fetch a collection of [`User`](doc:client-api-types#section-user-a-id-user-a-) objects.

This array is a serialized representation of an [Immutable List](https://facebook.github.io/immutable-js/docs/#/List) held internally by the client instance. Use the [`updateUser`](#section-updateuser) method to operate on this list.

**Arguments**

N/A

**Returns**
[block:parameters]
{
  "data": {
    "0-0": "`Users`",
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-1": "Array",
    "0-2": "An Array of [`User`](doc:client-api-types#section-user-a-id-user-a-) objects"
  },
  "cols": 3,
  "rows": 1
}
[/block]
**<sub>[Back to top](#)</sub>**
- - -

# resetConversationState

Reset the conversation state to an empty Object (requires version **0.0.8** or later).

**Arguments**

N/A

**Returns**

N/A

**Example Usage**

```js
console.log(client.getConversationState()) // {foo: 'bar'}

client.resetConversationState()

console.log(client.getConversationState()) // {}
```

**<sub>[Back to top](#)</sub>**
- - -

# resetUser

```js
(userId:String): → {void}
```

Reset the state of a particlar [`User`](doc:client-api-types#section-user-a-id-user-a-) by providing an `id` or _all users_ by leaving the arguments blank.

**Arguments**
[block:parameters]
{
  "data": {
    "0-0": "`id`",
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-1": "String (optional)",
    "0-2": "The id of the user to reset. If left blank, _all users_ will be reset."
  },
  "cols": 3,
  "rows": 1
}
[/block]
**Returns**

N/A

**Example Usage**

```js
client.resetUser('foo1bar')

// or, to reset ALL users
client.resetUser()
```

**<sub>[Back to top](#)</sub>**
- - -

# runFlow

```js
(flowDefinition:FlowObject): → {void}
```

Initiate a [`Flow`](doc:client-api-types#section-flowdefinition-a-id-flow-definition-a-) sequence.
[block:callout]
{
  "type": "info",
  "body": "Read this [guide](doc:conversation-flow).",
  "title": "Learn more about managing conversation flows"
}
[/block]
**Arguments**

See [FlowDefinition](doc:client-api-types#section-flowdefinition-a-id-flow-definition-a-)

**Returns**

N/A

**Example Usage**

```js
client.runFlow({
  classifications: {},
  eventHandlers: {},
  streams: {
    streamName: [step1, step2],
    otherStreamName: [step2],
    main: 'otherStreamName',
    end: [help],
  },
})
```

**<sub>[Back to top](#)</sub>**
- - -

# sendResult

```js
(): → {Promise<InvocationResultSuccess | InvocationResultError | InvocationValidationError)>}
```

Sends logic invocation result to Init.ai API. `sendResult` returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) which will only resolve once your invocation data has been validated and successfully sent to the Init.ai API.

This Promise will be rejected if the API request fails or if validation of your data fails. Currently, only `addSuggestion` values will return validation errors.

**Note:** (requires version **0.0.14** or later).

**Arguments**
N/A

**Returns**

The Promise returned from `sendResult` will resolve or reject with one of the following:
[block:code]
{
  "codes": [
    {
      "code": "{\n  body: 'OK',\n  error: null\n}",
      "language": "javascript",
      "name": "Success"
    },
    {
      "code": "{\n  status: 400,\n  message: 'There was an error validating your data. Please try again.',\n  type: 'VALIDATION_ERROR'\n}",
      "language": "javascript",
      "name": "Validation Error"
    },
    {
      "code": "{\n  status: 500, // HTTP status code\n  message: 'There was an error sending your results. Please try again.',\n  type: 'API_ERROR'\n}",
      "language": "javascript",
      "name": "API/Network Error"
    }
  ]
}
[/block]
**Example Usage**
[block:code]
{
  "codes": [
    {
      "code": "client\n  .sendResult()\n  .then(result => {\n    console.log(result) // {body: 'OK', error: null}\n  })\n  .catch(error => {\n    console.log(error)\n  })",
      "language": "javascript",
      "name": ""
    }
  ]
}
[/block]
**<sub>[Back to top](#)</sub>**
- - -

# updateConversationState

```js
(key:Any, value:Any | model:Object | keyPath:Array, value:Any): → {void}
```

Allows the writing of new values and overwriting of previously set values.

The Client API uses [Immutable.js](https://facebook.github.io/immutable-js/) under the hood. The ConversationState is stored as an immutable [Map](https://facebook.github.io/immutable-js/docs/#/Map). As such, many of the constructs for setting values are accessible via the documentation in that library.

**Arguments**

*Update using a `key`, `value` pair:*

[block:parameters]
{
  "data": {
    "0-0": "`key`",
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-1": "String",
    "0-2": "The key to set on the Conversation State for later lookup. If this `key` already exists, the associated `value` will be used to overwrite the currently set value.",
    "1-0": "`value`",
    "1-1": "Any",
    "1-2": "The value associated with the key"
  },
  "cols": 3,
  "rows": 2
}
[/block]
*Update using an Object literal*

To assign an `Object` on the root level of the state object, provide the desired structure as the first argument.
[block:parameters]
{
  "data": {
    "0-0": "`model`",
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-1": "Object",
    "0-2": "An Object to be merged into the Map.\n\n\nSetting values in this manner implements a [`deep merge strategy`](https://facebook.github.io/immutable-js/docs/#/Map/mergeDeep) where any values/Object provided in this structure will extend existing values and new values will be merged in. If you do not wish to merge values and would prefer to overwrite them completely, use the `keymap` strategy discussed below."
  },
  "cols": 3,
  "rows": 1
}
[/block]
*Update using a keypath and value*
[block:parameters]
{
  "data": {
    "0-0": "`keypath`",
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-1": "Array",
    "0-2": "The hierarchy of keys expressed as an array where the last entry is the nested property to receive the update. <br /><br />Learn more about [`keymaps`](https://facebook.github.io/immutable-js/docs/#/Map/setIn).",
    "1-0": "`value`",
    "1-1": "Any",
    "1-2": "The value associated with the keypath"
  },
  "cols": 3,
  "rows": 2
}
[/block]
**Returns**

N/A

**Example Usage**

```js
// key, value pair
client.updateConversationState('foo', 'bar')
client.getConversationState() // => {foo: 'bar'}
client.updateConversationState('foo', 'baz')
client.getConversationState() // => {foo: 'baz'}

// Object literal
client.updateConversationState({city: 'Chicago'})
client.getConversationState() // => {chicago: 'Chicago'}
client.updateConversationState({city: 'San Francisco'})
client.getConversationState() // => {city: 'San Francisco'}

// Deep merge with Object literal
client.updateConversationState({
  place: {
    city: {name: 'San Francisco'}
  }
})
client.getConversationState() // => {place: {city: {name: 'San Francisco'}}}
client.updateConversationState({
  place: {
    state: {name: 'CA'}
  }
})
client.getConversationState() // => {place: {city: {name: 'San Francisco'}, state: {name: 'CA'}}}


// Keypath
// Assumes this state: {foo: {bar: {baz: 'quux'}}}
client.updateConversationState(['foo', 'bar', 'baz'], 'foo')
client.getConversationState() // => {foo: {bar: {baz: 'foo'}}}
```

**<sub>[Back to top](#)</sub>**
- - -

# updateUser

```js
(userId:String, key:String, value:String|Object|null): → {void}
```


Update data for a specific user. Only a subset of keys on the User object may be updated. Those include:

* `first_name:String`
* `last_name:String`
* `remote_id:String`
* `metadata:Obect|null`

**Arguments**
[block:parameters]
{
  "data": {
    "0-0": "`userId`",
    "h-0": "Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-1": "String",
    "0-2": "The id of the user",
    "1-0": "`key`",
    "1-1": "String",
    "1-2": "This may be any of the accepted keys: `first_name`, `last_name`, `remot_id`, or `metadata`.",
    "2-0": "`value`",
    "2-1": "Any",
    "2-2": "The value associated with the key. This value is a String in all cases except when the `key` is `metadata` in which case it must be an Object or `null`."
  },
  "cols": 3,
  "rows": 3
}
[/block]
**Returns**

N/A

**Example Usage**

```js
/** Updating metadata **/
client.updateUser('123', 'metadata', {city: 'Chicago'})
// {metadata: {city: 'Chicago'}}

// The updateUser method does not merge values, as such previous values will be overwritten.
client.updateUser('123', 'metadata', {state: 'il'})
// {metadata: {state: 'il'}}

// To update the value properly, the entire object must be provided
client.updateUser('123', 'metadata', {city: 'Chicago', state: 'il'})
// {metadata: {city: 'Chicago', state: 'il'}}

// Reset metadata
client.updateUser('123', 'metadata', null)

/** Update first_name, last_name, or remote_id **/
client.updateUser('123', 'first_name', 'Jeremy')
client.updateUser('123', 'last_name', 'Roenick')
client.updateUser('123', 'remote_id', '27')
```