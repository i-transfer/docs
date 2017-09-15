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

```js
  const collectTicker = client.createStep({
    extractInfo() {
      let ticker = firstOfEntityRole(client.getMessagePart(), 'ticker')

      if (ticker) {
        client.updateConversationState({
          requestedTicker: ticker,
        })
      }
    },

    satisfied() {
      return Boolean(client.getConversationState().requestedTicker)
    },

    prompt() {
      client.addResponse('prompt_ticker')

      // If the next message is a 'decline', like 'don't know'
      // or a ticker, the stream 'request_price' will be run
      client.expect('request_price', ['decline', 'provide_ticker'])
      client.done()
    }
  })

  const providePrice = client.createStep({
    satisfied() {
      return false // This forces the step to be activated
    },

    prompt() {
      client.addResponse('provide_price', {
        ticker: client.getConversationState().requestedTicker,
        price: '$ 28.34',
      })
      client.done()
    }
  })

  client.runFlow({
    classifications: {
      'request_price': 'getPrice',
    },
    streams: {
      main: 'getPrice',
      getPrice: [collectTicker, providePrice],
    }
  })
```

# Collecting and confirming a piece of data

Expectations and multi-[Step](doc:conversation-flow#section-steps) [Streams](doc:conversation-flow#section-streams) can be used to collect and then confirm a piece of data before use.

This pattern uses two state values: one for a requested value and one for a confirmed value. It then uses two [Steps](doc:conversation-flow#section-steps)  in sequence: one that collects the requested value and one that prompts for confirmation and then sets the confirmed state value and proceeds if confirmation is received.

```js
  const collectTicker = client.createStep({
    extractInfo() {
      let ticker = firstOfEntityRole(client.getMessagePart(), 'ticker')

      if (ticker) {
        client.updateConversationState({
          requestedTicker: ticker,
          confirmedTicker: null,
        })
      }
    },

    satisfied() {
      return Boolean(client.getConversationState().requestedTicker)
    },

    prompt() {
      client.addResponse('prompt_ticker')

      client.expect('request_price', ['decline', 'provide_ticker'])
      client.done()
    }
  })

  const confirmTicker = client.createStep({
    satisfied() {
      return Boolean(client.getConversationState().confirmedTicker)
    },

    prompt() {
      let baseClassification = client.getMessagePart().classification.base_type.value
      if (baseClassification === 'affirmative') {
        client.updateConversationState({
          confirmedTicker: client.getConversationState().requestedTicker,
        })
        return 'init.proceed'
      } else if (baseClassification === 'decline') {
        client.updateConversationState({
          requestedTicker: null, // Clear the requestedTicker so it's re-asked
          confirmedTicker: null,
        })

        client.addResponse('reprompt_ticker')
        client.done()
      }

      client.addResponse('confirm_ticker', {
        ticker: client.getConversationState().requestedTicker,
      })

      // If the next message is a 'decline', like 'don't know'
      // An 'affirmative', like 'yeah', or 'that's right'
      // or a ticker, the stream 'request_price' will be run
      client.expect('request_price', ['affirmative', 'decline', 'provide_ticker'])
      client.done()
    }
  })

  const providePrice = client.createStep({
    satisfied() {
      return false // This forces the step to be activated
    },

    prompt() {
      client.addResponse('provide_price', {
        ticker: client.getConversationState().requestedTicker,
        price: '$ 28.34',
      })
      client.done()
    }
  })

  client.runFlow({
    classifications: {
      'request_price': 'getPrice',
    },
    streams: {
      main: 'getPrice',
      getPrice: [collectTicker, confirmTicker, providePrice],
    }
  })
```

# Collecting the same data from multiple places

If you need to collect the same type of information in multiple streams, you can put the information collection steps into their own Stream and include that into multiple other [Streams](doc:conversation-flow#section-streams) by name.

When you set an expectation using `client.expect(...)` in this pattern, you will not be able to hardcode the Stream name to return logic to.

Use `client.getStreamName()` to get the name of the Stream to return to when use `client.expect(...)`

```js
const collectTicker = client.createStep({
  extractInfo() {
    let ticker = firstOfEntityRole(client.getMessagePart(), 'ticker')

    if (ticker) {
      client.updateConversationState({
        requestedTicker: ticker,
        confirmedTicker: null,
      })
    }
  },

  satisfied() {
    return Boolean(client.getConversationState().requestedTicker)
  },

  prompt() {
    client.addResponse('prompt_ticker')

    client.expect(client.getStreamName(), ['decline', 'provide_ticker'])
    client.done()
  }
})

const confirmTicker = client.createStep({
  satisfied() {
    return Boolean(client.getConversationState().confirmedTicker)
  },

  prompt() {
    let baseClassification = client.getMessagePart().classification.base_type.value
    if (baseClassification === 'affirmative') {
      client.updateConversationState({
        confirmedTicker: client.getConversationState().requestedTicker,
      })
      return 'init.proceed'
    } else if (baseClassification === 'decline') {
      client.updateConversationState({
        requestedTicker: null, // Clear the requestedTicker so it's re-asked
        confirmedTicker: null,
      })

      client.addResponse('reprompt_ticker')
      client.done()
    }

    client.addResponse('confirm_ticker', {
      ticker: client.getConversationState().requestedTicker,
    })

    // If the next message is a 'decline', like 'don't know'
    // An 'affirmative', like 'yeah', or 'that's right'
    // or a ticker, the current stream will be returned to
    client.expect(client.getStreamName(), ['affirmative', 'decline', 'provide_ticker'])
    client.done()
  }
})

const providePrice = client.createStep({
  // ...
})

const provideVolume = client.createStep({
  // ...
})

client.runFlow({
  classifications: {
    'request_price': 'getPrice',
    'request_volume': 'getVolume',
  },
  streams: {
    main: 'getPrice',
    getTicker: [collectTicker, confirmTicker],
    getPrice: ['getTicker', providePrice],
    getVolume: ['getTicker', provideVolume],
  }
})
```

# Yes / no branching

If you need to ask a user a yes / no question, and then direct the conversation from that result,
you can use a state variable to store the answer to the question and then return the name of the Stream
to proceed to from the `next` and `prompt` functions.

```js
const showStuffForAdults = client.createStep({
  // ...
})

const showStuffForMinors = client.createStep({
	// ...
})

const checkIfOverEighteen = client.createStep({
  satisfied() {
    return (typeof client.getConversationState().overAgeEighteen !== 'undefined')
  },

  next() {
    const isOverEighteen = client.getConversationState().overAgeEighteen
    if (isOverEighteen === true) {
    	return 'adult'
    } else if (isOverEighteen === false) {
    	return 'minor'
    }
  }

  prompt() {
    let baseClassification = client.getMessagePart().classification.base_type.value
    if (baseClassification === 'affirmative') {
      client.updateConversationState({
        overAgeEighteen: true,
      })
      return 'init.proceed' // `next` from this step will get called
    } else if (baseClassification === 'decline') {
      client.updateConversationState({
        overAgeEighteen: false,
      })
      return 'init.proceed' // `next` from this step will get called
    }

    client.addResponse('ask_over_eighteen')

    // If the next message is a 'decline', like 'don't know'
    // or an 'affirmative', like 'yeah', or 'that's right'
    // then the current stream will be returned to
    client.expect(client.getStreamName(), ['affirmative', 'decline'])
    client.done()
  }
})

client.runFlow({
  streams: {
    main: 'showContent',
    showContent: [checkIfOverEighteen],
    adult: [showStuffForAdults],
    minor: [showStuffForMinors],
  }
})
```

# Collecting information in sequence

To collect information in sequence, simply order the collection Steps in a [Stream](doc:conversation-flow#section-streams).

In this example, we will collect the country, city, and address for a delivery.

As each piece of information is populated, the corresponding [Step](doc:conversation-flow#section-steps) will become satisfied.

Note that since `extractInfo` is run on every Step for each message, this information
might be collected out of order. For example a user could provide the city before the country,
in which case when the Stream is entered the application will prompt for country, skip the already
satisfied Step that would ask for the city, and the prompt for address.

```js
const collectCountry = client.createStep({
  extractInfo() {
    let country = firstOfEntityRole(client.getMessagePart(), 'country')

    if (country) {
      client.updateConversationState({
        deliveryCountry: country,
      })
    }
  },

  satisfied() {
    return Boolean(client.getConversationState().deliveryCountry)
  },

  prompt() {
    client.addResponse('prompt_country')

    // If the next message is classified as providing the country, city,
    // or address, route back to the current stream
    client.expect(client.getStreamName(), ['provide_country', 'provide_city', 'provide_address'])
    client.done()
  }
})

const collectCity = client.createStep({
  extractInfo() {
    let city = firstOfEntityRole(client.getMessagePart(), 'city')

    if (city) {
      client.updateConversationState({
        deliveryCity: city,
      })
    }
  },

  satisfied() {
    return Boolean(client.getConversationState().deliveryCity)
  },

  prompt() {
    client.addResponse('prompt_city')

    // If the next message is classified as providing the country, city,
    // or address, route back to the current stream
    client.expect(client.getStreamName(), ['provide_country', 'provide_city', 'provide_address'])
    client.done()
  }
})

const collectAddress = client.createStep({
  extractInfo() {
    let address = firstOfEntityRole(client.getMessagePart(), 'address')

    if (address) {
      client.updateConversationState({
        deliveryAddress: address,
      })
    }
  },

  satisfied() {
    return Boolean(client.getConversationState().deliveryAddress)
  },

  prompt() {
    client.addResponse('prompt_address')

    // If the next message is classified as providing the country, city,
    // or address, route back to the current stream
    client.expect(client.getStreamName(), ['provide_country', 'provide_city', 'provide_address'])
    client.done()
  }
})

const confirmOrder = client.createStep({
  satisfied() {
    return false // This forces the step to be activated
  },

  prompt() {
    client.addResponse('confirm_order', {
      address: client.getConversationState().address,
      city: client.getConversationState().city,
      country: client.getConversationState().country,
    })
    client.done()
  }
})

client.runFlow({
  streams: {
    main: 'checkout',
    collectDeliveryAddress: [collectState, collectCity, collectAddress],
    checkout: ['collectDeliveryAddress', confirmOrder]
  }
})
```

# Follow up questions on topic

_EXAMPLE COMING SOON_

# FAQs and Question-Answering

_EXAMPLE COMING SOON_

# Sending a carousel or list of items.

See [Sending Responses](doc:sending-responses) 

# Proactively sending messages to users

To send messages to users without their input, you need to trigger an event from outside of Init.ai. See the [Inbound Events](doc:events) reference for more.

# Handling payments

Payments can be handled by hosting a payment form on an external server in a web page suitable for mobile use. After the payment succeeds or fails, [post an event](doc:events) your Init.ai application.