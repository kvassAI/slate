# Plans

## Plans Object

Attribute | Type | Description
--------- | ---- | -------
created | `boolean` | The date the Plan was generated.
modified | `boolean` | The date the Plan was last updated.
deleted | `boolean` | Flag that set Plan object to deleted.
name | `string` | The name of the Plan
status | `string`| Status on Plan. <br> Status types: `CREATED`, `ACTIVE`, `INACTIVE`, `DELETED`
**interval_unit** | `string` | The frequency with which a Subscription should act upon. <br> interval_unit choices: `DAY`, `WEEK`, `MONTH`, `MONTH_END`, `ANNUAL`
interval_count | `int`| Total umber of intervals_units
billing_interval | `string`| Defines billing frequency. <br> Choices: `WEEK`, `MONTH`, `MONTH_END`
static_plan | `boolean`| Defines if it is allowed to change the Plan
items | [`Order_items`](#orders) | [`Products`](#products) Items associated in Plan. <br> The total_amount will be calculated based on the total sum of the Products.
currency | `string` | ISO 4217
total_amount | `int` | Total amount per interval_unit. If missing, it is the sum of Product Items
plan_discount | `float` | Set discount on Plan. Min 0.0, max 1.0. This will affect the total_amount
setup_cost | `float` | A fee for setting up a new Plan. Default = 0
initial_fail_amount_action | `string` | Decides what happens if a payment fails in the [`Subscription`](#subscriptions) <br> Choices `CANCEL`, `CONTINUE`
max_fail_attempts | `int`| If `initial_fail_amount_action` is `CONTINUE`, number of interval_unit that is allowed to fail before the [`Subscription`](#subscriptions) stops.
auto_bill_amount | `boolean` | Automatically bills the outstanding balance in the next billing cycle

## Create a new Plan

This creates a new Plan. The Plan is not [`User`](#users) specific,
but an User could customize a Plan for their needs. If The Company
don't want a pre-set Plan, that is not customizable,
set `static_plan` to `true`.

> Definition

```
POST https://api.shareactor.io/plans
```

> Example request:

``` http
POST /products HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io

{
    "interval_unit": "WEEK",
    "name": "Plan Deluxe",
    "currency": "NOK",
    "static_plan": true,
    "billing_interval": "MONTH",
    "items": [{"product": "<product1_id>", "quantity": 1},
              {"product": "<product2_id>", "quantity": 2, "discount": 0.5}],
    "initial_fail_amount_action": "CONTINUE",
    "max_fail_attempts": 1,
    "auto_bill_amount": false,
    "note": "This is a note regarding the Plan",
    "interval_count": false,
    "total_amount": false,
    "plan_discount": 0,
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
    "plan_discount": 0.0,
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
    "static_plan": true
}
```

Argument | Type | Description
-------- | ---- | -------



## Retrieve a Plan

Retrieve a Plan, based on its id.

> Definition

```
GET https://api.shareactor.io/plans/<plan_id>
```

> Example request:

``` http
GET /products/<product_id> HTTP/1.1
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
     "plan_discount": 0.0, 
     "billing_interval": "MONTH", 
     "currency": "NOK", 
     "name": "Golden Plan",
     "interval_unit": "WEEK",
     "auto_bill_amount": false,
     "static_plan": false,
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
GET /products HTTP/1.1
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
        "static_plan": false,
        "billing_interval": "MONTH",
        "_id": {
            "$oid": "59317069d57ba2781c739479"
        },
        "plan_discount": 0.0,
        "total_amount": 500.0,
        "company": {
            "$oid": "59317069d57ba2781c739471"
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
size | `int` | Number of items to retrieve
page | `int` | Which page to retrieve. _default page size is 50_
include_deleted| `boolean` | If `true`, deleted products are also listed


## Update Plan

Update a Plan. This is only possible if `static_plan` is `false`.
> Definition

```
PUT https://api.shareactor.io/plans/<plan_id>
```

> Example request:

``` http
PUT /products HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

Updates a Plan with a given ID. For example:

Argument | Type | Description
-------- | ---- | -----
**plan_id** | `string` | ID of the queried Plan