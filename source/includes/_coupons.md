# Coupons

Coupons are a great way to offer discounts or vouchers to your Customers. They can be used together with multiple business models: Orders, Invoices and Subscriptions.


## Coupon Object

Attributes | Types | Description
---------- | ----- | -----------
**name** | `string` | Name of the coupon
**code** | `string` | The code to be used by a costumer. By default, the code is automatically generated (a string of 6 characters long, e.g: Nz42kL), but you can also create our own.
percent_off | `number` | The discount percentage, should be between `0.` and `1.`, for example: 0.2 means 20% of the total amount.
amount_off | `number` | Discount amount, for example: 100.50, means that the next payment will be decreased by 100.50.
currency | `string` | ISO 4217 code of currency, for example: "EUR"
starting_date | `object` | Define when can the coupon start being used. By default, it'll start at the time of creation, but it could be defined for the future. If defined in the future, it means the coupon could not be used before the `starting_date`, `timestamp` format. _Default `now`_
ending_date | `object` | The date when the coupon will stop being available. `timestamp` format.
max_redemptions | `integer` | The number of times a coupon can be used. When the `used_count` is equal to the `max_redemption` then the coupon becomes unusable.
used_count | `integer` | Automatically incremented by each use.


As you can see, there are two type of discount: percent_off and amount_off.
It's important to remember: a coupon can only have one of them at the same time.
But if you want to change the type of discount you can do it easily with a `PUT coupons/<coupon_id>` (There is an example [here](#switch-the-discount-type))
<br/>
<aside class="notice">
There are 3 different ways a Coupon can become unusable:
 <br/>- When the `ending_date` is the current date
 <br/>- When the `max_redemptions` is reached
 <br/>- It's also possible to combine `ending_date` and `max_redemptions`. In that case, the coupon will become unsuable if `max_redemptions` is reached or at the `ending_date`.
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
    "deleted": false,
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
**code** | `string` | The code to be used by a costumer. By default, the code is automatically generated (a string of 6 characters long, e.g: Nz42kL), but you can also create our own.
percent_off | `number` | The discount percentage, should be between `0.` and `1.`, for example: 0.2 means 20% of the total amount.
amount_off | `number` | Discount amount, for example: 100.50, means that the next payment will be decreased by 100.50.
currency | `string` | ISO 4217 code of currency, for example: "EUR"
starting_date | `object` | Define when can the coupon start being used. By default, it'll start at the time of creation, but it could be defined for the future. If defined in the future, it means the coupon could not be used before the `starting_date`, `timestamp` format. _Default `now`_
ending_date | `object` | The date when the coupon will stop being available. `timestamp` format.
max_redemptions | `integer` | The number of times a coupon can be used. When the `used_count` is equal to the `max_redemption` then the coupon becomes unusable.


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
    "deleted": false,
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
**coupon_id** | `string` | ID of the queried Coupon

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
        "deleted": false,
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
        "deleted": false,
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
status | `string` | Status of Coupons (`active`). By default there is no value, the API returns coupons regardless the status.
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
        "deleted": false,
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
    "deleted": false,
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
**code** | `string` | The code to be used by a costumer. By default, the code is automatically generated (a string of 6 characters long, e.g: Nz42kL), but you can also create our own.
percent_off | `number` | The discount percentage, should be between `0.` and `1.`, for example: 0.2 means 20% of the total amount.
amount_off | `number` | Discount amount, for example: 100.50, means that the next payment will be decreased by 100.50.
currency | `string` | ISO 4217 code of currency, for example: "EUR"
starting_date | `object` | Define when can the coupon start being used. By default, it'll start at the time of creation, but it could be defined for the future. If defined in the future, it means the coupon could not be used before the `starting_date`, `timestamp` format. _Default `now`_
ending_date | `object` | The date when the coupon will stop being available. `timestamp` format.
max_redemptions | `integer` | The number of times a coupon can be used. When the `used_count` is equal to the `max_redemption` then the coupon becomes unusable.
used_count | `integer` | Automatically incremented by each use.

### Switch The Discount Type

In the example we assume the coupon `57ee9c72d76d431f85222420` has a `percent_off` discount type, and we are going to change its type to `amount_off`.
<br/>To do it you just have to put `amount_off` and the value in the data, of course you can do the reverse as well, change the `amount_off` to the `percent_off` type.

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
    "deleted": false,
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
