---
title: "Presenting Suggestions"
excerpt: ""
---
Suggestions are a powerful utility, but require presentation to a human (such as an **agent**) to be effective. The presentation of suggestions varies depending on the application. In some situations, such as in a CRM, you may embed a custom UI via an iFrame or plugin. In other circumstances, you may have your own, custom-built workspace that requires connections to your Init.ai application.

The two mechanism to push data to your UI are either the [Monitor client](https://github.com/init-ai/initai-js#monitor-client), the [SuggestionsNew webhook](doc:webhooks#suggestionsnew) or a combination of the two.
[block:api-header]
{
  "title": "Using the Monitor Client"
}
[/block]
Init.ai provides a JavaScript library that will allow your browser-based application to communicate with the internal API. This library includes a basic [API Client](https://github.com/init-ai/initai-js//README.md#api-client) as well as a [Monitor client](https://github.com/init-ai/initai-js#monitor-client). The monitor client allows your application to receive suggestions over a websocket so you can create a real-time experience.

To create a monitor client instance, you will need to grab your [API Token](doc:tokens-and-authentication#section-1-retrieve-a-jwt-from-the-console) and have an Init.ai user ID or `remote_id` available.
[block:callout]
{
  "type": "info",
  "body": "In the case of custom applications embedded in CRMs, unique identifiers for users are typically provided for you and accessed via their APIs"
}
[/block]
## Subscribing to suggestions
[block:code]
{
  "codes": [
    {
      "code": "import { createAPIClient, createMonitorClient } from 'initai-js'\n\nconst handleNewSuggestions = suggestions =>\n  console.log('Suggestions', suggestions)\n\ncreateMonitorClient({\n  apiClient: createAPIClient({ token: 'my-api-token' }),\n  userId: '123-some-user'\n}).then(monitorClient =>\n  monitorClient.on('suggestions:new', handleNewSuggestions)\n)",
      "language": "javascript"
    }
  ]
}
[/block]
Each time new suggestions are added to the conversation, the `handleNewSuggestions` function will be called.

## Initializing with Suggestions

Generally, when initializing your application you will want to render any currently stored suggestions. You can manually fetch the suggestions using the API Client.
[block:code]
{
  "codes": [
    {
      "code": "import { createAPIClient, createMonitorClient } from 'initai-js'\n\nconst userId = '123-some-user'\nconst token = 'my-api-token'\n\nconst handleNewSuggestions = suggestions =>\n  console.log('Suggestions', suggestions)\n\nconst apiClient = createAPIClient({ token })\n\ncreateMonitorClient({\n  apiClient,\n  userId\n}).then(monitorClient => {\n  // Fetch existing suggestions\n  apiClient.fetchSuggestions({ userId }).then(({ suggestions }) => {\n    handleNewSuggestions(suggestions)\n\n    // Subscribe to new suggestions\n    monitorClient.on('suggestions:new', handleNewSuggestions)\n  })\n})",
      "language": "javascript"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Using webhooks"
}
[/block]
Your existing [logic webhook server](https://docs.init.ai/docs/webhooks) will receive an event of type `SuggestionsNew` which will notify your server each time new suggestions are added to the conversation.

This is useful in the case where you would like to control your UI and subscriptions yourself or if you need to distribute suggestions data to various clients.
[block:callout]
{
  "type": "info",
  "body": "See the [SuggestionsNew webhook](doc:webhooks#suggestionsnew) docs for details"
}
[/block]