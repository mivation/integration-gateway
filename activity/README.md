# Activity Data Feed

The activity data feed is the standard feed for customer/partner data to come into Mivation applications from activity management solutions and phone systems.

### Fields
| Field | Required | Type/Format | Example(s) | Description|
|-------|----------|-------------|---------|------------|
| `id` | No | string | `02784e36-e70f4c86595b` | A unique record identifier for this transaction in the source system of record. Any string, up to 255 chars in length, is valid.  This value, if provided, is expected to be unique, per source. Subsequent records received from the same source with the same "id" will be treated as updates.|
| `type` | Yes | string | `call` | The type of activity being logged. Standard supported types are "call", "quote", "sale", "meeting", etc... Custom types can also be used. |
| `timestamp` | Yes | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) | `2020-05-18T21:15:00-5:00` | The date and time that the activity occurred.  Note the inclusion of the UTC offset.  If the timestamp is UTC then it should be in format `2020-05-18T21:15:00Z`.|
| `user_email` | Option | email address | `jonny.test@mivation.com` | The email address to identify the user that completed the activity.  One of either `use_email` or `user_alias`, but not both, must be provided.| 
| `user_alias` | Option | string | `jontes5` | The internal "alias" or "username" used by the organization or source system to identify the user that completed the activity.  The alias must be pre-registered with Mivation via "user-registry" transmission. *One of either `use_email` or `user_alias`, but not both, must be provided.|
| `contact` | No | string | `Jane S` | This field can be used to annotate the contact that user was working with on activities where that is relevant.  This should ideally contain an internal value or non-PII data such as a listed phone number or a partial name. |
| `href` | No | [URL](https://en.wikipedia.org/wiki/URL) | `https://my.crm.net/02784e36` | A url that the user can use for accessing the associated activity in the source system.|
|-|-|-|-|-|
| `end_timestamp` | No | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) | `2020-05-18T21:40:30Z` | For activity types, such as `call`, where there is a specific start and end time, this field provides the ending time.  This will be used, along with `timestamp`, to calculate `duration`, unless that is explicitly provided.|
| `duration` | No | [ISO 8601#D](https://en.wikipedia.org/wiki/ISO_8601#Durations) | `PT25M30S` | For activity types, such as `call`, where there is a specific length of elapsed time, this field provides that value.  This will be used, along with `timestamp`, to calculate `end_timestamp`, unless that is explicitly provided. |
| `product` | No | string | `Renters` | For activities such as `quote` where product information applies, this field provides that value. | 
| `amount` | No | decimal | `123.45` | For activities such as `quote` or `sale` where there is a relevant amount, this field provides that value.|
| `tags` | No | string [ ] | `["tag1","tag2",tag3"]` | For activities from source systems that connect simple tag values to the activity that can be used to determine applicability for competition scoring rules.  |
|-|-|-|-|-|
| `void` | No | boolean | false | Used in combination with `id` to void or delete a previousley sent activity.  When `void` is `true`, `id` is required and all other values are ignored.|

### Custom Fields
Nearly any other name/value pair that is valid [JSON](https://en.wikipedia.org/wiki/JSON#Data_types_and_syntax) can be provided, including simple nested objects and arrays of primitive types. The only significant limitation is that arrays of nested objects are not currently supported.  Custom fields may or may not be processed by the target system, depending on the defined rules.  (see examples below for custom `direction` and `product_line` fields)

### Example

```yaml
{
  "format": "activity",
  "payload": [
    { 
      "id": "02784e36-e70f4c86595b",
      "type": "call",
      "timestamp": "2020-05-18T21:15:00-5:00",
      "duration": "PT25M30S",
      "user_email": "jonny.test@mivation.com",
      "contact": "+18774289403",
      "direction": "outbound"
    },
    { 
      "id": "25b8d74f-3becb0ede39f",
      "type": "quote",
      "timestamp": "2020-05-18T21:20:00-5:00",
      "user_email": "jonny.test@mivation.com",
      "contact": "Jane S",
      "product_line": "fire",
      "product": "Renters",
      "amount": 123.45,
      "href" : "https://my.quotes.net/25b8d74f-3becb0ede39f",
      "tags": ["win_back","quote_delivered"]
    },
    {
      "id": "044d54a9-a5939dbae02e",
      "void": true
    }
  ]
}
```


 
