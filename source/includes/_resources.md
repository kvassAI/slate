# Resources

The resource model is [`User`](#Users) specific attributes;
attributes he possess or resources under his control.

## Resources Object

Attribute | Type | Description
--------- | ---- | -------
_id | `object` | The resource ID
user | `object`  | [`User`](#Users) associated with the resource
name | `string` | Name of Resource item
description | `string`| Description of resource item
image_url | `string` | Image associated with resource
reference_id | `string` | Reference id associated with resource
product | `object`  | [`Product`](#Products) associated with the resource
status | `string` | Status for the resource. Default is `CREATED`. Additional: `PROCESSING`, `UTILIZED`, `IDLE`

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
    "product": "58f9f856b70e2a56c4a0db3d",
    "description": "description of the resource",
    "name": "name of resource",
    "image_url": "http://docs.shareactor.io/images/logo.png"}
```
``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "updated": {"$date": 1500275774067},
    "_id": {"$oid": "596c643ed57ba203be2cf1c9"},
    "user": {"$oid": "57ee9c72d76d431f85111432"},
    "description": "description of the resource",
    "image_url": "http://docs.shareactor.io/images/logo.png",
    "name": "name of resource",
    "product": {"$oid": "58f9f856b70e2a56c4a0db3d"},
    "status": "CREATED",
    "deleted": false,
    "created": {"$date": 1500275774067},
    "company": {"$oid": "57ee9c71d76d431f8511142f"}
}

```


Attribute | Type | Description
--------- | ---- | -------
name | `string` | Name of Resource item
description | `string`| Description of resource item
status | `string` | Status for the resource. Default is `CREATED`. Additional: `PROCESSING`, `UTILIZED`, `IDLE`
product | `string`  | [`Product`](#Products) id to be associated to the resource
image_url | `string` | Image url associated with resource
reference_id | `string` | Customizable id for resource


## Receive a Resource by id
Receive a resource, based on its id.

> Definition

```
GET https://api.shareactor.io/resources/<resource_id>
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
    "description": "description of the resource",
    "image_url": "http://docs.shareactor.io/images/logo.png",
    "name": "name of resource",
    "product": {"$oid": "58f9f856b70e2a56c4a0db3d"},
    "status": "CREATED",
    "deleted": false,
    "created": {"$date": 1500275774067},
    "company": {"$oid": "57ee9c71d76d431f8511142f"}
}

```


## Update resource
Update attributes in resource

Attribute | Type | Description
--------- | ---- | -------
name | `string` | Name of Resource item
description | `string`| Description of resource item
status | `string` | Status for the resource. Default is `CREATED`. Additional: `PROCESSING`, `UTILIZED`, `IDLE`
product | `string`  | [`Product`](#Products) id to be associated to the resource
image_url | `string` | Image url associated with resource
reference_id | `string` | Customizable id for resource

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
    "_id": {"$oid": "596c643ed57ba203be2cf1c9"},
    "user": {"$oid": "57ee9c72d76d431f85111432"},
    "description": "An updated Resource",
    "image_url": "http://docs.shareactor.io/images/logo.png",
    "name": "Resource 2",
    "product": {"$oid": "58f9f856b70e2a56c4a0db3d"},
    "status": "IDLE",
    "deleted": false,
    "created": {"$date": 1500275774067},
    "company": {"$oid": "57ee9c71d76d431f8511142f"}
}

```


## Receive all resources
Return an array with all resources associated with request and company.
It is possible to add filters and sort to the query.


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
include_deleted | `boolean`| if `true` the request also returns deleted subscriptions
size | `number` | Default 10. Number of subscriptions per page.
page | `number` | Default 0.
from_date | `number` | Start date, `timestamp` format. _default is None_
to_date | `number` | End date, `timestamp` format. _default is None_
sort | `string` | Default is `-created`, thus latest created first.
status | `string` | Default is `CREATED`


## Delete resource
Delete the resource associated with resource id.


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
    "description": "An updated Resource",
    "image_url": "http://docs.shareactor.io/images/logo.png",
    "name": "Resource 2",
    "product": {"$oid": "58f9f856b70e2a56c4a0db3d"},
    "status": "IDLE",
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "_id": {"$oid": "596c643ed57ba203be2cf1c9"},
    "updated": {"$date": 1500278314163},
    "user": {"$oid": "57ee9c72d76d431f85111432"}
}
```


## Resources Status

Arguments | Description
--------- | -----
CREATED | "CREATED"
PROCESSING | "PROCESSING"
UTILIZED | "UTILIZED"
IDLE | "IDLE"
