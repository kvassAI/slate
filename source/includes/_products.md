# Products

## Product Object

Attribute | Type | Description
--------- | ---- | -------
active | `boolean` | Flag that sets Product object to active.
created | `boolean` | The date the Product was generated.
modified | `boolean` | The date the Product was last updated.
deleted | `boolean` | Flag that set Issuer object to deleted.
company_take | 
name | `string` | Name is a product's name
price | `float` | Price is a product's price
currency |  `string` | Currency is a product's currency (NOK, EUR, ...)
business_rules | `lsit` |
description | `string` | Description about a product
short_description | `string` | Short description about a product
main_product | `boolean` | To know if a product is main product or not
_sub_products | `list` | It is a list of sub products.
parents | `list` | Product can be a sub product it has one or more parents. So it's a list of products 
default_position | `geoPoint` | Define Product position.
max_distance | `int` |
tags | `list` | List of Tags associated with Product.
properties | `dict` | Properties is a dictionary where you can add somme new properties.
path | `string` | Path is the Product path for website
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
   "active":False,
   "tags":[  

   ],
   "vat":0.0,
   "company":{  
      "$oid":"58f9f856b70e2a56c4a0db37"
   },
   "deleted":False,
   "company_take":-1.0,
   "parents":[  

   ],
   "main_product":True,
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
business_riles | `list` |
description | `string` | Description of the Product
short_description | `string` | Short description of Product
path | `string` | Path of the Product. Dault `'/'`
main_product | `boolean` | To know if Product is main or not. Default `True`
_sub_product | `list` | List of sub products. Default `[]`
parents | `list` | List of Product parents. Default `[]`
tags | `string` | Lit of tags associate with Product
default_position | `geoPoint` | Define Product position. Default `[-1, -1]`
properties | `dict` | Properties of the Product
max_distance | `int` | 
provider | [`Provider`](#provider) | 
company_take | `float` | 
vat | `float` | Default `0`
slug | `` | 

## Retrieve an Product

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
   "deleted":False,
   "company_take":-1.0,
   "max_distance":0,
   "description":"description",
   "created":{  
      "$date":1492781492460
   },
   "vat":0.0,
   "properties":{  

   },
   "active":True,
   "name":"product_name",
   "modified":{  
      "$date":1492781492461
   },
   "company":{  
      "$oid":"58fa09b4b70e2a4d743fdac2"
   },
   "currency":"NOK",
   "path":"/",
   "main_product":True,
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
**product_id** | `string` | Id of the queried Product
