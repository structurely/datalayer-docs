# Leads

The Lead resource represents a single contact possibly with a conversation in homechat. This conversation includes messages by agents, the lead, and Holmes.

## Schemas

### Lead

Field | Type | Description | Readable? | Writable?
----- | ---- | ----------- | --------- | ---------
id | `ObjectId` | The ID of the lead this resource represents. | Yes | No
name | `String` | The full name of the lead. | Yes | Yes
email | `String` | The email of the lead. | Yes | Yes
phone | `String` | The phone number of the lead in the format `+15551234567`. | Yes | No
readiness | `String` | The readiness of the lead. Can be one of the following values: `active`, `just_looking`, `researching`, or an empty string. | Yes | No
firstContact | `Float` | The number of seconds after the unix epoch since first contact was made with this lead. | Yes | No
lastContact | `Float` | The number of seconds after the unix epoch since the latest contact was made with this lead. | Yes | No
muted | `Boolean` | Indicates whether Holmes is muted in the conversation or not. | Yes | Yes
conversation | `LeadConversation` | The conversation metadata object for this lead. | Yes | No

### LeadConversation

Field | Type | Description | Readable? | Writable?
----- | ---- | ----------- | --------- | ---------
collection | `String` | Always the value `lead.conversation`. | Yes | No
total | `Integer` | The total number of messages sent by the lead, Holmes, and the agent. | Yes | No
next[<sup>[1]</sup>](#schema-leadconversation-note-1) | `String` | The url for the next 100 or fewer messages in the sequence. | Yes | No

<ol style="font-size: 12px">
  <li id="schema-leadconversation-note-1"> If this object is not part of a response in a sequence, the next value will be the url for the latest 100 messages for this lead conversation.</li>
</ol>

### LeadMessage

Field | Type | Description | Readable? | Writable?
----- | ---- | ----------- | --------- | ---------
id | `ObjectId` | The ID of the message for this conversation. | Yes | No
received | `Float` | The unix timestamp this message was received/sent. | Yes | No
text | `String` | The text of the message that was sent or received. | Yes | No
type[<sup>[1]</sup>](#schema-leadmessage-note-1) | `String` | The type of the message. It will either be `message`, `response`, or `agentMessage`. | Yes | No

<ol style="font-size: 12px">
  <li id="schema-leadmessage-note-1">The type <code>message</code> is set if the message was sent by the lead. The type <code>response</code> is set if the message was sent by Holmes. The type <code>agentMessage</code> is set if the message was sent by the agent from homechat.</li>
</ol>

## List Leads
```shell
curl 'https://api.structurely.com/v1/leads?ownerId=59275069dec26a0d20fcc41e&name=john' \
  -H 'X-Api-Authorization: myapikey'
```

> The above command returns JSON structured like this:

```json
{
  "_metadata": {
    "collection": "leads",
    "limit": 10,
    "offset": 0,
    "total": 3
  },
  "leads": [
    {
      "id": "5b317f7f97f5a50048e1a516",
      "name": "John Doe",
      "email": "jdoe@example.com",
      "phone": "+15551234567",
      "readiness": "",
      "firstContact": 1529970559.365,
      "lastContact": 1530572384.613,
      "muted": false,
      "conversation": {
        "collection": "lead.conversation",
        "total": 10,
        "next": "https://api.structurely.com/v1/leads/5b317f7f97f5a50048e1a516/conversation"
      }
    },
    {
      "id": "5a8b7ce54aecff0034c31776",
      "name": "john doe",
      "email": "the.real.jdoe@example.com",
      "phone": "+15551112222",
      "readiness": "active",
      "firstContact": 1519090916.902,
      "lastContact": 1519090973.948,
      "muted": false,
      "conversation": {
        "collection": "lead.conversation",
        "total": 2,
        "next": "https://api.structurely.com/v1/leads/5a8b7ce54aecff0034c31776/conversation"
      }
    },
    {
      "id": "5a690b0ef4a8dc00359afb57",
      "name": "John Harkin",
      "email": "jharkin@example.com",
      "phone": "+15557654321",
      "readiness": "just_looking",
      "firstContact": 1516833549.595,
      "lastContact": 1516833576.27,
      "muted": true,
      "conversation": {
        "collection": "lead.conversation",
        "total": 4,
        "next": "https://api.structurely.com/v1/leads/5a690b0ef4a8dc00359afb57/conversation"
      }
    }
  ]
}
```

This endpoint returns a list of leads sorted and/or filtered by the given parameters. This request requires the ownerId parameter which refers to an agent id.

### HTTP Request

`GET https://api.structurely.com/v1/leads`

### Query Parameters

Parameter | Type | Description | Required? | Default
--------- | ---- | ----------- | --------- | -------
ownerId | `ObjectId` | The id of the agent that owns this lead | Yes | None
limit | `Integer[1,100]` | The number of results to return | No | `10`
offset | `Integer[0,)` | Indicates the number of results to skip | No | `0`
sort[<sup>[1]</sup>](#list-leads-note-1) | `String` | A comma separated list of fields to sort by | No | `-lastContact`
name[<sup>[2]</sup>](#list-leads-note-2) | `String` | A partial or full name to search | No | None
email[<sup>[2]</sup>](#list-leads-note-2) | `String` | A partial or full email to search | No | None
phone[<sup>[2]</sup>](#list-leads-note-2) | `String` | A partial or full phone number to search | No | None

<ol style="font-size: 12px">
  <li id="list-leads-note-1">The sort string allows multiple fields with left to right precedence with optional <code>-</code> or <code>+</code> for controlling ascending and descending order</li>
  <li id="list-leads-note-2">String search fields use a case insensitive partial filter that can match any part of the string</li>
</ol>

<aside class="notice">
Be sure to replace myapikey with your API key.
</aside>

<aside class="warning">
The <code>ownerId</code> parameter will be removed in the future. Because of the way the system is set up currently, this parameter must be used until changes can be made to the API. Users will be notified before these changes are made to the API.
</aside>

## Get Lead

```shell
curl 'https://api.structurely.com/v1/leads/5ab1973c62c5be0034c2102c' \
  -H 'X-Api-Authorization: myapikey'
```

> The above command returns JSON structured like this:

```json
{
  "id": "5ab1973c62c5be0034c2102c",
  "name": "John Doe",
  "email": "jdoe@example.com",
  "phone": "+15551234567",
  "readiness": "researching",
  "firstContact": 1529970559.365,
  "lastContact": 1530572384.613,
  "muted": false,
  "conversation": {
    "collection": "lead.conversation",
    "next": "https://api.structurely.com/v1/leads/5ab1973c62c5be0034c2102c/conversation",
    "total": 108
  }
}
```

This endpoint returns a specific lead.

### HTTP Request

`GET https://api.structurely.com/v1/leads/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | The ID of the lead to retrieve

## Get Lead Conversation

```shell
curl 'https://api.structurely.com/v1/leads/5ab1973c62c5be0034c2102c/conversation' \
  -H 'X-Api-Authorization: myapikey'
```

> The above command returns JSON structured like this:

```json
{
  "_metadata": {
    "collection": "lead.conversation",
    "next": "https://api.structurely.com/v1/leads/5ab1973c62c5be0034c2102c/conversation?before=1540400827.866",
    "total": 108
  },
  "conversation": [
    {
      "id": "5bd0a6bbbfd5b5002268f971",
      "received": 1540400827.866,
      "text": "It's always helpful to have financing figured out early in the process!",
      "type": "response"
    },
    ,
    ,
    ,
    {
      "id": "5c018167def58d0021d267cd",
      "received": 1543602535.414,
      "text": "Thank's for all the help. I'm really excited to see this home tomorrow.",
      "type": "message"
    }
  ]
}
```

This endpoint returns a specific lead's conversaton. It will return no more than 100 messages at a time. The `_metadata.next` value gives the url that will return the next batch of messages in the sequence. If there are no more messages `_metadata.next` will be `null`. If the `before` parameter is used, the next batch will be before the earliest message and if `after` is used, the next batch will be after the latest message. If there is no parameter defined the latest 100 messages are returned and the `_metadata.next` url will be for the previous batch with a before parameter. The `_metadata.total` value returns the total number of messages in the conversation.

### HTTP Request

`GET https://api.structurely.com/v1/leads/<id>/conversation`

### URL Parameters

Parameter | Description
--------- | -----------
id | The ID of the lead to retrieve the conversation for

### Query Parameters

Parameter | Type | Description | Required? | Default
--------- | ---- | ----------- | --------- | -------
before[<sup>[1]</sup>](#lead-conversation-note-1) | `Float` | The unix timestamp all messages must have been received before | No | None
after[<sup>[1]</sup>](#lead-conversation-note-1) | `Float` | The unix timestamp all messages must have been received after | No | None

<ol style="font-size: 12px">
  <li id="lead-conversation-note-1">The <code>before</code> and <code>after</code> parameters are mutually exclusive. Neither is required but only one may be present.</li>
</ol>

## Update Lead

```shell
curl 'https://api.structurely.com/v1/leads/5ab1973c62c5be0034c2102c' \
  -X PATCH \
  -H 'X-Api-Authorization: myapikey' \
  -H 'Content-Type: application/json' \
  -d '{ "muted": true }'
```

> The above command returns JSON structured like this:

```json
{
  "id": "5ab1973c62c5be0034c2102c",
  "name": "John Doe",
  "email": "jdoe@example.com",
  "phone": "+15551234567",
  "readiness": "just_looking",
  "firstContact": 1529970559.365,
  "lastContact": 1530572384.613,
  "muted": true,
  "conversation": {
    "collection": "lead.conversation",
    "next": "https://api.structurely.com/v1/leads/5ab1973c62c5be0034c2102c/conversation",
    "total": 108
  }
}
```

This endpoint updates a specific lead and returns the modified lead object.

### HTTP Request

`PATCH https://api.structurely.com/v1/leads/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | The ID of the lead to update

### Body Parameters[<sup>[^]</sup>](#leads-schemas-lead)

Parameter | Type
--------- | ----
name | `String`
email | `String`
muted | `Boolean`

<aside class="notice">
Be sure to include the header `Content-Type: application/json` with your request.
</aside>
