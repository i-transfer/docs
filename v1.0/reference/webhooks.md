---
title: "Webhooks"
excerpt: ""
---
Webhooks provide a way for you to customize and extend the Init.ai platform to suit your needs. You can use webhooks for collecting custom metrics, monitoring conversations, or processing logic yourself. When an event you are subscribed to is triggered, your webhook will be sent a `POST` request with a `JSON` payload.

Your webhook must use HTTPS and return a `200` response or it will be suspended and will require you to manually re-validate your endpoint via the console.
[block:callout]
{
  "type": "info",
  "body": "To test your webhook server locally, you may use a tool such as [ngrok](https://ngrok.com/) or [localtunnel.me](https://localtunnel.github.io/www/) to expose your local environment to the internet. Be sure to update the Webhook URL to your production environment when you are ready to go live.\n\n***Do not*** rely on a locally running service for production!",
  "title": "Testing webhooks locally"
}
[/block]
# Messages

The Messages webhook will notify you when a message has been sent to one of your users in a conversation. The payload for this event will contain `MessageOutbound` in the `event_type` field.

It can be configured to also notify you when an inbound message from a user has been processed by the Init.ai NLP and is ready for logic to be run. In this scenario, you will need to host your [Conversation Logic](https://docs.init.ai/docs/adding-logic) yourself. You will be expected to send an asynchronous response to the Init.ai API upon completion of the logic run in order to update the conversation state and/or send a reply.

## MessageOutbound

The `MessageOutbound` event is triggered when your application sends a message to a user in the conversation.

### Example payload

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

### Payload reference

| Key | Type | Description |
| --- | --- | --- |
| event_type | String | `MessageOutbound` |
| application | Object | Basic metadata about the Init.ai project. |
| &nbsp;&nbsp;&nbsp;&nbsp;id | String | The Init.ai project id |
| &nbsp;&nbsp;&nbsp;&nbsp;name | String | The Init.ai project name |
| &nbsp;&nbsp;&nbsp;&nbsp;created_at | String | Timestamp of Init.ai project creation |
| &nbsp;&nbsp;&nbsp;&nbsp;updated_at | String | Timestamp of Init.ai project's last update |
| app_user | Object | Basic metadata about the user the message was sent to. |
| &nbsp;&nbsp;&nbsp;&nbsp;id | String | The user's id in the Init.ai database |
| &nbsp;&nbsp;&nbsp;&nbsp;remote_id | String | The user's id in an external system such as a messaging service. |
| &nbsp;&nbsp;&nbsp;&nbsp;created_at | String | Timestamp of the user's record creation |
| &nbsp;&nbsp;&nbsp;&nbsp;updated_at | String | Timestamp of user's last update |
| data | Object | Specific data about the message and its contents. |
| &nbsp;&nbsp;id | String | The id of the current outbound message. |
| &nbsp;&nbsp;content_type | String | A string describing the expected content type. This will be one of the following: `image`, `video`, `audio`, `text`, `file`, `event`, `carousel_list`, `postback`, `postback-action`, `prepared-outbound-message` |
| &nbsp;&nbsp;content | String&verbar;Object | The actual message content (this varies based on `content_type`). |
| &nbsp;&nbsp;created_at | String | The timestamp of the message creation in the Init.ai system |

## LogicInvocation

The `LogicInvocation` event is triggered when after a user sends a message to your application and the Init.ai NLP system processes it. This event is used to allow you to host your Conversation Logic in an environment of your choosing.

Upon receiving this webhook, it is expected that you send the result of your logic run to the Init.ai API. See the [logic response](#logic-reponse) section for details on how to compose the request.

> Learn more about using the **Messages Webhook** to self-host your logic in this [tutorial]().

### Example payload

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

### Payload Reference

| Key | Type | Description |
| --- | --- | --- |
| event_type | String | `LogicInvocation` |
| data | Object | |
| &nbsp;&nbsp;version | String | The verision of the Init.ai API used to generate this webhook |
| &nbsp;&nbsp;format | String | The expected format of the payload. This will always be `plain_object`. |
| &nbsp;&nbsp;payload | Object | An object representing the current inbound message or event. |
| &nbsp;&nbsp;&nbsp;&nbsp;deprecation_warning | String | Provides a warning regarding keys and structures that may be deprecated. |
| &nbsp;&nbsp;&nbsp;&nbsp;execution_data | Object | Deprecated, use `invocation_data` instead. |
| &nbsp;&nbsp;&nbsp;&nbsp;invocation_data | Object | Metadata regarding the current logic invocation. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;invocation_id | String | The `id` of the current invocation (required for sending a response) |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;initiated_at | String | Timestamp representing the invocation initialization. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;auth_token | String | A temporary [JWT](https://jwt.io) token required to authenticate the subsequent request to transmit the result of logic processing. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;api | Object | Metadata regarding the API for composing the subseqent reply. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;base_url | String | The Init.ai API url for use in composition of subsequent requests. |
| &nbsp;&nbsp;&nbsp;&nbsp;current_conversation | Object | The representation of the current conversation. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;state | Object | An arbitrary object representing state stored during previous logic executions. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;id | String | The id of the current conversation in the Init.ai database. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;conversation_message_index_to_process | Number | This will always default to 0. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;messages | Array | A list of inbound messages from a user that need to be processed by the logic system. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sender_role | String | A string representing the entity which sent the message. This will typically be `end-user` |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sender_relative_id | String |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;parts | Array | A list of individual message parts (generally split by sentence). |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;content_type | String | A string describing the expected content type. See [content types](#content-types) |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;content | String&verbar;Object | The actual message content (this varies based on `content_type`). |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;classification | Object | The determined intent of the message part. See [Classifications](https://docs.init.ai/docs/classifications-templates#section-overview) for more. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;overall_confidence | Number | A value, 0-1 denoting the accuracy of the intent classification. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;base_type | Object | The general category of the intent. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;confidence | Number | A value, 0-1 denoting the accuracy of the base type. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;value | String | The raw string value of the base type. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sub_type | Object | The variant of the general category for this intent. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;confidence | Number | A value, 0-1 denoting the accuracy of the sub type. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;value | String | The raw string value of the sub type. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;slots | Object | The slotted tokens representing entities in the current message part. This object is keyed by entity base types. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;entity | String | The raw string value of the entity base type. Learn more about [Entities](https://docs.init.ai/docs/classifications-templates#section-entities). |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;base_type | String | One of the following: `string` (default), `number`, `currency`, `time`, `duration`, `ordinal`, `temperature`, `distance`, `volume`, `amount-of-money`, `url`, `phone-number` |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;roles | Array | An array of strings representing keys on the values_by_role object. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;values_by_role | Object | An object, keyed on entity roles containing data specific to each slot. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;value | String | Value denoting the slotted token in the message. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;raw_value | String | The raw value denoting the slotted token in the message. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;parsed | null&verbar;Object | This value will be `null` for entities of a base type of `string`. For other base types, see [this reference](https://docs.init.ai/docs/entity-types-and-slot-parsing) for expected values. |
| &nbsp;&nbsp;&nbsp;&nbsp;users | Object | The representation of the users participating in this conversation. This object is keyed by user ids. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;app_id | String | The Init.ai project id. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;id | String | The user id in the Init.ai database. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;platform_user_id | String | The platform user id in the Init.ai database. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;remote_id | String | The user's id in an external system such as a messaging service. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;first_name | String | The user's first name |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;last_name | String | The user's last name |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;metadata | Object | _Default: `null`_. An arbitrary object which you can use to persist specific user data. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;minimum_token_issued_at | Number | _Default: 0_. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;created_at | String | Timestamp of the user's record creation |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;updated_at | String | Timestamp of user's last update |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;deleted_at | String | _Default: `null`_. Timestamp of user's deletion |

### Logic response

When using the `LogicInvocation` event, it is up to you to provide the result of your logic run to the Init.ai API. The `result` Object is constructed automatically by the [InitNode client](https://docs.init.ai/docs/client-api) so you generally will not need to do this work yourself. However, should you wish to manage your conversation logic/flow on your own, you will need to create a result with the following format.

#### Payload Reference

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

```
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