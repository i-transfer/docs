---
title: "Tutorial 2: Basic weather"
excerpt: ""
---
In this tutorial, you're going to build your own personal weather app.

It's going to be able to tell you simple information about weather in various places and remember the last one you asked about.

By the end it should handle simple interactions like this:

```
User: What's the weather in New York right now?
App: It's currently 82 degrees and sunny in New York
```
[block:callout]
{
  "type": "info",
  "title": "Dig into the code",
  "body": "If you want to play around with the code you can create a new project using this code. Simply select the `Example: basic weather` template in the [create project](https://console.init.ai/#/create-project) screen!"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "1. Connect to GitHub"
}
[/block]
First, Connect your GitHub account. We use this to log you in, and when you create your first project we will provision a scaffold for your application. This will contain all of the pertinent pieces for your project, including training data, and conversation Flow scripts.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/89d9aa0-connect-gh.png",
        "connect-gh.png",
        1709,
        1064,
        "#fbfbfb"
      ]
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "2. Create a New Project"
}
[/block]
Create a new project by clicking New Project. Fill out the fields (they will populate the GitHub repo and README.md). When you hit **Create Project** we are creating a new GitHub repo on your behalf with a specific directory structure used for Init.ai projects.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/50ef6ba-create-project-view.png",
        "create-project-view.png",
        1933,
        1064,
        "#455b9e"
      ]
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "GitHub permissions",
  "body": "You may be prompted to upgrade your GitHub privileges to give Init.ai access to public repos."
}
[/block]
You can name your project whatever you like but we'll be referring to ours as **Umbrella**.
[block:api-header]
{
  "type": "basic",
  "title": "3. Environment Setup"
}
[/block]
To make changes to your project, you need to clone your project repository and prepare your local development environment. You can find the clone URL under the first Getting Started step in your project overview.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/315052e-clone-install.gif",
        "clone-install.gif",
        780,
        420,
        "#e8eaef"
      ]
    }
  ]
}
[/block]
After you've cloned that repo `cd` into it's directory and run

```
npm install; npm start
```

This will install the project dependencies and run the `initai-dev-server` start task which allows our training interface to save training data to your repository directory.
[block:api-header]
{
  "type": "basic",
  "title": "4. Add Training Data"
}
[/block]
Right now, your **language model** only knows very basic parts of a conversation, like hello, goodbye, thanks, yes, no. It also knows about various ways to say each of those. But we need to expand its knowledge to **weather**. To do that, we need to provide examples of conversations it might have about the weather. This data will be used to train a new language model based on those examples.

With the Dev Server running from the previous step, navigate to the Teach interface in the Console. Open the training interface by clicking the icon in the top right corner of the console and navigate to the **Teach** tab. This is where we will create the initial training data for the language model.

## Adding sample conversations to training data

First, let's create some two-message conversations where the app provides the current temperature. We will enter the messages into the Teach interface, update the classification of each message, then annotate the [entity](doc:conversation-markup-language#section-slots-entities) within each message.

For each human message, annotate the city [entity](doc:conversation-markup-language#section-slots-entities) and add a message [classification](doc:conversation-markup-language#classification). For the app's message, annotate both the temperature and the city slots, as well as the message classification. We will call human message classification `ask_current_weather/temperature` and the app's message `provide_weather/current`.

Tip: If you need to understand any of the terms used in this document, take a look at the [key terms and concepts](../basics/key_terms_and_concepts.md) reference.

### Conversation 1 - temperature

    Human: What is the temperature in New York?
    App: It is 72 degrees in New York right now.

To add the human message, just type the text into the text field at the bottom of Teach, like "What is the temperature in New York?" and press Enter.

> **Note:** You'll have a chance to be creative soon. For now, we recommend entering the text exactly so that the images correspond with the text in this tutorial.

Entering the message from the application is slightly different. When you enter the text, begin with a slash command `/r` and then the message. So your input should look like this: `/r It is 72 degrees in New York right now.` Press `Enter` to send the message. The `/r` tells the UI that the message is from the application.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/04e9e7a-console_csi_respond_umbrella.png",
        "console_csi_respond_umbrella.png",
        600,
        244,
        "#f9fafa"
      ]
    }
  ]
}
[/block]
> **Note:** The circles to the left of the message's sender. The little emoji avatar means the message is from the user (human) while robot avatar corresponds to a message from the app. **If you accidentally enter a message with the wrong sender, click the circle to change the message's direction.**

When you type each message, Init.ai will classify the text into the closest classification it knows about by default. At this point though, the Umbrella app does not know about weather yet, so the classifications will be incorrect. These incorrect classifications appear below the message text, such as `greeting/human` in the image.

To update the message classifications, click the incorrect classification name below each message. A dialog will appear where you can select a different classification or create a new one by entering the [base type](doc:key-terms-and-concepts#section-base-type) and [sub type](doc:key-terms-and-concepts#section-sub-type).
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/041fe82-console_csi_classify_message_umbrella_human.png",
        "console_csi_classify_message_umbrella_human.png",
        600,
        662,
        "#5a77e5"
      ]
    }
  ]
}
[/block]
- For the human message, the base type is `ask_current_weather`, and the sub type is `temperature`.
- For the app message, the base type is `provide_weather` and the sub type is `current`.

To annotate [slots](doc:key-terms-and-concepts#section-slots), click and drag your mouse over the words to include in the slots. Creating a classification for slots works similarly to messages.

Highlight both words in the phrase "New York" in the human message. A dialog will appear to select the phrase's entity type, but that list will be empty since we have not created any entities yet. Click `Create New`.

This slot is a string, and it represents a city. Keep the [base type](doc:key-terms-and-concepts#section-base-type) of `string` and enter `city` in the field marked 'Value' in the entity creation dialog, then `Save` the new entity. 'Value' is the entity's sub type.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/08a7c72-console_csi_slot_city.png",
        "console_csi_slot_city.png",
        600,
        692,
        "#5977e4"
      ]
    }
  ]
}
[/block]
Also click and drag over "New York" in the application message. This time, you will be able to select the newly created `city` entity from the pre-populated dialog.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/db689f2-console_csi_slot_city_2.png",
        "console_csi_slot_city_2.png",
        600,
        622,
        "#f7f8f8"
      ]
    }
  ]
}
[/block]
Repeat the slot tagging for "72", marking it as a `string` with type `temperature`. There is a base type for `temperature` already, but we will discuss non-string entities in a later tutorial, so let's stick with the classification `string/temperature`.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/bf8c5df-console_csi_slot_temperature.png",
        "console_csi_slot_temperature.png",
        600,
        622,
        "#5879e8"
      ]
    }
  ]
}
[/block]
After entering each message and annotating the slots and classifications, be sure to save the conversation with a unique name, like "**check_current_temperature_01**".

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/262ec58-console_csi_save_convo_umbrella_1.png",
        "console_csi_save_convo_umbrella_1.png",
        600,
        1058,
        "#fafbfb"
      ]
    }
  ]
}
[/block]
After you press `Save` in the UI, take a look at your application's local Git repository. You will see a new file, named "check_current_temperature_01.md" in the conversations directory. The Teach UI has communicated with the [Dev Server](doc:dev-server) and saved the file for you. If you'd like to see how the annotations get translated into [Conversation Markup Language](doc:conversation-markup-language) (CML for short), Init.ai's variant of Markdown, open the file in a text editor. CML is easily readable and editable and you are free to work on the files directly in conjunction with the use of the Conversation Trainer in your browser.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/14b28d1-console_csi_cli_one_file_changed.png",
        "console_csi_cli_one_file_changed.png",
        575,
        161,
        "#2c2c34"
      ],
      "caption": "The Console told the Conversation Training Server to create a CML file in your local repo"
    }
  ]
}
[/block]
#### Conversation 2 - temperature

Now we will provide another example conversation. Init.ai uses the different training examples to help understand different ways of saying the same things. You don't need to list every possible way of saying the same thing. Init.ai will take the examples you provide and extrapolate to messages it has never seen before.

In our second conversation, we again have the user asking for the temperature in a city and the application providing an answer.

```
Human: tell me the temperature in Chicago
App: It is currently 60 degrees in Chicago
```
Tag the human message as `ask_current_weather/temperature` and the application message as `provide_weather/current`. As with the previous conversation, "**Chicago**" should be tagged `city` and "**60**" should be tagged `temperature`.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/d582b87-console_csi_umbrella_convo_2.png",
        "console_csi_umbrella_convo_2.png",
        600,
        254,
        "#f9f9fa"
      ]
    }
  ]
}
[/block]
After entering each message and annotating the slots and classifications, be sure to save the conversation with a unique name, like "**check_current_temperature_02**".

The generated CML should look like this:

```
tell me the temperature in [Chicago](city)
* ask_current_weather/temperature

< It is currently [60](temperature) degrees in [Chicago](city)
* provide_weather/current
```

### Conversation 3 - temperature

Similarly, use the Teach interface to create this conversation

```
Human: please let me know how warm it is in Los Angeles right now
App: In Los Angeles it is currently 85 degrees
```

yielding the CML

```
please let me know how warm it is in [Los Angeles](city) right now
* ask_current_weather/temperature

< In [Los Angeles](city) it is currently [85](temperature) degrees
* provide_weather/current
```

### Conversation 4 - temperature

Now exercise your creativity and make another conversation where a person asks the current temperature in a city of your choice and the app provides the current temperature and repeats the city name.

## Conversations with weather conditions

### Conversation 5 - weather

For the fifth conversation, we'll change things up a little bit. Let's have the user ask the weather instead of just the temperature. In response, the application will provide the conditions along with the temperature.

This time, create the conversations using the [Conversation Markup Language](doc:conversation-markup-language) format directly. You may use whatever text editor you would like. Writing CML directly is a great alternative to using the Teach interface when you need to make bulk changes or annotate conversations from other sources.

You can copy and paste this CML:

```
tell me the weather in [Chicago](city)
* ask_current_weather/conditions

< It is currently [60](temperature) degrees and [partly cloudy](condition) in [Chicago](city)
* provide_weather/current
```

into a file and save it "**check_current_weather_01.md**".

### Conversation 6 - weather

Similarly, copy

```
[Seattle](city) weather
* ask_current_weather/conditions

< It is [65](temperature) degrees right now and [rainy](condition) in [Seattle](city)
* provide_weather/current
```
and save it as "**check_current_weather_02.md"**.

## Conversations asking for the city

For good measure, let's add two more conversations asking the user for the city they want the weather for.

These conversations will show Umbrella how should ask for a city if none is provided by the user.

Each of these conversations will be only two messages long, starting with a greeting. You can copy and paste the CML, and name the files something like "prompt_city_01.md".

### Conversation 7 - asking for city

Save this as `prompt_city_01.md`:

```
Hi
* greeting

< What city would you like the weather for?
* prompt/weather_city

[Seattle](city) please
* ask_current_weather/conditions
```

### Conversation 8 - asking for city

Save this as `prompt_city_02.md`:

```
hey there
* greeting

< Please tell me the city you would like the weather for
* prompt/weather_city

[san francisco](city)
* ask_current_weather/conditions
```

More examples of conversations in CML can be found in your repository's `/conversations/example` directory. Those are the conversations we use to supply responses when you first create your app. You can delete these before training your app.

### Start training your language model

Now that you have ten example conversations, let's send them to Init.ai to start training your app's language model.

Add the new conversations to a Git commit and push the commit to GitHub.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9db2f7d-tutorial_2_first_push_cli_fewer.png",
        "tutorial_2_first_push_cli_fewer.png",
        567,
        276,
        "#dcd1c3"
      ]
    }
  ]
}
[/block]
In the Console, you'll see that Init.ai has received notification of the push and has started a new deploy. Since you updated your app's training data, this deploy will involved training a new language model. This can take up to ~6 minutes but is getting faster over time.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/26c0a4e-console_dsi_model_training_stated.png",
        "console_dsi_model_training_stated.png",
        1716,
        512,
        "#e5eaeb"
      ]
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "If there was an error in the Markdown, your deploy may fail. The Console has a deploy status page which will give you some information to help troubleshoot the issue."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "5. Conversation Flow"
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "During development, while using the [Dev Server](doc:dev-server) in [\"Local Testing\"](doc:dev-server#section-local-testing) mode, your code will execute on your local machine. Otherwise, your JavaScript will be executed in an [AWS Lambda](https://aws.amazon.com/lambda/) function provisioned on your behalf."
}
[/block]
While the language model is training, let's start making Umbrella a bit smarter but filling in its business logic.

Our example conversations represented a user interacting with Umbrella to accomplish a very specific goal: checking the current weather.

Init.ai's Client API models parts of a conversation where the user is trying to accomplish a single goal with the concept of a Stream. A Stream is a series of sequential Steps that can collect or provide information for a user.

Let's create a **Stream** representing checking and providing weather.

In the Flow in our index.js `handle` function, add a new value to the Flow so that it looks something like:
[block:code]
{
  "codes": [
    {
      "code": "// ...\n\nclient.runFlow({\n  classifications: {},\n  streams: {\n    main: 'hi',\n    hi: [sayHello],\n    getWeather: [collectCity, provideWeather],\n  }\n})\n\n// ...\n",
      "language": "javascript",
      "name": "behavior/scripts/index.js"
    }
  ]
}
[/block]
This new Stream, called `getWeather` will contain two steps: making sure Umbrella knows the city the user wants the weather for and then actually providing the weather conditions to the user.

Note that `collectCity` and `provideWeather` are each JavaScript objects. They reference Steps which we haven't created yet. Let's create placeholders for these steps so that our JavaScript will not error when run.

Since these objects reference the `client` object, they should be defined within the scope of the `handle` function in index.js.
[block:code]
{
  "codes": [
    {
      "code": "// ...\n\nconst collectCity = client.createStep({\n  satisfied() {\n    return Boolean(client.getConversationState().weatherCity)\n  },\n\n  prompt() {\n    // Need to prompt user for city    \n    console.log('Need to ask user for city')\n    client.done()\n  },\n})\n\nconst provideWeather = client.createStep({\n  satisfied() {\n    return false\n  },\n\n  prompt() {\n    // Need to provide weather\n    client.done()\n  },\n})\n\n// ...",
      "language": "javascript",
      "name": null
    }
  ]
}
[/block]
Now all objects are defined, but incoming messages will not route to this new Stream.

Let's modify the Flow so that the user reaches `getWeather` Stream by default. We do this by assigning it to the special `main` Stream, which the Client API uses as a starting point when traversing the conversation structure.
[block:code]
{
  "codes": [
    {
      "code": "// ...\n\nclient.runFlow({\n  classifications: {},\n  streams: {\n    main: 'getWeather',\n    hi: [sayHello],\n    getWeather: [collectCity, provideWeather],\n  }\n})\n\n// ...",
      "language": "javascript",
      "name": "behavior/scripts/index.js"
    }
  ]
}
[/block]
If we save these changes and push them, our app will now run through the `getWeather` Stream, reach the unsatisfied `collectCity` Step, and then do nothing. An unsatisfied Step is a Step whose `satisfied` function returns `false`, indicating that the Step has not fulfilled its purpose and would like to interact with the user. Try doing this using the Chat interface within the Console after committing and pushing these changes. Your ongoing model training will not be affected.

Your app will not respond, but if you check the Logs for this conversation, you will see the line "Need to ask user for city" in the output.
[block:callout]
{
  "type": "info",
  "body": "When you are chatting with your application, you can use the slash command `/reset` to clear the current conversation and start fresh."
}
[/block]
## Determine the city the user is referring to

Our goal is to provide the weather in a specific city. To do this, we're going to need a few pieces of information:

- The city the user wants the weather for
- The weather in that city, from a weather API
- Whether the user wants the temperature and conditions or just the temperature
- The time the user would like the weather for

As a first step, let's extract from the conversation the city that the user wants the weather for.

In our `collectCity` step, we'll define the special function `extractInfo`. This will be run before the conversation Flow is traversed so that steps may find and storing information in incoming messages.

Modify the `collectCity` step to look like this:
[block:code]
{
  "codes": [
    {
      "code": "const collectCity = client.createStep({\n  satisfied() {\n    return Boolean(client.getConversationState().weatherCity)\n  },\n\n  extractInfo() {\n    const city = client.getFirstEntityWithRole(client.getMessagePart(), 'city')\n\n    if (city) {\n      client.updateConversationState({\n        weatherCity: city,\n      })\n\n      console.log('User wants the weather in:', city.value)\n    }\n  },\n\n  prompt() {\n    client.addResponse('prompt/weather_city')\n    client.done()\n  },\n})",
      "language": "javascript",
      "name": "behavior/scripts/index.js"
    }
  ]
}
[/block]
Now when a message is processed, the `extractInfo` function on each step will be called. The function we just defined will take the value of the city slot and will store on the conversation state the value of the slot, with the key `weatherCity`. Note that the value in the slot is not just a string, it is a JavaScript object containing both the raw incoming text and a processed text value.

In this step, we used the method `client.getFirstEntityWithRole()`. This method extracts data from a `slots` object that looks roughly like this:

```js
slots: {
  'city': {
    ...
    values_by_role: {
      'generic': [
      {
          raw_value: 'Philadelphia',
          ...
        }
      ]
    }
  }
}
```

After these two code changes, commit and push your code.

If your language model has finished training (check the Deploys section of the console), try saying 'hello'. Your message does not provide a city, so when this step is reached no city will be extracted and `satisfied` will return false. This will cause the `prompt` function to be run, sending a message of type `prompt/weather_city` to the user. You should see output asking you for the city in Chat.

Init.ai is taking examples of `prompt/weather_city` outbound messages from your application's training data, finding ones that represent the same type of data your logic code is trying to send, and then forming an appropriate message text. Think of it like a templating system.

Now try asking the weather in a city. Say something like "What's the weather in Denver?". Your language model should be flexible enough to understand you if you ask the question slightly differently than in your example conversations, but for testing at this point just use one of the human-sent messages from the first batch of training conversations where you ask the temperature in a city.

Again, Umbrella will not output anything yet, but if you review the logs you should see the output of this log line:

```js
console.log('User wants the weather in:', city.value)
```

## Give the user fake weather

Before we take the time to fetch the weather from a real API, let's have Umbrella send back some fake weather data. To do this, we'll update the `provideWeather` step.
[block:code]
{
  "codes": [
    {
      "code": "const provideWeather = client.createStep({\n  satisfied() {\n    return false\n  },\n\n  prompt() {\n    let weatherData = {\n      temperature: 60,\n      condition: 'sunny',\n      city: client.getConversationState().weatherCity.value,\n    }\n\n    client.addResponse('provide_weather/current', weatherData)\n    client.done()\n  }\n})",
      "language": "javascript",
      "name": "behavior/scripts/index.js"
    }
  ]
}
[/block]
Note the new lines we added to `prompt`.

First, we're assembling the data we want to tell the user, in this case a temperature value, a weather condition, and a city.

Second, we send a message to the user. Note that we don't actually specify the text we want to send. Instead we provide the type of message we want to send along with the data. Init.ai takes the message type and matches the provided data with ways sending it, as specified in the app's training data, and picks an appropriate text to send.

Init.ai matches the slots in the example messages from the application's training data to the data you wish to provide. Because of this matching, make sure the keys in the object you provide to `addResponse` match the types of the slots in the example application messages in your application's training data.
[block:api-header]
{
  "type": "basic",
  "title": "6. Fetch Weather Data"
}
[/block]
We are going to use [OpenWeatherMap.org's API](http://openweathermap.org/api) to fetch weather information. It is free for testing, and unlike other weather APIs, it combines location lookup and fetching weather into one API call.

Head to [https://home.openweathermap.org/users/sign_up](https://home.openweathermap.org/users/sign_up) to sign up for a free developer account and get an API key.

The API is very straightforward. You make a `GET` request to:

```bash
$ http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=<YOUR_APP_ID>&q=<LOCATION>
```

and you will receive a response as JSON.
[block:callout]
{
  "type": "info",
  "body": "For this example, we will use the [request module](https://www.npmjs.com/package/request) from npm. To include this in your project, make sure to install it in your dependencies."
}
[/block]
```bash
$ npm install --save request
```

We will need to modify our `prompt` function to be asynchronous as well. Init.ai will detect the arity of the `prompt` function, and if the function accepts a parameter, Init will pass a callback in the second argument.

It's convenient to place utility code like an API client in a different module, and we'll do that with the OpenWeatherMap API client. Make a file in the `behavior/scripts/lib` directory called `getCurrentWeather.js`. You can copy and paste this code into it and save:
[block:code]
{
  "codes": [
    {
      "code": "'use strict'\n\nconst request = require('request')\n\nmodule.exports = function getCurrentWeather(locationName, next) {\n  const appId = '<YOUR APP ID>'\n\n  const requestUrl = `http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=${appId}&q=${locationName}`\n\n  console.log('Making HTTP GET request to:', requestUrl)\n\n  request(requestUrl, (err, res, body) => {\n    if (err) {\n      throw new Error(err)\n    }\n\n    if (body) {\n      const parsedResult = JSON.parse(body)\n      next(parsedResult)\n    } else {\n      next()\n    }\n  })\n}",
      "language": "text",
      "name": "behavior/scripts/lib/getCurrentWeather.js"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Be sure to replace `<YOUR APP ID>` with the value from the OpenWeatherMap console."
}
[/block]
Now back in `index.js,` let's require this module at the top of the file:
[block:code]
{
  "codes": [
    {
      "code": "'use strict'\n\nconst getCurrentWeather = require('./lib/getCurrentWeather')\n\n// ...",
      "language": "javascript",
      "name": "behavior/scripts/index.js"
    }
  ]
}
[/block]
Finally, we update the `provideWeather` prompt to be asynchronous and use the new `getCurrentWeather` module to get real weather.
[block:code]
{
  "codes": [
    {
      "code": "const provideWeather = client.createStep({\n  satisfied() {\n    return false\n  },\n\n  prompt(callback) {\n    getCurrentWeather(client.getConversationState().weatherCity.value, resultBody => {\n      if (!resultBody || resultBody.cod !== 200) {\n        console.log('Error getting weather.')\n        callback()\n        return\n      }\n\n      const weatherDescription = (\n        resultBody.weather.length > 0 ?\n        resultBody.weather[0].description :\n        null\n      )\n\n      const weatherData = {\n        temperature: resultBody.main.temp,\n        condition: weatherDescription,\n        city: resultBody.name,\n      }\n\n      console.log('sending real weather:', weatherData)\n      client.addResponse('provide_weather/current', weatherData)\n      client.done()\n\n      callback()\n    })\n  }\n})",
      "language": "text"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "7. Test"
}
[/block]
Now that Umbrella is mostly-functional, let's test it some more in the Console. As in previous steps, use the Chat interface to interact with your conversational app as though you are a user.

## Debugging

If something isn't working as expected, head back to your terminal where the [Dev Server](doc:dev-server) is running and check for any errors in the JavaScript. If you are not running the Dev Server in ["Local Testing"](doc:dev-server#section-local-testing) mode, head to the Logs section of the console to investigate potential issues.
[block:api-header]
{
  "type": "basic",
  "title": "8. Deploy"
}
[/block]
Umbrella seems to work! But right now it only lives within the confines of the Init.ai console. That really limits its usefulness. So let's deploy it to a messaging channel.

For this tutorial, we're going to use Telegram. Init.ai supports many other messaging channels, either directly or through Smooch, and also supports SMS and Twilio's IP Messaging.

Inside your project click `Settings > Telegram` and follow the instructions to create a telegram bot and connect it.

Once you've got telegram connected try sending it a message (Try asking it for the weather :smile: )!

## Complete logic source

In case you need it, here are working copies of the JavaScript source for this tutorial.
[block:code]
{
  "codes": [
    {
      "code": "'use strict'\n\nconst getCurrentWeather = require('./lib/getCurrentWeather')\n\nexports.handle = function handle(client) {\n  const sayHello = client.createStep({\n    satisfied() {\n      return Boolean(client.getConversationState().helloSent)\n    },\n\n    prompt() {\n      client.addResponse('welcome')\n      client.addResponse('provide/documentation', {\n        documentation_link: 'http://docs.init.ai',\n      })\n      client.addResponse('provide/instructions')\n      client.updateConversationState({\n        helloSent: true\n      })\n      client.done()\n    }\n  })\n\n  const untrained = client.createStep({\n    satisfied() {\n      return false\n    },\n\n    prompt() {\n      client.addResponse('apology/untrained')\n      client.done()\n    }\n  })\n\n  const collectCity = client.createStep({\n    satisfied() {\n      return Boolean(client.getConversationState().weatherCity)\n    },\n\n    extractInfo() {\n     const city = client.getFirstEntityWithRole(client.getMessagePart(), 'city')\n      if (city) {\n        client.updateConversationState({\n          weatherCity: city,\n        })\n        console.log('User wants the weather in:', city.value)\n      }\n    },\n\n    prompt() {\n      client.addResponse('prompt/weather_city')\n      client.done()\n    },\n  })\n\n  const provideWeather = client.createStep({\n    satisfied() {\n      return false\n    },\n\n    prompt(callback) {\n      getCurrentWeather(client.getConversationState().weatherCity.value, resultBody => {\n        if (!resultBody || resultBody.cod !== 200) {\n          console.log('Error getting weather.')\n          callback()\n          return\n        }\n\n        const weatherDescription = (\n          resultBody.weather.length > 0 ?\n          resultBody.weather[0].description :\n          null\n        )\n\n        const weatherData = {\n          temperature: resultBody.main.temp,\n          condition: weatherDescription,\n          city: resultBody.name,\n        }\n\n        console.log('sending real weather:', weatherData)\n        client.addResponse('provide_weather/current', weatherData)\n        client.done()\n\n        callback()\n      })\n    },\n  })\n\n  client.runFlow({\n    classifications: {},\n    streams: {\n      main: 'getWeather',\n      hi: [sayHello],\n      getWeather: [collectCity, provideWeather],\n    }\n  })\n}",
      "language": "javascript",
      "name": "behavior/scripts/index.js"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "'use strict'\n\nconst request = require('request')\n\nmodule.exports = function getCurrentWeather(locationName, next) {\n  const appId = '<YOUR APP ID>'\n\n  const requestUrl = `http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=${appId}&q=${locationName}`\n\n  console.log('Making HTTP GET request to:', requestUrl)\n\n  request(requestUrl, (err, res, body) => {\n    if (err) {\n      throw new Error(err)\n    }\n\n    if (body) {\n      const parsedResult = JSON.parse(body)\n      next(parsedResult)\n    } else {\n      next()\n    }\n  })\n}",
      "language": "javascript",
      "name": "behavior/scripts/lib/getCurrentWeather.js"
    }
  ]
}
[/block]