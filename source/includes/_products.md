# Products

## Product Object

Attribute | Type | Description
--------- | ---- | -------
active | `boolean` | Flag that sets Product object to active.
created | `boolean` | The date the Product was generated.
modified | `boolean` | The date the Product was last updated.
deleted | `boolean` | Flag that set Product object to deleted.
company_take |
name | `string` | The name of the product
price | `float` | The price of the product
currency |  `string` | Currency of the product (NOK, EUR, ...) ISO 4217
business_rules | `list` |
description | `string` | Product description
short_description | `string` | Short description about a product
main_product | `boolean` | To know if a product is main product or not
_sub_products | `list` | It is a list of sub products under the main product.
parents | `list` | The sub product parent, overhead product.
default_position | `geoPoint` | Define Product position.
max_distance | `int` |
tags | `list` | List of Tags associated with Product.
properties | `dict` | Properties is a dictionary where you could add product properties.
path | `string` | Path is the Product URL for website
vat | `float` |
slug | `string` |

## Create a Product

``` http
POST /products HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io

{
  "product": 
  {
        "description": "It is a test product",
        "currency": "NOK",
        "price": 3.14,
        "name": "product_name",
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
   "description":"zuuped prod",
   "modified":{  
      "$date":1492777046369
   },
   "_sub_products":[  

   ],
   "name":"product_name",
   "properties":{  

   },
   "price":3.14,
   "active":false,
   "tags":[  

   ],
   "vat":0.0,
   "company":{  
      "$oid":"58f9f856b70e2a56c4a0db37"
   },
   "deleted":false,
   "company_take":-1.0,
   "parents":[  

   ],
   "main_product":true,
   "currency":"NOK",
   "business_rules":[  

   ],
   "path":"/"
}
```

Creates a new Product.

Argument | Type | Description
-------- | ---- | -------
**name** | `string` | Name of the Product
**price** | `float` | Price of the Product
currency | `string` | Currency of the Product. Default `'NOK'`
business_rules | `list` |
description | `string` | Product description
short_description | `string` | Short description for a Product
path | `string` | Path of the Product. Default `'/'`
main_product | `boolean` | If Product is main product or not. Default `True`
_sub_product | `list` | List of sub products. Default `[]`
parents | `list` | List of Product parents. Default `[]`
tags | `string` | List of tags associated with Product
default_position | `geoPoint` | Define Product position. Default `[-1, -1]`
properties | `dict` | Product properties
max_distance | `int` | 
provider | [`Provider`](#provider) | 
company_take | `float` | 
vat | `float` | Default `0`
slug | `` | 

## Retrieve a Product

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
   "_cls":"Product",
   "parents":[  

   ],
   "tags":[  
      "fuzz"
   ],
   "price":3.14,
   "_sub_products":[  

   ],
   "deleted":false,
   "company_take":-1.0,
   "max_distance":0,
   "description":"description",
   "created":{  
      "$date":1492781492460
   },
   "vat":0.0,
   "properties":{  

   },
   "active":true,
   "name":"product_name",
   "modified":{  
      "$date":1492781492461
   },
   "company":{  
      "$oid":"58fa09b4b70e2a4d743fdac2"
   },
   "currency":"NOK",
   "path":"/",
   "main_product":true,
   "_id":{  
      "$oid":"58fa09b4b70e2a4d743fdac4"
   },
   "business_rules":[  

   ],
   "default_position":[  
      -1,
      -1
   ]
}
```

Retrieves Product with ID

Argument | Type | Description
-------- | ---- | --------
**product_id** | `string` | Product Id


## List all Products

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
      "active":true,
      "metadata":{  
         "charge_id":"ch_1ADY0SEeeXxFpLJti58RcN7w"
      },
      "user":{  
         "$oid":"59033056b70e2a1e086d5218"
      },
      "subject":{  
         "_cls":"Invoice",
         "_ref":"59033056b70e2a1e086d521b"
      },
      "company":{  
         "$oid":"5903304cb70e2a1e086d5213"
      },
      "payment_date":{  
         "$date":1493381731652
      },
      "currency":"NOK",
      "created":{  
         "$date":1493381732307
      },
      "_id":{  
         "$oid":"59033265b70e2a1e086d521f"
      },
      "modified":{  
         "$date":1493381746495
      },
      "human_id":"VPXXN7",
      "payment_method":{  
         "$oid":"59033058b70e2a1e086d521c"
      },
      "current_state":"succeeded",
      "deleted":false,
      "amount":10.0
   }
]
```

Retrieves a list of all Products.

Arguments | Type | Description
--------- | ---- | -----------
size | `int` | Number of items to retrieve
page | `int` | Which page to retrieve. _default page size is 50_
include_deleted| `boolean` | If `true`, deleted products are also listed
sorting | `string` | Field used for sorting results
lat | `float` | Define the latitude
lng | `float` | Define the longitude
all | `boolean` | If `true` get all products