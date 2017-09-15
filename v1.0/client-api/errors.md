---
title: "Errors"
excerpt: ""
---
The Init.ai API uses common HTTP status codes to denote errors.
[block:parameters]
{
  "data": {
    "h-0": "Status Code",
    "h-1": "Description/Cause",
    "0-0": "`400`",
    "h-2": "Example",
    "0-1": "Invalid data in request body.",
    "0-2": "",
    "1-0": "`401`",
    "1-1": "`Authorization` header is missing or provided token is invalid.",
    "1-2": "",
    "2-0": "`404`",
    "2-1": "Resource is not found",
    "2-2": "```\n{\n  \"message\":\"Not found or not authorized\",\n  \"code\":404\n}\n```",
    "3-0": "`5xx`",
    "3-1": "A failure occurred within the Init.ai system"
  },
  "cols": 2,
  "rows": 4
}
[/block]