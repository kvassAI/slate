# Subscriptions

The Subscription method enables you to charge [`Customers`](#Users) at
specified intervals set by a [`Plan`](#Plans) of their choice.

We offer a `license` subscription method. The customers will be
charged the amount specified in the plan at an interval set by the
plan.

## Subscriptions Object

Attribute | Type | Description
--------- | ---- | -------
_id | `object` | The subscription ID.
method | `string` | Name of subscription method. `license`
name | `string` | The name of the subscription type.
note | `string` | A short description of the description.
user | `object`  | [`User`](#Users) ID associated with the subscription.
status | `string` | Status for the subscription. _default is `CREATED`, also available are `ACTIVE`, `FUTURE`, `NON_RENEWING`and `CANCELLED`_
plan | `object` | ID of the [`Plan`](#Plans) associated with the subscription.
payment_method | `object` | ID of already created payment method.
payments | `array` | Array of [`Payment`](#payments) objects associated with the subscription.
starting_date | `string`| Start date, `timestamp` format. _default is current date if missing_ 
ending_date | `string`| End date of subscription, `timestamp` format .
interval_total | `number` | Number of intervals set in the [`Plan`](#Plans) the subscription will run.
infinite | `boolean` | If `true`, the subscription will run until the user stops it.
current_billing_date_period_start | `string`| The date on which the customer was billed last.
current_billing_date_period_end | `string`| The date on which the customer will be billed next. This will also be the date on which the next current_billing_date_period_start of the subscription starts.
prorate_amount | `number` | The amount prorated. Currency set in [`Plan`](#Plans).
prorate_date | `number`| The date of the last time an amount was prorated
last_billing_amount | `number`| The amount that was subtracted at the last payment. `plan.total_amount`+`prorate_amount`
total_fail_attempts | `number` | The number of failed payment attempts in the subscription. See [`Plan`](#Plans)`.max_fail_attempts

## Create a New Subscription
To create a new subscription, the user needs to have a plan associated with the
subscription. Additionally, the user needs to specify a subscription method.
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
    "subscription_method": "license"
}
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json
{
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "active": true,
    "status": "CREATED",
    "name": "LICENSE",
    "method": "license",
    "_id": {"$oid": "5964a0ead57ba2036750a3b4"},
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
**plan** | `string` | ID of the [`Plan`](#Plans) associated with the subscription.
**subscription_method** | `string` | Name of subscription method. `license`
note | `string` | A short description.
start_now | `boolean`| If `true`, the subscription will start now.

## Start Subscription
To start a subscription, the user needs to first create a subscription.
The user could create and start the subscription in one API call. Then you
will have to add `{"start_now": true}` when creating a new subscription.
When starting the subscription, we need an associated payment_method in the request data.

A subscription could either have a set ending_date, where infinite = `false`,
or not have an ending_date, where infinite = `true`.
The ending date could be set by the user, either as an ending_date
or as number of interval_units from the starting_date to the ending_date.
If the user set more than one of the parameters
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
    "_cls": "SubscriptionMethod.LicenseSubscription",
    "infinite": false,
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
**payment_method** | `string` | ID of an already created payment method.
**subscription_method** | `string` | Name of subscription method. `license`
starting_date | `string`| Start date, `timestamp` format. _default is current date if missing_ 
ending_date | `string`| End date, `timestamp` format.
interval_total | `number` | Number of intervals, set in the [`Plan`](#Plans), the subscription will run.
infinite | `boolean` | If `true`, the subscription will run until the user stops it.

## Create Subscription with a pre-set company specific Plan

If your company only allows one type of [`plan`](#Plans), this method could be utilized.
The customer could add their `plan.items` and name of the plan
when creating a new subscription, and the rest of the plan is set by the
company specifics.

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

{"plan": {"name": "Plan Name",
          "items": [{"product": "<product_id>", "quantity": 2}],
          },
"subscription_method": "license",
"start_now": true,
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
    "prorate_amount": 0.0,'
    "name": "LICENSE",
    "payments": [],
    "status": "ACTIVE",
    "active": true,
    "updated": {"$date": 1499768160086},
    "_cls": "SubscriptionMethod.LicenseSubscription",
    "infinite": false,
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
**subscription_method** | `string` | Name of subscription method
**plan** | `array`| An array with name and items regarding the [`plan`](#Plans).
start_now | `boolean`| If true, the subscription will start now. Needs a payment_method if `start_now` is true.
payment_method | `string` | ID of an already created payment method.
starting_date | `string`| Start date, `timestamp` format. _default is current date if missing_
ending_date | `string`| End date, `timestamp` format.
interval_total | `number` | Number of intervals, set in the [`Plan`](#Plans), the subscription will run.
infinite | `boolean` | If `true`, the subscription will run until the user stops it.


## Update Subscription
The user could update certain fields in the subscription. If, when starting
the subscription, the user could only update one of the fields infinite, ending_date or interval_total, they could use this to update the fields.

> Definition

```
PUT https://api.shareactor.io/subscriptions/<subscription_id>
```
> Example request:

``` http
PUT /subscriptions/5964a0ead57ba2036750a3b4 HTTP/1.1
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
    "_cls": "SubscriptionMethod.LicenseSubscription",
    "infinite": true,
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
interval_total | `number` | Number of intervals, set in the [`Plan`](#Plans), the subscription will run
infinite | `boolean` | If `true`, the subscription will run until the User stops it
ending_date | `string`| End date, `timestamp` format
payment_method | `string` | ID of an already created payment method

## Stop Subscription
To stop the subscription, the user could either choose to stop it on a chosen
`ending_date` or, if missing, at the current time. The remaining amount will be prorated
to the user at the next `current_billing_date_period_end` if [`Plan`](#Plans).`prorate`
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
    "_cls": "SubscriptionMethod.LicenseSubscription",
    "infinite": false,
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
ending_date | `string`| End date, `timestamp` format


## Receive Subscription by ID
Receive a subscription, based on its ID.

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
    "_cls": "SubscriptionMethod.LicenseSubscription",
    "infinite": false,
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

## Get All Subscriptions for a User
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
include_deleted | `boolean`| If `true`, the request also returns all deleted subscriptions
size | `number` | Number of subscriptions per page _default is 10_
page | `number` | Which page to retrieve _default is 0_
sort | `string` | Field used to sort results _default is `-modified`_
status | `string` | The subscription status _default is `ACTIVE`_

## Get All Subscriptions
Receives a list of all subscriptions associated with a company.

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
include_deleted | `boolean`| If `true`, the request also returns all deleted subscriptions
size | `number` | Number of subscriptions per page _default is 10_
page | `number` | Which page to retrieve _default is 0_
sort | `string` | Field used to sort results _default is `-modified`_
status | `string` | The subscription status _default is `ACTIVE`_
