---
title: "Generating Suggestions"
excerpt: ""
---
Suggestions can be generated at the following layers:

- via a [LogicInvocation](https://docs.init.ai/docs/invocation-lifecycle) – in response to an inbound message or [event](https://docs.init.ai/docs/inbound-events-api)
- via the [Remote API](https://docs.init.ai/reference#create-suggestion)  
- Automatically via the Init.ai NLP system – Learn [more](doc:types-of-suggestions#section-auto-suggestions)
[block:api-header]
{
  "title": "via Logic Invocation"
}
[/block]
When an end user sends a message to a conversation or an [Inbound Event](https://docs.init.ai/docs/inbound-events-api) is triggered, a logic invocation is run. Using the [Node.js SDK](https://docs.init.ai/docs/node-js-sdk) you can add suggestions to a conversation using the [`addSuggestion`](https://docs.init.ai/docs/node-js-sdk-client-methods#section-addsuggestion) method.

For example, to send a **raw** suggestion, you can use the following:
[block:code]
{
  "codes": [
    {
      "code": "client.addSuggestion({ text: 'Some raw message' })\nclient.sendResult()",
      "language": "javascript",
      "name": "Generate a raw suggestion"
    }
  ]
}
[/block]
To send a intent, provide an `intent` and corresponding `data`. For example, assuming you have a training data example message like: 
[block:code]
{
  "codes": [
    {
      "code": "agent> i have [3](number/ticket_count) [bleacher](seat_type) tickets available for this [Saturday](time/gameday) at [Wrigley](venue_name)\n* provide_ticket_availability_count",
      "language": "markdown"
    }
  ]
}
[/block]
you can use logic like this to generate and suggest a message the `I have 3 Loge box tickets available for this Saturday at Fenway`:
[block:code]
{
  "codes": [
    {
      "code": "client.addSuggestion({\n  intent: 'provide_ticket_availability_count',\n  data: {\n    seat_type: 'Loge',\n    venue_name: 'Fenway',\n    'number/ticket_count': 3,\n    'time/gameday': 'Saturday',\n  },\n})\nclient.sendResult()",
      "language": "javascript"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "See the [SDK reference](https://docs.init.ai/docs/node-js-sdk-client-methods#section-addsuggestion) for more examples"
}
[/block]

[block:api-header]
{
  "title": "via Remote API"
}
[/block]
The Remote API exposes the ability to generate suggestion(s) for a given message in a conversation. See the [docs](https://docs.init.ai/reference#create-suggestion)

## Using correlation ids

When generating a suggestion via the Remote API, it is helpful to add a correlation_id string to each suggestion. This allows for the API to return validation errors specific to any suggestions that may fail.
[block:api-header]
{
  "title": "Auto Suggestions"
}
[/block]
The Init.ai platform is capable of generating suggestions automatically for basic interactions. See [this section](doc:types-of-suggestions#section-auto-suggestions) for more.
[block:api-header]
{
  "title": "Passing metadata"
}
[/block]
When composing suggestions, you may pass arbitrary metadata for later use on the client. This is an arbitrary Object which Init.ai simply passes from the invocation to a connected client. This can be useful when creating custom correlation ids, alternative data fetching strategies, etc.