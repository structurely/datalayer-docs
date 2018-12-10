# Leads

The Lead resource represents a single contact possibly with a conversation in homechat. This conversation includes messages by agents, the lead, and Holmes.

## Schemas

### Lead

Field | Type | Description | Readable? | Writable?
----- | ---- | ----------- | --------- | ---------
id | `ObjectId` | The ID of the lead this resource represents. | Yes | No
name | `String` | The full name of the lead | Yes | Yes
email | `String` | The email of the lead | Yes | Yes
phone | `String` | The phone number of the lead or the format `+15551234567` | Yes | No
firstContact | `Float` | The number of seconds after the unix epoch since first contact was made with this lead | Yes | No
lastContact | `Float` | The number of seconds after the unix epoch since the latest contact was made with this lead | Yes | No
muted | `Boolean` | Indicates whether Holmes is muted in the conversation or not. | Yes | Yes

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
      "firstContact": 1529970559.365,
      "lastContact": 1530572384.613,
      "muted": false
    },
    {
      "id": "5a8b7ce54aecff0034c31776",
      "name": "john doe",
      "email": "the.real.jdoe@example.com",
      "phone": "+15551112222",
      "firstContact": 1519090916.902,
      "lastContact": 1519090973.948,
      "muted": false
    },
    {
      "id": "5a690b0ef4a8dc00359afb57",
      "name": "John Harkin",
      "email": "jharkin@example.com",
      "phone": "+15557654321",
      "firstContact": 1516833549.595,
      "lastContact": 1516833576.27,
      "muted": true
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
  "firstContact": 1529970559.365,
  "lastContact": 1530572384.613,
  "muted": false
}
```

This endpoint returns a specific lead.

### HTTP Request

`GET https://api.structurely.com/v1/leads/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | The ID of the lead to retrieve

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
  "firstContact": 1529970559.365,
  "lastContact": 1530572384.613,
  "muted": true
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
