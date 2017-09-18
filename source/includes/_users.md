# Users

User is the class that holds all the CRUD functions for your customers and is the parent class to Provider.  

## User object

Attributes | Type | Description
---------- | ---- | -----
**first_name** | `string` | First name of the user
**last_name** | `string` | Last name of the user
email | `string` | E-mail of the user
mobile_phone_number | `string` | Phone number of the user
addresses | `array` |  An `array`of [`Addresses`](#address) associated with the user
billing_address | [`object`](#address) | Billing Address of the user
bio | `string` | Biographic note about the user
tags | `string` | List of tags associated with user
roles | `string` | List of roles associated with user

## Create a User

Creates a new user.

> Definition

```
POST https://api.shareactor.io/users
```

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
    "mobile_phone_number": "+4712345678",
    "addresses": [{
                "city":"Danielsen",
                "service":"google",
                "alias":"",
                "country":"Paraguay",
                "zip_code":"0556",
                "state":"Oslo",
                "street_name":"Iversenstien 7"
             }]
  }
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "_id":{"$oid":"57ee9c72d76d431f85111432"},
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
  "deleted":true
}
```

Argument | Type | Description
-------- | ---- | -----
first_name | `string` | First name of the user
last_name | `string` | Last name of the user
email | `string` | E-mail of the user
mobile_phone_number | `string` | Phone number of the user
billing_address | `object` | Billing [`Address`](#address) of the user
bio | `string` | Biographic note about the user
tags | `array` | List of tags associated with the user


## Retrieve a User

Retrieves a user with the given ID.

> Definition

```
GET https://api.shareactor.io/users/<user-id>
```

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
  "_id":{"$oid":"57ee9c72d76d431f85111432"},
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
  "deleted":true
}
```


Argument | Type | Description
-------- | ---- | ------
**user_id** | `string` | ID of the desired user


## Update a User

Updates an existing user.

> Definition

```
PUT https://api.shareactor.io/users/<user-id>
```

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
  "_id":{"$oid":"57ee9c72d76d431f85111432"},
  "_cls":"User",
  "created":{"$date":1475428903950},
  "modified":{"$date":1475428903951},
  "first_name":"Oliver",
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
  "deleted":false
}
```


Argument | Type | Description
-------- | ---- | ------
**user_id** | `string` | ID of the user to update
first_name | `string` | First name of the user
last_name | `string` | Last name of the user
email | `string` | E-mail of the user
mobile_phone_number | `string` | Phone number of the user
billing_address | `object`| Billing [`Address`](#address) of the user
bio | `string` | Biographic note about the user
tags | `array` | List of tags associated with the user


## Delete a User

Deletes a User.

> Definition

```
DELETE https://api.shareactor.io/users/<user-id>
```

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
  "_id":{"$oid":"57ee9c72d76d431f85111432"},
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
  "deleted":true
}
```

Argument | Type | Description
-------- | ---- | -------
**user_id** | `string` | ID of the user to delete


## List All Users

Retrieves a list of all users associated with a company.

> Definition

```
GET https://api.shareactor.io/users
```

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
  "_id":{"$oid":"57ee9c72d76d431f85111432"},
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
  "deleted":false
  }
]
```

Argument | Type | Description
-------- | ---- | -------
size | `number` | Number of items to retrieve _default size is 50_
page | `number` | Which page to retrieve _default page 0_
sort | `string` | Field used for sorting results _default is `last_name`_
from_date | `number` | Start date, `timestamp` format _default is `None`_
to_date | `number` | End date, `timestamp` format _default is `None`_
date_filter | `string` | Date field used to filter results _default is `created`_
include_deleted | `boolean` | If `true`, deleted users are also listed _default is `false`_


## Search User

Retrieves a list of users whose first or last name matches a given query.

> Definition

```
GET https://api.shareactor.io/users/search
```

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
  "_id":{"$oid":"57ee9c72d76d431f85111432"},
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
  "deleted":false
  }
]
```

Argument | Type | Description
-------- | ---- | -----
query | `string` | What you want to search, e.g., **first name**, **last name** or **id**
size | `number` | Number of items to retrieve _default is 10_
page | `number` | Which page to retrieve _default is 0_
sort | `string` | Field used for sorting results  _default is `-created` if missing; other parameters in the user model could be used, e.g., "last_name" or "first_name"_
from_date | `number` | Start date, `timestamp` format _default is `None`_
to_date | `number` | End date, `timestamp` format _default is `None`_
date_filter | `string` | Date field used to filter results _default is `created`_
include_deleted | `boolean` | If `true`, deleted users are also listed _default is `false`_

## List All Orders for a User

Retrieves a list of all orders associated with a given user.

> Definition

```
GET https://api.shareactor.io/users/<user-id>/orders
```

> Example request:
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

Arguments | Type | Description
-------- | ----- | -----
**user_id** | `string` | ID of the user to retrieve Orders for.
size | `number` | Number of items to retrieve.
page | `number` | Which page to retrieve. _default is 10_
sort | `string` | Field used for sorting results. _default is `created`_
from_date | `number` | Start date, `timestamp` format. _default is current date minus 15 days_
to_date | `number` | End date, `timestamp` format. _default current date plus 15 days_
date_filter | `string` | Date field used to filter results. _default is `created`_
order_status | `string` | Orders with a specific status are also listed. Values can be separated by a comma, e.g., 'success, processing'.
delivery_status | `string` | Orders with a specific delivery status are also listed. Values can be separated by a comma, e.g.,  'assigning, done, created.'

### order_status

Arguments | Description
--------- | -----
CREATED | Order has been created
PROCESSING | Order is being processed
DECLINED | Order has been declined for a particular reason
FAILED | Order has failed for a particular reason
SUCCESS | Order succeeded
CANCELLED | Order has been cancelled

### delivery_status

Arguments | Description
--------- | -----
CREATED | Delivery has been created
PENDING | Delivery is pending
PROCESSING | Delivery is being processed
ASSIGNING | Delivery is being assigned to a provider
PICKUP_STARTED | Pickup for the package has been sent
PICKUP_ARRIVED | Pickup for the package has arrived
DROPOFF_STARTED | The package has been sent to be dropped off
DROPOFF_ARRIVED | The package has been dropped off
CANCELLED | Delivery is cancelled
DONE | Delivery is done
UNKNOWN | Delivery status is unknown
QUEUED | Delivery is in queue

