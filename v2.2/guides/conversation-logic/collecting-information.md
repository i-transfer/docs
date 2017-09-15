---
title: "Collecting Information"
excerpt: ""
---
When an inbound message or event is received, the logic system has an opportunity to collect any relevant information from that message and store in on the conversation state.

This is done by calling the `extractInfo` method on all Steps defined in Flow.

The `extractInfo` function should read the values on the `slots` on the message and update the conversation state accordingly.

This information extraction happens before the Flow is traversed since it may affect the `satisfied` state of each Step.

[block:callout]
{
  "type": "info",
  "title": "Extracting Slots",
  "body": "The [Node.js SDK](doc:node-js-sdk)  provides helper methods to extract slots:\n\n- [`getEntities`](doc:node-js-sdk-client-methods#section-getentities)\n- [`getFirstEntityWithRole`](doc:node-js-sdk-client-methods#section-getfirstentitywithrole)"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Expecting a follow up"
}
[/block]
When a Step's `prompt` function is called, the Step may define an expectation that the next message from a user will be of a certain classification and that control of the conversation should route back to it.

This is useful situations like asking a user a question. The incoming answer might be classified as something that would normally trigger a generic answer, but the statements means something specific in the context of the last message the application sent to the user.
[block:code]
{
  "codes": [
    {
      "code": "client.expect(client.getStreamName(), ['provide_city', 'affirmative', 'negative'])",
      "language": "javascript"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "[`expect`](doc:node-js-sdk-client-methods#section-expect)",
  "title": "View the Node.js SDK reference"
}
[/block]