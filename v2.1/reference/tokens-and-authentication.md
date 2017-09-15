---
title: "Tokens and Authentication"
excerpt: ""
---
The public Init.ai APIs consist of the [Remote API](doc:remote-api) and the [Inbound Events API](doc:inbound-events-api). Both APIs require authentication for all actions.

Init.ai authenticates and authorizes requests by checking the `Authorization` token in incoming HTTP requests. The header value must be of the form `Bearer <TOKEN>`. This document describes how to retrieve or generate the TOKEN value.

Init.ai use JWT tokens which securely embed identifying information using a cryptographic hash. For background information on JWT, check out [jwt.io](https://jwt.io/) which contains information about the specification and links to libraries to make working with JWTs easier in various programming languages.

There are two ways to get a valid JWT:
[block:api-header]
{
  "title": "1. Retrieve a JWT from the Console"
}
[/block]
Access the Settings page of your project in the Console and copy the value provided. Note that this token never expires.


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/bde0a49-screenshot_token_copying.png",
        "screenshot_token_copying.png",
        1488,
        316,
        "#32333d"
      ],
      "caption": "Copy the \"API Token\" value in the Settings page of your project."
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "2. Generate your own JWT"
}
[/block]
JWT tokens can be generated from a set of JSON data and a 'signing secret'.

Using JWT libraries in the programming language of your choosing, you can generate JWTs periodically or for each request.
[block:callout]
{
  "type": "info",
  "title": "JWT Resources",
  "body": "If you are generating your own token, be sure to check [https://jwt.io/](https://jwt.io/) for useful guides, resources, and links."
}
[/block]
Take the JSON:
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"app_id\": \"<PROJECT_ID>\",\n  \"exp\": <EXPIRES AT TIMESTAMP>,\n  \"iat\": <ISSUED AT TIMESTAMP>,\n  \"iss\": \"platform\",\n  \"type\": \"remote\"\n}",
      "language": "json"
    }
  ]
}
[/block]
1. Substitute your `project_id`, found in the URL of the Console when viewing your project (ie `https://console.init.ai/#/project/<PROJECT_ID>/settings`), for the `<PROJECT_ID>`

2. Substitute the current Unix timestamp in seconds, as an integer with no timezone offset, for the `<ISSUED AT TIMESTAMP>`

3. Substitute the desired expiration time, as an integer Unix timestamp in seconds with no timezone offset, for the `<ISSUED AT TIMESTAMP>`.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/6f00cf6-screenshot_secret_copying.png",
        "screenshot_secret_copying.png",
        1488,
        316,
        "#32333d"
      ],
      "caption": "Copy the \"Token Secret\" value in the Settings page of your project and use it to sign your JWT tokens."
    }
  ]
}
[/block]
4. Sign the resulting JSON using your JWT library and the signing secret in the Settings page of your project in the Console. Use the algorithm `HS256` (`HMACSHA256`) if you have the option.


[block:callout]
{
  "type": "warning",
  "title": "Security tips",
  "body": "The signing secret should be treated as a password and kept secure, along with any currently valid JWT tokens.\n\nNote that providing an expiration is not required but is _highly_ recommended for security reasons."
}
[/block]