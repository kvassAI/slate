# Plans

The Plan is a method utilized by the [`Subscription`](#subscriptions) method.
Its main feature is to set the billing cycle, currency, product items, and
the duration of the subscription. The interval_unit and the billing_interval
is the most important fields and defines the period of time between
each subscription action and each payment. The next billing- and interval dates are
calculated from the starting date of the subscription.

## Plans Object

Attribute | Type | Description
--------- | ---- | -------
name | `string` | The name of the Plan
note | `string` | A short description
interval_unit | `string` | The frequency that the Subscription acts upon. <br> interval_unit choices: `DAY`, `WEEK`, `MONTH`, <br> `MONTH_END`, `ANNUAL`
interval_count | `integer`| Total umber of intervals_units
billing_interval | `string`| Defines billing frequency. <br> Choices: `WEEK`, `MONTH`, `MONTH_END`
static | `boolean`| Defines if it is allowed to change the Plan
items | `array` | List of [`Order_items`](#orders) containing [`Products`](#products) Items associated in Plan. <br> The total_amount will be calculated based on the total sum of the Products.
currency | `string` | ISO 4217 standard
total_amount | `integer` | Total amount per `interval_unit`. If missing, it is the sum of Product Items
discount | `number` | Set discount on Plan. Min 0.0, max 1.0. This will affect the total_amount
setup_cost | `number` | A fee for setting up a new Plan. Default = 0
initial_fail_amount_action | `string` | Decides what happens if a payment fails in the [`Subscription`](#subscriptions) <br> Choices `CANCEL`, `CONTINUE`
max_fail_attempts | `integer`| If `initial_fail_amount_action` is `CONTINUE`, number of interval_unit that is allowed to fail before the [`Subscription`](#subscriptions) stops.
auto_bill_amount | `boolean` | Automatically bills the outstanding balance in the next billing cycle

## Create a new Plan

This creates a new Plan. The Plan is not [`User`](#users) specific,
but an User could create a Plan based on their demands. If The Company
wants a non-customizable plan, <br> set `static_plan` to `true`
when creating a new plan.

> Definition

```
POST https://api.shareactor.io/plans
```

> Example request:

``` http
POST /plans HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
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
    "auto_bill_amount": false,
    "note": "This is a note regarding the Plan",
    "interval_count": false,
    "total_amount": false,
    "discount": 0,
    "setup_cost": 100
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
    "setup_cost": 100.0,
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
    "static": true
}
```

Argument | Type | Description
-------- | ---- | -------
**interval_unit** | `string` | The frequency that the Subscription acts upon. <br> interval_unit choices: `DAY`, `WEEK`, `MONTH`, `MONTH_END`, `ANNUAL`
**billing_interval** | `string`| Defines billing frequency. <br> Choices: `WEEK`, `MONTH`, `MONTH_END`
**currency** | `string`| The currency for the subscription. ISO 4217


## Retrieve a Plan

Retrieve a Plan, based on its id.

> Definition

```
GET https://api.shareactor.io/plans/<plan_id>
```

> Example request:

``` http
GET /plans/<plan_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
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
     "auto_bill_amount": false,
     "static": false,
     "setup_cost": 0.0, 
     "total_amount": 500.0,
     "company": {
        "$oid": "57ee9c71d76d431f8511142f"}
 }
```

Argument | Type | Description
-------- | ---- | -------
**plan_id** |`string`| Plan id

## Get list of all Plans associated with Company

> Definition

```
GET https://api.shareactor.io/plans
```

> Example request:

``` http
GET /plans HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
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
        "setup_cost": 0.0,
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
        "auto_bill_amount": false,
        "status": "CREATED"
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
size | `integer` | Number of items to retrieve
page | `integer` | Which page to retrieve. _default page size is 50_
sorting | `string`| Field used for sorting results. <br> If missing, the default is `-created`
include_deleted| `boolean` | If `true`, deleted products are also listed


## Update Plan

Update a Plan. This is only possible if `static_plan` is `false`.
Not all fields are possible to update. See fields in Argument under.
> Definition

```
PUT https://api.shareactor.io/plans/<plan_id>
```

> Example request:

``` http
PUT /plans HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

Updates a Plan with a given ID. For example:

Argument | Type | Description
-------- | ---- | -----
**plan_id** | `string` | ID of the queried Plan
active |`boolean`| Default = `true`
name |`string`|
note |`string`|
initial_fail_amount_action |`string`| Choices: `CONTINUE`, `CANCEL`
max_fail_attempts |`integer`|
discount |`number`|
total_amount |`number`|


## Delete Plan
> Definition

```
DELETE https://api.shareactor.io/plans/<plan_id>
```

> Example request:

``` http
DELETE /plans/<plans_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

Deletes a plan with a given ID.
