# Events

The events are a way of tracking any important activity on the API
and keeping a history of transactions on your data.

## Event Object

Attribute | Type | Description
--------- | ---- | -------
_id | `object` | The event ID.
**type** | `string` | The type of event. Can be any from the [`event type list`](#list-of-event-types)
data | `object` | An object containing the event type, timestamp and any additional data necessary


## Get All Events
Receives a list of all events.

> Definition

```
GET https://api.shareactor.io/events
```
> Example request:

``` http
GET /events HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "_id": {"$oid": "5964a0ead57ba2036750a3b4"},
        "type": "order.created",
        "created": {"$date": 1499767018360},
        "company": {"$oid": "57ee9c71d76d431f8511142f"},
        "data" {"type": "order.created"}
    }
]
```

Attribute | Type | Description
--------- | ---- | -------
size | `number` | Number of subscriptions per page _default is 10_
page | `number` | Which page to retrieve _default is 0_
sort | `string` | Field used to sort results _default is `-created`_


## Receive Event by ID
Receive an event, based on its ID.

> Definition

```
GET https://api.shareactor.io/events/<event_id>
```
> Example request:

``` http
GET /events/5964a0ead57ba2036750a3b4 HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "_id": {"$oid": "5964a0ead57ba2036750a3b4"},
    "type": "order.created",
    "created": {"$date": 1499767018360},
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "data" {"type": "order.created"}
}
```

## Events Search

Retrieves a list of Events associated with search.

> Definition

```
GET https://api.shareactor.io/events/search
```

> Example request by account_number:

``` http
GET /events/search/query=order HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "_id": {"$oid": "5964a0ead57ba2036750a3b4"},
        "type": "order.created",
        "created": {"$date": 1499767018360},
        "company": {"$oid": "57ee9c71d76d431f8511142f"},
        "data" {"type": "order.created"}
    },
    {
        "_id": {"$oid": "5964a0ead57ba2036750a3b4"},
        "type": "order.updated",
        "created": {"$date": 1499767018360},
        "company": {"$oid": "57ee9c71d76d431f8511142f"},
        "data" {"type": "order.updated"}
    }
]
```

Arguments | Type | Description
--------- | ---- | -------
**query** | `string` | What you want to search, like **id** or **type**
size | `number` | number of items to retrieve
page | `number` | which page to retrieve. _default page size is 10_
sort | `string` | field used for sorting results. If missing default is "-created"
from_date | `number` | Start date, `timestamp` format. _default is None_
to_date | `number` | End date, `timestamp` format. _default is None_

## List of Event Types

Model name | Description
---------- | ------
`order.created` | Order created
