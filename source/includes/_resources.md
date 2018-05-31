# Resources

The resource model is [`User`](#Users) specific attributes, assets or retail resources;
resources they possess or that under their ownership. The resource
could have name, description and a chosen reference id. Additionally, it can have
an image and product associated with it.

There is two types of resources, `asset` or `retail`. If not specified, an `asset` resource
 is created. The `asset` resource is an asset that belongs to the user. The `retail` resource is
  a sellable object that the user possess, but is able to sell. Thus, it has a price, currency and VAT.

## Resources Object

Attribute | Type | Description
--------- | ---- | -------
_id | `object` | The resource ID
method | `string` | The resource type. `asset` or `retail`.
user | `object`  | [`User`](#Users) associated with the resource
name | `string` | Name of the resource item
description | `string`| Description of the resource item
image_url | `string` | Image associated with the resource
reference_id | `string` | Reference ID associated with the resource
product | `object`  | [`Product`](#Products) associated with the resource
tags | `array` | An array of tags
human_id | `string` | 6 character ID.
status | `string` | Status of the resource _default is `CREATED`, other options are `PROCESSING`, `UTILIZED` or `IDLE`_

## Retail Resource Object
In addition to the attributes in the `asset` resource, the `retail` resource has the following attributes.

Attribute | Type | Description
--------- | ---- | -------
_sub_products | `array`| An array with sub-products associated with the resource
base_price | `number` | The sum of the `product.price` and price in all the `_sub_products`
retail_price | `number` | The amount that this resource is sold for
currency | `string` | The currency in the main product in the resource
vat | `number` | The VAT from the main product in the resource

## Create a New Resource
Create a new resource associated with the  [`User`](#Users).

> Definition

```
POST https://api.kvass.ai/resources
```


> Example request:

``` http
POST /resources HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "product": "58f9f856b70e2a56c4a0db3d",
    "description": "description of the resource",
    "name": "name of resource",
    "image_url": "http://reference.kvass.ai/images/logo.png",
    "reference_id": "abc1234567890abc"}
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "updated": {"$date": 1500275774067},
    "_id": {"$oid": "596c643ed57ba203be2cf1c9"},
    "method" "asset",
    "user": {"$oid": "57ee9c72d76d431f85111432"},
    "description": "description of the resource",
    "reference_id": "abc1234567890abc",
    "image_url": "http://reference.kvass.ai/images/logo.png",
    "name": "name of resource",
    "product": {"$oid": "58f9f856b70e2a56c4a0db3d"},
    "status": "CREATED",
    "human_id" "1EBY8A",
    "deleted": false,
    "created": {"$date": 1500275774067},
    "company": {"$oid": "57ee9c71d76d431f8511142f"}
}

```


Arguments | Type | Description
--------- | ---- | -------
name | `string` | Name of the resource item
description | `string`| Description of the resource item
status | `string` | Status of the resource _default is `CREATED`; other options are `PROCESSING`, `UTILIZED` or `IDLE`_
product | `string`  | [`Product`](#Products) ID to be associated with the resource
image_url | `string` | Image url associated with the resource
reference_id | `string` | Customizable ID for the resource

## Create Several New Resources in Bulk
This call creates several new resources for a [`User`](#Users) in
one API call. Use the same attributes as when creating a new resource
for a user, but in an `array`.


> Definition

```
POST https://api.kvass.ai/resources/bulk
```


> Example request:

``` http
POST /resources/bulk HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai

[
   {"product": "58f9f856b70e2a56c4a0db3d",
    "method" "asset",
    "description": "description of the first resource",
    "name": "name of first resource",
    "reference_id": "abc1234567890abc",
    "image_url": "http://reference.kvass.ai/images/logo.png"},
   {"product": "58f9f856b70e2a56c4a0db3d",
    "description": "description of the second resource",
    "name": "name of second resource",
    "reference_id": "abcdefghijklm",
    "image_url": "http://reference.kvass.ai/images/logo.png"}
]
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {"updated": {"$date": 1502100873778},
    "method" "asset",
    "_id": {"$oid": "596c643ed57ba203be2cf1c9"},
    "user": {"$oid": "57ee9c72d76d431f85111432"},
    "description": "description of the resource",
    "image_url": "http://reference.kvass.ai/images/logo.png",
    "name": "name of resource",
    "product": {"$oid": "58f9f856b70e2a56c4a0db3d"},
    "reference_id": "abc1234567890abc",
    "status": "CREATED",
    "deleted": false,
    "created": {"$date": 1502100873778},
    "company": {"$oid": "57ee9c71d76d431f8511142f"}},
    {"updated": {"$date": 1502100873778},
    "method" "asset",
    "_id": {"$oid": "596c643ed57ba203be2cf1c9"},
    "user": {"$oid": "57ee9c72d76d431f85111432"},
    "description": "description of the resource",
    "image_url": "http://reference.kvass.ai/images/logo.png",
    "name": "name of resource",
    "product": {"$oid": "58f9f856b70e2a56c4a0db3d"},
    "reference_id": "abcdefghijklm",
    "status": "CREATED",
    "deleted": false,
    "created": {"$date": 1502100873778},
    "company": {"$oid": "57ee9c71d76d431f8511142f"}}
]

```



## Retreive a Resource by ID
Retreive a resource based on its ID.

> Definition

```
GET https://api.kvass.ai/resources/<resource_id>
```

> Example request:

``` http
GET /resources/596c643ed57ba203be2cf1c9 HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "updated": {"$date": 1500275774067},
    "_id": {"$oid": "596c643ed57ba203be2cf1c9"},
    "method" "asset",
    "user": {"$oid": "57ee9c72d76d431f85111432"},
    "description": "description of the resource",
    "image_url": "http://reference.kvass.ai/images/logo.png",
    "reference_id": "abc1234567890abc",
    "name": "name of resource",
    "product": {"$oid": "58f9f856b70e2a56c4a0db3d"},
    "status": "CREATED",
    "deleted": false,
    "created": {"$date": 1500275774067},
    "company": {"$oid": "57ee9c71d76d431f8511142f"}
}

```


## Update Resource
Update attributes in a resource.

Arguments | Type | Description
--------- | ---- | -------
name | `string` | Name of the resource item
description | `string`| Description of the resource item
status | `string` | Status of the resource _default is `CREATED`; other options are `PROCESSING`, `UTILIZED` or `IDLE`_
product | `string`  | [`Product`](#Products) ID to be associated with the resource
image_url | `string` | Image url associated with the resource
reference_id | `string` | Customizable ID for the resource

> Definition

```
PUT https://api.kvass.ai/resources/<resource_id>
```

> Example request:

``` http
PUT /resources/596c643ed57ba203be2cf1c9 HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "name": "Resource 2",
    "description": "An updated Resource",
    "status": "IDLE"
}
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "updated": {"$date": 1500276434527},
    "method" "asset",
    "_id": {"$oid": "596c643ed57ba203be2cf1c9"},
    "user": {"$oid": "57ee9c72d76d431f85111432"},
    "description": "An updated Resource",
    "image_url": "http://reference.kvass.ai/images/logo.png",
    "reference_id": "abc1234567890abc",
    "name": "Resource 2",
    "product": {"$oid": "58f9f856b70e2a56c4a0db3d"},
    "status": "IDLE",
    "deleted": false,
    "created": {"$date": 1500275774067},
    "company": {"$oid": "57ee9c71d76d431f8511142f"}
}

```


## List All Resources
Return an `array` with all resources associated with the request and company.
It is possible to add filters and sorting to the query.


> Definition

```
GET https://api.kvass.ai/resources
```

> Example request:

``` http
GET /resources HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

Arguments | Type | Description
--------- | ---- | -------
include_deleted | `boolean`| If `true`, the request also returns deleted resources
size | `number` | Number of resources per page _default is 10_
page | `number` | Defines which page to retrieve _default is 0_
from_date | `number` | Start date, `timestamp` format _default is `None`_
to_date | `number` | End date, `timestamp` format _default is `None`_
sort | `string` | Field used to sort results _default is `-created`_
status | `string` | Status of the resource _default is `CREATED`; other options are `PROCESSING`, `UTILIZED` or `IDLE`_


## Delete Resource
Delete the resource associated with the resource ID.


> Definition

```
DELETE https://api.kvass.ai/resources/<resource_id>
```


> Example request:

``` http
DELETE /resources/596c643ed57ba203be2cf1c9 HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "created": {"$date": 1500275774067},
    "method" "asset",
    "deleted": true,
    "description": "An updated Resource",
    "image_url": "http://reference.kvass.ai/images/logo.png",
    "name": "Resource 2",
    "product": {"$oid": "58f9f856b70e2a56c4a0db3d"},
    "reference_id": "abc1234567890abc",
    "status": "IDLE",
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "_id": {"$oid": "596c643ed57ba203be2cf1c9"},
    "updated": {"$date": 1500278314163},
    "user": {"$oid": "57ee9c72d76d431f85111432"}
}
```


## Resource Search
Search by ID, name, reference_id, description and image_url. Returns
an `array` that matches the query. Filter the query by using pagination.

Arguments | Type | Description
--------- | ---- | -------
query | `string`| Partial or full string of `name`, `reference_id`, `description` or `image_url`
include_deleted | `boolean`| If `true`, the request also returns deleted resources
size | `number` | Number of resources per page _default is 10_
page | `number` | Defines which page to retrieve _default is 0_
from_date | `number` | Start date, `timestamp` format _default is `None`_
to_date | `number` | End date, `timestamp` format _default is `None`_
sort | `string` | Field used to sort results _default is `-created`_

> Definition

```
GET https://api.kvass.ai/resources/search
```


> Example request:

``` http
GET /resources/search?query=updated HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
{
    "created": {"$date": 1500275774067},
    "method" "asset",
    "deleted": true,
    "description": "An updated Resource",
    "image_url": "http://reference.kvass.ai/images/logo.png",
    "reference_id": "abc1234567890abc",
    "name": "Resource 2",
    "product": {"$oid": "58f9f856b70e2a56c4a0db3d"},
    "status": "IDLE",
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "_id": {"$oid": "596c643ed57ba203be2cf1c9"},
    "updated": {"$date": 1500278314163},
    "user": {"$oid": "57ee9c72d76d431f85111432"}
}
]
```


## Resource Status
Default status for the resource is `CREATED`. Change the status
by updating the resource.

Arguments | Description
--------- | -----
CREATED | "CREATED"
PROCESSING | "PROCESSING"
UTILIZED | "UTILIZED"
IDLE | "IDLE"
ON_THE_WAY_FROM | "ON_THE_WAY_FROM"
ON_THE_WAY_TO | "ON_THE_WAY_TO"
DONE | "DONE"
