---
title: "List messages"
excerpt: "Get messages in a user's current conversation."
---
# Message properties

## `content` vs `text`

The `content` of the message is the raw content and may include metadata or structured data about images, buttons, files, etc.

The `text` is the text of the message as it appears in the conversation.

If the `content_type` is `text`, then `content` will be equal to `text`. For other values of `content_type`, `content` will be JSON and `text` will be human readable or empty.