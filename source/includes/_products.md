# Products

If your company is selling some or lot of products our API would be happy to help you to create yours show them
on your app.
The Products are using in a [`Product`](#products) as a **sub-product**, in an [`Order`](#orders) and in a [`Resource`](#ressources).



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

If you want your favorite API to create your fancy Products you need to follow the notice below.
Products required at least three important things to be created: the best **name**, an attractive **price** and of course a **currency**.

> Definition

```
POST https://api.shareactor.io/products
```

> Example request:

``` http
POST /products HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.shareactor.io

{
  "product": 
  {
        "description": "It is a test product",
        "currency": "NOK",
        "price": 3.14,
        "name": "Product Name",
        "vat": 15
    }
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{  
   "_id":{  
      "$oid":"58f9f856b70e2a56c4a0db3d"
   },
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
   "company":{  
      "$oid":"57ee9c71d76d431f8511142f"
   },
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
GET https://api.shareactor.io/products/<product_id>
```

> Example request:

``` http
GET /products/<product_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.shareactor.io
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

Create a product is pretty good but how can you retrieve one of them?
Easy, you only need the product ID then you will receive all details about it.

Argument | Type | Description
-------- | ---- | --------
**product_id** | `string` | The product's ID


## Get List of All Products Associated with a Company


> Definition

```
GET https://api.shareactor.io/products
```

> Example request:

``` http
GET /products HTTP/1.1
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

Retrieve one is nice but if you want more products?
To retrieve more than one Product you could use the following route `GET https://api.shareactor.io/products`.
By the way, like other get list you can use different arguments to optimise your retrieving.

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

> Definition

```
GET https://api.shareactor.io/products/search
```

> Example request:

``` http
GET /products/search?query=description HTTP/1.1
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

Retrieve a simple list is ok but if you want to search a specific product... it means more difficult, right?

Actually, no because our API allows you to do it and simply! 

It's pretty similar as the previous method but you should use `GET https://api.shareactor.io/products/search` and it's more accurate.
For that, you must use the **query** to be more specific.

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
PUT https://api.shareactor.io/products/<product_id>/sub_products
```

> Example request:

``` http
PUT /products/<product_id>/sub_products HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.shareactor.io

{
    "add": [<product_id>, <product_id>]
}

```


