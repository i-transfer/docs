---
title: "Node.js SDK"
excerpt: "The Node.js SDK is used to manage conversation logic, event invocations, and compose replies for your Init.ai application."
---
[block:api-header]
{
  "title": "Requirements"
}
[/block]
* Ensure you have [Node.js](https://nodejs.org/en/download/) v4.3.2 or later installed
* Ensure you have a working [development](webhooks#section-development) or [production](webhooks#section-production) webhook to handle logic invocations
[block:api-header]
{
  "title": "Installation"
}
[/block]
Install from [npm](https://www.npmjs.com/)

```
npm i -S initai-node
```
[block:api-header]
{
  "title": "Usage"
}
[/block]
**Include the library in your project**

```javascript
const InitClient = require('initai-node')
``` 

**Instantiate a client instance** 

The payload sent to your webhook for the [LogicInvocation](webhooks#section-logicinvocation) event type (See [webhooks](doc:webhooks) docs) contains an Object for you to provide to your client instance.

* `data` (Object): The [Logic invocation data payload](webhooks#section-logicinvocation-example-body) received from your webhook.
* `options` (Object): A configuration object that must include a succeed callback
  * `succeed` (Function) – A callback that will be invoked when calling [`client.done()`](node-js-sdk-client-methods#section-done). This callback takes a LogicResult object as its only argument. The LogicResult object must be provided in your API response for the logic invocation.

```javascript
const client = InitClient.create(data, {succeed: (result) => {
  // Send `result` to Init.ai
})
```

The resulting client allows you to manage conversation state, flow, and composition of replies. Explore the [methods reference](doc:node-js-sdk-client-methods) or check out [Conversation Logic](doc:conversation-logic) guide.
[block:api-header]
{
  "title": "Sending the logic result"
}
[/block]
Once your logic code has completed, it is up to you to send the **Result Object** to our API for processing (and sending any necessary replies to users).
[block:callout]
{
  "type": "info",
  "body": "Feel free to use the [`sendLogicResult.js`](https://github.com/init-ai/sample-logic-server/blob/master/src/sendLogicResult.js) module from the [sample-logic-server](https://github.com/init-ai/sample-logic-server/blob/master/src/sendLogicResult.js) or continue reading for a breakdown of this API call.",
  "title": "Check out the demo!"
}
[/block]
The `succeed` callback which you provide when creating a client instance will receive a **Result Object** with the following shape as its only argument (all you need to do is pass this directly).

```javascript
// Result object
{
  execution_id: String,
  conversation_state: Object,
  conversation_state_patch: Array,
  user_metadata_updates: Object, 
  users_patch: Array,
  reset_users: Array,
  messages: Array<Object>
}
```
See the [Logic response](doc:webhooks#section-logic-response) reference for details on sending the request.