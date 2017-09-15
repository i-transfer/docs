---
title: "Authentication"
excerpt: ""
---
The Remote API requires that each request contain your application's **API Token** (previously known as a *Remote Token*). This token allows external services to manage users and conversations on behalf of your Init.ai application.

To obtain your Remote Token, visit the console and navigate to the settings of your current project and copy the provided token.
[block:callout]
{
  "type": "info",
  "title": "JWT Tokens",
  "body": "JWTs or JSON Web Tokens are signed tokens used to authenticate requests between services.\nSee https://jwt.io/ for more info.\n\nSee [Tokens and Authentication](doc:tokens-and-authentication) for more information and details on how to generate your own."
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/1160643-Screenshot_2017-04-24_14.26.16.png",
        "Screenshot 2017-04-24 14.26.16.png",
        1459,
        560,
        "#33353f"
      ]
    }
  ]
}
[/block]
If a request contains an invalid token or is missing the `Authorization` header, a `401` response will be returned.