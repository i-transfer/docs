---
title: "Training Settings"
excerpt: "Customize training time versus accuracy"
---
In order to provide quick feedback during design and development of your conversational application, we use a quicker training method by default.

However, if you are preparing to deploy to real users you should enable `performance model training` in the settings page: `https://console.init.ai/#/project/<appID>/settings`.

With this setting enabled, natural language models will take longer to train, but the model may achieve more accurate results.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/f8e42a5-Screen_Shot_2017-05-19_at_5.19.25_PM.png",
        "Screen Shot 2017-05-19 at 5.19.25 PM.png",
        722,
        300,
        "#404248"
      ],
      "sizing": "smart",
      "border": false
    }
  ]
}
[/block]