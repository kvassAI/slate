# Plans

The Plans method is utilized by the [`Subscriptions`](#subscriptions) method.
Its main feature is to set the billing cycle, currency, product items and
the duration of a subscription. The interval_unit and the billing_interval
are the most important fields and define the period of time between
each subscription action and each payment. The next billing and interval dates are
calculated from the starting date of the subscription.

Only Admins could create, edit or delete a plan.
If your company want to allow your customers to create plans,
see `Create Subscription with a pre-set company specific Plan` in the [`Subscriptions`](#subscriptions).

## Plans Object

Attribute | Type | Description
--------- | ---- | -------
name | `string` | The name of the plan.
note | `string` | A short description of the plan.
interval_unit | `string` | The frequency that the subscription acts upon. <br> Choices: `DAY`, `WEEK`, `MONTH`, <br> `MONTH_END`, `ANNUAL`
interval_count | `integer`| Total number of interval_units.
billing_interval | `string`| Defines billing frequency. <br> Choices: `WEEK`, `MONTH`, `MONTH_END`
static | `boolean`| Defines if plan is allowed to be changed.
items | `array` | List of items associated with Plan. See description below.
units | `number` | Total number of items in the plan.
currency | `string` | Three letter ISO currency code as defined by ISO 4217.
total_amount | `integer` | The amount to be charged on the interval specified. If missing, this will be calculated as the sum of the `items`.
discount | `number` | Set a discount on a plan. Min 0.0, max 1.0. This will be calculated on the total_amount.
initial_fail_amount_action | `string` | Decides what happens if a payment fails in the [`Subscription`](#subscriptions). <br> Choices: `CANCEL`, `CONTINUE`
max_fail_attempts | `integer`| If `initial_fail_amount_action` is `CONTINUE`, this is the number of interval_units that is allowed to fail before the [`Subscription`](#subscriptions) stops.
prorate | `boolean` | Flag telling us whether to prorate amount if user stops the subscription in the middle of a billing cycle.


### Plan items

The plan could contain items as a list. These items describe the products the plan is based on.
The items consist of a [`product`](#products), `quantity` and `discount`
The `quantity` describes the number of that product. The `discount` sets the percent of
discount of that product, from 0.0 to 1.0, where 0.0 is no discount and 1.0 is a 100% discount.
The `total_amount` of the plan will be calculated from the total sum of the products.

## Create a new Plan

This creates a new plan. The plan is not [`user`](#users) specific,
but a user could create a plan based on their demands. If you
want a non-customizable plan, <br> set `static_plan` to `true`
when creating a new plan.

> Definition

```
POST https://api.kvass.ai/plans
```

> Example request:

``` http
POST /plans HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.shareactor.io

{
    "interval_unit": "WEEK",
    "name": "Plan Deluxe",
    "currency": "NOK",
    "static": true,
    "billing_interval": "MONTH",
    "items": [{"product": "<product1_id>", "quantity": 1},
              {"product": "<product2_id>", "quantity": 2, "discount": 0.5}],
    "initial_fail_amount_action": "CONTINUE",
    "max_fail_attempts": 1,
    "note": "This is a note regarding the Plan",
    "interval_count": false,
    "total_amount": false,
    "discount": 0,
    "prorate": false
}

```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "_id": {
        "$oid": "5931697ed57ba271c0c7de66"},
    "created": {
        "$date": 1496410494652},
    "updated": {
        "$date": 1496410494652},
    "auto_bill_amount": false,
    "billing_interval": "MONTH",
    "initial_fail_amount_action": "CONTINUE",
    "max_fail_attempts": 1,
    "discount": 0.0,
    "name": "Plan Deluxe",
    "total_amount": 300.0,
    "interval_unit": "WEEK",
    "items": [
        {"discount": 0.0,
         "product": {"$oid": "5931697ed57ba271c0c7de64"},
         "quantity": 1},
        {"discount": 0.5,
         "product": {"$oid": "5931697ed57ba271c0c7de65"},
         "quantity": 2}],
    "status": "CREATED",
    "currency": "NOK",
    "note": "This is a note regarding the Plan",
    "deleted": false,
    "company": {
        "$oid": "57ee9c71d76d431f8511142f"},
    "static": true,
    "prorate": false
}
```

Argument | Type | Description
-------- | ---- | -------
**interval_unit** | `string` | The frequency that the Subscription acts upon. <br> interval_unit choices: `DAY`, `WEEK`, `MONTH`, `MONTH_END`, `ANNUAL`
**billing_interval** | `string`| Defines billing frequency. <br> Choices: `WEEK`, `MONTH`, `MONTH_END`
**currency** | `string`| Three letter ISO currency code as defined by ISO 4217.7


## Retrieve a Plan

Retrieve a particular Plan, based on its id.

> Definition

```
GET https://api.kvass.ai/plans/<plan_id>
```

> Example request:

``` http
GET /plans/<plan_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "_id": {
        "$oid": "5931697ed57ba271c0c7de66"},
    "created": {
        "$date": 1496410494652},
    "updated": {
        "$date": 1496411756242},
    "items": [
        {"discount": 0.0,
         "product": {"$oid": "5931697ed57ba271c0c7de64"},
         "quantity": 1},
        {"discount": 0.5,
         "product": {"$oid": "5931697ed57ba271c0c7de65"},
         "quantity": 2}],
     "deleted": false,
     "discount": 0.0,
     "billing_interval": "MONTH", 
     "currency": "NOK", 
     "name": "Golden Plan",
     "interval_unit": "WEEK",
     "static": false,
     "total_amount": 500.0,
     "company": {
        "$oid": "57ee9c71d76d431f8511142f"},
    "prorate" false
 }
```

Argument | Type | Description
-------- | ---- | -------
**plan_id** |`string`| The unique id of a particular plan

## Get list of all Plans associated with Company

> Definition

```
GET https://api.kvass.ai/plans
```

> Example request:

``` http
GET /plans HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.shareactor.io
```

```http
HTTP/1.1 200 OK
Content-Type: application/json
[
    {
        "created": {
            "$date": 1496412265911
        },
        "interval_unit": "WEEK",
        "currency": "NOK",
        "name": "Golden Plan",
        "updated": {
            "$date": 1496412265911
        },
        "static": false,
        "billing_interval": "MONTH",
        "_id": {
            "$oid": "59317069d57ba2781c739479"
        },
        "discount": 0.0,
        "total_amount": 500.0,
        "company": {
            "$oid": "57ee9c71d76d431f8511142f"
        },
        "deleted": false,
        "items": [{
                "discount": 0.0,
                "quantity": 1,
                "product": {
                    "$oid": "59317069d57ba2781c739477"
                }
            },
            {
                "discount": 0.0,
                "quantity": 2,
                "product": {
                    "$oid": "59317069d57ba2781c739478"
                }
            }
        ],
        "status": "CREATED",
        "prorate" false
    },
    {
        "created": {
            "$date": 1496412265930
        },
        "interval_unit": "WEEK",
        "...": "..."
    }
]
```

Argument | Type | Description
-------- | ---- | -------
size | `integer` | Number of items to retrieve.
page | `integer` | Which page to retrieve. _default page size is 50_
sorting | `string`| Field used for sorting results. <br> If missing, the default is `-created`.
include_deleted| `boolean` | If `true`, deleted products are also listed.


## Update Plan

Update a Plan. This is only possible if `static_plan` is `false`.
Not all fields are possible to update. See fields in Argument below.
> Definition

```
PUT https://api.kvass.ai/plans/<plan_id>
```

> Example request:

``` http
PUT /plans HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.shareactor.io
```

Argument | Type | Description
-------- | ---- | -----
**plan_id** | `string` | ID of the queried plan.
active |`boolean`| Indicates if the plan is currently active. _default is `true`_
name |`string`| Name of the plan.
note |`string`| Note regarding the plan.
initial_fail_amount_action |`string`| Indicates what happens when payment for the plan fails.  Choices: `CONTINUE`, `CANCEL`
max_fail_attempts |`number`| How many times a payment can fail in the subscription, but still continue.
discount |`number`| Between 0.0 an 1.0, where 1.0 is 100% discount on the total_price.
total_amount |`number`| The total cost of the subscription payment. If set, the total_amount is not based on the sum of products.


## Delete Plan
> Definition

```
DELETE https://api.kvass.ai/plans/<plan_id>
```

> Example request:

``` http
DELETE /plans/<plans_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.shareactor.io
```

Deletes a plan with a given ID.
