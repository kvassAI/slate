# Users

Users are your typical Customer.

## User object

Attributes | Type | Description
---------- | ---- | -----
first_name | `string` | First name of the User
last_name | `string` | Last name of the User
email | `string` | E-mail of the User
mobile_phone_number | `string` | Phone number of the User
addresses | `list` [`Address`](#address) | List of Addresses belonging to User
billing_address | [`Address`](#address) | Billing Address of User
bio | `string` | Biographic note about the User
tags | `string` | List of Tags associated with User
roles | `string` | List of Roles associated with User. _default `user`_
deleted | `boolean` | Flag that sets User object to deleted

## Create a User

> Definition

POST https://api.shareactor.io/users

> Example request:

``` http
POST /users HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io

{
  "user": {
    "first_name": "John",
    "last_name": "Doe",
    "email": "john@email.com",
    "mobile_phone_number": "+4712345678"
  }
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "_id":{
    "$oid":"57ee9c72d76d431f85111432"
  },
  "_cls":"User",
  "created":{
    "$date":1475428903950
  },
  "modified":{
    "$date":1475428903951
  },
  "first_name":"John",
  "last_name":"Doe",
  "email":"john@email.com",
  "mobile_phone_number":"+4712345678",
  "addresses":[],
  "billing_address":{},
  "bio":"",
  "note":"",
  "avatar":"",
  "company":{
    "$oid":"57ee9c71d76d431f8511142f"
  },
  "tags":[],
  "stripe_customer_id":"",
  "roles":[
    "user"
  ],
  "deleted":false
  }
```

Creates a new User.

Argument | Type | Description
-------- | ---- | -----
first_name | `string` | First name of the User
last_name | `string` | Last name of the User
email | `string` | E-mail of the User
mobile_phone_number | `string` | Phone number of the User
billing_address | [`Address`](#address) | Billing Address of User
bio | `string` | Biographic note about the User
tags | `string` | List of Tags associated with User


## Retrieve a User

> Definition

GET https://api.shareactor.io/users/<user-id>

> Example request:

``` http
GET /users/<user-id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "_id":{
    "$oid":"57ee9c72d76d431f85111432"
  },
  "_cls":"User",
  "created":{
    "$date":1475428903950
  },
  "modified":{
    "$date":1475428903951
  },
  "first_name":"John",
  "last_name":"Doe",
  "email":"john@email.com",
  "mobile_phone_number":"+4712345678",
  "addresses":[],
  "billing_address":{},
  "bio":"",
  "note":"",
  "avatar":"",
  "company":{
    "$oid":"57ee9c71d76d431f8511142f"
  },
  "tags":[],
  "stripe_customer_id":"",
  "roles":[
    "user"
  ],
  "deleted":false
  }
```

Retrieves User with the given ID.

Argument | Type | Description
-------- | ---- | ------
**user_id** | `string` | ID of the desired User


## Update a User

> Definition

PUT https://api.shareactor.io/users/<user-id>

> Example request:

``` http
PUT /users/<user-id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io

{
  "first_name": "Oliver"
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "_id":{
    "$oid":"57ee9c72d76d431f85111432"
  },
  "_cls":"User",
  "created":{
    "$date":1475428903950
  },
  "modified":{
    "$date":1475428903951
  },
  "first_name":"Oliver",
  "last_name":"Doe",
  "email":"john@email.com",
  "mobile_phone_number":"+4712345678",
  "addresses":[],
  "billing_address":{},
  "bio":"",
  "note":"",
  "avatar":"",
  "company":{
    "$oid":"57ee9c71d76d431f8511142f"
  },
  "tags":[],
  "stripe_customer_id":"",
  "roles":[
    "user"
  ],
  "deleted":false
  }
```

Updates an existing User.

Argument | Type | Description
-------- | ---- | ------
**user_id** | `string` | ID of the User to update
first_name | `string` | First name of the User
last_name | `string` | Last name of the User
email | `string` | E-mail of the User
mobile_phone_number | `string` | Phone number of the User
billing_address | [`Address`](#address) | Billing Address of User
bio | `string` | Biographic note about the User
tags | `string` | List of Tags associated with User


## Delete a User

> Definition

DELETE https://api.shareactor.io/users/<user-id>

> Example request:

``` http
DELETE /users/<user-id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "_id":{
    "$oid":"57ee9c72d76d431f85111432"
  },
  "_cls":"User",
  "created":{
    "$date":1475428903950
  },
  "modified":{
    "$date":1475428903951
  },
  "first_name":"Oliver",
  "last_name":"Doe",
  "email":"john@email.com",
  "mobile_phone_number":"+4712345678",
  "addresses":[],
  "billing_address":{},
  "bio":"",
  "note":"",
  "avatar":"",
  "company":{
    "$oid":"57ee9c71d76d431f8511142f"
  },
  "tags":[],
  "stripe_customer_id":"",
  "roles":[
    "user"
  ],
  "deleted":true
  }
```

Deletes a User.


Argument | Type | Description
-------- | ---- | -------
**user_id** | `string` | ID of the User to delete


## List all Users

> Definition

GET https://api.shareactor.io/users

> Example request:

``` http
GET /users HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
  "_id":{
    "$oid":"57ee9c72d76d431f85111432"
  },
  "_cls":"User",
  "created":{
    "$date":1475428903950
  },
  "modified":{
    "$date":1475428903951
  },
  "first_name":"John",
  "last_name":"Doe",
  "email":"john@email.com",
  "mobile_phone_number":"+4712345678",
  "addresses":[],
  "billing_address":{},
  "bio":"",
  "note":"",
  "avatar":"",
  "company":{
    "$oid":"57ee9c71d76d431f8511142f"
  },
  "tags":[],
  "stripe_customer_id":"",
  "roles":[
    "user"
  ],
  "deleted":false
  }
]
```

Retrieves a list of all Users be logging to some Company.

Argument | Type | Description
-------- | ---- | -------
size | `int` | number of items to retrieve. _default size is 50_
page | `int` | which page to retrieve. _default page 0_
order_by | `string` | field used for sorting results. _default is `last_name`_


## Search User

> Definition

GET https://api.shareactor.io/users/search

> Example request:

``` http
GET /users/search?query=doe HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
  "_id":{
    "$oid":"57ee9c72d76d431f85111432"
  },
  "_cls":"User",
  "created":{
    "$date":1475428903950
  },
  "modified":{
    "$date":1475428903951
  },
  "first_name":"John",
  "last_name":"Doe",
  "email":"john@email.com",
  "mobile_phone_number":"+4712345678",
  "addresses":[],
  "billing_address":{},
  "bio":"",
  "note":"",
  "avatar":"",
  "company":{
    "$oid":"57ee9c71d76d431f8511142f"
  },
  "tags":[],
  "stripe_customer_id":"",
  "roles":[
    "user"
  ],
  "deleted":false
  }
]
```

Retrieves a list of Users whose first or last name match a given query.

Argument | Type | Description
-------- | ---- | -----
query | `string` | Query to use for searching
size | `int` | Number of items to retrieve _default is 10_
page | `int` | Which page to retrieve _default is 0_


## List all Orders for a User

``` http
GET /users/<user-id>/orders HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {}
]
```

Retrieves a list of all Orders associated with a given User.

Arguments | Type | Description
-------- | ----- | -----
**user_id** | `string` | ID of the user to retrieve Orders from
size | `int` | number of items to retrieve. _default is 10_
page | `int` | which page to retrieve. _default is 0_
order_by | `string` | field used for sorting results. _default `created`_
from_date | `timestamp` | Start date. _default now minus 15 days_
to_date | `timestamp` | End date._default now plus 15 days_
date_filter | `string` | Date field used for filter results. _default `created`_
order_status | `string` | Order with specific status are also listed. The value accept comma like separator, e.q: 'success, processing'
delivery_status | `string` | Order with specific delivery status are also listed. The value accept comma like separator, e.q: 'assigning, done, created'

### order_status

Arguments | Description
--------- | -----
CREATED | Order created
PROCESSING | Order is on processing
DECLINED | Order declined for certain reason
FAILED | Order failed for certain reason
SUCCESS | Order succeeded
CANCELLED | Order cancelled

### delivery_status

Arguments | Description
--------- | -----
CREATED | Delivery created
PENDING | Delivery is on pending
PROCESSING | Delivery is on processing
ASSIGNING | Delivery is assigning to a provider
PICKUP_STARTED | Delivery starting to pickup the package
PICKUP_ARRIVED | Delivery arrived on destination
DROPOFF_STARTED | Delivery starting to drop off the package
DROPOFF_ARRIVED | Delivery arrived on destination
CANCELLED | Delivery cancelled
DONE | Delivery is done
UNKNOWN | Delivery status is unknown
QUEUED | Delivery is in queue
    