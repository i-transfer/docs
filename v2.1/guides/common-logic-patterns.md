---
title: "Common Logic Patterns"
excerpt: ""
---
[block:callout]
{
  "type": "info",
  "body": "If you need to understand any of the terms used in this document, take a look at the [key terms and concepts](../basics/key_terms_and_concepts.md).",
  "title": "Key terms"
}
[/block]
# Wrapping [`sendResult`](doc:node-js-sdk-client-methods#section-sendresult)

It is often cumbersome and redundant to attach Promise handlers to `sendResult` multiple times in your codebase. A useful pattern is to create your own `done` function that wraps this call. 
[block:code]
{
  "codes": [
    {
      "code": "const done = () => {\n  client.sendResponse().then(() => console.log('Done!'))\n}\n\nconst collectTicker = client.createStep({\n  extractInfo() {\n    let ticker = firstOfEntityRole(client.getMessagePart(), 'ticker')\n    if (ticker) {\n      client.updateConversationState({ requestedTicker: ticker })\n    }\n  },\n  satisfied() {\n    return Boolean(client.getConversationState().requestedTicker)\n  },\n  prompt() {\n    client.addResponse('prompt_ticker')\n    // If the next message is a 'decline', like 'don't know'\n    // or a ticker, the stream 'request_price' will be run\n    client.expect('request_price', ['decline', 'provide_ticker'])\n    done()\n  }\n})\n\nconst providePrice = client.createStep({\n  satisfied() {\n    return false // This forces the step to be activated\n  },\n  prompt() {\n    client.addResponse('provide_price', {\n      ticker: client.getConversationState().requestedTicker,\n      price: '$ 28.34'\n    })\n    done()\n  }\n})\n",
      "language": "javascript",
      "name": "Wrapping `sendResult`"
    }
  ]
}
[/block]
# Watching for data in messages

The `extractInfo` function is called for ALL Steps in a [Flow](doc:key-terms-and-concepts#section-flow) prior to the Flow being traversed. This method can be placed on a [Step](doc:key-terms-and-concepts#section-step) to take information from a message part and then save that information to the conversation state for later use.

Say we are creating a stock price application that requires a ticker symbol. We might have a step to collect the ticker and another step to provide the current price.
[block:code]
{
  "codes": [
    {
      "code": "const collectTicker = client.createStep({\n    extractInfo() {\n      let ticker = client.getFirstEntityWithRole(client.getMessagePart(), 'ticker')\n\n      if (ticker) {\n        client.updateConversationState({\n          requestedTicker: ticker,\n        })\n      }\n    },\n\n    satisfied() {\n      return Boolean(client.getConversationState().requestedTicker)\n    },\n\n    prompt() {\n      client.addResponse('prompt_ticker')\n    }\n  })\n\n  const providePrice = client.createStep({\n    satisfied() {\n      return false // This forces the step to be activated\n    },\n\n    prompt() {\n      client.addResponse('provide_price', {\n        ticker: client.getConversationState().requestedTicker,\n        price: '$ 28.34',\n      })\n    }\n  })\n\n  client.runFlow({\n    classifications: {\n      'provide_ticker': 'getPrice',\n      'request_price': 'getPrice',\n    },\n    streams: {\n      main: 'getPrice',\n      getPrice: [collectTicker, providePrice],\n    }\n  })",
      "language": "javascript",
      "name": null
    }
  ]
}
[/block]
# Prompting a user and returning to the same location in the Flow

In some circumstances you might ask a user for specific type of information or data.

It is possible to specify that the logic system should return to the same [Stream](doc:conversation-flow#section-streams) in the conversation if this data is provided in the next message. This next-message routing is done by specifying the expected classifications of the next message.

The means you don't have to specify the classification for messages containing this data in the `classifications` mapping in the [Flow](doc:conversation-flow#section-streams) definition. You may, but if a per-message expectation is set that takes precedence in routing.

For example, in this example the application expects the user to provide a ticker symbol for a stock or to say that they don't know. Calling `client.expect` in the `collectTicker` [Step](doc:conversation-flow#section-steps)  will change the routing for the next message and cause matching classifications to return to the specified [Stream](doc:conversation-flow#section-streams) in the conversation.
[block:code]
{
  "codes": [
    {
      "code": "const collectTicker = client.createStep({\n  extractInfo() {\n    let ticker = firstOfEntityRole(client.getMessagePart(), 'ticker')\n\n    if (ticker) {\n      client.updateConversationState({\n        requestedTicker: ticker\n      })\n    }\n  },\n\n  satisfied() {\n    return Boolean(client.getConversationState().requestedTicker)\n  },\n\n  prompt() {\n    client.addResponse('prompt_ticker')\n\n    // If the next message is a 'decline', like 'don't know'\n    // or a ticker, the stream 'request_price' will be run\n    client.expect('request_price', ['decline', 'provide_ticker'])\n    client.sendResult().then(() => console.log('Done!'))\n  }\n})\n\nconst providePrice = client.createStep({\n  satisfied() {\n    return false // This forces the step to be activated\n  },\n\n  prompt() {\n    client.addResponse('provide_price', {\n      ticker: client.getConversationState().requestedTicker,\n      price: '$ 28.34'\n    })\n    client.sendResult().then(() => console.log('Done!'))\n  }\n})\n\nclient.runFlow({\n  classifications: {\n    request_price: 'getPrice'\n  },\n  streams: {\n    main: 'getPrice',\n    getPrice: [collectTicker, providePrice]\n  }\n})\n",
      "language": "javascript"
    }
  ]
}
[/block]
# Collecting and confirming a piece of data

Expectations and multi-[Step](doc:conversation-flow#section-steps) [Streams](doc:conversation-flow#section-streams) can be used to collect and then confirm a piece of data before use.

This pattern uses two state values: one for a requested value and one for a confirmed value. It then uses two [Steps](doc:conversation-flow#section-steps)  in sequence: one that collects the requested value and one that prompts for confirmation and then sets the confirmed state value and proceeds if confirmation is received.
[block:code]
{
  "codes": [
    {
      "code": "const collectTicker = client.createStep({\n  extractInfo() {\n    let ticker = firstOfEntityRole(client.getMessagePart(), 'ticker')\n\n    if (ticker) {\n      client.updateConversationState({\n        requestedTicker: ticker,\n        confirmedTicker: null\n      })\n    }\n  },\n\n  satisfied() {\n    return Boolean(client.getConversationState().requestedTicker)\n  },\n\n  prompt() {\n    client.addResponse('prompt_ticker')\n\n    client.expect('request_price', ['decline', 'provide_ticker'])\n    client.sendResult().then(() => console.log('Done!'))\n  }\n})\n\nconst confirmTicker = client.createStep({\n  satisfied() {\n    return Boolean(client.getConversationState().confirmedTicker)\n  },\n\n  prompt() {\n    let baseClassification = client.getMessagePart().classification.base_type\n      .value\n    if (baseClassification === 'affirmative') {\n      client.updateConversationState({\n        confirmedTicker: client.getConversationState().requestedTicker\n      })\n      return 'init.proceed'\n    } else if (baseClassification === 'decline') {\n      client.updateConversationState({\n        requestedTicker: null, // Clear the requestedTicker so it's re-asked\n        confirmedTicker: null\n      })\n\n      client.addResponse('reprompt_ticker')\n      client.sendResult().then(() => console.log('Done!'))\n    }\n\n    client.addResponse('confirm_ticker', {\n      ticker: client.getConversationState().requestedTicker\n    })\n\n    // If the next message is a 'decline', like 'don't know'\n    // An 'affirmative', like 'yeah', or 'that's right'\n    // or a ticker, the stream 'request_price' will be run\n    client.expect('request_price', ['affirmative', 'decline', 'provide_ticker'])\n    client.sendResult().then(() => console.log('Done!'))\n  }\n})\n\nconst providePrice = client.createStep({\n  satisfied() {\n    return false // This forces the step to be activated\n  },\n\n  prompt() {\n    client.addResponse('provide_price', {\n      ticker: client.getConversationState().requestedTicker,\n      price: '$ 28.34'\n    })\n    client.sendResult().then(() => console.log('Done!'))\n  }\n})\n\nclient.runFlow({\n  classifications: {\n    request_price: 'getPrice'\n  },\n  streams: {\n    main: 'getPrice',\n    getPrice: [collectTicker, confirmTicker, providePrice]\n  }\n})\n",
      "language": "javascript"
    }
  ]
}
[/block]
# Collecting the same data from multiple places

If you need to collect the same type of information in multiple streams, you can put the information collection steps into their own Stream and include that into multiple other [Streams](doc:conversation-flow#section-streams) by name.

When you set an expectation using `client.expect(...)` in this pattern, you will not be able to hardcode the Stream name to return logic to.

Use `client.getStreamName()` to get the name of the Stream to return to when use `client.expect(...)`
[block:code]
{
  "codes": [
    {
      "code": "const collectTicker = client.createStep({\n  extractInfo() {\n    let ticker = firstOfEntityRole(client.getMessagePart(), 'ticker')\n\n    if (ticker) {\n      client.updateConversationState({\n        requestedTicker: ticker,\n        confirmedTicker: null\n      })\n    }\n  },\n\n  satisfied() {\n    return Boolean(client.getConversationState().requestedTicker)\n  },\n\n  prompt() {\n    client.addResponse('prompt_ticker')\n\n    client.expect(client.getStreamName(), ['decline', 'provide_ticker'])\n    client.sendResult().then(() => console.log('Done!'))\n  }\n})\n\nconst confirmTicker = client.createStep({\n  satisfied() {\n    return Boolean(client.getConversationState().confirmedTicker)\n  },\n\n  prompt() {\n    let baseClassification = client.getMessagePart().classification.base_type\n      .value\n    if (baseClassification === 'affirmative') {\n      client.updateConversationState({\n        confirmedTicker: client.getConversationState().requestedTicker\n      })\n      return 'init.proceed'\n    } else if (baseClassification === 'decline') {\n      client.updateConversationState({\n        requestedTicker: null, // Clear the requestedTicker so it's re-asked\n        confirmedTicker: null\n      })\n\n      client.addResponse('reprompt_ticker')\n      client.sendResult().then(() => console.log('Done!'))\n    }\n\n    client.addResponse('confirm_ticker', {\n      ticker: client.getConversationState().requestedTicker\n    })\n\n    // If the next message is a 'decline', like 'don't know'\n    // An 'affirmative', like 'yeah', or 'that's right'\n    // or a ticker, the current stream will be returned to\n    client.expect(client.getStreamName(), [\n      'affirmative',\n      'decline',\n      'provide_ticker'\n    ])\n    client.sendResult().then(() => console.log('Done!'))\n  }\n})\n\nconst providePrice = client.createStep(\n  {\n    // ...\n  }\n)\n\nconst provideVolume = client.createStep(\n  {\n    // ...\n  }\n)\n\nclient.runFlow({\n  classifications: {\n    request_price: 'getPrice',\n    request_volume: 'getVolume'\n  },\n  streams: {\n    main: 'getPrice',\n    getTicker: [collectTicker, confirmTicker],\n    getPrice: ['getTicker', providePrice],\n    getVolume: ['getTicker', provideVolume]\n  }\n})\n",
      "language": "javascript"
    }
  ]
}
[/block]
# Yes / no branching

If you need to ask a user a yes / no question, and then direct the conversation from that result,
you can use a state variable to store the answer to the question and then return the name of the Stream
to proceed to from the `next` and `prompt` functions.
[block:code]
{
  "codes": [
    {
      "code": "const showStuffForAdults = client.createStep(\n  {\n    // ...\n  }\n)\n\nconst showStuffForMinors = client.createStep(\n  {\n    // ...\n  }\n)\n\nconst checkIfOverEighteen = client.createStep({\n  satisfied() {\n    return typeof client.getConversationState().overAgeEighteen !== 'undefined'\n  },\n\n  next() {\n    const isOverEighteen = client.getConversationState().overAgeEighteen\n    if (isOverEighteen === true) {\n      return 'adult'\n    } else if (isOverEighteen === false) {\n      return 'minor'\n    }\n  },\n\n  prompt() {\n    let baseClassification = client.getMessagePart().classification.base_type\n      .value\n    if (baseClassification === 'affirmative') {\n      client.updateConversationState({\n        overAgeEighteen: true\n      })\n      return 'init.proceed' // `next` from this step will get called\n    } else if (baseClassification === 'decline') {\n      client.updateConversationState({\n        overAgeEighteen: false\n      })\n      return 'init.proceed' // `next` from this step will get called\n    }\n\n    client.addResponse('ask_over_eighteen')\n\n    // If the next message is a 'decline', like 'don't know'\n    // or an 'affirmative', like 'yeah', or 'that's right'\n    // then the current stream will be returned to\n    client.expect(client.getStreamName(), ['affirmative', 'decline'])\n    client.sendResult().then(() => console.log('Done!'))\n  }\n})\n\nclient.runFlow({\n  streams: {\n    main: 'showContent',\n    showContent: [checkIfOverEighteen],\n    adult: [showStuffForAdults],\n    minor: [showStuffForMinors]\n  }\n})\n",
      "language": "javascript"
    }
  ]
}
[/block]
# Collecting information in sequence

To collect information in sequence, simply order the collection Steps in a [Stream](doc:conversation-flow#section-streams).

In this example, we will collect the country, city, and address for a delivery.

As each piece of information is populated, the corresponding [Step](doc:conversation-flow#section-steps) will become satisfied.

Note that since `extractInfo` is run on every Step for each message, this information
might be collected out of order. For example a user could provide the city before the country,
in which case when the Stream is entered the application will prompt for country, skip the already
satisfied Step that would ask for the city, and the prompt for address.
[block:code]
{
  "codes": [
    {
      "code": "const collectCountry = client.createStep({\n  extractInfo() {\n    let country = firstOfEntityRole(client.getMessagePart(), 'country')\n\n    if (country) {\n      client.updateConversationState({\n        deliveryCountry: country\n      })\n    }\n  },\n\n  satisfied() {\n    return Boolean(client.getConversationState().deliveryCountry)\n  },\n\n  prompt() {\n    client.addResponse('prompt_country')\n\n    // If the next message is classified as providing the country, city,\n    // or address, route back to the current stream\n    client.expect(client.getStreamName(), [\n      'provide_country',\n      'provide_city',\n      'provide_address'\n    ])\n    client.sendResult().then(() => console.log('Done!'))\n  }\n})\n\nconst collectCity = client.createStep({\n  extractInfo() {\n    let city = firstOfEntityRole(client.getMessagePart(), 'city')\n\n    if (city) {\n      client.updateConversationState({\n        deliveryCity: city\n      })\n    }\n  },\n\n  satisfied() {\n    return Boolean(client.getConversationState().deliveryCity)\n  },\n\n  prompt() {\n    client.addResponse('prompt_city')\n\n    // If the next message is classified as providing the country, city,\n    // or address, route back to the current stream\n    client.expect(client.getStreamName(), [\n      'provide_country',\n      'provide_city',\n      'provide_address'\n    ])\n    client.sendResult().then(() => console.log('Done!'))\n  }\n})\n\nconst collectAddress = client.createStep({\n  extractInfo() {\n    let address = firstOfEntityRole(client.getMessagePart(), 'address')\n\n    if (address) {\n      client.updateConversationState({\n        deliveryAddress: address\n      })\n    }\n  },\n\n  satisfied() {\n    return Boolean(client.getConversationState().deliveryAddress)\n  },\n\n  prompt() {\n    client.addResponse('prompt_address')\n\n    // If the next message is classified as providing the country, city,\n    // or address, route back to the current stream\n    client.expect(client.getStreamName(), [\n      'provide_country',\n      'provide_city',\n      'provide_address'\n    ])\n    client.sendResult().then(() => console.log('Done!'))\n  }\n})\n\nconst confirmOrder = client.createStep({\n  satisfied() {\n    return false // This forces the step to be activated\n  },\n\n  prompt() {\n    client.addResponse('confirm_order', {\n      address: client.getConversationState().address,\n      city: client.getConversationState().city,\n      country: client.getConversationState().country\n    })\n    client.sendResult().then(() => console.log('Done!'))\n  }\n})\n\nclient.runFlow({\n  streams: {\n    main: 'checkout',\n    collectDeliveryAddress: [collectState, collectCity, collectAddress],\n    checkout: ['collectDeliveryAddress', confirmOrder]\n  }\n})\n",
      "language": "javascript"
    }
  ]
}
[/block]
# Follow up questions on topic

_EXAMPLE COMING SOON_

# FAQs and Question-Answering

_EXAMPLE COMING SOON_

# Sending a carousel or list of items.

See [Sending Responses](doc:sending-responses) 

# Proactively sending messages to users

To send messages to users without their input, you need to trigger an event from outside of Init.ai. See the [Inbound Events API](doc:inbound-events-api) reference for more.

# Handling payments

Payments can be handled by hosting a payment form on an external server in a web page suitable for mobile use. After the payment succeeds or fails, [post an event](doc:events) your Init.ai application.