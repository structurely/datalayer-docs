# Agents

The Agent resource represents an account in homechat. An agent has settings and owns leads.

<aside class="warning">
This API resource is subject to change. Be prepared to make changes in the future.
</aside>

## Schemas

### Agent

Field | Type | Description | Readable? | Writable?
----- | ---- | ----------- | --------- | ---------
id | `ObjectId` | The ID of the agent this resource represents. | Yes | No
email | `String` | The email of the agent. This is unique and used for login credentials | Yes | No

## List Agents
```shell
curl 'https://api.structurely.com/v1/agents?userId=1025&email=john.richards191919%40example.com' \
  -H 'X-Api-Authorization: myapikey'
```

> The above command returns JSON structured like this:

```json
{
    "_metadata": {
        "collection": "agents",
        "limit": 10,
        "offset": 0,
        "total": 1
    },
    "agents": [
        {
            "id": "5b92de82f434f50034330e46",
            "email": "john.richards191919@example.com"
        }
    ]
}
```

This endpoint returns a list of agents sorted and/or filtered by the given parameters. This request requires the user_id parameter which refers to a third party integration user id (e.g. liondesk).

### HTTP Request

`GET https://api.structurely.com/v1/agents`

### Query Parameters

Parameter | Type | Description | Required? | Default
--------- | ---- | ----------- | --------- | -------
userId[<sup>[1]</sup>](#list-agents-note-1) | `String,Integer` | The third party user id | Yes | None
limit | `Integer[1,100]` | The number of results to return | No | `10`
offset | `Integer[0,)` | Indicates the number of results to skip | No | `0`
sort[<sup>[2]</sup>](#list-agents-note-2) | `String` | A comma separated list of fields to sort by | No | `email`
email[<sup>[3]</sup>](#list-agents-note-3) | `String` | A partial or full email to search | No | None

<ol style="font-size: 12px">
  <li id="list-agents-note-1">This field expects the request to be coming from a system that has already integrated with Homechat. This field is expected to be removed in the near future and is only here to assist in finding an agent in Homechat that has an account in a different system.</li>
  <li id="list-agents-note-2">The sort string allows multiple fields with left to right precedence with optional <code>-</code> or <code>+</code> for controlling ascending and descending order</li>
  <li id="list-agents-note-3">String search fields use a case insensitive partial filter that can match any part of the string</li>
</ol>

<aside class="notice">
Be sure to replace myapikey with your API key.
</aside>

<aside class="warning">
The <code>userId</code> parameter will be removed in the future. Because of the way the system is set up currently, this parameter must be used until changes can be made to the API. Users will be notified before these changes are made to the API.
</aside>
