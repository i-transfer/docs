---
title: "Working with Git repos"
excerpt: ""
---
# The deploy process

Init.ai projects are deployed by pushing to Github.

Github will notify Init.ai of pushes through a webhook. After receiving the webhook, Init.ai will pull the latest commit from `master` and then redeploy your project.

# Requirements

Init.ai requires two things to be properly configured in Github to deploy your project:

- a proper webhook URL set
- access to pull from the Git repository

By default Init.ai will scaffold a public repository when you create an project, providing access, and will set the webhook URL.

# Making a repository private

Init.ai can read from private repositories by using a Deploy Key. A Deploy Key is a special SSH key that provides access to a single repository owned by you or an organization you control. For more information, see Github's documentation about [Deploy Keys](https://developer.github.com/guides/managing-deploy-keys/).

When your project is created, Init.ai will generate and store a Deploy Key specific to that project. You can retrieve the public Deploy Key in the Settings > Repository pane for the project in the Console.