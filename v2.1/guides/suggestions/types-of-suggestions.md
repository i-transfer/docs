---
title: "Types of Suggestions"
excerpt: ""
---
Suggestions are a powerful yet simple to use mechanism in multi-party conversations. There are essentially three “types” of suggestions:


- A **"templated"** suggestion which allows you to use prepared messages from your training data and populate any slot values with custom data.
- A **"raw"** suggestion which allows you to provide an arbitrary string to be delivered to your service/agent.
- **“Auto-suggestions”** which are provisioned by Init.ai’s machine learning system when appropriate
[block:api-header]
{
  "title": "Templated Suggestions"
}
[/block]
**Templated** suggestions are similar to [responses](doc:sending-responses#section-sending-text) which allow you to send programmatically generated responses based on the current message's intent. Using a **templated** suggestion would be useful in a variety of scenarios. For example, if our training data contained an example message like this:
[block:code]
{
  "codes": [
    {
      "code": "agent> There are [4](number/flight_options_available) flights available on [American Airlines](airline)\n* provide_flight_options/available_count",
      "language": "markdown",
      "name": "Agent providing flight info"
    }
  ]
}
[/block]
You could look up the flights for a given airline using your API of choice and provide your agent with a pre-built suggestion (or suggestions) such as:
[block:code]
{
  "codes": [
    {
      "code": "* There are 2 flights available on Delta\n* There are 7 flights available on United",
      "language": "text",
      "name": "Flight info suggestions"
    }
  ]
}
[/block]
The generation of these suggestions is handled in the [Logic](TODO:Link/To/Logic) layer which you can read more about [here](TODO:Link/To/Logic)
[block:api-header]
{
  "title": "Raw suggestions"
}
[/block]
**Raw** suggestions can be used to do anything from providing dynamically generated message responses to brand-tone-specific message suggestions. These suggestions are composed in your logic layer and allow for arbitrary strings to be sent to your connected clients.
[block:api-header]
{
  "title": "Auto suggestions"
}
[/block]
For suggestion scenarios where the response doesn't need to change depending on the individual user or data, Init.ai can automatically provision suggestions without the need to explicitly generate them from a logic invocation or Remote API request.

Automatic suggestions occur only when:
- A logic invocation or Remote API call has not sent explicit instructions for suggestions
- The next predicted message includes examples with no slots in the training data
[block:callout]
{
  "type": "info",
  "body": "As a conversation progresses between two or more parties, Init.ai's machine learning system automatically predicts the next message (type and sender). This system uses the training data contained within your project to pattern match similar conversational sequences to make its prediction.",
  "title": "Predicted messages"
}
[/block]
Automatic suggestions great for basic interactions such as greetings, thank-yous, FAQs, etc. However, more complex interactions require integrating logic and need to happen programatically.