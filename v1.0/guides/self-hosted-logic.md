---
title: "Self Hosted Logic"
excerpt: ""
---
By default, your Init.ai project is scaffolded with a JavaScript file to manage your [Conversation Logic](https://docs.init.ai/docs/adding-logic). Behind the scenes, an [AWS Lambda](https://aws.amazon.com/lambda/) function is provisioned for your project and each time a user sends a message to your conversation this function is invoked. There are however cases where you may wish to host this  yourself. This is often the case when you would like to use specific version of Node.js, integrate into your own build pipeline, or generally have control over your own code execution.

If you feel like diving right in, the full code for this example can be found [here](https://github.com/init-ai/self-hosted-logic-demo).

# Getting started

To get started, you will need a webhook server and you will need to have it accessible to the web (meaning not running via `localhost`). The following tutorial uses [restify](http://restify.com/) and includes steps for deploying your server to [Heroku](https://heroku.com). You may use any framework and hosting service you like, however, Init.ai only provides a client for Node.js to manage conversation logic.

# Setup a webhook server

## Install dependencies

Start by installing the [restify](https://www.npmjs.com/package/restify) package from npm.

```
npm i -S restify
```

**Create the server**

Next, create a file called `server.js` in the root of your Init.ai project.

> **Note:** In order to preserve the Local Testing functionality provided by the [Dev Server](https://docs.init.ai/docs/dev-server), this server must be written within your Init.ai project scaffold.

```javascript
const restify = require('restify')

const server = restify.createServer()
const PORT = process.env.PORT || 4044

server.use(restify.bodyParser())

/**
 * Add a POST request handler for webhook invocations
 */
server.post('/', (req, res, next) => {
  const eventType = req.body.event_type
  const eventData = req.body.data

  // Handle the LogicInvocation event type
  if (eventType === 'LogicInvocation') {
    // This is the payload you will need to provide the Init.ai Node client
    console.log(eventData.payload)
  }

  // Immediately return a 200 to acknowledge receipt of the Webhook
  res.send(200)
})

/**
 * Start the server on the configured port (default is 4044)
 */
server.listen(PORT, () => {
  console.log('%s listening at %s', server.name, server.url)
})
```

You can start this server by running `node server.js`. It will run on port 4044 by default and exposes the root route (`/`) to accept `POST` requests.

The request handler is setuo to check the body payload for the `event_type`, in this case, `LogicInvocation`.

**Setup InitClient**

Now that your server is setup, you will need to invoke your logic handler – the file in `behavior/scripts/index.js`.

First, import the [initai-node](https://www.npmjs.com/package/initai-node) module into `server.js`. At the top of the file, add the `InitClient` declaration:

```javascript
const restify = require('restify')
const InitClient = require('initai-node')

...
```

You will also need to import a reference to your logic script handler. Below the `InitClient` declaration, add the following:

```javascript
const projectLogicScript = require('./behavior/scripts')
```

Next, inside of your handler, instantiate the `InitClient` and invoke your logic handler:

```javascript
...

  if (eventType === 'LogicInvocation') {
    const initNodeClient = InitClient.create(eventData, {
      succeed(result) {
        sendLogicResult(eventData.payload, result)
      }
    })

    projectLogicScript.handle(initNodeClient)
  }

...
``` 

You will notice we are passing two arguments to create the `InitClient` instance. The event data payload and an object with a `succeed` method on it. This `succeed` method is invoked under the hood as a result of calling `client.done()` in your logic.

**Configure logic response**

Once your code has processed the logic, you need to send the result back to the Init.ai API. The invocation payload provides an API base url as well as a [JWT Token](https://jwt.io) to authenticate the request. You can use those to compose your API request and provide the logic result in the body. Inside of `succeed` we call `sendLogicResult` – let's go ahead and write that now.

```javascript
/**
* Send the result of the logic invoation to Init.ai
**/
function sendLogicResult(invocationPayload, result) {
  const invocationData = invocationPayload.invocation_data
  const client = restify.createClient({url: invocationData.api.base_url})

  const requestConfig = {
    headers: {
      'accept': 'application/json',
      'authorization': `Bearer ${invocationData.auth_token}`,
      'content-type': 'application/json',
    },
    method: 'POST',
    path: `/api/v1/remote/logic/invocations/${invocationData.invocation_id}/result`,
  }

  const resultPayload = {
    invocation: {
      invocation_id: invocationData.invocation_id,
      app_id: invocationPayload.current_application.id,
      app_user_id: Object.keys(invocationPayload.users)[0],
    },
    result: result,
  }

  client.post(requestConfig, (err, req) => {
    if (err) {
      console.error(err)
    }

    req.on('result', (err, res) => {
      res.body = ''
      res.setEncoding('utf8')

      res.on('data', (chunk) => {
        res.body += chunk
      })

      res.on('end', () => {
        console.log(`Result sent successfully`, res.body)
      })
    })

    req.write(JSON.stringify(resultPayload))
    req.end()
  })
}
```

# Deploy to Heroku

In order for Init.ai to route logic invocations to your server via a webhook, it will need to be accessible to the outside world. We will walk through the steps of deploying your project to Heroku.

It is recommended that you first familiarize yourself with deploying Node.js applications on Heroku before continuing. Read [Getting started on Heorku with Node.js](https://devcenter.heroku.com/articles/getting-started-with-nodejs#introduction).

**Add a Procfile**

In the root of your project, add a file called `Procfile` with the following content:

```
web: node server.js
```

Make sure to add and commit the `Procfile` to your repository.

**Setup Heroku app**

With the `Procfile` ready to go, create a Heroku app:

1. Install the Heroku toolbelt (see link above)
1. Run `heroku login`
1. Run `heroku create`
1. Run `git push heroku master`

This will generate a URL pointing to your web server on Heroku. You will need this URL to configure your Messages Webhook in the Init.ai console.

## Configure webhook in console

Once you have a service running to handle a LogicInvocation webhook, you will need to configure your Init.ai project to route logic to it. In the console, navigate to **Settings > Webhooks > Messages** and add your webhook URL.

<img width="1320" alt="webhook-setup" src="https://cloud.githubusercontent.com/assets/1217116/21461833/f653ea22-c91a-11e6-9a40-149e5037a0ba.png">

**Enable LogicInvocation**

You must enable the `LogicInvocation` event to bypass the AWS Lamba execution of your Conversation Logic in favor of this endpoint.

Local testing will still behave as documented – meaning your webhook will not be invoked when in [Local Testing]() mode.

<img width="1292" alt="webhook-logic-bypass" src="https://cloud.githubusercontent.com/assets/1217116/21461832/f6538500-c91a-11e6-88e2-f83b45b1bbe3.png">

## Try it out

Now that you have your server running and have configured your webhook – head to Chat in your console and start interacting with your service. You should see responses generated by the logic deployed to your webhook server.

If you are using Heroku, from within the project directory, you can run:

```
heroku logs --tail
```

This will allow you to monitor your requests and inspect any errors.

# Disabling

You may decide you want to resume sending logic invocations to AWS Lambda via Init.ai internally – in that case you have a few options:

1. Uncheck the `LogicInvocation` event setting in the webhook configuration in the console (this will still call your webhook for outbound messages)
1. Disable the webhook in the console UI
1. Remove the Webhook entirely from the UI (you will need to re-configure our webhook from scratch after removing it)

# Code

The full code for this tutorial is available [here](https://github.com/init-ai/self-hosted-logic-demo).