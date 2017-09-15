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
[block:callout]
{
  "type": "info",
  "title": "Sample Logic Server",
  "body": "We provide a starter project for you to get quickly up and running with the Node.js SDK. Feel free to try it out: https://github.com/init-ai/sample-logic-server"
}
[/block]

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

```javascript
const client = InitClient.create(data)
```

The resulting client allows you to manage conversation state, flow, and composition of replies. Explore the [methods reference](doc:node-js-sdk-client-methods) or check out [Conversation Logic](doc:conversation-logic) guide.
[block:api-header]
{
  "title": "Sending the logic result"
}
[/block]
Prior to version **0.0.14**, it was required that you manually send a logic invocation result to our API. With version 0.0.14, a new method [sendResult](doc:node-js-sdk-client-methods#section-sendresult) sendResult was added which handles this call for you.