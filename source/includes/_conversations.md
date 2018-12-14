# Conversations

The Conversation resource represents a conversational exchange with Aisa. This is distict from a lead because it interfaces directly with aisa rather than showing up for an agent in homechat.

<aside class="warning">
This API resource is subject to change. Be prepared to make changes in the future.
</aside>

## Schemas

### Conversation

Field | Type | Description | Readable? | Writable? | Required? | Default
----- | ---- | ----------- | --------- | --------- | --------- | -------
id | `ObjectId` | The ID of the conversation this resource represents. | Yes | No | No
settings | `ConversationSettings` | The settings for holmes per this conversation. | Yes | No | Yes
slots | `List<ConversationSlot>` | A list of extracted or pre-filled values relating to the conversation. | Yes | No | Yes
muted | `Boolean` | A boolean flag to tell Holmes whether or not this conversation is muted. A muted conversation can still receive messages but will generate no replies. | Yes | Yes | No | `false`
messages | `List<ConversationItem>` | A list of lead messages and system responses with the context value with each response. | No | No | Yes

### ConversationSettings

Field | Type | Description | Readable? | Writable? | Required? | Default
----- | ---- | ----------- | --------- | --------- | --------- | -------
timeZone | `String` | The time zone the realtor and/or lead. (See [list](https://gist.github.com/heyalexej/8bf688fd67d7199be4a1682b3eec7568)) | Yes | No | Yes
holmesName | `String` | The name of conversation AI to use in responses. | Yes | No | No | Aisa

### ConversationSlot

Field | Type | Description | Readable? | Writable? | Required?
----- | ---- | ----------- | --------- | --------- | ---------
name[<sup>[a]</sup>](#conversations-slots-and-contexts-slots) | `String` | The name of the slot. | Yes | No | Yes
value[<sup>[a]</sup>](#conversations-slots-and-contexts-slots) | `Any` | The value of the slot. | Yes | No | Yes

### ConversationItem

Field | Type | Description | Readable? | Writable? | Required?
----- | ---- | ----------- | --------- | --------- | ---------
message | `Message` | A message the lead sent. | No | Yes | Yes[<sup>[1]</sup>](#schema-conversationitem-note-1)
respone | `Message` | A respone sent by the system. | No | Yes | Yes[<sup>[1]</sup>](#schema-conversationitem-note-1)
context[<sup>[b]</sup>](#conversations-slots-and-contexts-contexts) | `String` | The context set by the response in this conversation item. | No | Yes | Yes[<sup>[2]</sup>](#schema-conversationitem-note-2)

<ol style="font-size: 12px">
  <li id="schema-conversationitem-note-1"><code>message</code> and <code>response</code> are mutually exclusive. At least one is required to be present but not both.</li>
  <li id="schema-conversationitem-note-2"><code>context</code> is only required if the <code>response</code> field is used.</li>
</ol>

### Message

Field | Type | Description | Readable? | Writable? | Required?
----- | ---- | ----------- | --------- | --------- | ---------
text | `String` | The text of the message. | No | Yes | Yes
received | `DateTime` | The time the message was sent or received. | No | Yes | Yes
_metadata[<sup>[1]</sup>](#schema-message-note-1) | `MessageMetaData` | An optional metadata object to set the outgoing context for a response. | No | Yes | No

<ol style="font-size: 12px">
  <li id="schema-message-note-1"><code>_metadata</code> is only used when creating a response object.</li>
</ol>

### MessageMetaData

Field | Type | Description | Readable? | Writable? | Required?
----- | ---- | ----------- | --------- | --------- | ---------
context | `String` | The outgoing context of this response. Used by Holmes to classify messages sent after this response. | No | Yes | No

## Slots and Contexts

### Slots

Name | Type | Description
---- | ---- | -----------
name | String | The name of the lead.
email | String | The email of the lead.
phone | String | The phone number of the lead.
address | String |  The address of the property a buyer lead is interested in.
selling_address | String | The address of the property a seller lead is trying to sell.
lead_types | List<String> | A list of types of leads. Can be any combination of `buyer`, `seller`, or `renter`.
lead_source | String | The source of the lead. Can be any string.
agent_name | String | The name of the agent Holmes is messaging on the behalf of.
agent_phone | String | The phone of the agent.
agent_email | String | The email of the agent.
agency_name | String | The name of the agency the agent works for or Holmes is messaging on the behalf of.
office_location | String | The general location of the realtor office the system is representing.

### Contexts

Name |
---- |
general |
expect_address |
expect_agent_status |
expect_appointment |
expect_baths |
expect_beds |
expect_call_availability |
expect_contingency |
expect_email |
expect_financing_status |
expect_lead_type |
expect_listing_appointment |
expect_location |
expect_name |
expect_phone |
expect_price |
expect_selling_address |
expect_showing_appointment |
expect_timeframe |

## Create Conversation

```shell
curl 'https://api.structurely.com/v1/conversations' \
  -X POST \
  -H 'X-Api-Authorization: myapikey' \
  -H 'Content-Type: application/json' \
  -d '{ "settings": { "time_zone": "America/Chicago" }, "slots": [{ "name": "email", "value": "jdoe@example.com" }], "messages": [{ "response": { "text": "Hello, what is your name?", "received": "2018-12-08T15:20:00.000Z" }, "context": "expect_name }, { "message": { "text": "John", "received": "2018-12-08T16:34:00.000Z" } }] }'
```

> The above command returns JSON structured like this:

```json
{
    "id": "5c09a4416241ea2c293275b8",
    "settings": {
        "holmesName": "Aisa",
        "timeZone": "America/Chicago"
    },
    "slots": [
        {
            "name": "email",
            "value": "jdoe@example.com"
        }
    ],
    "muted": false
}
```

This endpoint creates and returns a new conversation object.

### HTTP Request

`POST https://api.structurely.com/v1/conversations`

### Body Parameters[<sup>[^]</sup>](#conversations-schemas-conversation)

Parameter | Type
--------- | ----
settings | `ConversationSettings`
slots | `List<ConversationSlot>`
muted | `Boolean`
messages | `List<ConversationItem>`

<aside class="notice">
Be sure to include the header `Content-Type: application/json` with your request.
</aside>

## Get Conversation

```shell
curl 'https://api.structurely.com/v1/conversations/5c09a4416241ea2c293275b8' \
  -H 'X-Api-Authorization: myapikey'
```

> The above command returns JSON structured like this:

```json
{
    "id": "5c09a4416241ea2c293275b8",
    "settings": {
        "holmesName": "Aisa",
        "timeZone": "America/Chicago"
    },
    "slots": [
        {
            "name": "name",
            "value": "John Doe"
        },
        {
            "name": "email",
            "value": "jdoe@example.com"
        },
        {
            "name": "phone",
            "value": "+15551234567"
        },
        {
            "name": "address",
            "value": "123 Main St."
        },
        {
            "name": "agent_name",
            "value": "Andi Agent"
        },
        {
            "name": "lead_types",
            "value": [
                "buyer"
            ]
        }
    ],
    "muted": false
}
```

This endpoint returns a specific conversation.

### HTTP Request

`GET https://api.structurely.com/v1/conversations/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | The ID of the conversation to retrieve

## Update Conversation

```shell
curl 'https://api.structurely.com/v1/conversations/5c09a4416241ea2c293275b8' \
  -X PATCH \
  -H 'X-Api-Authorization: myapikey' \
  -H 'Content-Type: application/json' \
  -d '{ "muted": true }'
```

> The above command returns JSON structured like this:

```json
{
    "id": "5c09a4416241ea2c293275b8",
    "settings": {
        "holmesName": "Aisa",
        "timeZone": "America/Chicago"
    },
    "slots": [
        {
            "name": "email",
            "value": "jdoe@example.com"
        }
    ],
    "muted": true
}
```

This endpoint updates and returns a specific conversation object.

### HTTP Request

`PATCH https://api.structurely.com/v1/conversations/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | The ID of the conversation to update

### Body Parameters[<sup>[^]</sup>](#conversations-schemas-conversation)

Parameter | Type
--------- | ----
muted | `Boolean`

<aside class="notice">
Be sure to include the header `Content-Type: application/json` with your request.
</aside>

## Create Message

```shell
curl 'https://api.structurely.com/v1/conversations/5c09a4416241ea2c293275b8/messages'
  -X POST \
  -H 'X-Api-Authorization: myapikey' \
  -H 'Content-Type: application/json' \
  -d '{ "text": "Hello World", "received": "2018-12-09T11:44:00.000Z" }'
```

> The above command returns JSON structured like this:

```json
{
    "_metadata": {
        "conversation": "5c09a4416241ea2c293275b8",
        "messages": 2,
        "responses": 1
    },
    "message": {
        "id": "5c09a45e6241ea2c293275bf",
        "text": "Hello World",
        "received": "2018-12-09T11:44:00.000Z"
    }
}
```

This endpoint creates a new lead message for the conversation for us to extract and classify.

### HTTP Request

`POST https://api.structurely.com/v1/conversations/<id>/messages`

### URL Parameters

Parameter | Description
--------- | -----------
id | The ID of the conversation to retrieve

### Body Parameters[<sup>[^]</sup>](#conversations-schemas-message)

Parameter | Type
--------- | ----
text | `String`
received | `DateTime`

<aside class="notice">
Be sure to include the header `Content-Type: application/json` with your request.
</aside>

## Create Response

```shell
curl 'https://api.structurely.com/v1/conversations/5c09a4416241ea2c293275b8/responses'
  -X POST \
  -H 'X-Api-Authorization: myapikey' \
  -H 'Content-Type: application/json' \
  -d '{ "_metadata": { "context": "expect_showing_appointment" }, "text": "Hello There", "received": "2018-12-09T11:44:00.000Z" }'
```

> The above command returns JSON structured like this:

```json
{
    "_metadata": {
        "conversation": "5c09a4416241ea2c293275b8",
        "context": "expect_showing_appointment",
        "messages": 2,
        "responses": 2
    },
    "response": {
        "id": "5c09a4416241ea2c293275b5",
        "text": "Hello There",
        "received": "2018-12-09T11:44:00.000Z"
    }
}
```

This endpoint creates a new system response for the conversation. This does nothing in our system other than act as a send receipt notifying us that an outbound message was sent on behalf of the system/realtor.

### HTTP Request

`POST https://api.structurely.com/v1/conversations/<id>/responses`

### URL Parameters

Parameter | Description
--------- | -----------
id | The ID of the conversation to retrieve

### Body Parameters[<sup>[^]</sup>](#conversations-schemas-message)

Parameter | Type
--------- | ----
_metadata | `MessageMetaData`
text | `String`
received | `DateTime`

<aside class="notice">
Be sure to include the header `Content-Type: application/json` with your request.
</aside>
