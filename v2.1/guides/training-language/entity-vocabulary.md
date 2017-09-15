---
title: "Entity Vocabulary"
excerpt: ""
---
Entity vocabularies provide a powerful way to expand the definition of your Entities. At a basic level they allow you to provide specific synonyms to words (or phrases) that our system will parse when it sees it within a message.


## Basics

For example, say we are building something that needs to recognize some variations of New York City. We could create an entry for New York City with the synonyms “The Big Apple”, “New York New York”, or “NYC”. And any time a user sends a message with one of the synonyms, our system (and in turn your logic) will understand they  mean “New York City”.


## Fuzzy Matching

 Additionally, if provided enough entries the system can interpret users’ misspellings of synonyms. For example, if a user typed in “The Bug Apple” instead the system would likely understand what the intended city was.


## How to use

Within one of your projects in the [Console](https://console.init.ai), navigate to **Language › Entities** and click on an entity in the list. Here you’ll see some details about an entity, including how many times it is used, and what intents it is associated with, along with an interface for adding new vocabulary.

![Entity detail view](https://d2mxuefqeaa7sj.cloudfront.net/s_6BDE9714827FE71E4B45D68BF5A2D87C678C29CC495EDF0565D36C8DA76C19FA_1494293925247_file.png)

1. Create an entry
![Create and entry](https://d2mxuefqeaa7sj.cloudfront.net/s_6BDE9714827FE71E4B45D68BF5A2D87C678C29CC495EDF0565D36C8DA76C19FA_1494294174988_add-entry.gif)

2. Add synonyms
![Add synonyms](https://d2mxuefqeaa7sj.cloudfront.net/s_6BDE9714827FE71E4B45D68BF5A2D87C678C29CC495EDF0565D36C8DA76C19FA_1494294116438_add-synonyms.gif)


The resulting NLP result when the system sees “The Big Apple” will look like this:

```json
    {
      "raw_value": "The Big Apple",
      "value": "New York City",
      "canonicalized": {
        "canonical_value": "New York City",
        "closest_synonym": "The Big Apple",
        "score": 1
      },
      "parsed": null,
      "start_char": 0,
      "end_char": 12,
      "start_token": 0,
      "end_token": 2
    }
```

You can see the `raw_value` the text within the message will be returned with an actual value of `New York City`.