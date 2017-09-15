---
title: "Send message"
excerpt: "Post message to conversation"
---
# Content Types

## text

The content of the message to send as a `String`.
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H 'Authorization: Bearer <REMOTE_TOKEN>' \\\n-H 'Content-Type: application/json' \\\n-d '{\n  \"content_type\": \"text\",\n  \"content\": \"Hello!\",\n  \"sender_role\": \"end-user\"\n}' \\\nhttps://api.init.ai/v1/users/{user_id}/conversations/current/messages",
      "language": "curl",
      "name": "Send text to conversation"
    }
  ]
}
[/block]
## image

The description of an image, as an Object to be sent to the conversation
[block:parameters]
{
  "data": {
    "h-0": "Key",
    "h-1": "Type",
    "0-0": "`alternative_text`",
    "0-1": "`String`",
    "h-2": "Value/Description",
    "0-2": "Text to be displayed on the client related to the image",
    "1-0": "`image_url`",
    "1-1": "`String`",
    "1-2": "A fully qualified URL for the image",
    "2-0": "`mime_type`",
    "2-1": "`String`",
    "2-2": "The media type of the image such as `image/jpeg`, `image/png`, `image/gif`"
  },
  "cols": 3,
  "rows": 3
}
[/block]
**Example** 
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H 'Authorization: Bearer <REMOTE_TOKEN>' \\\n-H 'Content-Type: application/json' \\\n-d '{\n  \"content_type\": \"image\",\n  \"content\": {\n    \"alternative_text\": \"test image\",\n    \"image_url\": \"http://test.com/test.png\",\n    \"mime_type\": \"image/png\"\n  },\n  \"sender_role\": \"end-user\"\n}' \\\nhttps://api.init.ai/v1/users/{user_id}/conversations/current/messages",
      "language": "curl",
      "name": "Send image to conversation"
    }
  ]
}
[/block]
## postback-action

Action returned after a user clicks a button or other non-text element in a conversation.
[block:parameters]
{
  "data": {
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Value/Description",
    "0-0": "`text`",
    "0-1": "`String`",
    "0-2": "The raw text relating to the postback event.",
    "1-0": "`data`",
    "1-1": "`String|Object`",
    "1-2": "A String or Object describing the postback event.\n\nThis will be available as the `data` value of the message part `content` when application logic is invoked.",
    "2-0": "`stream`",
    "2-1": "`String`",
    "2-2": "The name of a Stream that will be run in the Init.ai Client SDK conversation flow system.\n\nThis will be available as the `stream` value of the message part `content` when application logic is invoked."
  },
  "cols": 3,
  "rows": 3
}
[/block]
**Example**
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H 'Authorization: Bearer <REMOTE_TOKEN>' \\\n-H 'Content-Type: application/json' \\\n-d '{\n  \"content_type\": \"postback-action\",\n  \"content\": {\n    \"text\": \"Order accepted\",\n    \"data\": {\"orderNumber\": 123, \"status\": \"accepted\"},\n    \"stream\": \"handleCompletedOrder\"\n  },\n  \"sender_role\": \"end-user\"\n}' \\\nhttps://api.init.ai/v1/users/{user_id}/conversations/current/messages",
      "language": "curl",
      "name": "Send postback action to conversation"
    }
  ]
}
[/block]