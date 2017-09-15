---
title: "Webhooks"
excerpt: ""
---
Webhooks provide a way for you to integrate your Init.ai application into your existing services. While required for processing logic, you can also use webhooks for collecting custom metrics, monitoring conversations, or to back custom UIs.

When an event you are subscribed to is triggered, your webhook will be sent a `POST` request with a `JSON` payload.

Webhooks are required to be served over HTTPS and return a `200` response or it will be suspended and will require you to manually re-validate your endpoint via the console.
[block:api-header]
{
  "title": "Developing locally"
}
[/block]
During development, you may wish to test your application against a locally running server. This has two benefits – first, you will be able to iterate on your local machine without having to constantly deploy each change to a service and second, you can work on your application without disrupting any production traffic.
[block:callout]
{
  "type": "info",
  "title": "Reference webhook server",
  "body": "We have put together a reference implementation of a webhooks server for use with our platform. Feel free to use it to get up and running quickly!\n\nhttps://github.com/init-ai/sample-logic-server\n\n**Do not** rely on a locally running service for production!"
}
[/block]


## Configuring "development" webhooks

In the console, you can configure "development" webhooks to use against the Chat UI. While you may provide a link to any running server, this is intended for use on your local machine.

### Exposing localhost

You will need to expose your locally running server to the world for this to work. We recommend using a tool such as [ngrok](https://ngrok.com/) or [localtunnel](https://localtunnel.github.io/www/). The [sample-logic-server](https://github.com/init-ai/sample-logic-server) repo ships with an ngrok wrapper as a dependency, so feel free to use that.

### Managing webhooks in the console

#### Development

When using the built-in chat module in the console, you have the option to add a development webhook. Simply open chat by clicking the icon in the bottom right corner, then clicking **Manage Webhooks** and add your development webhook URL.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/2499655-add-dev-webhook-7.gif",
        "add-dev-webhook-7.gif",
        1400,
        976,
        "#2c2e37"
      ]
    }
  ]
}
[/block]
#### Production

To setup your "production" webhook, navigate to the **Settings* view of your project and enter your webhook URL in the Logic Webhook field.

This webhook will work both for the in-console Chat as well any [messaging connections](doc:messaging-connections)  you may have set up.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/0246f2c-add-prod-webhook-2.gif",
        "add-prod-webhook-2.gif",
        1351,
        978,
        "#2e2f39"
      ]
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Webhook event types"
}
[/block]
It is important to understand the message/event [invocation lifecycle](doc:invocation-lifecycle) before setting up your webhooks.

The request body sent to your webhook will contain JSON with a key of `event_type`:

```json
{
  "event_type": "LogicInvocation|MessageOutbound|Test",
  ...
}
```

Currently, there are three event types that your endpoint will receive:

* [Test](#section-test) – Sent when your webhook is initially setup and on attempts to re-validate
* [LogicInvocation](#section-logicinvocation) – Sent when an inbound message or event is received
* [MessageOutbound](#section-messageoutbound) – Sent when your application sends a message to a conversation
[block:api-header]
{
  "title": "Test"
}
[/block]
The `Test` event is triggered when you add a webhook via the console UI or when revalidation of the endpoint is attempted.

## Test example body

```json
{
  event_type: 'Test',
  application: null,
  app_user: null,
  data: {
    id: '',
    content_type: '',
    content: 'Test',
    created_at: '0001-01-01T00:00:00Z'
  } 
}
```
[block:api-header]
{
  "title": "LogicInvocation"
}
[/block]
The `LogicInvocation` event is triggered when after a user sends a message to your application or an inbound event is triggered and the NLP system processes it.

Upon receiving this webhook, it is expected that you send the result of your logic run to the Init.ai API. See the [logic response](#section-logicinvocation-response) section for details on how to compose the request.

## LogicInvocation example body

```json
{
  "event_type": "LogicInvocation",
  "data": {
    "version": "1",
    "format": "plain_object",
    "payload": {
      "deprecation_warning": "WARNING: do not use the fields current_conversation.__private_temp_user_id, execution_data, current_application.external_name, scripts, or common_scripts. These may be removed at any time.",
      "execution_data": {
        "execution_id": "000",
        "execution_mode": "",
        "deprecation_warning": "WARNING: execution_data is deprecated and may be removed at any time. Use invocation_data instead.",
        "initiated_at": "2016-12-20T20:50:57.959667636Z",
        "auth_token": "jwt.token",
        "api": {
          "base_url": "https://api.init.ai"
        },
      },
      "invocation_data": {
        "invocation_id": "invocation_id",
        "initiated_at": "2016-12-20T20:50:57.959667636Z",
        "auth_token": "jwt.token",
        "api": {
          "base_url": "https://api.init.ai"
        }
      },
      "current_conversation": {
        "state": {},
        "id": "",
        "conversation_message_index_to_process": 0,
        "messages": [
          {
            "sender_role": "end-user",
            "sender_relative_id": "",
            "parts": [
              {
                "content_type": "text",
                "content": "hi",
                "classification": {
                  "overall_confidence": 0.99914087988077,
                  "base_type": {
                    "confidence": 0,
                    "value": "greeting"
                  },
                  "sub_type": {
                    "confidence": 0,
                    "value": ""
                  }
                },
                "predicted_next_message": {
                  "overall_confidence": 0.79441527690397,
                  "direction": {
                    "confidence": 0,
                    "value": "output"
                  },
                  "base_type": {
                    "confidence": 0,
                    "value": "welcome"
                  },
                  "sub_type": {
                    "confidence": 0,
                    "value": "request_player_name"
                  },
                  "predicted_response": {
                    "name": "welcome/request_player_name",
                    "auto_fill_capable": true
                  }
                },
                "slots": {}
              }
            ]
          }
        ],
        "__private_temp_user_id": "__private_temp_user_id"
      },
      "users": {
        "user_id": {
          "app_id": "app_id",
          "id": "user_id",
          "platform_user_id": "platform_user_id",
          "remote_id": null,
          "first_name": "joel",
          "last_name": "quenneville",
          "metadata": null,
          "minimum_token_issued_at": 0,
          "created_at": "2016-11-28T14:11:10.126221Z",
          "updated_at": "2016-11-28T14:11:10.126221Z",
          "deleted_at": null
        }
      },
      "current_application": {
        "internal_name": "",
        "id": "app_id",
        "external_name": "",
        "environment": {},
        "outbound_message_templates": {
          "apology/untrained": {
            "variants": [
              {
                "template_slots": [
                  {
                    "plurality": "single",
                    "base_type": "url",
                    "entity": "website",
                    "role": "",
                    "count": 1
                  }
                ]
              },
              {
                "template_slots": [
                  {
                    "plurality": "single",
                    "base_type": "url",
                    "entity": "website",
                    "role": "",
                    "count": 1
                  }
                ]
              }
            ]
          },
          "request/player_stats": {
            "variants": [
              {"template_slots": null},
              {"template_slots": null}
            ]
          },
          "request_player/team": {
            "variants": [
              {
                "template_slots": [
                  {
                    "plurality": "single",
                    "base_type": "string",
                    "entity": "player_team",
                    "role": "",
                    "count": 1
                  }
                ]
              },
              {
                "template_slots": [
                  {
                    "plurality": "single",
                    "base_type": "string",
                    "entity": "player_team",
                    "role": "",
                    "count": 1
                  }
                ]
              }
            ]
          }
        }
      }
    }
  }
}
```

### LogicInvocation response

When using the `LogicInvocation` event, it is up to you to provide the result of your logic run to the Init.ai API. The `result` Object is constructed automatically by the [Node.js SDK](doc:node-js-sdk) client instance so you generally will not need to do this work yourself. However, should you wish to manage your conversation logic/flow on your own, you will need to create a result with the following format.

### Payload Reference

| Key | Type |Description |
| --- | --- | --- |
| invocation | Object | Metadata pertaining to invocation |
| &nbsp;&nbsp;&nbsp;&nbsp;invocation_id | String | Current invocation id |
| &nbsp;&nbsp;&nbsp;&nbsp;app_id | String | The id of the current Init.ai application |
| &nbsp;&nbsp;&nbsp;&nbsp;app\_user\_id | String |ID of the current user |
| result | |
| &nbsp;&nbsp;&nbsp;&nbsp;payload | Object | |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;execution_id | String |Current invocation id |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;conversation_state | Object | The resulting conversation state |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;users_patch | Array | A [JSON Patch](http://jsonpatch.com/) description of the mutations to the `users` object. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;reset_users | Array<String> | A list of user ids to be reset to a default state |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;messages | Array | |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;parts_object | Object | |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;parts | Array<Message response>| A list of message responses. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;content_type | String | The type of message to be sent as one of the following: `image`, `video`, `audio`, `text`, `file`, `event`, `carousel_list`, `postback`, `postback-action`, `prepared-outbound-message` |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;content | String&verbar;Object | The actual message content (this varies based on `content_type`). |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;to | String | The user id of the targeted recipient |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;to_type | String | The only currently valid value is `app_user_id` |

#### Example request

```curl
POST https://api.init.ai/api/v1/remote/logic/invocations/{invocation_id}/result

Headers:
Authorization: Bearer {invocation_auth_token}
Content-type: application/json

POST Body:
{
  "invocation": {
    "invocation_id": "{invocation_id}",
    "app_id": "{app_id}",
    "app_user_id": "{app_user_id}"
  },
  "result": {
    "payload": {
      "execution_id": "{invocation_id}",
      "conversation_state": {},
      "users_patch": [],
      "reset_users": [],
      "messages": [
        {
          "parts": [
            {
              "content_type": "text",
              "content": "local 1",
              "to": "{end_user_id}",
              "to_type": "app_user_id"
            },
            {
              "content_type": "text",
              "content": "local 2",
              "to": "end_user_id",
              "to_type": "app_user_id"
            },
            {
              "content_type": "text",
              "content": "local 3",
              "to": "end_user_id",
              "to_type": "app_user_id"
            }
          ]
        }
      ]
    }
  }
}
```
[block:api-header]
{
  "title": "MessageOutbound"
}
[/block]
The `MessageOutbound` event is triggered when your application sends a message to a user in the conversation.

### MessageOutbound example body

```json
{
  "event_type": "MessageOutbound",
  "application": {
    "id": "app_id",
    "name": "hockey stats",
    "description": "stats",
    "created_at": "2016-11-28T14:10:14.87014Z",
    "updated_at": "2016-12-19T22:54:09.053291Z"
  },
  "app_user": {
    "id": "user_id",
    "remote_id": null,
    "first_name": "artemi",
    "last_name": "panarin",
    "created_at": "2016-11-28T14:11:10.126221Z",
    "updated_at": "2016-11-28T14:11:10.126221Z"
  },
  "data": {
    "id": "message_id",
    "content_type": "text",
    "content": "What's shea weber's plus-minus?",
    "created_at": "2016-12-20T20:50:42.708617Z"
  }
}
```