---
title: "Entity types and slot parsing"
excerpt: ""
---
Init.ai locates slots based on the contents of examples it has seen in training data from your application.

An entity represents a conceptual datatype that might occur in a conversation.

Entities by default are of type `string`. Strings are currently not processed, and are passed to your logic code untouched.

Slots are specific instances of entities occurring with a conversation.

Non-`string` entity types get special treatment. After the Init.ai conversational NLP system locates a slot within a message, if its type is determined to be an entity that is more specific than `string`, Init.ai will apply post-processing to the detected phrase to make it easier to parse.

> **Note:** `location` entity types are processed via the Microsoft Bing Knowledge API.  All other entity types are processed using the [Duckling library](https://duckling.wit.ai/).

This document provides examples of what the parsed values look like.

Within the logic, the parsed value is inserted into slot value as the `parsed` value. For `string` entities, `parsed` will be `null`.

# location

Usable for regions, cities, or addresses.

Training data:

```
I'm looking in [san francisco](location/city)
```

Parsed value:

```
  "entities": {
    "location/city": {
      "entity": "city",
      "base_type": "location",
      "roles": [
        "generic"
      ],
      "values_by_role": {
        "generic": [
          {
            "raw_value": "san francisco",
            "parsed": {
              "latitude": 37.7798614501953,
              "longitude": -122.429046630859,
              "bounding_box": [
                {
                  "latitude": 37.9256744384766,
                  "longitude": -122.66365814209
                },
                 {
                  "latitude": 37.6489105224609,
                  "longitude": -122.183052062988
                }
              ],
              "parsed_address": {
                "streetAddress": "",
                "addressLocality": "",
                "addressRegion": "",
                "postalCode": "",
                "addressCountry": "",
                "neighborhood": "",
                "text": ""
              }
            }
          }
        ]
      }
    }
  },
```

Training data:

```
I'm looking in [123 main st nyc](location/address)
```

Parsed value:

```
  "entities": {
    "location/address": {
      "entity": "address",
      "base_type": "location",
      "roles": [
        "generic"
      ],
      "values_by_role": {
        "generic": [
          {
            "raw_value": "123 broadway nyc",
            "parsed": {
              "latitude": 40.708839501953,
              "longitude": -74.011133581055,
              "bounding_box": [
                {
                  "latitude": "",
                  "longitude": ""
                },
                 {
                  "latitude": "",
                  "longitude": ""
                }
              ],
              "parsed_address": {
                "streetAddress": "123 Broadway",
                "addressLocality": "New York",
                "addressRegion": "NY",
                "postalCode": "10006",
                "addressCountry": "US",
                "neighborhood": "",
                "text": "123 Broadway, New York, NY, 10006"
              }
            }
          }
        ]
      }
    }
  },
```

# time

Also usable for dates.

Training data:

```
the date was [october 14, 2007](time/date)
```

Parsed value:

```
  "entities": {
    "time/date": {
      "entity": "date",
      "base_type": "time",
      "roles": [
        "generic"
      ],
      "values_by_role": {
        "generic": [
          {
            "raw_value": "october 14, 2007",
            "parsed": {
              "dimension": "time",
              "results": [
                {
                  "body": "october 14, 2007",
                  "value": {
                    "grain": "day",
                    "type": "value",
                    "value": "2007-10-14T00:00:00.000Z",
                    "values": []
                  }
                }
              ],
              "text": "october 14, 2007"
            }
          }
        ]
      }
    }
  },
```

# duration

Training data:

```
it will take [five hours](duration/how_long)
```

Parsed value:

```
  "entities": {
    "duration/how_long": {
      "entity": "how_long",
      "base_type": "duration",
      "roles": [
        "generic"
      ],
      "values_by_role": {
        "generic": [
          {
            "raw_value": "five hours",
            "parsed": {
              "dimension": "duration",
              "results": [
                {
                  "body": "five hours",
                  "value": {
                    "hour": 5,
                    "normalized": {
                      "unit": "second",
                      "value": 18000
                    },
                    "unit": "hour",
                    "value": 5
                  }
                }
              ],
              "text": "five hours"
            }
          }
        ]
      }
    }
  },
```

# amount-of-money

Training data:

```
it costs [$1](amount-of-money/money)
```

Parsed value:

```
  "entities": {
    "amount-of-money/money": {
      "entity": "money",
      "base_type": "amount-of-money",
      "roles": [
        "generic"
      ],
      "values_by_role": {
        "generic": [
          {
            "raw_value": "$1",
            "parsed": {
              "dimension": "amount-of-money",
              "results": [
                {
                  "body": "$1",
                  "value": {
                    "type": "value",
                    "unit": "$",
                    "value": 1
                  }
                }
              ],
              "text": "$1"
            }
          }
        ]
      }
    }
  },
```

# number

Training data:

```
I need [five](number/how_many) of them
```

Parsed value:

```
  "entities": {
    "number/how_many": {
      "entity": "how_many",
      "base_type": "number",
      "roles": [
        "generic"
      ],
      "values_by_role": {
        "generic": [
          {
            "raw_value": "five",
            "parsed": {
              "dimension": "number",
              "results": [
                {
                  "body": "five",
                  "value": {
                    "type": "value",
                    "value": 5
                  }
                }
              ],
              "text": "five"
            }
          }
        ]
      }
    }
  },
```

# ordinal

Training data:

```
tell me more about the [second](ordinal/which_one) one
```

Parsed value:

```
  "entities": {
    "ordinal/which_one": {
      "entity": "which_one",
      "base_type": "ordinal",
      "roles": [
        "generic"
      ],
      "values_by_role": {
        "generic": [
          {
            "raw_value": "second",
            "parsed": {
              "dimension": "ordinal",
              "results": [
                {
                  "body": "second",
                  "value": {
                    "value": 2
                  }
                }
              ],
              "text": "second"
            }
          }
        ]
      }
    }
  },
```

# temperature

Training data:

```
it is [ninety eight degrees](temperature/ambient) outside
```

Parsed value:

```
  "entities": {
    "temperature/ambient": {
      "entity": "ambient",
      "base_type": "temperature",
      "roles": [
        "generic"
      ],
      "values_by_role": {
        "generic": [
          {
            "raw_value": "ninety eight degrees",
            "parsed": {
              "dimension": "temperature",
              "results": [
                {
                  "body": "ninety eight degrees",
                  "value": {
                    "type": "value",
                    "unit": "degree",
                    "value": 98
                  }
                }
              ],
              "text": "ninety eight degrees"
            }
          }
        ]
      }
    }
  },
```

# distance

Training data:

```
the destination is [4 km](distance/destination) away
```

Parsed value:

```
  "entities": {
    "distance/destination": {
      "entity": "destination",
      "base_type": "distance",
      "roles": [
        "generic"
      ],
      "values_by_role": {
        "generic": [
          {
            "raw_value": "4 km",
            "parsed": {
              "dimension": "distance",
              "results": [
                {
                  "body": "4 km",
                  "value": {
                    "type": "value",
                    "unit": "kilometre",
                    "value": 4
                  }
                }
              ],
              "text": "4 km"
            }
          }
        ]
      }
    }
  },
```

# volume

Training data:

```
I would like [1 gallon](volume/requested) of milk
```

Parsed value:

```
  "entities": {
    "volume/requested": {
      "entity": "requested",
      "base_type": "volume",
      "roles": [
        "generic"
      ],
      "values_by_role": {
        "generic": [
          {
            "raw_value": "1 gallon",
            "parsed": {
              "dimension": "volume",
              "results": [
                {
                  "body": "1 gallon",
                  "value": {
                    "type": "value",
                    "unit": "gallon",
                    "value": 1
                  }
                }
              ],
              "text": "1 gallon"
            }
          }
        ]
      }
    }
  },
```

# url

Training data:

```
please visit [https://foo.com/path/path?ext=%23&foo=bla](url/destination)
```

Parsed value:

```
  "entities": {
    "url/destination": {
      "entity": "destination",
      "base_type": "url",
      "roles": [
        "generic"
      ],
      "values_by_role": {
        "generic": [
          {
            "raw_value": "https://foo.com/path/path?ext=%23&foo=bla",
            "parsed": {
              "dimension": "url",
              "results": [
                {
                  "body": "https://foo.com/path/path?ext=%23&foo=bla",
                  "value": {
                    "value": "https://foo.com/path/path?ext=%23&foo=bla"
                  }
                }
              ],
              "text": "https://foo.com/path/path?ext=%23&foo=bla"
            }
          }
        ]
      }
    }
  },
```

# phone-number

Training data:

```
update my number to [+33 4 76095663 ext 897](phone-number/contact)
```

Parsed value:

```
  "entities": {
    "phone-number/contact": {
      "entity": "contact",
      "base_type": "phone-number",
      "roles": [
        "generic"
      ],
      "values_by_role": {
        "generic": [
          {
            "raw_value": "+33 4 76095663 ext 897",
            "parsed": {
              "dimension": "phone-number",
              "results": [
                {
                  "body": "+33 4 76095663 ext 897",
                  "value": {
                    "value": "+33 4 76095663 ext 897"
                  }
                }
              ],
              "text": "+33 4 76095663 ext 897"
            }
          }
        ]
      }
    }
  },
```

# email

Training data:

```
update my email to [user@domain.com](email/contact)
```

Parsed value:

```
  "entities": {
    "email/contact": {
      "entity": "contact",
      "base_type": "email",
      "roles": [
        "generic"
      ],
      "values_by_role": {
        "generic": [
          {
            "raw_value": "user@domain.com",
            "parsed": {
              "dimension": "email",
              "results": [
                {
                  "body": "user@domain.com",
                  "value": {
                    "value": "user@domain.com"
                  }
                }
              ],
              "text": "user@domain.com"
            }
          }
        ]
      }
    }
  },
```