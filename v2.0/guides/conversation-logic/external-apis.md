---
title: "External APIs"
excerpt: ""
---
At any point during your logic execution, you may make calls to third-party services as needed. The [Node.js SDK](doc:node-js-sdk) also supports asynchronous operations within the `prompt` method of a [Step](doc:conversation-flow#section-steps).

Whether or not `prompt` is executed asynchronously depends on the arity of the function as you define it (the number or arguments). By providing a single argument, Init.ai will invoke `prompt` with a callback (called `next` in this document) which you may call when you are ready send a response or update your conversation state.

In the following examples, the `prompt` method of `getCity` is treated as asynchronous and the logic invocation will remain open until the network request is resolved.

## Finishing execution

In this first example, the logic invocation is terminated by calling `client.done()` from within a callback. 

When your application logic is hosted by Init.ai, calling `client.done()` will send any queued messages and update any conversation state, and then terminate immediately. The `next` callback does not need to be called.

```javascript
const axios = require('axios')

const getCity = client.createStep({
  satisfied() {
    return false
  },

  prompt(next) {
    axios('https://httpbin.org/get').then((response) => {
      client.setConversationState({city: 'Chicago'})
      client.done()
    }).catch((error) => {
       throw new Error(error)
    })
  }
})
```

## Continuing execution to another Step or Stream

In this second example, the logic invocation will continue processing the current [Stream](doc:conversation-flow#section-streams) because the provided `next` callback is invoked.

```javascript
const axios = require('axios')

const getCity = client.createStep({
  satisfied() {
    return false
  },

  prompt(next) {
    axios('https://httpbin.org/get').then((response) => {
      client.setConversationState({city: 'Chicago'})
      next('init.proceed')
    }).catch((error) => {
       throw new Error(error)
    })
  }
})
```

The `next` function may be called with a single argument to continue processing. 
- If you call `next` with the argument `init.proceed`, processing within the current Stream will advance to the next Step.
- If you call `next` with a Stream name, then processing will advance to that Stream.