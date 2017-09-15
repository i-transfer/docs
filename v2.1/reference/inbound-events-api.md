---
title: "Inbound Events API"
excerpt: "The Inbound Events API is designed to allow you to emit messages to your conversational application from external sources."
---
Events can be sent after a user completes or fails to complete an external payment, order, or other activity outside the conversation. Events can also be used to trigger outbound messages on a schedule, by periodically sending events to the conversation from your API.
[block:api-header]
{
  "title": "Obtain an API Token"
}
[/block]
To send an event to your application, you will need to grab the API token from the [console](https://console.init.ai). The API Token is a [JSON Web Token (JWT)](https://jwt.io/introduction/) which is used to authenticate requests to our API. Find your token by navigating to the **Settings** page in the console.

See our documentation about [Tokens and Authentication](doc:tokens-and-authentication) for more information.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/03a2a0a-Screenshot_2017-04-24_14.26.16.png",
        "Screenshot 2017-04-24 14.26.16.png",
        1459,
        560,
        "#33353f"
      ]
    }
  ]
}
[/block]
You must include this token as an `Authorization`  header in your `POST` request when emitting an event:

```
Authorization: Bearer <JWT>
```

# Payload
[block:parameters]
{
  "data": {
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "`app_user_id`",
    "0-1": "`String`",
    "0-2": "The ID of the user, assigned by Init.ai, that you with to send the event to. The ID is accessible on the user object within the conversation logic. Typically you would send this ID to an API you control for storage and later use with the Events API. \n\nSee the [`getUsers()`](node-js-sdk-client-methods#section-getusers) for more information.",
    "1-0": "`event_type`",
    "1-1": "`String`",
    "1-2": "This is an arbitrary value that will be provided to you during logic processing. See [Handling Events](doc:handling-events).\n\n**Note:** it is a good idea to use a consistent schema for naming your events. A useful pattern is colon-separated namespaces: `user:location:change`",
    "2-0": "`data`",
    "2-1": "`Object`",
    "2-2": "This is an arbitrary Object which you can send along with the request and available during logic runs."
  },
  "cols": 3,
  "rows": 3
}
[/block]
# Usage

### From your server
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST https://api.init.ai/api/v1/webhook/event \\\n-H 'Content-Type: application/json' \\\n-H 'Authorization: Bearer <JWT_TOKEN>' \\\n-d '{\n  \"app_user_id\": \"<APP_USER_ID>\",\n  \"event_type\": \"<YOUR_EVENT>\",\n  \"data\": {}\n}'",
      "language": "curl"
    }
  ]
}
[/block]
### In Logic Handlers

Learn more about [handling events](doc:handling-events)  in the [Conversation Logic](doc:conversation-logic) guide.
[block:code]
{
  "codes": [
    {
      "code": "const InitClient = require('initai-node')\n\nmodule.exports = function runLogic(eventData) {\n  return new Promise(resolve => {\n    const client = InitClient.create(eventData)\n    const done = () => client.sendResult().then(resolve)\n\n    const sayHello = client.createStep({\n      satisfied() {\n        return false\n      },\n\n      prompt() {\n        client.addResponse('app:response:name:hello')\n        done()\n      }\n    })\n\n    const handleEvent = function(eventType, payload) {\n      client.addTextResponse('Received event of type: ' + eventType)\n      done()\n    }\n\n    const handleAppointmentComplete = function(eventType, payload) {\n      client.updateConversationState({ hasVisited: true })\n      client.addTextResponse('Thank you for visiting!')\n      done()\n    }\n\n    const handleAppointmentScheduled = function(eventType, payload) {\n      client.updateConversationState({\n        hasAppointmentScheduled: true,\n        nextAppointment: payload.date\n      })\n\n      client.addTextResponse(`Looking forward to seeing you on ${payload.date}`)\n      done()\n    }\n\n    console.log('Received message:', client.getMessagePart())\n\n    client.runFlow({\n      eventHandlers: {\n        // '*' Acts as a catch-all and will map all events not included in this\n        // object to the assigned function\n        '*': handleEvent,\n        'appointment:complete': handleAppointmentComplete,\n        'appointment:scheduled': handleAppointmentScheduled\n      },\n      streams: {\n        main: 'hi',\n        hi: [sayHello]\n      }\n    })\n  })\n}",
      "language": "javascript"
    }
  ]
}
[/block]