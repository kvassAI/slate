# Coupons

Coupon is usable to offer a discount to your customer on different type of bill: Order, Invoice, Subscription.


## Coupon Object

Attributes | Types | Description
---------- | ----- | -----------
**name** | `string` | Name of the coupon
**code** | `string` | The code will be used by a costumer. The code is automatically generated (a string of 6 characters long, e.g: Nz42kL), but you can also create our own code.
**currency** | `string` | ISO 4217 code of currency, for example: "EUR"
percent_off | `number` | Is the discount percentage, can be set to `0.` to `1.`, e.q: 0.2 means 20% of the amount.
amount_off | `number` | Is the discount amount, e.q: 100.50, so the next payment will be decreased by 100.50.
starting_date | `object` | Define when the coupon is usable, it could now or in the future. If it defined in the future, it means the coupon could not be used before the `starting_date`, `timestamp` format. _Default `now`_
ending_date | `object` | The date when the coupon will be unusable, it means after that day any customer could use it anymore, `timestamp` format.
max_redemptions | `integer` | The number of time a coupon can be used, when the `used_count` is equal of `max_redemption` then any customer could use the coupon anymore.
used_count | `integer` | Automatically increment by each use.


As you can see, there is two type of discount: percent_off and amount_off.
Is important to remember: a coupon can only have one of them in the meantime.
But if you want to change the type of discount you can do it easily by a `PUT coupons/<coupon_id>` (There is an example [here](#switch-the-discount-type))
<br/>
<aside class="notice">
Regarding how a coupon can be automatically unusable, there are 3 ways:
 <br/>- When the `ending_date` is the current date
 <br/>- When the `max_redemptions` is reached
 <br/>- It's also possible to combine `ending_date` and `max_redemptions`. In that case, the coupon will be unsuable `max_redemptions` is reached or at the `ending_date`.
</aside>

## Create a Coupon

> Definition

```
POST https://api.kvass.ai/coupons
```

> Example request:

```http
POST /coupons HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "name": "new coupon",
    "code": "SUMMER2018shirt",
    "currency": "NOK",
    "percent_off": 0.2,
    "starting_date": 1533026949,
    "ending_date": 1533081600,
    "max_redemption": 150
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "_id": {"$oid":"57ee9c72d76d431f85222420"},
    "created": "2018-07-31T10:50:43.511Z",
    "updated": "2018-07-31T11:00:43.511Z",
    "active": true,
    "deleted: false,
    "company":{"$oid":"57ee9c71d76d431f8511142f"},
    "name": "new coupon",
    "code": "SUMMER2018shirt",
    "currency": "NOK",
    "percent_off": 0.2,
    "starting_date": "2018-07-31T10:50:43.511Z",
    "ending_date": "2018-08-1T12:00:00.000Z",
    "max_redemption": 150,
    "used_count": 0
}
```

Creates a new Coupon.

Attributes | Types | Description
---------- | ----- | -----------
**name** | `string` | Name of the coupon
**currency** | `string` | ISO 4217 code of currency, for example: "EUR"
code | `string` | The code will be used by a costumer. The code is automatically generated (a string of 6 characters long, e.g: Nz42kL), but you can also create our own code.
percent_off | `number` | Is the discount percentage, can be set to `0.` to `1.`, e.q: 0.2 means 20% of the amount.
amount_off | `number` | Is the discount amount, e.q: 100.50, so the next payment will be decreased by 100.50.
starting_date | `integer` | Define when the coupon is usable, it could now or in the future. If it defined in the future, it means the coupon could not be used before the `starting_date`, `timestamp` format. _Default `now`_
ending_date | `integer` | The date when the coupon will be unusable, it means after that day any customer could use it anymore, `timestamp` format.
max_redemptions | `integer` | The number of time a coupon can be used, when the `used_count` is equal of `max_redemption` then any customer could use the coupon anymore.
used_count | `integer` | Automatically increment by each use.


## Get a Coupon
> Definition

```
GET https://api.kvass.ai/coupons/<coupon_id>
```

> Example request:

```http
GET /coupons/<coupon_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "_id": {"$oid":"57ee9c72d76d431f85222420"},
    "created": "2018-07-31T10:50:43.511Z",
    "updated": "2018-07-31T10:50:43.511Z",
    "active": true,
    "deleted: false,
    "company":{"$oid":"57ee9c71d76d431f8511142f"},
    "name": "new coupon",
    "code": "SUMMER2018shirt",
    "currency": "NOK",
    "percent_off": 0.2,
    "starting_date": "2018-07-31T10:50:43.511Z",
    "ending_date": "2018-08-1T12:00:00.000Z",
    "max_redemption": 150,
    "used_count": 0
}
```

Retrieves a Coupon with a given ID.

Argument | Type | Description
-------- | ---- | ------
**coupon_id** | `string` | ID of the queried coupon

## List all Coupon

> Definition

```
GET https://api.kvass.ai/coupons
```

> Example request:

```http
GET /coupons HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "_id": {"$oid":"57ee9c72d76d431f85222420"},
        "created": "2018-07-31T10:50:43.511Z",
        "updated": "2018-07-31T10:50:43.511Z",
        "active": true,
        "deleted: false,
        "company":{"$oid":"57ee9c71d76d431f8511142f"},
        "name": "new coupon",
        "code": "SUMMER2018shirt",
        "currency": "NOK",
        "percent_off": 0.2,
        "starting_date": "2018-07-31T10:50:43.511Z",
        "ending_date": "2018-08-1T12:00:00.000Z",
        "max_redemption": 150,
        "used_count": 0
    },
    {
        "_id": {"$oid":"57ee9c72d76d431f85222421"},
        "created": "2018-07-31T10:50:43.511Z",
        "updated": "2018-07-31T10:50:43.511Z",
        "active": true,
        "deleted: false,
        "company":{"$oid":"57ee9c71d76d431f8511142f"},
        "name": "Second coupon",
        "code": "AVx46pp",
        "currency": "NOK",
        "amount_off": 150,
        "starting_date": "2018-07-31T10:50:43.511Z",
        "max_redemption": 200,
        "used_count": 10
    }
]
```

Retrieves a list of all Coupons associated with the company.

Arguments | Type | Description
--------- | ---- | ------
size | `number` | Number of items to retrieve
page | `number` | Which page to retrieve. _default is 10_
sort | `string` | Field used for sorting results. _default sorting is `-due_date`_.
status | `string` | Status of Coupons (`active`). By default there is no value, the API returns coupons regardless the status. `coupon_status` can be `true` or `false`.
from_date | `integer` | Start date, `timestamp` format. _default is None_
to_date | `integer` | End date, `timestamp` format. _default is None_
date_filter | `string` | Date field used for filter results. _default is `created`_


## Coupons Search

Retrieves a list of Coupons associate with the company.

> Definition

```
GET https://api.kvass.ai/coupons/search
```

> Example request:

```http
GET /coupons/search/query=SUMMER HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "_id": {"$oid":"57ee9c72d76d431f85222420"},
        "created": "2018-07-31T10:50:43.511Z",
        "updated": "2018-07-31T10:50:43.511Z",
        "active": true,
        "deleted: false,
        "company":{"$oid":"57ee9c71d76d431f8511142f"},
        "name": "new coupon",
        "code": "SUMMER2018shirt",
        "currency": "NOK",
        "percent_off": 0.2,
        "starting_date": "2018-07-31T10:50:43.511Z",
        "ending_date": "2018-08-1T12:00:00.000Z",
        "max_redemption": 150,
        "used_count": 0
    }
]
```

Arguments | Type | Description
--------- | ---- | -------
**query** | `string` | What you want to search, like **id**, **name**, or **code**
size | `number` | number of items to retrieve
page | `number` | which page to retrieve. _default page size is 10_
sort | `string` | field used for sorting results. If missing default is "-created". Could be other parameters in the Invoice model, like "-created" or "status".
from_date | `number` | Start date, `timestamp` format. _default is None_
to_date | `number` | End date, `timestamp` format. _default is None_
date_filter | `string` | Date field used for filter results. _default is `created`_
include_deleted | `boolean` | If `true`, deleted invoices are also listed. _default is `false`_

## Update a Coupon

Updates an Invoice with a given ID.

> Definition

```
PUT https://api.kvass.ai/coupons/<coupon_id>
```

> Example request:

```http
PUT /coupons/<coupon_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "name": "Summer should pass"    
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "_id": {"$oid":"57ee9c72d76d431f85222420"},
    "created": "2018-07-31T10:50:43.511Z",
    "updated": "2018-07-31T10:50:43.511Z",
    "active": true,
    "deleted: false,
    "company":{"$oid":"57ee9c71d76d431f8511142f"},
    "name": "Summer should pass",
    "code": "SUMMER2018shirt",
    "currency": "NOK",
    "percent_off": 0.2,
    "starting_date": "2018-07-31T10:50:43.511Z",
    "ending_date": "2018-08-1T12:00:00.000Z",
    "max_redemption": 150,
    "used_count": 0
}
```

Argument | Type | Description
-------- | ---- | -----
**coupon_id** | `string` | ID of the Coupon
**name** | `string` | Name of the coupon
**currency** | `string` | ISO 4217 code of currency, for example: "EUR"
code | `string` | The code will be used by a costumer. The code is automatically generated (a string of 6 characters long, e.g: Nz42kL), but you can also create our own code.
percent_off | `number` | Is the discount percentage, can be set to `0.` to `1.`, e.q: 0.2 means 20% of the amount.
amount_off | `number` | Is the discount amount, e.q: 100.50, so the next payment will be decreased by 100.50.
starting_date | `integer` | Define when the coupon is usable, it could now or in the future. If it defined in the future, it means the coupon could not be used before the `starting_date`, `timestamp` format. _Default `now`_
ending_date | `integer` | The date when the coupon will be unusable, it means after that day any customer could use it anymore, `timestamp` format.
max_redemptions | `integer` | The number of time a coupon can be used, when the `used_count` is equal of `max_redemption` then any customer could use the coupon anymore.
used_count | `integer` | Automatically increment by each use.

### Switch The Discount Type

In the example we assume the coupon `57ee9c72d76d431f85222420` has a `percent_off` discount type, and we are going to change his type to `amount_off`.
<br/>To do it you just have to put `amount_off` and the value in the data, of course you can do the reverse, change the `amount _ off` to the `percent_off` type.

> Definition

```
PUT https://api.kvass.ai/coupons/<coupon_id>
```

> Example request:

```http
PUT /coupons/<coupon_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "amount_off": 2500
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "_id": {"$oid":"57ee9c72d76d431f85222420"},
    "created": "2018-07-31T10:50:43.511Z",
    "updated": "2018-07-31T10:50:43.511Z",
    "active": true,
    "deleted: false,
    "company":{"$oid":"57ee9c71d76d431f8511142f"},
    "name": "Summer should pass",
    "code": "SUMMER2018shirt",
    "currency": "NOK",
    "amount_off": 2500,
    "starting_date": "2018-07-31T10:50:43.511Z",
    "ending_date": "2018-08-1T12:00:00.000Z",
    "max_redemption": 150,
    "used_count": 0
}
```


## Delete a Coupon

Deletes an coupon with a give ID.

> Definition

```
DELETE https://api.kvass.ai/coupons/<coupon_id>
```

> Example request:

``` http
DELETE /coupons/<coupon_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

Argument | Type | Description
-------- | ---- | -----
**coupon_id** | `string` | ID of the Coupon
