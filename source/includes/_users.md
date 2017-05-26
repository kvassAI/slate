# Users

Users are your typical Customer.

## User object

Attributes | Description
---------- | -------
first_name | First name of the User
last_name | Last name of the User
email | E-mail of the User
mobile_phone_number | Phone number of the User
addresses | List of Addresses belonging to User
billing_address | Billing Address of User
bio | Biographic note about the User
tags | List of Tags associated with User
roles | List of Roles associated with User
deleted | Flag that sets User object to deleted

## Create a User

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
    "$oid":"[User_id](#users)"
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
    "$oid":"[Company_id](#companies)"
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

Argument | Description
---------- | -------
first_name | First name of the User
last_name | Last name of the User
email | E-mail of the User
mobile_phone_number | Phone number of the User
billing_address | Billing Address of User
bio | Biographic note about the User
tags | List of Tags associated with User


## Retrieve a User

``` http
GET /users/<user_id> HTTP/1.1
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
    "$oid":"[User_id](#users)"
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
    "$oid":"[Company_id](#companies)"
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

Argument | Description
---------- | -------
**user_id** | ID of the desired User


## Update a User

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
    "$oid":"[User_id](#users)"
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
    "$oid":"[Company_id](#companies)"
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

Argument | Description
---------- | -------
user-id | ID of the User to update
first_name | First name of the User
last_name | Last name of the User
email | E-mail of the User
mobile_phone_number | Phone number of the User
billing_address | Billing Address of User
bio | Biographic note about the User
tags | List of Tags associated with User


## Delete a User

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
    "$oid":"[User_id](#users)"
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
    "$oid":"[Company_id](#companies)"
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


Argument | Description
---------- | -------
user-id | ID of the User to delete


## List all Users

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
    "$oid":"[User_id](#users)"
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
    "$oid":"[Company_id](#companies)"
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

Retrieves a list of all Users beloging to some Company.

Argument | Description
---------- | -------
size | number of items to retrieve
page | which page to retrieve. _default page size is 10_
order_by | field used for sorting results


## Search User

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
    "$oid":"[User_id](#users)"
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
    "$oid":"[Company_id](#companies)"
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

Argument | Description
---------- | -------
query | Query to use for searching
size | Number of items to retrieve
page | Which page to retrieve. _default page size is 10_


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

Argument | Description
---------- | -------
user-id | ID of the user to retrieve Orders from
size | number of items to retrieve
page | which page to retrieve. _default page size is 10_
order_by | field used for sorting results
