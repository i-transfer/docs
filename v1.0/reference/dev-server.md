---
title: "Dev Server"
excerpt: "The Dev Server provides training server and toolkit for developing Projects on the Init.ai platform."
---
> 
[block:callout]
{
  "type": "info",
  "body": "The `Dev Server` was formerly known as the `CLI`. If you are currently using the legacy `initai-cli` package, please consider upgrading."
}
[/block]
View this project on [Github](https://github.com/init-ai/initai-dev-server).

# Installation

When a project is created, the `package.json` will contain the Dev Server in its dependencies. All you will need to do is run `npm install` to fetch this dependency.

## Node.js Version

Make sure you have Node.js installed. We recommend using version [`4.3.2`](https://nodejs.org/en/download/releases/) (see below). If you are using a tool such as [`nvm`](https://github.com/creationix/nvm) simply run:

```bash
$ nvm install 4.3.2
$ nvm use 4.3.2
```

This recommendation is made due to end ensure you are running the same version of Node that is available in [AWS Lambda](http://docs.aws.amazon.com/lambda/latest/dg/current-supported-versions.html). While the developer tools do not _currently_ interact with or represent Lambda, upcoming features will require this version.

# Local Bridge

The Dev Server acts as a bridge between your production console and your local file system. This will start a server on your local machine that can communicate with the console running in your browser. It does this by facilitating a websocket connection between the two allowing changes to conversation data in the browser to be persisted locally via CML files.

To run the server, make sure you are in the directory of your working project (`cd /path/to/my-project`) and run the `server` command:

```bash
$ npm start
```
[block:callout]
{
  "type": "info",
  "title": "You must run the Dev Server on the same machine as your browser",
  "body": "The Dev Server communicates over a local websocket connection. As such, it is required to be on the same machine as your browser."
}
[/block]
You will notice the console will reflect successful connection status immediately. You can of course, at any time, kill this server and the browser will no longer have access to any locally running codebase.

# Local Testing

When the Dev Server is running from within an Init.ai project, you can toggle "Local Testing" mode from the Conversation Trainer in your browser.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9b021e4-toggle-local-testing.png",
        "toggle-local-testing.png",
        595,
        305,
        "#bfbfbf"
      ]
    }
  ]
}
[/block]
This will now run the JavaScript for your project on your local machine allowing you to handle errors, debug issues, and log out any data you may need.

It is important to note that if you stop the Dev Server task, your script executions will once again run against the most recently deployed code in the Init.ai system and not be available for debugging via your terminal.

# Localhost and HTTPS

Because the console itself is served over `https`, any attempts to address a non-https (or non-wss) service on `localhost` will fail. The Dev Server is configured to run over `https` and uses the [`wss`](https://devcenter.heroku.com/articles/websocket-security#wss) protocol to facilitate the connection between your browser and your Init.ai project on your local file system. Since this server is indeed running at your discretion on your machine, the proper certificates and keys are bundled with the CLI.

Our service leverages the [**localhost.daplie.com**](https://github.com/Daplie/localhost.daplie.com-certificates) library to provide the appropriate configurations for this environment. When the server is spun up, it is accessible via [https://localhost.daplie.com:8443](https://localhost.daplie.com:8443). This is the address the console attempts to connect with.

# Switching Projects

When switching between projects in the console, you must hop back to your terminal, `cd` into the new project directory and restart the Dev Server from that directory.

# Supported Platforms

* Mac OS X 10.10+
* Windows 8.1+
* Linux

# Upgrading from "CLI"

The Dev Server was previously shipped as part the `initai-cli`. To simplify per-project training, the CLI was deprecated in favor of this new standalone package. To use the Dev Server in one of your projects, install the package and add a `start` task to your `package.json`.

**Install the dev server**

```bash
$ npm install --save initai-dev-server
```

**Your `package.json` should end up looking something like this:**

```json
{
  "name": "my cool init.ai project",
  "version": "0.0.0",
  "description": "A conversational masterpiece",
  "main": "behavior/scripts/index.js",
  "author": "username <user@name.com>",
  "license": "UNLICENSED",
  "scripts": {
    "start": "initai-dev-server"
  },
  "dependencies": {
    "initai-dev-server": "^0.4.3",
    "initai-node": "^0.0.6"
  }
}
```

Now, instead of needing a global installation of the `initai-cli` and having to run `iai watch`, you can simply run `npm start` from your project directory.