# Events

The events are a way of tracking any important activity on the API
and keeping a history of transactions on your data.

## Event Object

Attribute | Type | Description
--------- | ---- | -------
_id | `object` | The event ID.
**kind** | `string` | The kind of event. Can be any from the [`event type list`](#list-of-event-types)
data | `object` | It is an `EventData` object.
author | `object` | [`User`](#users) which triggers the event

### EventData Object

Attribute | Type | Description
--------- | ---- | -------
**object_type** | `string` | It is the object type like `order`, `product`, etc...
**object** | `string` | The full object details.
**kind** | `string` | The kind of event. Can be any from the [`event type list`](#list-of-event-types)
previous_data | `object` | If a document is updated, the previous data is stored here.

## Get All Events
Receives a list of all events.

> Definition

```
GET https://api.kvass.ai/events
```
> Example request:

``` http
GET /events HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "_id": {"$oid": "5964a0ead57ba2036750a3b4"},
        "kind": "order.created",
        "created": {"$date": 1499767018360},
        "company": {"$oid": "57ee9c71d76d431f8511142f"},
        "data": {
            "object_type": "order",
            "kind": "order.created",
            "object": {
                "units":2,
                "delivery_status":"ACCEPTED",
                "currency":"NOK",
                "human_id":"51Q4LN",
                "payments":[],
                "modified":{"$date":1495196003948},
                "provider":{<provider_details>},
                "top_up_amount":0.0,
                "stripe_charge_id":"",
                "order_status":"CREATED",
                "company":{"$oid":"591ee15db70e2a10acb65362"},
                "billing_address":{<billing_address_details>},
                "deliveries":[],
                "items":[{"quantity":60, "discount":0.9, "product":{<product_details>}}],
                "note":"",
                "created":{"$date":1495264401233},
                "delivery_address":{<address_details>},
                "total_quantity":120,
                "_id":{"$oid":"591ee163b70e2a10acb653a7"},
                "total_amount":1200.0,
                "stripe_refund_id":"",
                "override_company_take":-1.0,
                "user":{<user_details>},
                "delivery_time":{"$date":1497009600000}
            }
        },
        "author": {
            "_id":{"$oid":"57ee9c72d76d431f85111432"},
            "_cls":"User",
            "created":{"$date":1475428903950},
            "modified":{"$date":1475428903951},
            "first_name":"John",
            "last_name":"Doe",
            "email":"john@email.com",
            "mobile_phone_number":"+4712345678",
            "addresses":[],
            "billing_address":{},
            "bio":"",
            "note":"",
            "avatar":"",
            "company":{"$oid":"57ee9c71d76d431f8511142f"},
            "tags":[],
            "stripe_customer_id":"",
            "roles":["user"],
            "deleted":true
        }
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
GET https://api.kvass.ai/events/<event_id>
```
> Example request:

``` http
GET /events/5964a0ead57ba2036750a3b4 HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "_id": {"$oid": "5964a0ead57ba2036750a3b4"},
    "kind": "order.created",
    "created": {"$date": 1499767018360},
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "data": {
        "object_type": "order",
        "kind": "order.created",
        "object": {
            "units":2,
            "delivery_status":"ACCEPTED",
            "currency":"NOK",
            "human_id":"51Q4LN",
            "payments":[],
            "modified":{"$date":1495196003948},
            "provider":{<provider_details>},
            "top_up_amount":0.0,
            "stripe_charge_id":"",
            "order_status":"CREATED",
            "company":{"$oid":"591ee15db70e2a10acb65362"},
            "billing_address":{<billing_address_details>},
            "deliveries":[],
            "items":[{"quantity":60, "discount":0.9, "product":{<product_details>}}],
            "note":"",
            "created":{"$date":1495264401233},
            "delivery_address":{<address_details>},
            "total_quantity":120,
            "_id":{"$oid":"591ee163b70e2a10acb653a7"},
            "total_amount":1200.0,
            "stripe_refund_id":"",
            "override_company_take":-1.0,
            "user":{<user_details>},
            "delivery_time":{"$date":1497009600000}
        }
    },
    "author": {
                  "_id":{"$oid":"57ee9c72d76d431f85111432"},
                  "_cls":"User",
                  "created":{"$date":1475428903950},
                  "modified":{"$date":1475428903951},
                  "first_name":"John",
                  "last_name":"Doe",
                  "email":"john@email.com",
                  "mobile_phone_number":"+4712345678",
                  "addresses":[],
                  "billing_address":{},
                  "bio":"",
                  "note":"",
                  "avatar":"",
                  "company":{"$oid":"57ee9c71d76d431f8511142f"},
                  "tags":[],
                  "stripe_customer_id":"",
                  "roles":["user"],
                  "deleted":true
              }
}
```

## Events Search

Retrieves a list of Events associated with search.

> Definition

```
GET https://api.kvass.ai/events/search
```

> Example request by account_number:

``` http
GET /events/search/query=order HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "_id": {"$oid": "5964a0ead57ba2036750a3b4"},
        "kind": "order.created",
        "created": {"$date": 1499767018360},
        "company": {"$oid": "57ee9c71d76d431f8511142f"},
        "data": {
            "object_type": "order",
            "kind": "order.created",
            "object": {
                "units":2,
                "delivery_status":"ACCEPTED",
                "currency":"NOK",
                "human_id":"51Q4LN",
                "payments":[],
                "modified":{"$date":1495196003948},
                "provider":{<provider_details>},
                "top_up_amount":0.0,
                "stripe_charge_id":"",
                "order_status":"CREATED",
                "company":{"$oid":"591ee15db70e2a10acb65362"},
                "billing_address":{<billing_address_details>},
                "deliveries":[],
                "items":[{"quantity":60, "discount":0.9, "product":{<product_details>}}],
                "note":"",
                "created":{"$date":1495264401233},
                "delivery_address":{<address_details>},
                "total_quantity":120,
                "_id":{"$oid":"591ee163b70e2a10acb653a7"},
                "total_amount":1200.0,
                "stripe_refund_id":"",
                "override_company_take":-1.0,
                "user":{<user_details>},
                "delivery_time":{"$date":1497009600000}
            }
        },
        "author": {
                      "_id":{"$oid":"57ee9c72d76d431f85111432"},
                      "_cls":"User",
                      "created":{"$date":1475428903950},
                      "modified":{"$date":1475428903951},
                      "first_name":"John",
                      "last_name":"Doe",
                      "email":"john@email.com",
                      "mobile_phone_number":"+4712345678",
                      "addresses":[],
                      "billing_address":{},
                      "bio":"",
                      "note":"",
                      "avatar":"",
                      "company":{"$oid":"57ee9c71d76d431f8511142f"},
                      "tags":[],
                      "stripe_customer_id":"",
                      "roles":["user"],
                      "deleted":true
                  }
    },
    {
        "_id": {"$oid": "5964a0ead57ba2036750a3b4"},
        "kind": "order.updated",
        "created": {"$date": 1499767018360},
        "company": {"$oid": "57ee9c71d76d431f8511142f"},
        "data": {
            "object_type": "order",
            "kind": "order.updated",
            "object": {
                "units":2,
                "delivery_status":"ACCEPTED",
                "currency":"NOK",
                "human_id":"51Q4LN",
                "payments":[],
                "modified":{"$date":1495196003948},
                "provider":{<provider_details>},
                "top_up_amount":0.0,
                "stripe_charge_id":"",
                "order_status":"CREATED",
                "company":{"$oid":"591ee15db70e2a10acb65362"},
                "billing_address":{<billing_address_details>},
                "deliveries":[],
                "items":[{"quantity":10, "discount":0.9, "product":{<product_details>}}],
                "note":"",
                "created":{"$date":1495264401233},
                "delivery_address":{<address_details>},
                "total_quantity":10,
                "_id":{"$oid":"591ee163b70e2a10acb653a7"},
                "total_amount":500.0,
                "stripe_refund_id":"",
                "override_company_take":-1.0,
                "user":{<user_details>},
                "delivery_time":{"$date":1497009600000}
            },
            "previous_data": {
                "items":[{"quantity":60, "discount":0.9, "product":{<product_details>}}],
                "total_quantity":120,
                "total_amount":1200.0
            }
        },
        "author": {
                      "_id":{"$oid":"57ee9c72d76d431f85111432"},
                      "_cls":"User",
                      "created":{"$date":1475428903950},
                      "modified":{"$date":1475428903951},
                      "first_name":"John",
                      "last_name":"Doe",
                      "email":"john@email.com",
                      "mobile_phone_number":"+4712345678",
                      "addresses":[],
                      "billing_address":{},
                      "bio":"",
                      "note":"",
                      "avatar":"",
                      "company":{"$oid":"57ee9c71d76d431f8511142f"},
                      "tags":[],
                      "stripe_customer_id":"",
                      "roles":["user"],
                      "deleted":true
                  }
    }
]
```

Arguments | Type | Description
--------- | ---- | -------
**query** | `string` | What you want to search, like **id** or **kind**
size | `number` | number of items to retrieve
page | `number` | which page to retrieve. _default page size is 10_
sort | `string` | field used for sorting results. If missing default is "-created"
from_date | `number` | Start date, `timestamp` format. _default is None_
to_date | `number` | End date, `timestamp` format. _default is None_

## List of Event Types

Model name | Description
---------- | ------
`order.created` | Order created
`order.updated` | Order updated
`order.cancelled` | Order cancelled
`invoice.created` | Invoice created
`invoice.updated` | Invoice updated
`invoice.deleted` | Invoice deleted
`user.created` | User created
`user.updated` | User updated
`user.deleted` | User deleted
`subscription.created` | Subscription created
`subscription.updated` | Subscription updated
`subscription.deleted` | Subscription deleted
`subscription.activated` | Subscription activated
`subscription.future` | Subscription postponed
`subscription.completed` | Subscription completed
`subscription.non_renewing` | Subscription stopping the next cycle
`subscription.cancelled` | Subscription cancelled
`payment.created` | Payment created
`payment.updated` | Payment updated
`payment.deleted` | Payment deleted
`payment.succeeded` | Payment succeeded
`payment.processing` | Payment processing
`payment.failed` | Payment failed
`payment.captured` | Payment captured
`payment.cancelled` | Payment cancelled
`product.created` | Product created
`product.updated` | Product updated
`product.deleted` | Product deleted
`plan.created` | Plan created
`plan.updated` | Plan updated
`plan.deleted` | Plan deleted
