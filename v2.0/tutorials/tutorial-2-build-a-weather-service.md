---
title: "Tutorial 2: Build a weather service"
excerpt: "In this tutorial, you're going to build your own personal weather service. It's going to be able to tell you simple information about weather in various places and remember the last one you asked about."
---
## 1. Create a new project

Make sure you are logged in and go ahead and [**create a new project**](https://console.init.ai/#/create-project). Add a project name and a quick description.


![Create project](https://d2mxuefqeaa7sj.cloudfront.net/s_CED46A285ECD99B1A6C72E7C8340268CE494323EA900C315CD09582BAA0AF413_1493137327515_file.png)



## 2. Add language data

Once your project creation is completed click the ‘**Language**’ link in the sidebar.

**Create sample conversations**
From here we want to add a new **Training Conversation.** Click the **Create Conversation** button to launch the editor. 


![CML Breakdown](https://d2mxuefqeaa7sj.cloudfront.net/s_CED46A285ECD99B1A6C72E7C8340268CE494323EA900C315CD09582BAA0AF413_1493138702668_file.png)


For the first conversation let’s use this data (go ahead and copy + paste it into your editor) then name this conversation **get-weather-01** and hit **Save**

**get-weather-01**

    user> What's the weather in [Seattle](city)?
    * request_weather
    
    service> [heavy rain](condition) / [53](temperature) in [Seattle](city)
    * provide_weather

Then **Save** and create a new document. Ideally we have a number of different conversations that represent these intents. Generally, the more data you have, the more accurate your model will be. 

Variations on similar conversations help the language model understand more complex sentences and conversations. We will add a few more conversations that use the same intent flow to help teach our language model.

Create a new Training Conversation by clicking the ‘new’ button and add this data.

**get-weather-02**

    user> Please let me know what it's like in [Los Angeles](city) right now
    * request_weather
    
    service> It looks like [haze](condition) and [72](temperature) degrees in
    [Los Angeles](city)
    * provide_weather

Notice that the intent of the user message is the same (asking for the weather in a city), but we’re providing a different city (**Los Angeles** in this case) and the response is also slightly different, but contains the same types of **entities** (a **city**, a **temperature**, and a **condition**).


> **Entities** are keywords in your messages that your language model needs to understand. They are used to help extract additional meaning from messages, and also to help populate dynamic data inside messages. In this tutorial we will create three entities `city` , `temperature`, and `condition`.
> 
> 📚 If you want to dive deeper check out our [Entity types and slot parsing guide](https://docs.init.ai/docs/entity-types-and-slot-parsing)

By simply providing the dataset with a variation on a particular response, we will programmatically pick the response based on the matching entities, and populate with dynamic data (more on this later).

We should provide at least **3** more conversations to round it out at 5. You can either create them yourself, or use the examples below.

**get-weather-03**

    user> Weather [New york city](city)
    * request_weather
    
    service> I'm seeing [clear sky](condition) and [48](temperature) degrees in
    [New york city](city)
    * provide_weather

**get-weather-04**

    user> Tell me the weather in [Chicago](city)
    * request_weather
    
    service> It looks like [broken clouds](condition) and [60](temperature)
    degrees  in [Chicago](city)
    * provide_weather

**get-weather-05**

    user> [Devner](city) weather
    * request_weather
    
    service> I'm seeing [sleet](condition) and [31](temperature) degrees in
    [Denver](city)
    * provide_weather

**A little onboarding**
The samples above will handle the bulk of the interactions, but what happens when a user first begins interacting? We should provide some very simple onboarding that prompts the user to enter a city after they say hello.

**onboarding-01**

    user> Hello
    * greeting
    
    service> Hey there, I'm your weather bot. What city do you need the weather
    in?
    * onboarding_prompt_city
    
    user> [San Francisco](city)
    * provide_city
    
    service> [fog](condition) / [53](temperature) in [San Francisco](city)
    * provide_weather

So, we’ve got a greeting, which is followed by an `onboarding_pro``m``pt_city` to prompt the user to give us a city and we’ve classified that message as `provide_city` . Next you’ll see that we’re using the same `provide_weather` intent we used above. Since we want the response to be consistent and the only data we need to fulfill that request is `city` we can use the same `intent` . Pretty cool, huh?

And, as above let’s add a few additional training conversations to make this flow a little stronger. Feel free to use these examples:

**onboarding-02**

    user> Hi
    * greeting
    
    service> Hey there, I'm your weather bot. What city do you need the weather
    in?
    * onboarding_prompt_city
    
    user> [Dallas](city)
    * provide_city
    
    service> [violent storm](condition) / [83](temperature) in [Dallas](city)
    * provide_weather

**onboarding-03**

    user> Howdy
    * greeting
    
    service> Hey there, I'm your weather bot. What city do you need the weather
    in?
    * onboarding_prompt_city
    
    user> [New York City](city)
    * provide_city
    
    service> [windy](condition) / [53](temperature) in [New York](city)
    * provide_weather


## 3. Train your model

Next, we need to update our language model with the **Training Conversations** we’ve got saved. Navigate to **Language > Models** in the sidebar. If this is the first time you’re visiting it, it’ll probably be empty 😄. Otherwise it will display all of your current and previous models.


> **Note**: this can take up to 10 minutes depending on the size of your dataset
## 4. Set up a webhook

Once we have some training data we need to give it some instructions on how it should behave. To do this, we need to wire up logic using the Init.ai JavaScript SDK.

**Quickstart**
We recommend using our [sample-logic-server](https://github.com/init-ai/sample-logic-server) if you’re just getting started. It bootstraps a webhook server that sends messages to Init.ai and can be used to deploy to services like Heroku and Zeit. Follow the steps to set up your local environment quick.


1. [Clone the sample logic server repo](https://github.com/init-ai/sample-logic-server)

```
git clone git@github.com:init-ai/sample-logic-server.git
```

> **Note:** Make sure you’ve got **Node v6.10** or **later** installed ([Download](https://nodejs.org/en/download/))


2. Install dependencies and start development task:

```
npm i && npm run
```

You should see this in your terminal:

![Terminal](https://d2mxuefqeaa7sj.cloudfront.net/s_15163A83AC5585DF3ACB2C253321A5A367F4967F97496B2043A7F447FEF02F52_1492905153947_82bc3948-2680-11e7-830e-3abd50fd2ab4.gif)

2. The Development URL should have copied to your clipboard. We’ll need to provide this inside the console

**Provide your webhook URL to the Chat interface**
Now that your server is running it connects to a local webhook that we set up. This will allow you to use the Test Chat feature in the console. Simply open chat by clicking the icon in the bottom right corner of the console, then click **Manage Webhooks** and add your development webhook URL to the input.


![Add dev webhook](https://files.readme.io/2499655-add-dev-webhook-7.gif)


This is a necessary step to test this project and allows you to quickly test, and iterate on your conversations in a development environment.

📚 [**Read more about Webhooks →**](https://docs.init.ai/docs/webhooks)


## 5. Conversation Logic

Now we need to add some logic to provide our application with the necessary steps to control the flow of the conversation. There’s a few different things we need to handle in order to get the proper responses and data to users. We will need to:


1. Ensure the user provides a city before providing the weather
2. Save the provided city and make a request to a weather API for that city
3. Dynamically populate responses with data from the weather API

**Set up the flow**
A Flow is the entire set of logic associated with a particular project. It is what runs every time a message is sent to your application, and determines how it should respond. Within the flow there are Streams. Streams are smaller pieces of a flow and combine multiple steps. The `runFlow`  method tells the system *how* it should execute your logic.

📚 [**Read more about conversation flow →**](https://docs.init.ai/docs/conversation-flow)

Open up the **src/runLogic.js** from the sample repo you cloned and replace the contents with this code to get started:

```javascript
// Basic empty scaffold for Logic.js
const InitClient = require('initai-node')
    
module.exports = function runLogic(eventData) {
  return new Promise((resolve) => {
    const client = InitClient.create(eventData, {succeed: resolve})
        
    // Your code will go here
    client.runFlow({
      streams: {
        main: 'getWeather',
        getWeather: [collectCity, provideWeather],
      }
    })
  })
}
```
Take a look at the `client.runFlow` method. This is how we define the **flow** of the conversation. Within this method we provide it an object of `streams`. We’ll dive deeper into this later, but the important piece is the array provided to `getWeather` . These are references to **step** functions we will have to write above.

Let’s create placeholders for the two **just above the `client.runFlow` method**.

```javascript
    // ...
    
    const collectCity = client.createStep({
      satisfied() { return Boolean(client.getConversationState().weatherCity) },
    
      prompt() {
        // Prompt the user for their city
        // We will use the onboarding_prompt_city intent from our training data
        client.addResponse('onboarding_prompt_city')
        client.done()
      },
    })
    
    const provideWeather = client.createStep({
      satisfied() { return false },
    
      prompt() {
        // Once we have the city we want to provide the user the weather
        // We will use the provide_weather intent from our training data
        client.addResponse('provide_weather')
        client.done()
      },
    })
    
    // ...
```

Inside each step is a `satisfied()` method which needs to return a true or false value. This instructs the system if it should run the `prompt` method within a step based on the criteria provided.

The other important piece in here is the `prompt()` . This is where we execute what should be sent back to a user. You’ll notice we have a method in the prompt called `addResponse` which has a string value. This value is actually an intent you used when creating your training conversations earlier.

**Extract City from a message**
We need to extract from the city from a message to provide the right weather report.

In our `collectCity` step, we'll define the special function `extractInfo`. This will be run before the conversation Flow is traversed so that steps may find and storing information in incoming messages.

Let’s also add a console log inside the conditional checking for `city` to see if we’re getting the right data from the message. Modify the `collectCity` step to look like this:

```javascript
    // ...
    
    const collectCity = client.createStep({
      satisfied() { return Boolean(client.getConversationState().weatherCity) },
      
      extractInfo() {
        // use the getFirstEntityWithRole method to exatract city from a message 
        const city = client.getFirstEntityWithRole(client.getMessagePart(), 'city')
        
        // If we have a city, update our conversation state
        if (city) {
          client.updateConversationState({
            weatherCity: city,
          })
          
          console.log('Grab the weather for: ', city)
        }
      },
      
      prompt() {
        // Prompt the user for their city
        // We will use the onboarding_prompt_city intent from our training data
        client.addResponse('onboarding_prompt_city')
        client.done()
      },
    })
    
    // ...
```

When a message is processed, the `extractInfo` function on each step will be called. The function we just defined will take the value of `city` and will store it on the **conversation state** with the key `weatherCity`. Note that the value in the slot is not just a string, it is a JavaScript object containing both the raw incoming text and a processed text value.

[**📚 Read more about conversation state →**](https://docs.init.ai/v1.0/docs/key-terms-and-concepts#section-conversation-state)

Now, try sending a message through (via the Chat interface), something like "What's the weather in Denver?” and check your terminal. If everything’s working you should see a log line inside the terminal `Grab the weather in: Denver` 😄


> **Note:** Make sure your model has finished training first, otherwise it won’t know what you’re talking about!

**Mock weather data**
Before fetch the weather from a real API, let's send back some fake weather data to make sure everything is wired up properly. To do this, we will add a `weatherData`  object with three keys and provide that object as a second argument to `client.addResponse()` . 

These keys match exactly what we named our entities in the training data (`[60](temperature) and [Sunny](condition) in [Chicago](city)`) and will be used to dynamically populate the response.

Update `provideWeather` so it looks like this:

```javascript
    // ...
    
    const provideWeather = client.createStep({
      satisfied() { return false },
      
      prompt() {
        const weatherData = {
          temperature: 60,
          condition: 'sunny',
          city: client.getConversationState().weatherCity.value,
        }
        
        client.addResponse('provide_weather', weatherData)
        client.done()
      },
    })
    
    // ...
```

Save, and ask for the weather in a city in Chat. If all went well you should see a response with our fake data.

## 4. Fetch weather data

Now, we want to get some actual weather data. We are going to use [OpenWeatherMap.org's API](http://openweathermap.org/api) to get weather information. It is free for testing, and unlike other weather APIs, it combines location lookup and fetching weather into one API call.

Head to [https://home.openweathermap.org/users/sign_up](https://home.openweathermap.org/users/sign_up) to sign up for a free developer account and get an API key.

**Making the request**
If you’re using the reference repo from above we’ve already got [Axios](https://www.npmjs.com/package/axios) installed and we can use that to make a `GET` request to the weather API for some weather data. If you’re not, go ahead and install using your favorite package manager.

Make a new file called `fetchWeather.js`. Then copy and paste this into it:

```javascript
    const axios = require('axios')
    
    module.exports = function fetchWeather(locationName) {
      // Be sure to replace this with the API key from your openweathermap account
      // https://home.openweathermap.org/api_keys
      const appId = '<YOUR_OPENWEATHER_APP_ID>'
      const requestUrl = `http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=${appId}&q=${locationName}`
    
      return axios.get(requestUrl)
    }
```

> Remember to add your API Key from openweather.org!

Now back in `run``Logic.js` , let's require this module at the top of the file:


    const fetchWeather = require('./fetchWeather')
    
    // ...

Finally, we update the `provideWeather` prompt to populate the response with real weather data from our `fetchWeather`  function. Since we’re returning a promise from `fetchWeather` we can wrap the body of `prompt` in the resolution and catch the error after.

```javascript
    // ...
    const provideWeather = client.createStep({
      satisfied() { return false },
      
      prompt() {
        fetchWeather(client.getConversationState().weatherCity.value).then(({data}) => {
          const {main, name, weather} = result
      
          const weatherData = {
            temperature: Math.round(main.temp),
            condition: weather[0].description,
            city: name,
          }
      
          client.addResponse('provide_weather', weatherData)
          client.done()
        }).catch(error => {console.error('Error fetching: weather', error)})
      }
    })
    // ...
```

## 6. Test!

Now that we’ve brought it all together, it should be responding as expected, let's test things in Console to make sure it’s all working!

**If it’s not working make sure:**

1. Your model is done training
2. Your local server is running
3. You’ve got the locally running URL set as a webhook in Chat
4. There are no errors in your Terminal


## Completed Code

**src/runLogic.js**

```javascript
    const InitClient = require('initai-node')
    const fetchWeather = require('./fetchWeather')
    
    module.exports = function runLogic(eventData) {
      return new Promise((resolve) => {
        const client = InitClient.create(eventData, {succeed: resolve})
    
        const collectCity = client.createStep({
          satisfied() { return Boolean(client.getConversationState().weatherCity) },
    
          extractInfo() {
            // use the getFirstEntityWithRole method to exatract city from a message
            const city = client.getFirstEntityWithRole(client.getMessagePart(), 'city')
    
            // If we have a city, update our conversation state
            if (city) {
              client.updateConversationState({ weatherCity: city })
            }
          },
    
          prompt() {
            // Prompt the user for their city
            // Using onboarding_prompt_city intent from our training data
            client.addResponse('onboarding_prompt_city')
            client.done()
          },
        })
    
        const provideWeather = client.createStep({
          satisfied() { return false },
    
          prompt() {
            // Fetch the weather using the cit we extracted from the message
            fetchWeather(client.getConversationState().weatherCity.value).then(({data}) => {
              const {main, name, weather} = data
    
              // Create the weatherData object
              const weatherData = {
                temperature: Math.round(main.temp),
                condition: weather[0].description,
                city: name,
              }
    
              // Pass weatherData to the provide_weather response
              client.addResponse('provide_weather', weatherData)
              client.done()
            }).catch(error => {console.error('Error fetching: weather', error)})
          },
        })
    
        // Execute the flow
        client.runFlow({
          streams: {
            main: 'getWeather',
            getWeather: [collectCity, provideWeather],
          }
        })
      })
    }
```

**fetchWeather.js**

```javascript
    const axios = require('axios')
    
    module.exports = function fetchWeather(locationName) {
      // Be sure to replace this with the API key from your openweathermap account
      // https://home.openweathermap.org/api_keys
      const appId = '<YOUR_OPENWEATHER_APP_ID>'
      const requestUrl = `http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=${appId}&q=${locationName}`
    
      return axios.get(requestUrl)
    }
```

## What's Next

Now that you can manage conversation state, send messages, extract slots, and contact APIs, you have most of tools needed to create powerful conversational apps!