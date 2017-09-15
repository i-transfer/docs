---
title: "Overview"
excerpt: ""
---
The public version of the Init.ai API is referred to as the **Remote API** and allows external systems to manage users and conversations related to these users on behalf of an Init.ai application. This API follows traditional [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) principles. Data is sent and received as JSON and is served over HTTPS.

The current version of the API lives at **https://api.init.ai/v1** . All endpoints are served relative to this base URL.

## CORS

[Cross-origin resource sharing](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS) – or CORS is not supported at this time. The API is designed to handle server-to-server communication only.