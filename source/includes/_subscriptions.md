# Subscriptions

The Subscription method enables you to charge [`Customers`](#Users) at
specified intervals set by a [`Plan`](#Plans) of their choice.

We offer a `licence` subscription. The Customers will be
charged the amount specified in the Plan, at an interval set by the
Plan.

## Subscriptions Object

Attribute | Type | Description
--------- | ---- | -------
name | `string` | The name of the Subscription type
note | `string` | A short description
user | `object`  | [`User`](#Users) ID associated with Subscription
method | `string`| Subscription method. For now set `licence`
status | `string` | Status for the subscription. Default is `CREATED`. Additional: `ACTIVE`, `FUTURE`, `NON_RENEWING`, `CANCELLED`
plan | `object` | ID of the [`Plan`](#Plans) associated with the subscription.
payment_method | `string` | ID of already created Payment Method
payments | `array` | Array of [`Payment`](#payments) objects associated with Subscription
starting_date | `string`| Timestamp for starting date of the subscription. If missing, default starting date is now.
ending_date | `string`| Timestamp for ending date of the subscription.
interval_total | `number` | Number of intervals, set in the [`Plan`](#Plans), the subscription will run.
infinite | `boolean` | If `true`, the subscription will run until the User stops it.
current_billing_date_period_start | `string`| The date on which the customer was billed last.
current_billing_date_period_end | `string`| The date on which the customer will be billed next. This will also be the date on which the next current_billing_date_period_start of the subscription starts.
prorate | `boolean` | flag telling us whether to prorate amount if User stops it in the middle of a billing cycle.
prorate_amount | `number` | The amount prorated. Currency set in [`Plan`](#Plans).

## Create a new subscription
To create a new subscription, The User needs to have a plan associated with the
subscription. Additionally, the User needs to specify a subscription method.
This will not start the subscription, only create it.

> Definition

```
POST https://api.shareactor.io/subscriptions
```


> Example request:

``` http
POST /subscriptions HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io

{
    "plan": "5931697ed57ba271c0c7de66",
    "subscription_method": "licence"
}
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json
{
    "prorate": false,
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "active": true,
    "status": "CREATED",
    "name": "LICENSE",
    "method": "license",
    "_id": {"$oid": "5964a0ead57ba2036750a3b4"},
    "trial": false,
    "deleted": false,
    "prorate_amount": 0.0,
    "_cls": "SubscriptionMethod.LicenseSubscription",
    "plan": {"$oid": "5931697ed57ba271c0c7de66"},
    "payments": [],
    "infinite": false,
    "created": {"$date": 1499767018360},
    "user": {"$oid": "57ee9c72d76d431f85111432"},
    "updated": {"$date": 1499767018360}
```

Attribute | Type | Description
--------- | ---- | -------
**method** | `string`| Subscription method. For now set `licence`
**plan** | `string` | ID of the [`Plan`](#Plans) associated with the subscription.
note | `string` | A short description
prorate | `boolean` | flag telling us whether to prorate amount if </br> User stops it in the middle of a billing cycle.

## Start subscription
To start a subscription, the User need to first create a subscription.
The User could create and start the subscription in one API call. Then you
will have to add `{"start_now": true}` when creating a new subscription.
When starting the subscription, we need an associated payment_method.

A subscription could either have a set ending_date, thus infinite = False,
or not have an ending_date, thus infinite = True.
The ending date could be set by the User, either as an ending_date
or as number of interval_units from the starting_date to the ending_date.
If the User set more than one of the parameters
infinite, ending_date or interval_total, the API overrides the
parameters by: infinite > ending_date > interval_total.

> Definition

```
POST https://api.shareactor.io/subscriptions/<subscription_id>/start
```
> Example request:

``` http
POST /subscriptions/5964a0ead57ba2036750a3b4/start HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io

{
    "payment_method": "<payment-method-id>"
}
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "plan": {"$oid": "5931697ed57ba271c0c7de66"},
    "payment_method": {"$oid": "<payment-method-id>"},
    "user": {"$oid": "57ee9c72d76d431f85111432"},
    "prorate_amount": 0.0,
    "name": "LICENSE",
    "payments": [],
    "status": "ACTIVE",
    "active": true,
    "updated": {"$date": 1499768160086},
    "prorate": false,
    "_cls": "SubscriptionMethod.LicenseSubscription",
    "infinite": false,
    "trial": false,
    "starting_date": {"$date": 1499854560000},
    "ending_date": {"$date": 1511954560000},
    "created": {"$date": 1499767018360},
    "current_billing_date_period_end": {"$date": 1502532960000},
    "current_billing_date_period_start": {"$date": 1499854560000},
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "deleted": false,
    "method": "license",
    "_id": {"$oid": "5964a0ead57ba2036750a3b4"}
}
```

Attribute | Type | Description
--------- | ---- | -------
**payment_method** | `string` | ID of already created Payment Method
starting_date | `string`| Timestamp for starting date of the subscription. If missing, default starting date is now.
ending_date | `string`| Timestamp for ending date of the subscription.
interval_total | `number` | Number of intervals, set in the [`Plan`](#Plans), the subscription will run.
infinite | `boolean` | If `true`, the subscription will run until the User stops it.

## Update subscription
The User could update some fields in the subscription. Like when starting
the subscription, the User could only update one of infinite, ending_date, interval_total.

> Definition

```
POST https://api.shareactor.io/subscriptions/<subscription_id>/update
```
> Example request:

``` http
POST /subscriptions/5964a0ead57ba2036750a3b4/update HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io

{
    "note": "A note regarding the subscription",
    "infinite": true
}
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "plan": {"$oid": "5931697ed57ba271c0c7de66"},
    "payment_method": {"$oid": "<payment-method-id>"},
    "user": {"$oid": "57ee9c72d76d431f85111432"},
    "prorate_amount": 0.0,
    "name": "LICENSE",
    "note": "A note regarding the subscription",
    "payments": [],
    "status": "ACTIVE",
    "active": true,
    "updated": {"$date": 1499773617984},
    "prorate": false,
    "_cls": "SubscriptionMethod.LicenseSubscription",
    "infinite": true,
    "trial": false,
    "starting_date": {"$date": 1499854560000},
    "created": {"$date": 1499767018360},
    "current_billing_date_period_end": {"$date": 1502532960000},
    "current_billing_date_period_start": {"$date": 1499854560000},
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "deleted": false,
    "method": "license",
    "_id": {"$oid": "5964a0ead57ba2036750a3b4"}
}
```

Attribute | Type | Description
--------- | ---- | -------
note | `string` | A short description
interval_total | `number` | Number of intervals, set in the [`Plan`](#Plans), the subscription will run.
infinite | `boolean` | If `true`, the subscription will run until the User stops it.
ending_date | `string`| Timestamp for ending date of the subscription.
payment_method | `string` | ID of already created Payment Method

## Stop subscription
To stop the subscription. The User could either choose stop it on a chosen
`ending_date` or, if missing, now. The remaining amount will be prorated
to the User at the next `current_billing_date_period_end` if `prorate`
is `true`.


> Definition

```
POST https://api.shareactor.io/subscriptions/<subscription_id>/stop
```
> Example request:

``` http
POST /subscriptions/5964a0ead57ba2036750a3b4/stop HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io

```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "plan": {"$oid": "5931697ed57ba271c0c7de66"},
    "payment_method": {"$oid": "<payment-method-id>"},
    "user": {"$oid": "57ee9c72d76d431f85111432"},
    "prorate_amount": -428.57,
    "name": "LICENSE",
    "note": "A note regarding the subscription",
    "payments": [],
    "status": "NON_RENEWING",
    "active": true,
    "updated": {"$date": 1499773617984},
    "prorate": false,
    "_cls": "SubscriptionMethod.LicenseSubscription",
    "infinite": false,
    "trial": false,
    "starting_date": {"$date": 1499854560000},
    "ending_date": {"$date": 1499860512000},
    "created": {"$date": 1499767018360},
    "current_billing_date_period_end": {"$date": 1502532960000},
    "current_billing_date_period_start": {"$date": 1499854560000},
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "deleted": false,
    "method": "license",
    "_id": {"$oid": "5964a0ead57ba2036750a3b4"}
}
```

Attribute | Type | Description
--------- | ---- | -------
ending_date | `string`| Timestamp for ending date of the subscription.


## Receive Subscription by id
Receive a subscription, based on its id.

> Definition

```
GET https://api.shareactor.io/subscriptions/<subscription_id>
```
> Example request:

``` http
GET /subscriptions/5964a0ead57ba2036750a3b4 HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "plan": {"$oid": "5931697ed57ba271c0c7de66"},
    "payment_method": {"$oid": "<payment-method-id>"},
    "user": {"$oid": "57ee9c72d76d431f85111432"},
    "prorate_amount": -428.57,
    "name": "LICENSE",
    "note": "A note regarding the subscription",
    "payments": [],
    "status": "NON_RENEWING",
    "active": true,
    "updated": {"$date": 1499773617984},
    "prorate": false,
    "_cls": "SubscriptionMethod.LicenseSubscription",
    "infinite": false,
    "trial": false,
    "starting_date": {"$date": 1499854560000},
    "ending_date": {"$date": 1499860512000},
    "created": {"$date": 1499767018360},
    "current_billing_date_period_end": {"$date": 1502532960000},
    "current_billing_date_period_start": {"$date": 1499854560000},
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "deleted": false,
    "method": "license",
    "_id": {"$oid": "5964a0ead57ba2036750a3b4"}
}
```

## Get all subscriptions for an User
Receives a list of all subscriptions associated with the user.
It is possible to add pagination to this request.

> Definition

```
GET https://api.shareactor.io/users/<user_id>/subscriptions
```
> Example request:

``` http
GET /users/57ee9c72d76d431f85111432/subscriptions HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

Attribute | Type | Description
--------- | ---- | -------
user_id | `string`| The ID for the user
include_deleted | `boolean`| if `true` the request also returns all deleted subscriptions
size | `number` | Number of subscriptions per page. Default is 10.
page | `number` | Default 0, the first page.
sort | `string` | Default is `-modified`, thus last modified first.
status | `string` | The subscription status. Default is `ACTIVE`.

## Get all subscriptions
Receives a list of all subscriptions associated with company.

> Definition

```
GET https://api.shareactor.io/subscriptions
```
> Example request:

``` http
GET /subscriptions HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

Attribute | Type | Description
--------- | ---- | -------
include_deleted | `boolean`| if `true` the request also returns all deleted subscriptions
size | `number` | Default 10. Number of subscriptions per page.
page | `number` | Default 0.
sort | `string` | Default is `-modified`, thus last modified first.
status | `string` | The subscription status. Default is `ACTIVE`.
