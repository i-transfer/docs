---
title: "Quickstart"
excerpt: "Here's how to get started really quick!"
---
We'll walk through some super basics of Init.ai. In this guide we'll get your account created, and setup your developer environment.
[block:api-header]
{
  "type": "basic",
  "title": "1. Connect to GitHub"
}
[/block]
First, if you haven't already. Head to the [Console](https://console.init.ai) and create your account (or log in if you've already created one)
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/49fa530-connect-gh.png",
        "connect-gh.png",
        1709,
        1064,
        "#fbfbfb"
      ],
      "caption": "Our login page"
    }
  ]
}
[/block]
You'll be asked to authorize your GitHub account and grant us access to use GitHub to log in to Init.ai.
[block:api-header]
{
  "type": "basic",
  "title": "2. Create a New Project"
}
[/block]
Create a new project by clicking New Project. Add a project name and a quick description (this will populate the GH repo and README.md). In the background, we are provisioning a new repo and pushing them to your GitHub.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/71edf53-create-project-view.png",
        "create-project-view.png",
        1933,
        1064,
        "#455b9e"
      ],
      "sizing": "full",
      "caption": "Project creation screen"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "3. Environment Setup"
}
[/block]
Next you'll need to set up your development environment. This is a critical component because we rely on tight integration with the code you write and our Natural Language Processing system. So, in order to make changes to your project (including training data and code), you need to:

1. Clone your project repository (Located within the first getting started stem in your project overview)
2. `cd` into your project directory
3. Run `npm install`
4. Run `npm start`
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/e069a96-clone-install.gif",
        "clone-install.gif",
        780,
        420,
        "#e8eaef"
      ],
      "caption": "Finding and copying your"
    }
  ]
}
[/block]
`npm start` kicks off the [Dev Server](doc:dev-server) which communicates with our training interface that you'll be using to create training data. Don't worry, we'll get into what more of this means in some tutorials!