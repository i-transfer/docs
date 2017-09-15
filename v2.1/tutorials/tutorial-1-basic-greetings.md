---
title: "Tutorial 1: Basic greetings"
excerpt: ""
---
## 1. Create a new project

Make sure you are logged in and go ahead and [**create a new project**](https://console.init.ai/#/create-project). Add a project name and a quick description.


![Create project](https://d2mxuefqeaa7sj.cloudfront.net/s_CED46A285ECD99B1A6C72E7C8340268CE494323EA900C315CD09582BAA0AF413_1493137327515_file.png)

## 2. Add language data

Once your project creation is completed click the ‘**Language**’ link in the sidebar.

**Create sample conversations**
From here we want to add a new **Training Conversation.** Click the **Create Conversation** button to launch the editor. 

For the first conversation let’s use this data (go ahead and copy + paste it into your editor) then name this conversation **greetings-01** and hit **Save.**

**greetings-01**

    user> Hey
    * greeting
    
    service> Hello world! I mean, Human
    * greeting
    
    user> Goodbye
    * goodbye
    
    service> See you later
    * goodbye

**Add a few more training conversations**
The key to good language understanding is data, generally the more good data you have, the smarter and more accurate your model will be. Go ahead and create a few other Training Conversations (1 or 2 more should be fine) that differ slightly from the example above. Feel free to use these examples:

**greetings-02**

    user> Hello
    * greeting
    
    service> Hello world! Wait you're Human
    * greeting
    
    user> Peace
    * goodbye
    
    service> Hasta la vista
    * goodbye

**greetings-03**

    user> Hi
    * greeting
    
    service> Howdy world!
    * greeting
    
    user> Bye
    * goodbye
    
    service> Until next time
    * goodbye

**What the data means**
Training data is broken down into a few key concepts. In these examples we’ve written **inbound messages** (from the user), **outbound messages** (from the system), and **intents**. Each message is followed directly by an **intent** (`* greeting`)which describes the meaning of that particular message.

We will use these **intents** as hooks to control how the conversation is executed in the following section.


## 3. Train your model

Next, we need to update our language model with the **Training Conversations** we’ve got saved. Navigate to **Language > Models** in the sidebar. If this is the first time you’re visiting it, it’ll probably be empty 😄. Otherwise it will display all of your current and previous models.


> **Note**: this can take up to 10 minutes depending on the size of your dataset
## 4. Set up a webhook

Once we have some training data we need to give it some instructions on how it should behave. To do this, we need to wire up logic using the Init.ai JavaScript SDK.

**Quickstart**
We recommend using our [sample-logic-server](https://github.com/init-ai/sample-logic-server) if you’re just getting started. It bootstraps a webhook server that sends messages to Init.ai and can be used to deploy to services like Heroku and Zeit. Follow the steps to set up your local environment quick.

[Clone this repo](https://github.com/init-ai/sample-logic-server)

```
 git clone git@github.com:init-ai/sample-logic-server.git
```

***Note:*** *Make sure you’ve got* ***Node v6.10*** *or* ***later*** *installed (*[*Download*](https://nodejs.org/en/download/)*)*

Install dependencies and start development task:

```
npm i && npm run dev
```

You should see this in your terminal:

![Terminal](https://d2mxuefqeaa7sj.cloudfront.net/s_15163A83AC5585DF3ACB2C253321A5A367F4967F97496B2043A7F447FEF02F52_1492905153947_82bc3948-2680-11e7-830e-3abd50fd2ab4.gif)


The Development URL should have copied to your clipboard. We’ll need to provide this inside the console

**Provide your webhook URL to the Chat interface**
Now that your server is running it connects to a local webhook that we set up. This will allow you to use the Test Chat feature in the console. Simply open chat by clicking the icon in the bottom right corner of the console, then click **Manage Webhooks** and add your development webhook URL to the input.


![Add dev webhook](https://files.readme.io/2499655-add-dev-webhook-7.gif)


This is a necessary step to test this project and allows you to quickly test, and iterate on your conversations in a development environment.

📚 [**Read more about Webhooks →**](https://docs.init.ai/docs/webhooks)


## Conversation Logic

Now that we’ve got that set up let’s jump into the fun stuff. We only have two intents that we want to focus on here (greeting, goodbye) and we want something to happen when a users messages the system with a greeting. Open up the **src/runLogic.js** from the demo repo you cloned and replace the contents with this code:
[block:code]
{
  "codes": [
    {
      "code": "// Code example for basic greeting\nconst InitClient = require('initai-node')\n\n// Code example for basic greeting\nconst runLogic = function(eventData) {\n  return new Promise((resolve, reject) => {\n    const client = InitClient.create(eventData)\n    const done = () => client.sendResult().then(resolve).catch(reject)\n\n    // Create a new 'step' function to handle a 'greeting' intent\n    const handleGreeting = client.createStep({\n      satisfied() {\n        return false\n      },\n\n      prompt() {\n        // Tell the system to respond with a saved 'greeting' response\n        // This is generated from saved messages with 'greeting' intents\n        client.addResponse('greeting')\n        done()\n      },\n    })\n\n    // Create a new 'step' function to handle a 'goodbye' intent\n    const handleGoodbye = client.createStep({\n      satisfied() {\n        return false\n      },\n\n      prompt() {\n        // Tell the system to respond with a saved 'goodbye' response\n        // This is generated from saved messages with 'goodbye' intents\n        client.addResponse('goodbye')\n        done()\n      },\n    })\n\n    client.runFlow({\n      // create a stream object with a reference to our step functions\n      streams: {\n        goodbye: handleGoodbye,\n        greeting: handleGreeting,\n      },\n      // Assign stream references to our classifications (intents)\n      classifications: {\n        goodbye: 'goodbye',\n        greeting: 'greeting',\n      },\n    })\n  })\n}\n\nmodule.exports = runLogic",
      "language": "javascript",
      "name": "src/runLogic.js"
    }
  ]
}
[/block]
📚 [**Read more about Conversation Logic and the Node.js SDK →**](https://docs.init.ai/v2.0/docs/node-js-sdk)

If your **Language Model** is done training and you’ve connected your local webhook to the Chat feature, you should be able to send a greeting and get a response!

**If it’s not working make sure:**

1. Your model is done training
2. Your local server is running
3. You’ve got the locally running URL set as a webhook in Chat
4. There are no errors in your Terminal

If it’s still not working, send us a message [support@init.ai](mailto:support@init.ai) and we’ll give you a hand!


## What’s next?

Nice work! You have just configured your project to respond conditionally based on training conversations and intents! The next tutorial will walk you through how to create a basic conversational Weather service. We will dive into intermediate features of the client API, add data gathering steps, and make calls to a third party weather API to populate responses.