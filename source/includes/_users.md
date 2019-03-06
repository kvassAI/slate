# Users

User is the class that holds all the CRUD functions for your customers and is the parent class to Provider.  

## User object

Attributes | Type | Description
---------- | ---- | -----
**first_name** | `string` | First name of the user
**last_name** | `string` | Last name of the user
salutation | `string` | Title of the user
email | `string` | E-mail of the user
mobile_phone_number | `string` | Phone number of the user
addresses | `array` |  An `array`of [`Addresses`](#address) associated with the user
billing_address | [`object`](#address) | Billing Address of the user
default_payment_method | `string` | Default payment method of the  user
bio | `string` | Biographic note about the user
tags | `array` | List of tags associated with user
roles | `array` | List of roles associated with user
voucher | `object` | [`Voucher`](#credits) of the user
note | `string` | Additional notes regarding the user

## Create a User

Creates a new user.

> Definition

```
POST https://api.kvass.ai/users/new
```

> Example request:

``` http
POST /users/new HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai

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
  "default_payment_method":"",
  "bio":"",
  "note":"",
  "avatar":"",
  "company":{"$oid":"57ee9c71d76d431f8511142f"},
  "tags":[],
  "stripe_customer_id":"",
  "roles":["user"],
  "deleted":true,
  "voucher": {}
}
```

Argument | Type | Description
-------- | ---- | -----
**first_name** | `string` | First name of the user
**last_name** | `string` | Last name of the user
salutation | `string` | Title of the user
email | `string` | E-mail of the user
mobile_phone_number | `string` | Phone number of the user
addresses | `array` |  An `array`of [`Addresses`](#address) associated with the user
billing_address | `object` | Billing [`Address`](#address) of the user
bio | `string` | Biographic note about the user
tags | `array` | List of tags associated with the user
note | `string` | Additional notes regarding the user


## Create a User with a JWT token
See [`Authorization`](#authorization) for how to get a JWT token.
With the route POST `/v2/users`, you could both create a new user with a JWT token, and
log in a user with their JWT token.

> Definition

```
POST https://api.kvass.ai/v2/users
```

> Example request:

``` http
POST /v2/users HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai

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
                "street_name":"Streetname 7"
             }]
  }
}
```


``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "roles": ["user"],
    "tags": [],
    "auth0_id": "some-id",
    "mobile_phone_number": "+4712345678",
    "billing_address":
        {"service": "google",
        "alias": " "},
    "voucher": {"uuid": "b498e02b-2031-4d84-b55b-b460d22f44b4",
                "consumed": 0,
                "initial_quantity": 0,
                "created": {"$date": 1512383756277},
                "expires": {"$date": 1543919756277}},
    "_cls": "User",
    "created": {"$date": 1512383756277},
    "addresses":
        [{"zip_code": "0556",
        "city": "Danielsen",
        "street_name": "Streetname 7",
        "country": "Paraguay",
        "alias": " ",
        "service": "google"}],
    "company": {"$oid": "57ee9c71d76d431f8511142f"},
    "email": "john@email.com",
    "national_id": "1234567890",
    "modified": {"$date": 1512383756279},
    "avatar": " ",
    "bio": " ",
    "stripe_customer_id": " ",
    "note": " ",
    "first_name": "John",
    "_id": {"$oid": "5a25250cd57ba213edcba515"},
    "new_customer": true,
    "last_name": "Doe",
    "last_login": {"$date": 1512383756278},
    "deleted": false,
    "voucher": {}
}

```



## Retrieve a User

> Definition

```
GET https://api.kvass.ai/users/<user-id>
```

> Example request:

``` http
GET /users/<user-id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
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
  "deleted":true,
  "voucher": {}
}
```

Retrieves a user with the given ID.

Argument | Type | Description
-------- | ---- | ------
**user_id** | `string` | ID of the desired user


## Update a User

Updates an existing user.

> Definition

```
PUT https://api.kvass.ai/users/<user-id>
```

> Example request:

``` http
PUT /users/<user-id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai

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
  "deleted":false,
  "voucher": {}
}
```


Argument | Type | Description
-------- | ---- | ------
**user_id** | `string` | ID of the user to update
first_name | `string` | First name of the user
last_name | `string` | Last name of the user
salutation | `string` | Title of the user
email | `string` | E-mail of the user
mobile_phone_number | `string` | Phone number of the user
billing_address | `object`| Billing [`Address`](#address) of the user
default_payment_method | `string` | Default payment method of the  user
bio | `string` | Biographic note about the user
tags | `array` | List of tags associated with the user
note | `string` | Additional notes regarding the user


## Delete a User

Deletes a User.

> Definition

```
DELETE https://api.kvass.ai/users/<user-id>
```

> Example request:

``` http
DELETE /users/<user-id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
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
  "default_payment_method":{"$oid":"62ee9c72d76d431f85111432"},
  "bio":"",
  "note":"",
  "avatar":"",
  "company":{"$oid":"57ee9c71d76d431f8511142f"},
  "tags":[],
  "stripe_customer_id":"",
  "roles":["user"],
  "deleted":true,
  "voucher": {}
}
```

Argument | Type | Description
-------- | ---- | -------
**user_id** | `string` | ID of the user to delete


## List All Users

> Definition

```
GET https://api.kvass.ai/users
```

> Example request:

``` http
GET /users HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
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
  "deleted":false,
  "voucher": {}
  }
]
```

Retrieves a list of all users associated with a company.

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

> Definition

```
GET https://api.kvass.ai/users/search
```

> Example request:

``` http
GET /users/search?query=doe HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
Host: api.kvass.ai
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
  "deleted":false,
  "voucher": {}
  }
]
```

Retrieves a list of users whose first or last name matches a given query.

Argument | Type | Description
-------- | ---- | -----
query | `string` | What you want to search, e.g., **first name**, **last name** or **id**
size | `number` | Number of items to retrieve _default is 10_
page | `number` | Which page to retrieve _default is 0_
sort | `string` | Field used for sorting results  _default is `-created` if missing; other parameters in the user model could be used, e.g., "last_name" or "first_name"_
from_date | `number` | Start date, `timestamp` format _default is `None`_
to_date | `number` | End date, `timestamp` format _default is `None`_
date_filter | `string` | Date field used to filter results. _default is `created`_
include_deleted | `boolean` | If `true`, deleted users are also listed _default is `false`_

## List All Orders for a User

> Definition

```
GET https://api.kvass.ai/users/<user-id>/orders
```

> Example request:
``` http
GET /users/<user-id>/orders HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {}
]
```

Retrieves a list of all orders associated with a given user.

Arguments | Type | Description
-------- | ----- | -----
**user_id** | `string` | ID of the user to retrieve Orders for.
size | `number` | Number of items to retrieve.
page | `number` | Which page to retrieve. _default is 10_
sort | `string` | Field used for sorting results. _default is `created`_
from_date | `number` | Start date, `timestamp` format. _default is current date minus 15 days_
to_date | `number` | End date, `timestamp` format. _default current date plus 15 days_
date_filter | `string` | Date field used to filter results. _default is `created`_
order_status | `string` | Orders with a specific status are also listed. Values can be separated by a comma, e.g., "success, processing".
delivery_status | `string` | Orders with a specific delivery status are also listed. Values can be separated by a comma, e.g.,  "assigning, done, created."

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


## List All Payment Methods for a User

> Definition

```
GET https://api.kvass.ai/users/<user-id>/payment_methods
```

> Example request:
``` http
GET /users/<user-id>/payment_methods HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json
[
    {
       "_id":{"$oid":<payment-method-id>},
       "_cls":"PaymentMethod.StripePaymentMethod",
       "created":{"$date":1538058324877},
       "modified":{"$date":1538058327405},
       "deleted":false,
       "active":true,
       "user":{"$oid":"57ee9c72d76d431f85111432"},
       "company":{"$oid":"591ee15db70e2a10acb65362"},
       "name":"Stripe",
       "method":"stripe",
       "token":"tok_1DF0QiEeeXxFpLJtthyCQWVz",
       "customer_id":"cus_DgNBPtNphW2W7r",
       "card":{
          "id":"card_1DF0QiEeeXxFpLJtg4AE1T5z",
          "object":"card",
          "address_city":null,
          "address_country":null,
          "address_line1":null,
          "address_line1_check":null,
          "address_line2":null,
          "address_state":null,
          "address_zip":null,
          "address_zip_check":null,
          "brand":"Visa",
          "country":"US",
          "cvc_check":"unchecked",
          "dynamic_last4":null,
          "exp_month":12,
          "exp_year":2019,
          "fingerprint":"npPw68vQg1usKUWb",
          "funding":"credit",
          "last4":"4242",
          "metadata":{},
          "name":null,
          "tokenization_method":null
       }
    }
]
```

Retrieves a list of all [Payments Method](#payment_methods) associated with a given user id.

Arguments | Type | Description
-------- | ----- | -----
**user_id** | `string` | ID of the user to retrieve Orders for.
size | `number` | Number of items to retrieve.
page | `number` | Which page to retrieve. _default is 10_
sort | `string` | Field used for sorting results. _default is `created`_
from_date | `number` | Start date, `timestamp` format. _default is current date minus 15 days_
to_date | `number` | End date, `timestamp` format. _default current date plus 15 days_
date_filter | `string` | Date field used to filter results. _default is `created`_
include_deleted | `boolean` | If `true`, deleted users are also listed _default is `false`_
