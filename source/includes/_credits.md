# Credits

A `Credit` is used to pay for an [`Order`](#orders) containing a [`Credit Product`](#credit-product-object).

Attribute | Type | Description
--------- | ---- | -------
**initial_quantity** | `number` | The number of Credits that will be deducted from a [User](#users). This value has no unit of measurement and is thus self-defined. _default set to 0_
uuid | `string` | The unique reference ID for the Credit.
expires |  `number` | The expiration date of the Credits. _default is set to a year from the date of creation_
consumed | `number` | The number of Credits the user has consumed.


## Grant Credits to User

Admins are able to update credits for the users. 


> Definition

```
PUT https://api.kvass.ai/users/<user_id>?expand=voucher
```

> Example request:

``` http
PUT /users/<user_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
  "voucher": 100
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "_id":{"$oid":"57ee9c72d76d431f85111999"},
  "_cls":"User",
  "created":{"$date":1475428903950},
  "modified":{"$date":1475428903951},
  "first_name":"John",
  "last_name":"Doe",
  "email":"john@email.com",
  "mobile_phone_number":"+4712345678",
  "addresses":[{
                "city":"Danielsen",
                "service":"google",
                "alias":"",
                "country":"Paraguay",
                "zip_code":"0556",
                "state":"Oslo",
                "street_name":"Iversenstien 7"
             }],
  "billing_address":{},
  "bio":"",
  "note":"",
  "avatar":"",
  "company":{"$oid":"57ee9c71d76d431f8511142f"},
  "tags":[],
  "stripe_customer_id":"",
  "roles":["user"],
  "deleted":true,
  "voucher": {
        'initial_quantity': 100, 
        'expires': {'$date': 1572339915160}, 
        'consumed': 0, 
        'created': {'$date': 1540803915160}, 
        'uuid': '60a4646c-a6ec-4d2b-a031-25cff5b9a8ae'
    }
}
```

Argument | Type | Description
-------- | ---- | -------
**user_id** | `string` | The [`user`](#users)'s unique ID.
**voucher** | `number` | The new value of the `initial_quantity`.
expires |  `number` | The expiration date of the Credits. _default is set to a year from the date of creation_

<aside class="notice">
Note that the new value assigned to voucher replaces the credit's value, instead of increasing the credit's value by this amount.
</aside>

## Credit Product Object

The Credit Product is a subclass of [`Products`](#products). The Credit Product is a type of product that can be bought with [`Credits`](#credits).

Attribute | Type | Description
--------- | ---- | -------
**name** | `string` | The name of the product
**currency** |  `string` | Three letter currency code in standard ISO 4217 format.
**vouchers_required** | `number` | The quantity of credits necessary to purchase the Credit Product ([`Credits`](#credits)) _default is 60_
price | `number` | The price of the product
price_change_percentage | `number`| If you have sub_products in an order with the `price_change_percentage` set, then the sub_products will change the **main product's** amount in the order accordingly to the `product.price * sub_product.price_change_percentage`
description | `string` | A full description of the product
short_description | `string` | A brief description of the product
main_product | `boolean` | Flag that marks whether or not it is a main product
_sub_products | `array` | A list of sub products under the main product
parents | `array` | The sub product's parent
default_position | `array` | The geo position for the product
tags | `array` | List of tags associated with the product
properties | `object` | The product's properties
vat | `number` | The percentage of VAT in the product price. Percent value between 0 and 1
max_distance | `number` |
slug | `string` |

A Credit Product is a special product that allows the user to redeem [`credits`](#credits) to pay for an [`order`](#orders) instead of their [`payment method`](#payment-methods).

## Create a New Credit Product
> Definition

```
POST https://api.kvass.ai/credit_products
```

> Example request:

``` http
POST /credit_products HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{   
    "name": "Credit Product Name",
    "currency": "NOK",
    "vouchers_required": 120,
    "description": "It is a test product"
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{  
   "_id":{"$oid":"58f9f856b70e2a56c4a0db35"},
   "max_distance":0,
   "created":{"$date":1492777046366},
   "default_position":[-1, -1],
   "_cls":"CreditProduct",
   "description":"",
   "modified":{"$date":1492777046369},
   "_sub_products":[],
   "name":"Credit Product Name",
   "properties":{},
   "vouchers_required": 120,
   "active":false,
   "tags":[],
   "vat":0.0,
   "company":{"$oid":"57ee9c71d76d431f8511142f"},
   "deleted":false,
   "company_take":-1.0,
   "parents":[],
   "main_product":true,
   "currency":"NOK",
   "path":"/"
}
```

This creates a new Credit Product.

Argument | Type | Description
-------- | ---- | -------
**name** | `string` | Name of the product
**currency**| `string` | Currency of the product _default set to `NOK`_
**voucher_required** | `number` | Sets how many [`Credits`](#credits) this Credit Products costs to acquire.
price | `number` | Price of the product
vat | `number` | Percentage of price to be paid for VAT _default set to `0.0`_
description | `string` | Full description of the product
short_description | `string` | Short description for the product
path | `string` | URL for the product _default set to `'/'`_
main_product | `boolean` | Flag that marks whether or not it is a main product _default set to `True`_
_sub_product | `array` | List of sub products under the product. _default set to `[]`_
parents | `array` | List of the parent products to the sub product. _default set to `[]`_
tags | `string` | List of tags associated with the product
default_position | `array` | Geo position of the product _default set to `[-1, -1]`_
properties | `object` | The product's properties
provider | `string` | The [`provider`](#providers) assigned to the product, defined by the provider's ID
expire_days | `number` | Sets how many days this Credit Product is available.

## Update a Credit Product

> Definition

```
PUT https://api.kvass.ai/credit_products/<bulk_id>
```

> Example request:

``` http
PUT /credit_products/<bulk_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "description": "It is a test credit product",
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{  
   "_id":{"$oid":"58f9f856b70e2a56c4a0db35"},
   "max_distance":0,
   "created":{"$date":1492777046366},
   "default_position":[-1, -1],
   "_cls":"CreditProduct",
   "description":"It is a test credit product",
   "modified":{"$date":1492777046369},
   "_sub_products":[],
   "name":"Credit Product Name",
   "properties":{},
   "vouchers_required": 120,
   "active":false,
   "tags":[],
   "vat":0.0,
   "company":{"$oid":"57ee9c71d76d431f8511142f"},
   "deleted":false,
   "company_take":-1.0,
   "parents":[],
   "main_product":true,
   "currency":"NOK",
   "path":"/"
}
```

This creates a new Credit Product.

Argument | Type | Description
-------- | ---- | -------
name | `string` | Name of the product
price | `number` | Price of the product
currency | `string` | Currency of the product _default set to `NOK`_
vat | `number` | Percentage of price to be paid for VAT _default set to `0.0`_
description | `string` | Full description of the product
short_description | `string` | Short description for the product
path | `string` | URL for the product _default set to `'/'`_
main_product | `boolean` | Flag that marks whether or not it is a main product _default set to `True`_
_sub_product | `array` | List of sub products under the product. _default set to `[]`_
parents | `array` | List of the parent products to the sub product. _default set to `[]`_
tags | `string` | List of tags associated with the product
default_position | `array` | Geo position of the product _default set to `[-1, -1]`_
properties | `object` | The product's properties
provider | `string` | The [`provider`](#providers) assigned to the product, defined by the provider's ID
voucher_required | `number` | Sets how many [`Credits`](#credits) this Credit Products costs to acquire.
expire_days | `number` | Sets how many days this Credit Products is available.


## Get a Credit Product
> Definition

```
GET https://api.kvass.ai/credit_products/<bulk_id>
```

> Example request:

``` http
GET /credit_products/<bulk_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{  
   "_id":{"$oid":"58f9f856b70e2a56c4a0db35"},
   "max_distance":0,
   "created":{"$date":1492777046366},
   "default_position":[-1, -1],
   "_cls":"CreditProduct",
   "description":"It is a test credit product",
   "modified":{"$date":1492777046369},
   "_sub_products":[],
   "name":"Credit Product Name",
   "properties":{},
   "vouchers_required": 120,
   "active":false,
   "tags":[],
   "vat":0.0,
   "company":{"$oid":"57ee9c71d76d431f8511142f"},
   "deleted":false,
   "company_take":-1.0,
   "parents":[],
   "main_product":true,
   "currency":"NOK",
   "path":"/"
}
```

Get a Credit Product based on the credit product's unique ID.

Attribute | Type | Description
--------- | ---- | -------
**bulk_id** | `string` | The credit product's ID


## Get List of All Credit Products Associated with a Company

> Definition

```
GET https://api.kvass.ai/credit_products
```

> Example request:

``` http
GET /credit_products HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai

[
    {
       "_cls":"CreditProduct",
       "parents":[],
       "tags":["fuzz", "Foo", "Bar"],
       "vouchers_required": 120,
       "_sub_products":[],
       "deleted":false,
       "company_take":-1.0,
       "max_distance":0,
       "description":"description",
       "created":{"$date":1492781492460},
       "vat":0.0,
       "properties":{},
       "active":true,
       "name":"Credit Product Name",
       "modified":{"$date":1492781492461},
       "company":{"$oid":"57ee9c71d76d431f8511142f"},
       "currency":"NOK",
       "path":"/",
       "main_product":true,
       "_id":{"$oid":"58f9f856b70e2a56c4a0db35"},
       "business_rules":[],
       "default_position":[-1,-1]
    }
]
```

Arguments | Type | Description
--------- | ---- | -----------
size | `number` | Number of items to retrieve. _default is 10_
page | `number` | Which page to retrieve. _default is 50_
lat | `number` | Define the latitude.
lng | `number` | Define the longitude.
all | `boolean` | If `true`, would return both of active and deleted and doesn't care of the attribute `main_product` credit products.


## Search Credit Product

Retrieves a list of all Credit Products associated with the search.

> Definition

```
GET https://api.kvass.ai/credit_products/search
```

> Example request:

``` http
GET /credit_products/search?query=description HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
       "_cls":"CreditProduct",
       "parents":[],
       "tags":["fuzz", "Foo", "Bar"],
       "vouchers_required": 120,
       "_sub_products":[],
       "deleted":false,
       "company_take":-1.0,
       "max_distance":0,
       "description":"description",
       "created":{"$date":1492781492460},
       "vat":0.0,
       "properties":{},
       "active":true,
       "name":"Credit Product Name",
       "modified":{"$date":1492781492461},
       "company":{"$oid":"57ee9c71d76d431f8511142f"},
       "currency":"NOK",
       "path":"/",
       "main_product":true,
       "_id":{"$oid":"58f9f856b70e2a56c4a0db35"},
       "business_rules":[],
       "default_position":[-1,-1]
    }
]
```

Arguments | Type | Description
--------- | ---- | -----------
**query** | `string` | What you want to search for, e.g., **name**, **description**, or **id**.
size | `number` | Number of items to retrieve. _default is 10_
page | `number` | Which page to retrieve. _default is 0_
sort | `string` | Field used for sorting results. _default is `created`_
lat | `number` | Define the latitude.
lng | `number` | Define the longitude.
include_deleted| `boolean` | If `true`, deleted products are also listed.


## Consume a Credit Product

Consume a Credit Product associated with a credit product's unique ID.


> Definition

```
POST https://api.kvass.ai/credit_products/<bulk_id>/consume
```

> Example request:

``` http
POST /credit_products/search?query=description HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
       "_cls":"CreditProduct",
       "parents":[],
       "tags":["fuzz", "Foo", "Bar"],
       "vouchers_required": 120,
       "_sub_products":[],
       "deleted":false,
       "company_take":-1.0,
       "max_distance":0,
       "description":"description",
       "created":{"$date":1492781492460},
       "vat":0.0,
       "properties":{},
       "active":true,
       "name":"Credit Product Name",
       "modified":{"$date":1492781492461},
       "company":{"$oid":"57ee9c71d76d431f8511142f"},
       "currency":"NOK",
       "path":"/",
       "main_product":true,
       "_id":{"$oid":"58f9f856b70e2a56c4a0db35"},
       "business_rules":[],
       "default_position":[-1,-1]
    }
]
```

Arguments | Type | Description
--------- | ---- | -----------
**query** | `string` | What you want to search for, e.g., **name**, **description**, or **id**.
size | `number` | Number of items to retrieve. _default is 10_
page | `number` | Which page to retrieve. _default is 0_
sort | `string` | Field used for sorting results. _default is `created`_
lat | `number` | Define the latitude.
lng | `number` | Define the longitude.
include_deleted| `boolean` | If `true`, deleted products are also listed.

## Credit Plan

A [`user`](#users) can also get credits by [`subscribing`](#subscriptions) to a `Credit Plan`.
A credit plan shares a lot of commonalities with a normal [`Plan`](#plans), but uses only 
[`Credit Product`](#credit-products) in the `plan.items` instead of regular [`Products`](#products).
The number of credits corresponding to the sum of the `plan.items.product.vouchers_required` will be charged at every billing interval for each [`user`](#users) . The `total_amount` in the plan,
 the amount that the user will be charged at every billing interval, will be the sum of all
 `plan.items.product.price`.


### Create a Credit Plan

Argument | Type | Description
-------- | ---- | -------
**method** | `string` | The type of Plan, in this case a `voucher_plan`.
**name** | `string` | The name of the Plan, in this case a `VOUCHER_PLAN`.
**interval_unit** | `string` | The frequency that the Subscription acts upon. <br> interval_unit choices: `DAY`, `WEEK`, `MONTH`, `MONTH_END`, `ANNUAL`
**billing_interval** | `string`| Defines billing frequency. <br> Choices: `WEEK`, `MONTH`, `MONTH_END`
**currency** | `string`| Three letter ISO currency code as defined by ISO 4217.7
**items** | `array` | An array of [`credit products`](#credit-products) and `quantity`.

> Definition

```
POST https://api.kvass.ai/plans
```

> Example request:

``` http
POST /plans HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "interval_unit": "WEEK",
    "name": "Voucher Plan",
    "currency": "NOK",
    "static": true,
    "billing_interval": "MONTH",
    "items": [{"product": "<credit_product1_id>", "quantity": 1}],
    "initial_fail_amount_action": "CONTINUE",
    "max_fail_attempts": 1,
    "note": "This is a note regarding the Plan"
}

```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "_id": {"$oid": "5931697ed57ba271c0c7de66"},
    "created": {"$date": 1496410494652},
    "updated": {"$date": 1496410494652},
    "auto_bill_amount": false,
    "billing_interval": "MONTH",
    "initial_fail_amount_action": "CONTINUE",
    "max_fail_attempts": 1,
    "name": "Voucher Plan",
    "method": "voucher_plan",
    "total_amount": 100.0,
    "interval_unit": "WEEK",
    "items": [
        {
            "discount": 0.0,
            "product": {"$oid": "58f9f856b70e2a56c4a0db35"},
            "quantity": 1
         }],
    "status": "CREATED",
    "currency": "NOK",
    "note": "This is a note regarding the Plan",
    "deleted": false,
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "static": true
}
```

## Redeem Credits in an Order

The User can redeem their credits to pay for Orders only if all the [`Order.items`](#orders) 
are of type [`Credit Products`](#credit-products)


> Definition

```
POST https://api.kvass.ai/orders/<order_id>/redeem?expand=user.voucher
```

> Example request & response:

``` http
POST /orders/<order_id>/redeem?expand=user.voucher HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai


```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
   "_id":{"$oid":"5bd6de64cb416c6806eb5c4e"},
   "company":{"$oid":"5bd6de52cb416c6806eb5c41"},
   "user":{"$oid":"5bd6de54cb416c6806eb5c42",
   "_id":{"$oid":"5bd6de54cb416c6806eb5c42"},
      "_cls":"User",
      "created":{"$date":1540808276567},
      "modified":{"$date":1540808292678},
      "first_name":"testy",
      "last_name":"Squeeze",
      "mobile_phone_number":"123456789465",
      "addresses":[],
      "billing_address":{
         "alias":"",
         "service":"google"
      },
      "voucher":{
         "initial_quantity":1,
         "expires":{"$date":1572344289089},
         "consumed":1,
         "created":{"$date":1540808276575},
         "uuid":"ed7e9ca2-64c2-49a4-b6c4-f006c07603aa"
      },
      "bio":"",
      "note":"",
      "avatar":"",
      "last_login":{"$date":1540808276583},
      "company":{"$oid":"5bd6de52cb416c6806eb5c41"},
      "tags":[],
      "stripe_customer_id":"cus_DsIPnTkTxM3UPf",
      "auth0_id":"some-id",
      "national_id":"",
      "new_customer":true,
      "is_company":false,
      "default_payment_method":{"$oid":"5bd6de5acb416c6806eb5c45"},
      "deleted":false,
      "receive_notifications":true
   },
   "items":[
      {
         "product":{"$oid":"5bd6de55cb416c6806eb5c44"},
         "quantity":1,
         "discount":0.0,
         "sub_products":[]
      }
   ],
   "units":1,
   "total_quantity":1,
   "total_amount":100.0,
   "top_up_amount":0.0,
   "top_up_vat":0.0,
   "delivery_time":{
      "$date":1572344292591
   },
   "payments":[],
   "currency":"NOK",
   "resources":[],
   "human_id":"JQLVRN",
   "note":"",
   "stripe_charge_id":"",
   "stripe_refund_id":"",
   "order_status":"SUCCESS",
   "delivery_status":"DONE",
   "created":{"$date":1540808292594},
   "modified":{"$date":1540808292682},
   "deliveries":[],
   "override_company_take":-1.0
}
```
