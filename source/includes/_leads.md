# Leads

The Lead resource represents a single contact possibly with a conversation in homechat. This conversation includes messages by agents, the lead, and Holmes.

## Fields

Field | Type | Description
----- | ---- | -----------
id | `ObjectId` | The ID of the lead this resource represents.
muted | `Boolean` | Indicates whether Holmes is muted in the conversation or not.

## Get Lead

```shell
curl 'https://api.structurely.com/v1/leads/5ab1973c62c5be0034c2102c' \
  -H 'X-Api-Authorization: myapikey'
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
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

<aside class="notice">
Be sure to replace myapikey with your API key.
</aside>

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
  "id": 1,
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

### Body Parameters

Parameter | Type | Description | Required?
--------- | ---- | ----------- | ---------
muted | `Boolean` | If true, Holmes will be muted if it is not already. If false, Holmes will be unmuted if not already. | No

<aside class="notice">
Be sure to include the header `Content-Type: application/json` with your request.
</aside>
