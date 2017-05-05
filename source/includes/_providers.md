# Providers

Provider is a child class of [`User`](#users).
Providers are your typical Providers.

## Provider object

Attributes | Type | Description
---------- | ---- | -------
max_travel_time | `float` | The maximum time of travel. _default 1.0_
max_travel_meters | `float` | The maximum range of travel _default 10000.0_
default_position | `geoPoint` | The Provider's position by default
default_address | `address` | The Provider's address by default. _default lambda: Address_
schedule | `schedule` | The Provider's schedule. _default lambda: Schedule_
orders | `list` | List of [`Orders`](#orders)
products | `list` | List of [`Products`](#products) of Provider. _default lambda: []_
available_on_bank_holidays | `boolean` | _default `false`_
country | `string` | The Provider's country. _default `'NOR'`_
roles | `list` | Define the rolls that a Provider can have. _default ['provider']_

## Retrieve an Product

``` http 
GET /providers HTTP/1.1 
Content-Type: application/json 
Authorization: Bearer <jwt> 
X-Share-Api-Key: <shareactor-api-key> 
Host: api.shareactor.io 
``` 

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{  
   "first_name":"Mari",
   "email":"johansenemil@gmail.com",
   "_id":{  
      "$oid":"590c0d5eb70e2a13944ad509"
   },
   "auth0_id":"",
   "tags":[  
      "fuzz"
   ],
   "available_on_bank_holidays":false,
   "mobile_phone_number":"+47 42 42 42 42",
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
   "note":",
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
         },
         {  
            "end_hour":18,
            "day_of_week":2,
            "end_minute":30,
            "start_minute":30,
            "start_hour":8
         },
         {  
            "end_hour":18,
            "day_of_week":3,
            "end_minute":30,
            "start_minute":30,
            "start_hour":8
         },
         {  
            "end_hour":18,
            "day_of_week":4,
            "end_minute":30,
            "start_minute":30,
            "start_hour":8
         },
         {  
            "end_hour":18,
            "day_of_week":5,
            "end_minute":30,
            "start_minute":30,
            "start_hour":8
         },
         {  
            "end_hour":18,
            "day_of_week":6,
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
   "last_name":"Iversen",
   "_cls":"User.Provider",
   "billing_address":{  
      "service":"google",
      "alias":"
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
      "$oid":"590c0d5eb70e2a13944ad508"
   },
   "deleted":false,
   "roles":[  
      "provider"
   ],
   "default_address":{  
      "service":"google",
      "alias":"",
      "geo":[  
         59.927152,
         10.741196
      ]
   },
   "bio":""
}
```

Retrieves the provider with a given ID

Arguments | Type | Description
--------- | ---- | ------
**providerid** | `string` | ID of the queried Provider

## List all Providers

``` http 
GET /providers HTTP/1.1 
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
       "first_name":"Mari",
       "email":"johansenemil@gmail.com",
       "_id":{  
          "$oid":"590c0d5eb70e2a13944ad509"
       },
       "auth0_id":"",
       "tags":[  
          "fuzz"
       ],
       "available_on_bank_holidays":false,
       "mobile_phone_number":"+47 42 42 42 42",
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
       "note":",
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
             },
             {  
                "end_hour":18,
                "day_of_week":2,
                "end_minute":30,
                "start_minute":30,
                "start_hour":8
             },
             {  
                "end_hour":18,
                "day_of_week":3,
                "end_minute":30,
                "start_minute":30,
                "start_hour":8
             },
             {  
                "end_hour":18,
                "day_of_week":4,
                "end_minute":30,
                "start_minute":30,
                "start_hour":8
             },
             {  
                "end_hour":18,
                "day_of_week":5,
                "end_minute":30,
                "start_minute":30,
                "start_hour":8
             },
             {  
                "end_hour":18,
                "day_of_week":6,
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
       "last_name":"Iversen",
       "_cls":"User.Provider",
       "billing_address":{  
          "service":"google",
          "alias":"
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
          "$oid":"590c0d5eb70e2a13944ad508"
       },
       "deleted":false,
       "roles":[  
          "provider"
       ],
       "default_address":{  
          "service":"google",
          "alias":"",
          "geo":[  
             59.927152,
             10.741196
          ]
       },
       "bio":""
    }
]
```

Retrieves a list of all Providers.

Arguments | Type | Description
--------- | ---- | ------
size | `int` | number of items to retrieve. _default is 10_
page | `int` | which page to retrieve. _default is 0_
include_deleted | `boolean` | If `true`, deleted products are also listed. _default is `false`_
latitude | `float` | Define the latitude. _default is `none`_
longitude | `float` | Define the longitude. _default is `none`_