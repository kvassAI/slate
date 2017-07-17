# Resources

The resource model is [`User`](#Users) specific attributes;
attributes he possess or resources under his control.

## Resources Object

Attribute | Type | Description
--------- | ---- | -------
_id | `object` | The resource ID
user | `object`  | [`User`](#Users) associated with the resource
image_url | `string` | Image associated with resource
items | `array` | List of items associated with resource
name | `string` | Name of Resource item
quantity | `number` | Quantity of resource item
description | `string`| Description of resource item

## Create a new resource
Created a new resource associated with the  [`User`](#Users).

> Definition

```
POST https://api.shareactor.io/resources
```


> Example request:

``` http
POST /resources HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io

{
"items":[
    {"name": "resource 1",
    "quantity": 1,
    "description": "A short description regarding the resource"},
    {"name": "resource 2",
    "quantity": 5,
    "description": "A short description regarding the resource"}
    ],
"image_url": "image url"}
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "updated": {"$date": 1500275774067},
    "_id": {"$oid": "596c643ed57ba203be2cf1c9"},
    "user": {"$oid": "57ee9c72d76d431f85111432"},
    "items": [
        {"quantity": 1,
         "description": "A short description regarding the resource",
         "name": "resource 1"},
        {"quantity": 5,
         "description": "A short description regarding the resource",
         "name": "resource 2"}
    ],
    "deleted": false,
    "created": {"$date": 1500275774067},
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "image_url": "image url"
}

```


Attribute | Type | Description
--------- | ---- | -------
image_url | `string` | Image associated with resource
items | `array` | List of items associated with resource
name | `string` | Name of Resource item
quantity | `number` | Quantity of resource item. If missing, default is 1.
description | `string`| Description of resource item


## Receive a Resource by id
Receive a resource, based on its id.

> Definition

```
GET https://api.shareactor.io/resource/<resource_id>
```

> Example request:

``` http
GET /resources/596c643ed57ba203be2cf1c9 HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "updated": {"$date": 1500275774067},
    "_id": {"$oid": "596c643ed57ba203be2cf1c9"},
    "user": {"$oid": "57ee9c72d76d431f85111432"},
    "items": [
        {"quantity": 1,
         "description": "A short description regarding the resource",
         "name": "resource 1"},
        {"quantity": 5,
         "description":
         "A short description regarding the resource",
         "name": "resource 2"}
    ],
    "deleted": false,
    "created": {"$date": 1500275774067},
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "image_url": "image url"
}

```


## Update resource
Update attributes in resource

Attribute | Type | Description
--------- | ---- | -------
image_url | `string` | Image associated with resource
items | `array` | List of items associated with resource
name | `string` | Name of Resource item
quantity | `number` | Quantity of resource item. If missing, default is 1.
description | `string`| Description of resource item


> Definition

```
PUT https://api.shareactor.io/resources/<resource_id>
```

> Example request:

``` http
PUT /resources/596c643ed57ba203be2cf1c9 HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io

{
"items":[
    {"name": "resource 1",
    "quantity": 1,
    "description": "A short description regarding the resource"},
    {"name": "resource 2",
    "quantity": 5,
    "description": "A short description regarding the resource"},
    {"name": "resource 3",
    "quantity": 2,
    "description": "A short description regarding the resource"},
    ]
}
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "_id": {"$oid": "596c643ed57ba203be2cf1c9"},
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "items":[
        {"name": "resource 1",
         "description": "A short description regarding the resource",
         "quantity": 1},
        {"name": "resource 2",
         "description": "A short description regarding the resource",
         "quantity": 5},
        {"name": "resource 3",
         "description": "A short description regarding the resource",
         "quantity": 2}
    ],
    "user": {"$oid": "57ee9c72d76d431f85111432"},
    "image_url": "image url",
    "created": {"$date": 1500275774067},
    "updated": {"$date": 1500276782322},
    "deleted": false
}

```


## Receive all resources
Return an array with all resources associated with company. It is
possible to filter and sort the results.


> Definition

```
GET https://api.shareactor.io/resources
```

> Example request:

``` http
GET /resources HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

Attribute | Type | Description
--------- | ---- | -------
include_deleted | `boolean`| if `true` the request also returns all deleted subscriptions
size | `number` | Default 10. Number of subscriptions per page.
page | `number` | Default 0.
sort | `string` | Default is `-created`, thus latest created first.


## Delete resource

> Definition

```
DELETE https://api.shareactor.io/resources/<resource_id>
```


> Example request:

``` http
DELETE /resources/596c643ed57ba203be2cf1c9 HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "created": {"$date": 1500275774067},
    "deleted": true,
    "items":[
        {"name": "resource 1",
         "description": "A short description regarding the resource",
         "quantity": 1},
        {"name": "resource 2",
         "description": "A short description regarding the resource",
         "quantity": 5},
        {"name": "resource 3",
         "description": "A short description regarding the resource",
         "quantity": 2}
    ],
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "_id": {"$oid": "596c643ed57ba203be2cf1c9"},
    "updated": {"$date": 1500278314163},
    "image_url": "image url",
    "user": {"$oid": "57ee9c72d76d431f85111432"}
}
