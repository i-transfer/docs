---
title: "Authentication"
excerpt: ""
---
The Remote API requires that each request contain your application's **Remote Token**. This token allows external services to manage users and conversations on behalf of your Init.ai application.

To obtain your Remote Token, visit the console and navigate to the settings of your current project: **Settings > Events > Inbound**  and copy the provided `JWT Token` from the Inbound Events UI.
[block:callout]
{
  "type": "info",
  "title": "The Remote Token is a JWT",
  "body": "JWTs or JSON Web Tokens are signed tokens used to authenticate requests between services.\nSee https://jwt.io/ for more info."
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/d9a1e18-console-inbound-events-jwt.png",
        "console-inbound-events-jwt.png",
        1204,
        489,
        "#2d303b"
      ]
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Why Inbound Events?",
  "body": "Changes are coming to the console to simplify token management. The currently exposed token for the Inbound Events webhook is the same Remote Token that would otherwise be provided."
}
[/block]
If a request contains an invalid token or is missing the `Authorization` header, a `401` response will be returned.