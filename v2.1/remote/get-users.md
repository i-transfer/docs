---
title: "List users"
excerpt: "Get all users"
---
Get a list of all users for the current application
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"users\": [\n    {\n      \"id\": \"c597b21a-50e6-4875-5f27-6c815c49d257\",\n      \"app_id\": \"99883642-fed8-4216-5cf9-474f834c9fc1\",\n      \"remote_id\": null,\n      \"first_name\": \"\",\n      \"last_name\": \"\",\n      \"minimum_token_issued_at\": 0,\n      \"created_at\": \"2016-12-14T15:22:07.727003Z\",\n      \"updated_at\": \"2016-12-14T15:22:07.727003Z\"\n    }\n  ],\n  \"pagination\": {\n    \"page_number\": 1,\n    \"page_size\": 10,\n    \"overall_page_count\": 1,\n    \"current_page_url\": \"https://api.init.ai/v1/users?page_number=1&page_size=10\",\n    \"first_page_url\": \"https://api.init.ai/v1/users?page_number=1&page_size=10\",\n    \"last_page_url\": \"https://api.init.ai/v1/users?page_number=1&page_size=10\",\n    \"previous_page_url\": \"\",\n    \"next_page_url\": \"\"\n  }\n}",
      "language": "json",
      "name": "Response"
    }
  ]
}
[/block]
Get a list of all users associated with the current project