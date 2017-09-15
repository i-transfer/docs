---
title: "Conversation Flow"
excerpt: ""
---
Init.ai's [Node.js SDK](doc:client-api) provides a structured way to define how your application handles interactions with your users and external systems.

From a high level, this management is broken down as follows: `Flow -> Streams -> Steps`.

# Flow

A `Flow` is constructed using `Streams`, classification mappings, auto responses and event handlers. In general, `Streams` and `Steps` are followed when the application is leading the conversation, and `classification mappings` when your user(s) are leading. The Init.ai platform is designed to handle both scenarios and allow you to seamlessly transition between the two.

Learn more about [constructing Flows](doc:client-api-types#section-flowdefinition).

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9e5ea58-logic_image_2.png",
        "logic_image_2.png",
        750,
        547,
        "#d6d5d1"
      ]
    }
  ]
}
[/block]
# Streams

A Stream is used to accomplish a high-level goal on behalf of the user or your application. Streams are commonly used to gather information such as shipping addresses, user preferences, or for making reservations.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7f86d1f-logic_image_3.png",
        "logic_image_3.png",
        414,
        378,
        "#dadad9"
      ]
    }
  ]
}
[/block]
## Stream composition

Streams are composable – meaning, a Stream may contain other Streams. For example, in a checkout process, you may wish to collect the user's address. The address collection itself requires several [[`Steps`](#section-steps)] (name, city, address, etc.). Therefore, address collection is a Stream in its own but may also be used as part of other `Streams` you have defined such as checkout or shipping.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/23ee516-logic_image_4.png",
        "logic_image_4.png",
        1254,
        404,
        "#454a4f"
      ]
    }
  ]
}
[/block]
# Steps

`Steps` are used to define the tasks required to complete a `Stream`. These steps, executed sequentially, each corresponds to a specific action in the conversation. Steps are frequently used for collecting or providing information. In the case of address collection for example, a `Step` may be used to get the user's city.

Learn more about [creating Steps](doc:client-api-types#section-stepdefinition).

# Flow traversal

The entry point to each invocation of logic is dependent on the type of inbound message and its NLP classification. The order in which your Flow is processed looks like this:

1. If the message is an [Inbound Event](doc:events), the Flow looks for an appropriate handler to execute and then terminates. If the message is not an event, routing continues.

2. For textual messages the Flow first checks if it can [**automatically respond**](doc:sending-responses#section-automatic-responses) to the inbound message. If a message is sent automatically, a callback may be triggered to mutate conversation state. If a message is not sent, routing continues.

3. The [`extractInfo`](doc:client-api-types#section-stepdefinition) method is called for _all Steps_ defined via `Streams`. This allows steps to run extra tasks, even if a response from the particular step is unwarranted.

4. The logic system checks to see if a previous logic invocation stated that it is **expecting a followup message** of a certain type. If a previous invocation registered this expectation, the logic system will check if the inbound message is of the expected type. If it is, the execution will be routed to the Stream that set that expectation. If not, routing continues.

5. If the inbound message matches a value in the **classification mapping**, execution will be routed to the Stream that matches that classification. Typically this is used to handle a user deviating from the expected conversation Flow. If a matching classification is not found, or the matching Stream is completely satisfied, routing continues.

6. The Flow follows the **Streams** defined, starting the the 'main' Stream. The 'main' Stream is the default entry point for an application. This Stream is followed, including other Streams that may be included, until a Step that is not satisfied is found. Then the unsatisfied Step's `prompt` function is executed.

7. **If all Streams in the Flow are satisfied, the Flow routes to the 'end' Stream**, which can provide a message indicating the end the application's logic has been encountered or the application can no longer handle the current conversation.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b05a504-logic_image_5.png",
        "logic_image_5.png",
        757,
        989,
        "#d6d5d3"
      ],
      "sizing": "full"
    }
  ]
}
[/block]
# Stream traversal

When a Flow traverses its Streams to guide a conversation path, it starts with the Stream named 'main'.

A Stream is a collection of Steps that are evaluated in sequence. A Stream may also include another Stream in place of a Step by referencing that Stream by name.

Each Stream has a name, defined by the string key corresponding to the Stream in the `Streams` configuration on the Flow definition. Stream names cannot be repeated.

A Stream is evaluated from first Step to last. When a Stream is evaluated, the `satisfied` function of each Step is called in order, from first Step to last. The first Step that returns false from `satisfied` is considered the active Step for that Stream. The active Step is the Step for which the `prompt` function is called.

When the name of another Stream is provided instead of a Step, that Stream is executed from start to finish. If an unsatisfied Step is found in the referenced Stream, it becomes the active Step. If all Steps in the referenced Stream are satisfied, execution proceeds to the next Step in the original Stream.

A Step may redirect the conversation to another Stream if desired. It may do so by defining a `next` function function and returning name of the Stream to go to next.

# Step traversal

When a `Step` is evaluated, its `satisfied` method is run. If it returns `true`, further processing of the `Step` will cease and the next `Step` in the `Stream` will be evaluated. If it returns `false` – the `Step's` `prompt` method will be called in order to attempt to satisfy this Step.

In this example `Step`, our `satisfied` method returns whether or not we have stored the `has_city` property on the conversation state. If `true`, the `prompt` method will be run with the `prompt_city` response (learn more about [Sending Responses](doc:sending-responses)).

```js
const getCity = client.createStep({
  satisfied() {
    return client.getConversationState().has_city
  }

  prompt() {
    client.addResponse('prompt_city') // Ask user for city
    client.done()
  }
})
```

Learn more about [creating Steps](doc:client-api-types#section-stepdefinition).