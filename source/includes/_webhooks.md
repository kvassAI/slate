# Webhooks

Webhooks allow you to keep track of your data without having to constantly poll our APIs for updates.

## Webhook Object

Attribute | Type | Description
--------- | ---- | -------
_id | `object` | The subscription ID.
event_type | `string` | The type of [`Event`](#events). Can be any from the [`event type list`](#list-of-event-types)
endpoint | `string` | The URL to call when the webhook is triggered
secret | `string` | A SHA-256 secret used to secure the webhook communication
active | `boolean` | Sets if related `Events` is dispatched. _Default is `true`_
deleted | `boolean` | Whether the webhook is deleted _Default is `false`_
verified | `boolean` | Whether the webhook endpoint is verified. _Default is `false`_

## Create a Webhook

> Definition

```
POST https://api.kvass.ai/webhooks
```


> Example request:

``` http
POST /webhooks HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "endpoint": "http://requestbin.fullcontact.com/asdfasdf",
    "event_type": "order.created"
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json
{
    "_id": {"$oid": "5964a0ead57ba2036750a3b4"},
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "created": {"$date": 1499767018360},
    "updated": {"$date": 1499767018360},
    "event_type": "order.created",
    "deleted": false,
    "secret": "ed06e2f4e7bdb5ee6050695aba5ed3f2725fd3394df22f3a8a0c2856123a",
    "endpoint": "http://requestbin.fullcontact.com/1o2mfpb1",
    "verified": false,
    "active": true
}
```

Attribute | Type | Description
--------- | ---- | -------
**event_type** | `string` | The type of [`Event`](#events). Can be any from the [`event type list`](#list-of-event-types)
**endpoint** | `string` | The URL to call when the webhook is triggered


## Update Webhook

> Definition

```
PUT https://api.kvass.ai/webhooks/<webhook_id>
```
> Example request:

``` http
PUT /webhooks/5964a0ead57ba2036750a3b4 HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "endpoint": "https://www.google.com"
}
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json
{
    "_id": {"$oid": "5964a0ead57ba2036750a3b4"},
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "created": {"$date": 1499767018360},
    "updated": {"$date": 1499767018360},
    "event_type": "order.created",
    "deleted": false,
    "secret": "ed06e2f4e7bdb5ee6050695aba5ed3f2725fd3394df22f3a8a0c2856123a",
    "endpoint": "https://www.google.com",
    "verified": false,
    "active": true
}
```

Attribute | Type | Description
--------- | ---- | -------
event_type | `string` | The type of [`Event`](#events). Can be any from the [`event type list`](#list-of-event-types)
endpoint | `string` | The URL to call when the webhook is triggered

Note that if the `endpoint` is updated, then you will need to [verify](#verify-the-webhook) the webhook
before related `Events` is dispatched.

## Receive Webhook by ID

> Definition

```
GET https://api.kvass.ai/webhooks/<webhook_id>
```
> Example request:

``` http
GET /webhooks/5964a0ead57ba2036750a3b4 HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "_id": {"$oid": "5964a0ead57ba2036750a3b4"},
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "created": {"$date": 1499767018360},
    "updated": {"$date": 1499767018360},
    "event_type": "order.created",
    "deleted": false,
    "secret": "ed06e2f4e7bdb5ee6050695aba5ed3f2725fd3394df22f3a8a0c2856123a",
    "endpoint": "https://www.google.com",    
    "verified": true,
    "active": true
}
```

## Get All Webhooks
Receives a list of all webhooks associated with a company.

> Definition

```
GET https://api.kvass.ai/webhooks
```
> Example request:

``` http
GET /webhooks HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "_id": {"$oid": "5964a0ead57ba2036750a3b4"},
        "company": {"$oid": "57ee9c71d76d431f8511142f"},
        "created": {"$date": 1499767018360},
        "updated": {"$date": 1499767018360},
        "event_type": "order.created",
        "deleted": false,
        "secret": "ed06e2f4e7bdb5ee6050695aba5ed3f2725fd3394df22f3a8a0c2856123a",
        "endpoint": "https://www.google.com",
        "verified": true,
        "active": true
    }
]
```

Attribute | Type | Description
--------- | ---- | -------
size | `number` | Number of subscriptions per page _default is 10_
page | `number` | Which page to retrieve _default is 0_
sort | `string` | Field used to sort results _default is `-modified`_

## Delete webhook

Deletes a webhook with a given ID.

> Definition

```
DELETE https://api.kvass.ai/webhooks/<webhook_id>
```

> Example request:

``` http
DELETE /webhooks/<webhook_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "_id": {"$oid": "5964a0ead57ba2036750a3b4"},
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "created": {"$date": 1499767018360},
    "updated": {"$date": 1499767018360},
    "event_type": "order.created",
    "deleted": true,
    "secret": "ed06e2f4e7bdb5ee6050695aba5ed3f2725fd3394df22f3a8a0c2856123a",
    "endpoint": "https://www.google.com",
    "verified": true,
    "active": true
}
```

Argument | Type | Description
-------- | ---- | -----
**webhook_id** | `string` | ID of the Webhook


## Verify the Webhook

The webhook has to verified before related data on related `Events` is dispatched. KVASS tries
to verify the endpoint when the webhook is created. If the endpoint returns an HTTP status code
2XX, then the webhook is set to `verified`.

If an endpoint is updated, then the webhook is not verified anymore. It is possible to manually try to verify
the endpoint by doing the following request.

 
> Definition

```
POST https://api.kvass.ai/webhooks/<webhook_id>/verify
```

> Example request:

``` http
POST /webhooks/<webhook_id>/verify HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "_id": {"$oid": "5964a0ead57ba2036750a3b4"},
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "created": {"$date": 1499767018360},
    "updated": {"$date": 1499767018360},
    "event_type": "order.created",
    "deleted": true,
    "secret": "ed06e2f4e7bdb5ee6050695aba5ed3f2725fd3394df22f3a8a0c2856123a",
    "endpoint": "https://www.google.com",
    "verified": true,
    "active": true
}
```

## Simulate a Webhook

It is possible to simulate the response from a webhook to check the format of the webhook data.

 > Definition

```
POST https://api.kvass.ai/webhooks/<webhook_id>/simulate
```

> Example request:

``` http
POST /webhooks/<webhook_id>/simulate HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```
``` http
HTTP/1.1 204 OK
Content-Type: application/json

{}
```