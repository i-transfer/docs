---
title: "Headers"
excerpt: ""
---
Requests against the API are expected to contain `Accept`, `Authorization` and `Content-Type` headers. Currently all endpoints use JSON to send and receive data.

```
Accept: application/json
Authorization: Bearer <TOKEN>
Content-Type: application/json
```