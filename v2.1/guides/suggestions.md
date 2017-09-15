---
title: "Suggestions"
excerpt: ""
---
**Suggestions** (or **suggested messages**) are a useful feature in multi-party conversational applications. Most often, suggestions are used to provide real-time options/data to human agents in workspaces such as CRMs, but the use cases do not end there.

The lifecycle of a “suggestion” is different than that of a typical “message” in an Init.ai application. Suggestions can be generated at any time but are overwritten upon each new logic invocation (or Remote API call to create a suggestion). When using the [Monitor Client](doc:presenting-suggestions#section-using-the-monitor-client) [(github)](https://github.com/init-ai/initai-js#monitor-client) or fetching suggestions via the [Remote API](doc:remote-api) only the most recent suggestions generated will be returned. Therefore, you should typically assume suggestions are related to the latest inbound message.

## Suggestions architecture

Init.ai provides real-time capabilities to enhance conversational transactions across a variety of integrations and scenarios. The most common use case is augmenting a customer support agent in an agent workspace such as a CRM. In this case, as well as in general “assistance” scenarios, there are three common actors:


- The **end user**
- The **human agent** (usually via an agent workspace or CRM)
- The **Init.ai application**

While the exact implementation of the conversational enhancements provided (typically a custom UI) to human agents varies, these systems typically look something like this:

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/4deeffb-Suggestions-Docs_1.png",
        "Suggestions-Docs (1).png",
        2181,
        634,
        "#b9dfe5"
      ],
      "caption": "Suggestions architecture"
    }
  ]
}
[/block]
Init.ai provides a number of mechanisms for you to sync UI in an agent workspace with a particular conversation. Through the use of [Webhooks](doc:webhooks)  and/or our  [Monitor Client](doc:presenting-suggestions#section-using-the-monitor-client) [(github)](https://github.com/init-ai/initai-js#monitor-client) you can build your own real-time integrations tailored to your needs.