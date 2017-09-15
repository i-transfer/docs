---
title: "Sending Responses"
excerpt: ""
---
Outbound messages can be sent as text, images, or in a structured format (for example, for use in Facebook Messenger). Within a single logic invocation you may apply as many responses as you wish. Each will be sent as an individual message to the end user.

# Sending text

The Init.ai platform ships with auto responses which allow you to send programmatically generated responses based on the current message's classification or intent. Using the [`addResponse`](doc:node-js-sdk-methods#section-addresponse) method in your logic, you can provide a classification string and an Object with data to populate the reply.
[block:code]
{
  "codes": [
    {
      "code": "client.addResponse('provide_weather', {\n  city: 'Philadelphia',\n  condition: 'sunny',\n})",
      "language": "javascript"
    }
  ]
}
[/block]
Init.ai will compose an outbound message based on the training data in the application's language model.
[block:callout]
{
  "type": "warning",
  "body": "Init.ai will not transform the text outside of the template slots in the outbound messages in the application's language data, so make sure that slots include any appropriate articles like 'a' or 'an' that may vary by tense or plurality."
}
[/block]
If you attempt to send an outbound message for which an exact message template is not available, Init.ai will attempt to match as closely as possible but might error if a match is not possible.

If an error occurs matching the message template, the error will be shown in the Logs in the Init.ai Console but you will not receive an error from the SDK.

# Sending text with suggested 'quick' replies

For application's connected via Smooch and a supported messaging channel, you can suggest replies to users using buttons that appear alongside text from your application.

To send quick replies, use the two methods [`addResponseWithReplies`](doc:node-js-sdk-methods#section-addresponsewithreplies) and [`makeReplyButton`](doc:node-js-sdk-methods#section-makereplybutton).

`makeReplyButton` will generate a JSON object defining a button structure. Pass an array of these button structures as the final argument to `addResponseWithReplies` to send the suggested replies along with the text that will be generated in the same manner as `addResponse`.


[block:code]
{
  "codes": [
    {
      "code": "const replies = [\n  client.makeReplyButton(\n    'Next day', // Button text\n    'https://../icon.png', // Icon URL\n    'check_weather', // Stream to route to\n    {myValue: 42} // Arbitrary payload\n  ),\n  // button 2,\n  // button 3\n]\nclient.addResponseWithReplies('provide_weather', {foo: 'bar'}, replies)",
      "language": "javascript"
    }
  ]
}
[/block]
For more information, see the Smooch documentation: https://docs.smooch.io/rest/#action-buttons.

Postback data is handled in the same way that postback data is handled for carousels.


[block:code]
{
  "codes": [
    {
      "code": "const postbackData = client.getPostbackData()\nif (postbackData) {\n  const myValue = postbackData.myValue // 42 from previous example\n}",
      "language": "javascript"
    }
  ]
}
[/block]
# Sending images

Using the [`addImageResponse`](doc:node-js-sdk-methods#section-addimageresponse) method, your application's logic can send images by providing the URL to the image.
[block:code]
{
  "codes": [
    {
      "code": "client.addImageResponse('https://www.example.com/image.jpg')",
      "language": "javascript"
    }
  ]
}
[/block]
Init.ai will send the image using the best-available transport for the messaging channel. For example, for Facebook Messenger and Telegram images will be inlined. For direct SMS connections a link will be provided, although for Twilio integrations via Smooch MMS may be used.

# Automatic responses

Init.ai's conversational NLP system is capable of predicting the type of message that should be sent to a user based on the history of the current conversation.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/bc7bb5c-logic_image_6.png",
        "logic_image_6.png",
        1051,
        521,
        "#cdcec9"
      ]
    }
  ]
}
[/block]
These predicted outbound messages are suitable for cases in which the appropriate message is the same for all users in all situations, such as general information about your application, your company, policies, etc. In particular, it is suitable for answer frequently asked questions and other cases in which the app is responding to a user with routing information, rather than sending custom or proactive information.

The system that sends outbound messages based on predictions is called the Auto Responder.

The Auto Responder is an opt-in system since it is not suitable for all cases. Opt-in occurs at the granularity of specific types of outbound messages. You can allow auto-responses of outbound messages based on classification base type or base type and subtype.

To configure the Auto Responder, modify the `autoResponses` object in the Flow definition before calling runFlow.

The configuration is a mapping between the classification of the predicted outbound message and the circumstances under which it can be automatically sent.

To enable the sending of a type of outbound message, create a key for that message classification in this configuration object, with the value of an empty JavaScript object. 


[block:callout]
{
  "type": "warning",
  "body": "Currently, when using auto responses, you must place a call to `client.sendResult()` after the call to `client.runFlow(...)`. This requirement will be removed in a future version of the Init.ai node library."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "client.runFlow({\n  ...\n  autoResponses: {\n    ...\n    <outbound_message_classication>: {} // Default minimum confidence.\n    ...\n  },\n  streams: {\n    ...\n  }\n})\nclient.sendResult().then(() => console.log('Done!'))",
      "language": "javascript"
    }
  ]
}
[/block]
`<outbound_message_classication>` is the name if the application's intent as defined in the training data. This is also the same type of value used in `client.addResponse(...)`.

If you would like to restrict the cases in which the message is sent, you can specify a minimum confidence level within this per object.
[block:code]
{
  "codes": [
    {
      "code": "client.runFlow({\n  ...\n  autoResponses: {\n    ...\n    <outbound_message_classication>: {\n      minimumConfidence: <float 0-1>\n    }\n    ...\n  },\n  streams: {\n    ...\n  }\n})\nclient.sendResult().then(() => console.log('Done!'))",
      "language": "javascript"
    }
  ]
}
[/block]
With the `<outbound_message_classication>` values of `welcome` and `you_are_welcome` this example becomes:
[block:code]
{
  "codes": [
    {
      "code": "client.runFlow({\n  ...\n  autoResponses: {\n    welcome: {\n      minimumConfidence: 0.2\n    },\n    you_are_welcome: {}, // Default minimum confidence\n    ...\n  },\n  streams: {\n    ...\n  }\n})\nclient.sendResult().then(() => console.log('Done!'))",
      "language": "javascript"
    }
  ]
}
[/block]
A `_continuation` is a special predicted message type that means that the next message is predicted to be from the end user or an agent. Init.ai can ignore messages in this state to give the user a chance to continue typing.

This may have the effect of making the application seem to not respond if too low of confidence is set.
[block:code]
{
  "codes": [
    {
      "code": "client.runFlow({\n  ...\n  autoResponses: {\n    _continuation: {\n      ignore: true,\n      minimumConfidence: 0.9\n    },\n    ...\n  },\n  streams: {\n    ...\n  }\n})\nclient.sendResult().then(() => console.log('Done!'))",
      "language": "javascript"
    }
  ]
}
[/block]