# Providers

Provider is a child class of [`User`](#users). They are users that provide
services to other users.

## Provider object

Attributes | Type | Description
---------- | ---- | -------
max_travel_time | `number` | The maximum travel time, in hours. _default is 1.0_
max_travel_meters | `number` | The maximum travel distance, in meters. _default is 10000.0_
default_position | `array` | The provider's default position.
default_address | `object` | The provider's default [`Address`](#address).
schedule | `object` | The provider's schedule.
orders | `array` | List of [`Orders`](#orders).
products | `array` | List of [`Products`](#products) the provider provides.
available_on_bank_holidays | `boolean` | Indicates if the provider wishes to be available on bank holidays.  _default `false`_
country | `string` | The provider's country. _default `NOR`_
roles | `array` | Define the roles that a provider can have. _Default is provider_

## Add a New Provider

> Definition

```
POST https://api.kvass.ai/providers
```

> Example request:

``` http
POST /providers HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "first_name": "Jane",
    "last_name": "Roe",
    "email": "jane@email.com",
    "mobile_phone_number": "+4712345678"
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
   "max_travel_meters":10000.0,
   "first_name":"Jane",
   "created":{
      "$date":1495195975641
   },
   "_id":{
      "$oid":"591ee147d57ba28fbe0a3892"
   },
   "default_address":{
      "service":"google",
      "alias":""
   },
   "roles":[
      "provider"
   ],
   "bio":"",
   "deleted":false,
   "national_id":"",
   "country":"NOR",
   "auth0_id":"",
   "email":"jane@email.com",
   "voucher":{
      "consumed":0,
      "uuid":"0000aaaa-aaaa-0000-aaaa-aaaa0000aa00",
      "created":{
         "$date":1495195975643
      },
      "expires":{
         "$date":1526731975643
      },
      "initial_quantity":0
   },
   "last_name":"Roe",
   "addresses":[],
   "note":"",
   "schedule":{
      "day_of_week":[
         {
            "end_minute":0,
            "start_hour":0,
            "start_minute":0,
            "day_of_week":0,
            "end_hour":0
         },
         {
            "end_minute":0,
            "start_hour":0,
            "start_minute":0,
            "day_of_week":5,
            "end_hour":0
         }
      ],
      "working_sections":[],
      "valid_from":{
         "$date":1495195200000
      }
   },
   "stripe_customer_id":"",
   "avatar":"",
   "billing_address":{
      "service":"google",
      "alias":""
   },
   "mobile_phone_number":"+4712345678",
   "orders":[],
   "modified":{
      "$date":1495195975643
   },
   "max_travel_time":1.0,
   "company":{
      "$oid":"57ee9c71d76d431f8511142f"
   },
   "products":[],
   "active":true,
   "available_on_bank_holidays":false,
   "tags":[],
   "_cls":"User.Provider"
}
```

Creates a new Provider.

Argument | Type | Description
---------- | ---- | -------
**first_name** | `string` | Provider's first name
**last_name** | `string` | Provider's last name
email | `string` | Provider's e-mail address
mobile_phone_number |  `string` | Provider's phone number
billing_address | `string` | Provider's billing address
bio | `string` | Biographic note about the provider


## Retrieve a Provider

> Definition

```
GET https://api.kvass.ai/providers/<provider_id>
```

> Example request:

``` http
GET /providers/<provider_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{  
   "first_name":"Jane",
   "email":"jane@email.com",
   "_id":{  
      "$oid":"591ee147d57ba28fbe0a3892"
   },
   "auth0_id":"",
   "tags":[  
      "fuzz"
   ],
   "available_on_bank_holidays":false,
   "mobile_phone_number":"+4712345678",
   "voucher":{  
      "initial_quantity":0,
      "created":{  
         "$date":1493962078102
      },
      "expires":{  
         "$date":1525498078102
      },
      "uuid":"e9878af0-c607-486c-872f-6e23e7a15821",
      "consumed":0
   },
   "orders":[],
   "note":"",
   "stripe_customer_id":"",
   "max_travel_time":1.0,
   "created":{  
      "$date":1493962078119
   },
   "schedule":{  
      "day_of_week":[  
         {  
            "end_hour":18,
            "day_of_week":0,
            "end_minute":30,
            "start_minute":30,
            "start_hour":8
         },
         {  
            "end_hour":18,
            "day_of_week":1,
            "end_minute":30,
            "start_minute":30,
            "start_hour":8
         }
      ],
      "valid_from":{  
         "$date":1493098078525
      },
      "working_sections":[]
   },
   "max_travel_meters":10000.0,
   "country":"NOR",
   "last_name":"Roe",
   "_cls":"User.Provider",
   "billing_address":{  
      "service":"google",
      "alias":""
   },
   "active":true,
   "products":[],
   "addresses":[],
   "national_id":"",
   "avatar":"",
   "modified":{  
      "$date":1493962078526
   },
   "company":{  
      "$oid":"57ee9c71d76d431f8511142f"
   },
   "deleted":false,
   "roles":[  
      "provider"
   ],
   "bio":""
}
```

Retrieves the Provider with a particular ID

Arguments | Type | Description
--------- | ---- | ------
**provider_id** | `string` | ID of the queried provider

## List All Providers

> Definition

```
GET https://api.kvass.ai/providers
```

> Example request:

``` http
GET /providers HTTP/1.1
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
       "first_name":"Jane",
       "email":"jane@email.com",
       "_id":{  
          "$oid":"591ee147d57ba28fbe0a3892"
       },
       "auth0_id":"",
       "tags":[  
          "fuzz"
       ],
       "available_on_bank_holidays":false,
       "mobile_phone_number":"+4712345678",
       "voucher":{  
          "initial_quantity":0,
          "created":{  
             "$date":1493962078102
          },
          "expires":{  
             "$date":1525498078102
          },
          "uuid":"e9878af0-c607-486c-872f-6e23e7a15821",
          "consumed":0
       },
       "orders":[],
       "note":"",
       "stripe_customer_id":"",
       "max_travel_time":1.0,
       "created":{  
          "$date":1493962078119
       },
       "schedule":{  
          "day_of_week":[  
             {  
                "end_hour":18,
                "day_of_week":0,
                "end_minute":30,
                "start_minute":30,
                "start_hour":8
             },
             {  
                "end_hour":18,
                "day_of_week":1,
                "end_minute":30,
                "start_minute":30,
                "start_hour":8
             }
          ],
          "valid_from":{  
             "$date":1493098078525
          },
          "working_sections":[]
       },
       "max_travel_meters":10000.0,
       "country":"NOR",
       "last_name":"Roe",
       "_cls":"User.Provider",
       "billing_address":{  
          "service":"google",
          "alias":""
       },
       "active":true,
       "products":[],
       "addresses":[],
       "national_id":"",
       "avatar":"",
       "modified":{  
          "$date":1493962078526
       },
       "company":{  
          "$oid":"57ee9c71d76d431f8511142f"
       },
       "deleted":false,
       "roles":[  
          "provider"
       ],
       "bio":""
    }
]
```

Retrieves a list of all Providers.

Arguments | Type | Description
--------- | ---- | ------
size | `integer` | Number of items to retrieve. _default is 10_
page | `integer` | Which page to retrieve. _default is 0_
include_deleted | `boolean` | If `true`, deleted products are also listed. _default is `false`_
latitude | `number` | Define the latitude. _default is `None`_
longitude | `number` | Define the longitude. _default is `None`_
