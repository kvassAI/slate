# Products

## Product Object

Attribute | Type | Description
--------- | ---- | -------
**name** | `string` | The name of the product
**currency** |  `string` | Three letter currency code in standard ISO 4217 format.
price | `number` | The price of the product
price_change_percentage | `number`| How much percentage the products changes the price
description | `string` | A full description of the product
short_description | `string` | A brief description of the product
main_product | `boolean` | Flag that marks whether or not it is a main product
_sub_products | `array` | A list of sub products under the main product
parents | `array` | The sub product's parent
default_position | `array` | The geo position for the product
tags | `array` | List of tags associated with the product
properties | `object` | The product's properties
vat | `number` | The percentage of VAT in the product price
max_distance | `number` |
slug | `string` |


### Price and price_change_percentage
When creating a product, you need one of the fields `price` or `price_change_percentage`.
The `price_change_percentage` could be used in the order.items when creating an order.


## Create a New Product

> Definition

```
POST https://api.kvass.ai/products
```

> Example request:

``` http
POST /products HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "description": "It is a test product",
    "currency": "NOK",
    "price": 3.14,
    "name": "Product Name",
    "vat": 15
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{  
   "_id":{  
    ":"58f9f856b70e2a56c4a0db3d"},
   "max_distance":0,
   "created":{  
      "$date":1492777046366
   },
   "default_position":[  
      -1,
      -1
   ],
   "_cls":"Product",
   "description":"",
   "modified":{  
      "$date":1492777046369
   },
   "_sub_products":[],
   "name":"Product Name",
   "properties":{},
   "price":3.14,
   "active":false,
   "tags":[],
   "vat":15.0,
   "company":{"$oid":"57ee9c71d76d431f8511142f"},
   "deleted":false,
   "company_take":-1.0,
   "parents":[],
   "main_product":true,
   "currency":"NOK",
   "path":"/"
}
```

This creates a new Product.

Argument | Type | Description
-------- | ---- | -------
**name** | `string` | Name of the product
**price** | `number` | Price of the product
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
max_distance | `number` |
provider | `string` | The [`provider`](#providers) assigned to the product, defined by the provider's ID
company_take | `number` |
business_rules | `array` |
slug | `array` |


## Retrieve a Product

> Definition

```
GET https://api.kvass.ai/products/<product_id>
```

> Example request:

``` http
GET /products/<product_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{  
   "_cls":"Product",
   "parents":[],
   "tags":["fuzz", "Foo", "Bar"],
   "price":3.14,
   "_sub_products":[],
   "deleted":false,
   "company_take":-1.0,
   "max_distance":0,
   "description":"description",
   "created":{"$date":1492781492460},
   "vat":0.0,
   "properties":{},
   "active":true,
   "name":"Product Name",
   "modified":{"$date":1492781492461},
   "company":{"$oid":"57ee9c71d76d431f8511142f"},
   "currency":"NOK",
   "path":"/",
   "main_product":true,
   "_id":{"$oid":"58f9f856b70e2a56c4a0db3d"},
   "business_rules":[],
   "default_position":[-1,-1]
}
```

Get a Product based on the product's unique ID.

Argument | Type | Description
-------- | ---- | --------
**product_id** | `string` | The product's ID


## Get List of All Products Associated with a Company

> Definition

```
GET https://api.kvass.ai/products
```

> Example request:

``` http
GET /products HTTP/1.1
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
       "_cls":"Product",
       "parents":[],
       "tags":["fuzz", "Foo", "Bar"],
       "price":3.14,
       "_sub_products":[],
       "deleted":false,
       "company_take":-1.0,
       "max_distance":0,
       "description":"description",
       "created":{"$date":1492781492460},
       "vat":0.0,
       "properties":{},
       "active":true,
       "name":"Product Name",
       "modified":{"$date":1492781492461},
       "company":{"$oid":"57ee9c71d76d431f8511142f"},
       "currency":"NOK",
       "path":"/",
       "main_product":true,
       "_id":{"$oid":"58f9f856b70e2a56c4a0db3d"},
       "business_rules":[],
       "default_position":[-1,-1]
    }
]
```

Retrieves a list of all Products.

Arguments | Type | Description
--------- | ---- | -----------
size | `number` | Number of items to retrieve. _default is 10_
page | `number` | Which page to retrieve. _default is 0_
sort | `string` | Field used for sorting results. _default is `created`_
from_date | `number` | Start date, `timestamp` format. _default is `None`_
to_date | `number` | End date, `timestamp` format. _default is `None`_
date_filter | `string` | Date field used to filter results. _default is `created`_
lat | `number` | Define the latitude.
lng | `number` | Define the longitude.
all | `boolean` | If `true`, get all products.
include_deleted| `boolean` | If `true`, deleted products are also listed.


## Search Products

Retrieves a list of all Products associated with the search.

> Definition

```
GET https://api.kvass.ai/products/search
```

> Example request:

``` http
GET /products/search?query=description HTTP/1.1
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
       "_cls":"Product",
       "parents":[],
       "tags":["fuzz", "Foo", "Bar"],
       "price":3.14,
       "_sub_products":[],
       "deleted":false,
       "company_take":-1.0,
       "max_distance":0,
       "description":"description",
       "created":{"$date":1492781492460},
       "vat":0.0,
       "properties":{},
       "active":true,
       "name":"Product Name",
       "modified":{"$date":1492781492461},
       "company":{"$oid":"57ee9c71d76d431f8511142f"},
       "currency":"NOK",
       "path":"/",
       "main_product":true,
       "_id":{"$oid":"58f9f856b70e2a56c4a0db3d"},
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
from_date | `number` | Start date, `timestamp` format. _default is `None`_
to_date | `number` | End date, `timestamp` format. _default is `None`_
date_filter | `string` | Date field used to filter results. _default is `created`_
lat | `number` | Define the latitude.
lng | `number` | Define the longitude.
include_deleted| `boolean` | If `true`, deleted products are also listed.


## Add sub-products to product
The product could have multiple sub-products associated with the product.
These are found in the field `_sub_products`.

Argument | Type | Description
-------- | ---- | --------
**product_id** | `string` | The product's ID

> Definition

```
PUT https://api.kvass.ai/products/<product_id>/sub_products
```

> Example request:

``` http
PUT /products/<product_id>/sub_products HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "add": [<product_id>, <product_id>]
}

```


## Credit Product Object

The Credit Product is based on the [`Products`](#products).

Attribute | Type | Description
--------- | ---- | -------
**vouchers_required** | `number` | How many vouchers ([`Credits`](#credits)) _default is 60_
**expire_day** |  `number` | _default is 365_

A Credit Product is a special product that allows the user to redeem [`vouchers`](#credits) to pay for an [`order`](#orders) instead of their [`payment method`](#payment-methods).

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
    "description": "It is a test product",
    "currency": "NOK",
    "vouchers_required": 120,
    "name": "Credit Product Name"
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
voucher_required | `number` | Sets how many [`vouchers`](#credits) this Credit Products costs to acquire.
expire_days | `number` | Sets how many days this Credit Products is available.

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
voucher_required | `number` | Sets how many [`vouchers`](#credits) this Credit Products costs to acquire.
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


## Get List of All Products Associated with a Company

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
