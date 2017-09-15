---
title: "Configuring Training Data"
excerpt: ""
---
When configuring your project, it is important to determine how you plan to use suggestions. If you intend to use only [**raw**](doc:types-of-suggestions#section-raw-suggestions) suggestions, no special annotation is required – simply train your model as you would for any other use case.

However, if you intend to make use of  [**templated**](doc:types-of-suggestions#section-raw-templated) suggestions, you will need to train your model using conversations with annotated agent or system messages.
[block:callout]
{
  "type": "info",
  "body": "By default, when using [**templated**](doc:types-of-suggestions#section-raw-templated) suggestions, Init.ai will first look for matching intents from data annotated as agent and then fall back to system intents.",
  "title": "Intent lookup"
}
[/block]
Only messages with an explicitly labeled intent will be used for suggestions. For example, this conversation will not result in a suggestion of "You are welcome" or "Bye.":
[block:code]
{
  "codes": [
    {
      "code": "user> thanks\n\nagent> You are welcome\n\nuser> bye\n\nagent> Bye.",
      "language": "markdown",
      "name": "Training data without intents"
    }
  ]
}
[/block]
but this conversation will:
[block:code]
{
  "codes": [
    {
      "code": "user> thanks\n\nagent> You are welcome\n* you_are_welcome\n\nuser> bye\n\nagent> Bye.\n* bye",
      "language": "markdown",
      "name": "Training data with intents"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "An agent example"
}
[/block]
As an example, if we want to provide an agent with suggestions for available tickets for a sporting event, we could write training data that looks like this:
[block:code]
{
  "codes": [
    {
      "code": "user> how many [bleacher](seat_type) tickets are available for this [Saturday](time/gameday) at [Wrigley](venue_name)?\n* request_ticket_availability_count\n\nagent> i have [3](number/ticket_count) [bleacher](seat_type) tickets available for this [Saturday](time/gameday) at [Wrigley](venue_name)\n* provide_ticket_availability_count",
      "language": "markdown",
      "name": "Training data for agent interaction"
    }
  ]
}
[/block]
> *Make sure to add variants of this conversation to enhance your training data* 

With training data like this available, we can hit a hypothetical ticketing API whenever the user asks about ticket availability and hydrate the **templated** suggestion appropriately.
[block:code]
{
  "codes": [
    {
      "code": "// ...\nconst getEntityValue = message => entity =>\n  getFirstEntityWithRole(message, entity).value\nconst lookupEntity = getEntityValue(client.getMessagePart())\n\nconst seatType = lookupEntity(message, 'seat_type') // Loge box\nconst venueName = lookupEntity(message, 'venue_name') // fenway park\nconst gameDay = lookupEntity(message, 'time/gameday') // Tuesday\n\n// Hit our fake ticketing API\nlookupTickets({\n  venue: venueName,\n  game: { day: gameDay },\n}).then(availableSeats => {\n  const matchingSeats = availableSeats.filter(seats => seats.type === seatType) // [..., ..., ...]\n\n  client.addSuggestion({\n    intent: 'provide_ticket_availability_count',\n    data: {\n      seat_type: seatType,\n      venue_name: venueName,\n      'number/ticket_count': matchingSeats.length,\n      'time/gameday': gameDay,\n    },\n  })\n  client.sendResponse()\n})\n// ...\n",
      "language": "javascript",
      "name": "Generating hydrated suggestions"
    }
  ]
}
[/block]
This will result in connected clients receiving a suggestion that looks like this:
[block:code]
{
  "codes": [
    {
      "code": "I have 3 Loge box tickets available for this Saturday at Fenway",
      "language": "text",
      "name": "Generated suggestion"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Configuring for auto-suggestions"
}
[/block]
As mentioned [previously](doc:types-of-suggestions#section-auto-suggestions), automatic suggestions are useful for basic conversational interactions.  Typical training data for such suggestions may look like these examples:
[block:code]
{
  "codes": [
    {
      "code": "user> hey\n* greeting\n\nagent> Hello\n* greeting",
      "language": "markdown",
      "name": "Basic greeting interaction"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "user> thank you\n* thanks\n\nagent> You are welcome.\n* you_are_welcome",
      "language": "markdown",
      "name": "Basic thank you interaction"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "user> What is your address?\n* frequent_questions/address\n\nagent> We are located at 135 West 20th Street.\n* frequent_answers/address",
      "language": "markdown",
      "name": "Basic hardcoded interaction"
    }
  ]
}
[/block]
### A note on accuracy

The accuracy of automatically generated suggestions depends heavily on the quality of your training data. In general, auto-suggestions will likely be lower quality than suggestions generated by your Logic.

All automatically generated suggestions include a `prediction_confidence` value in the `nlp_metadata` of the suggestion. You can use this value to handle only suggestions above a certain threshold or as an indicator to enhance your training data.