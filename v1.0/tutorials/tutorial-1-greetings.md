---
title: "Tutorial 1: Greetings"
excerpt: "We are going to start simple and create a conversational app that understands how to respond to hello and goodbye."
---
[block:api-header]
{
  "type": "basic",
  "title": "1. Connect to GitHub"
}
[/block]
If you haven't already, head to the [Init.ai console](https://console.init.ai/) and connect your GitHub account.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/3ecadae-connect-gh.png",
        "connect-gh.png",
        1709,
        1064,
        "#fbfbfb"
      ]
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "We use GitHub because everything is versioned and nice and neat in one place. Learn more about [Working with Git repos](doc:working-with-git-repos)"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "2. Create a New Project"
}
[/block]
Create a new project by clicking `New Project`. Add a project name and a quick description (this will populate the GitHub repo and README.md).
[block:callout]
{
  "type": "info",
  "body": "When you create your first project we ask to upgrade GitHub authorization to give us access to public repos. Our projects are backed by a Git repo with a specific directory format. This repo holds all of your logic and training data.",
  "title": "GitHub public repo access"
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/24faf9a-create-project-view.png",
        "create-project-view.png",
        1933,
        1064,
        "#455b9e"
      ]
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
To make changes to your project, you need to **clone your project repository** and **prepare your local development environment**. You can find the clone URL under the first Getting Started step in your project overview.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/049d381-clone-install.gif",
        "clone-install.gif",
        780,
        420,
        "#e8eaef"
      ]
    }
  ]
}
[/block]
After you've cloned that repo `cd` into its directory and run:

```
npm install
npm start
```
This will install the project dependencies and run the [`initai-dev-server`](doc:dev-server) task which allows our training interface to save training data to your projects directory.
[block:api-header]
{
  "type": "basic",
  "title": "4. Add Training Data"
}
[/block]
Now It is time to teach your application about the world as it pertains to your project. It already understands the basics of English. Therefore, we only need to teach based on what is specific to the project you are creating.

Ensure the [Dev Server](doc:dev-server) is running, *there is a status light in the bottom left of the training interface to indicate if it is connected*, and open up the **training interface** by clicking the button in the top right of your console and select the ‘**Teach**’ tab.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/64f96a2-teach-empty.png",
        "teach-empty.png",
        597,
        1062,
        "#fbfbfb"
      ],
      "caption": "Empty Teach interface"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "danger",
  "body": "If the [Dev Server](doc:dev-server) is disconnected this tab, along with some other features, will be inactive."
}
[/block]
Next, type **Hi**  into the text input and hit return. This tells the Natural Language Processing system to processes the message and annotate it with any data it understands about that message.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/925e827-user-message-breakdown.png",
        "user-message-breakdown.png",
        808,
        300,
        "#e6e4db"
      ]
    }
  ]
}
[/block]
In this case, it should return a "**Classification**" of greeting. The classification is represented as text underneath the message content and is what the NLP interprets as the meaning of the message.

Within the Teach tab, you are in control of any input and output messages. **This means you will be responding for the application anytime you are in Teach**. For every "response" you create, Init will store that as a **template** for responses.

Templates are a very powerful feature of the Init.ai platform and we'll dive deeper into that in a different guide.
[block:callout]
{
  "type": "info",
  "body": "Learn more about templates in the [Classifications & Templates](doc:classifications-templates#section-templates) guide"
}
[/block]
### Creating a response

To create a response we use a slash command `/r` to indicate if a message should be a response. Any time you enter `/r` in the Teach text input it will indicate that the message direction is outbound (from the system). Enter this into the text field:
```md
/r Hello world, I mean human
```
You'll notice that the Icon changed to a little robot to indicate the **message direction** is now a response.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/94958ca-teach-with-input-output.png",
        "teach-with-input-output.png",
        600,
        1058,
        "#fafbfb"
      ]
    }
  ]
}
[/block]
Let's take it just a little further. Add another input from the user:
```
Goodbye
```
and one more response:
```
/r See you later!
```
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ca78b2f-teach-with-turns.png",
        "teach-with-turns.png",
        600,
        1058,
        "#fafbfb"
      ],
      "caption": "It should look something like this!"
    }
  ]
}
[/block]
Now that we have a sample conversation let's be sure to save this file! Up at the top of the Teach view enter a title for this conversation, `greeting_01`, and hit save.
[block:callout]
{
  "type": "warning",
  "title": "Saving conversation files",
  "body": "It's important to note that the NLP system uses **full conversations** as training data so making separate files for each conversation is very important!"
}
[/block]
After you've saved that conversation file, **commit, and push it to GitHub**. This will send a signal to our system to use that data to start training your language model.
[block:callout]
{
  "type": "warning",
  "body": "Pushing new training data is **required** before the system can recognize the new data. Any time you create training data, you'll need to push it to initiate a training session!",
  "title": "Pushing new training data"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "5. Conversation Flow"
}
[/block]
The Client API is tightly integrated with the NLP and allows you to you customize the conversation Flow, and the state of the conversation programmatically.

Head to your project directory on your local file system. Navigate to `behavior/scripts/index.js` and open this file in your favorite code editor.

The client API lets us assign specific functions (we call them [Steps](doc:conversation-flow#section-steps)) to messages based on their classifications.

## Handle Greeting

To start, let's focus on `greeting`. In `behavior/scripts/index.js` add a `greeting` handler to the classifications key in the `client.runFlow` method. Next add a greeting **stream** key inside the `streams` object with a placeholder function called `handleGreeting`:
[block:code]
{
  "codes": [
    {
      "code": "client.runFlow({\n  classifications: {\n    // Add a greeting handler with a reference to the greeting stream\n    greeting: 'greeting'\n  },\n  streams: {\n    // Add a Stream for greetings and assign it a Step\n    greeting: handleGreeting,\n    main: 'onboarding',\n    onboarding: [sayHello],\n    end: [untrained]\n  }\n})",
      "language": "javascript",
      "name": "behavior/scripts/index.js"
    }
  ]
}
[/block]
Now that you have defined a **Stream** for `greeting` classifications, it's time to declare the **Step**. Above the `client.runFlow` call, add a new **Step**:
[block:code]
{
  "codes": [
    {
      "code": "const handleGreeting = client.createStep({\n  satisfied() {\n    return false\n  },\n\n  prompt() {\n    client.addResponse('greeting')\n    client.done()\n  }\n})",
      "language": "javascript",
      "name": "behavior/scripts/index.js"
    }
  ]
}
[/block]
Notice we return `false` from the `satisfied` method; this ensures `prompt` will be called. In `prompt`, we call two methods. First, we call `client.addResponse` and provide the string `greeting`. This is a reference to the `greeting` response template based on the training data you provided earlier. Second, we call `client.done` which signals that you have finished processing this message and that it is time to send a reply!

## Handle Goodbye 

Now, let's handle the `goodbye` classification we trained earlier. The process is very similar to `greeting` except that we are now writing our logic against messages classified as `goodbye`. Amend your `client.runFlow` method to account for `goodbye`.
[block:code]
{
  "codes": [
    {
      "code": "client.runFlow({\n  classifications: {\n    goodbye: 'goodbye',\n    greeting: 'greeting',\n  },\n  streams: {\n    // Add a Stream for goodbye and assign it a Step\n    goodbye: handleGoodbye,\n    greeting: handleGreeting,\n    main: 'onboarding',\n    onboarding: [sayHello],\n    end: [untrained]\n  }\n})",
      "language": "javascript",
      "name": "behavior/scripts/index.js"
    }
  ]
}
[/block]
Next, add a **Step** to handle it:
[block:code]
{
  "codes": [
    {
      "code": "const handleGoodbye = client.createStep({\n  satisfied() {\n    return false\n  },\n\n  prompt() {\n    client.addResponse('goodbye')\n    client.done()\n  }\n})",
      "language": "javascript",
      "name": "behavior/scripts/index.js"
    }
  ]
}
[/block]
At this point your `index.js` file should look like this:
[block:code]
{
  "codes": [
    {
      "code": "'use strict'\n\nexports.handle = function handle(client) {\n  const sayHello = client.createStep({\n    satisfied() {\n      return Boolean(client.getConversationState().helloSent)\n    },\n\n    prompt() {\n      client.addResponse('welcome')\n      client.addResponse('provide/documentation', {\n        documentation_link: 'http://docs.init.ai',\n      })\n      client.addResponse('provide/instructions')\n      client.updateConversationState({\n        helloSent: true\n      })\n      client.done()\n    }\n  })\n\n  const untrained = client.createStep({\n    satisfied() {\n      return false\n    },\n\n    prompt() {\n      client.addResponse('apology/untrained')\n     client.done()\n    }\n  })\n\n  const handleGreeting = client.createStep({\n    satisfied() {\n      return false\n    },\n\n    prompt() {\n      client.addResponse('greeting')\n      client.done()\n    }\n  })\n\n  const handleGoodbye = client.createStep({\n    satisfied() {\n      return false\n    },\n\n    prompt() {\n      client.addResponse('goodbye')\n     client.done()\n    }\n  })\n\n  client.runFlow({\n    classifications: {\n      goodbye: 'goodbye',\n      greeting: 'greeting'\n    },\n    streams: {\n      goodbye: handleGoodbye,\n      greeting: handleGreeting,\n      main: 'onboarding',\n      onboarding: [sayHello],\n      end: [untrained]\n    }\n  })\n}",
      "language": "javascript",
      "name": "behavior/scripts/index.js"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "6. Test"
}
[/block]
Now we can see if it's responding as we expect. Make sure you've still got the [Dev Server](doc:dev-server) running and head over to the **Chat** tab within the training interface.

At the bottom of the screen you locate and enable the toggle that says **Local testing** (If you don't see this, make sure you start the Dev Server!). This will output NLP results, and logic results (including errors) in your terminal allowing you to debug and iterate more quickly.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/2505c0c-toggle-local-testing.png",
        "toggle-local-testing.png",
        595,
        305,
        "#bfbfbf"
      ]
    }
  ]
}
[/block]
With **Local testing** enabled, Type **Hello**  into the text input and hit enter. You should see a response from that says "Hello world, I mean human". Next enter **Goodbye* and watch as it responds with "See you later!"

### Debugging

If for some reason, you do not receive the expected response, head back to your terminal where the Dev Server is running and check for any errors in the JavaScript. If you are not running the Dev Server in **Local Testing** mode, head to the Logs section of the console and find that conversation to investigate potential issues.