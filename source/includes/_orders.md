# Orders

Orders are the result of a User purchasing a given set of Products.
A key function of this feature os that you can associate a particular
product to a particular provider.


## Order object

Attributes | Type | Description
---------- | ---- | -------
**user** | `object` | [`User`](#users) associated with the order
**total_amount** | `number` | Amount as a number with two decimals. e.g., 12.34
**currency** | `string` | 3 letter ISO currency code as defined by [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217)
**order_status** | `string` | Status of the Order _default is CREATED, additionally there are: PROCESSING, SUCCESS, CANCELLED and FAILED_
**delivery_status** | `string` | Status of the delivery _default is PENDING, additionally there are: ACCEPTED, REJECTED, ON_THE_WAY, ARRIVED, DONE and READY_FOR_DELIVERY_
human_id | `string` | Human readable ID that identifies the order easily. e.g. `3AG7UA`
provider | `string` | Provider assigned to an order
items | `array` | List of items associated with an order
units | `number` | Number of unique products in an order
total_quantity | `number` | Total number of products in an order
top_up_amount | `number` | Extra amount of currency needed to fulfill the company's minimum order value 
delivery_time | `object` | Expected arrival time for delivery, `timestamp` format
delivery_address | `object` | [`Address`](#address) used for delivery
deliveries | `array` | Used to store the delivery history for an order
billing_address | `object` | [`Address`](#address) used for billing purposes
payments | `array` | List of [`Payment`](#payments) objects associated with order
note | `string` | Field used to send notes between an user and a provider


## Create an Order

> Definition

```
POST https://api.shareactor.io/orders
```

> Example request:

``` http
POST /orders HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io

{
  "user": "57ee9c72d76d431f85111432",
  "items": [
    {
      "product": "5703fce3308714000dea1ff1",
      "quantity": 1,
      "discount": 0.1
    },
    {
      "product": "5703fcde308714000dea1fe8",
      "quantity": 3
    }
  ],
  "delivery_time": 1472022000000,
  "delivery_address": {
    "city": "Oslo",
    "geo": [
      59.9294087,
      10.7643042
    ],
    "zip_code": "0557",
    "street_name": "Christies gate 34A",
    "country": "Norway"
  },
  "billing_address": {
    "city": "Oslo",
    "geo": [
      59.9294087,
      10.7643042
    ],
    "zip_code": "0557",
    "street_name": "Christies gate 34A",
    "country": "Norway"
  },
  "currency": "NOK",
  "note": "some type of note"
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "created": {"$date": 1488475795244},
  "company": {"$oid": "586faf7445ed910006373513"},
  "delivery_address": {
    "city": "Oslo",
    "geo": [
      59.9294087,
      10.7643042
    ],
    "zip_code": "0557",
    "street_name": "Christies gate 34A",
    "country": "Norway"
  },
  "billing_address": {
    "city": "Oslo",
    "geo": [
      59.9294087,
      10.7643042
    ],
    "zip_code": "0557",
    "street_name": "Christies gate 34A",
    "country": "Norway"
  },
  "_id": {"$oid": "58b85693d76d439fdacea41b"},
  "total_amount": 40.0,
  "delivery_time": {"$date": 1472022000000},
  "note": "some type of note",
  "total_quantity": 4,
  "order_status": "CREATED",
  "items": [
    {
      "product": "5703fce3308714000dea1ff1",
      "quantity": 1,
      "discount": 0.1
    },
    {
      "product": "5703fcde308714000dea1fe8",
      "quantity": 3
    }
  ],
  "user": {"$oid": "57ee9c72d76d431f85111432"},
  "units": 2,
  "delivery_status": "PENDING",
  "modified": {"$date": 1488475795244},
  "currency": "NOK"
}
```

Creates a new order.

Argument | Type | Description
-------- | ---- | -------
**user** | `object` | Id of the [`user`](#users) who's creating the order
**items** | `array` | List of [`order`](#orders)'s items
**currency** | `string` | 3 letter ISO currency code as defined by [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217)
delivery_time | `number` | Expected time of delivery, `timestamp` format
delivery_address | `object`  | [`Address`](#address) used for delivery
billing_address | `object` | [`Address`](#address) used for billing purposes
note | `string` | Field used to provide extra information to a provider

## Retrieve an Order

> Definition

```
GET https://api.shareactor.io/orders/<orderid>
```

> Example request:

``` http
GET /orders/<orderid> HTTP/1.1
Content-Type: application/json
authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-type: application/json

{
   "units":2,
   "delivery_status":"ACCEPTED",
   "currency":"NOK",
   "human_id":"51Q4LN",
   "payments":[],
   "modified":{"$date":1495196003948},
   "provider":{...},
   "top_up_amount":0.0,
   "stripe_charge_id":"",
   "order_status":"CREATED",
   "company":{"$oid":"591ee15db70e2a10acb65362"},
   "billing_address":{
      "city":"Danielsen",
      "service":"google",
      "alias":"",
      "country":"Paraguay",
      "zip_code":"0556",
      "state":"Oslo",
      "street_name":"Iversenstien 7"
   },
   "deliveries":[],
   "items":[
      {
         "quantity":60,
         "discount":0.9,
         "product":{
            "properties":{},
            "parents":[],
            "path":"/",
            "default_position":[-1, -1],
            "created":{"$date":1494479087000},
            "_id":{"$oid":"591ee15eb70e2a10acb65374"},
            "tags":[],
            "max_distance":0,
            "main_product":true,
            "modified":{"$date":1495195998969},
            "deleted":false,
            "name":"Ivar",
            "vat":0.0,
            "company":{"$oid":"591ee15db70e2a10acb65362"},
            "active":true,
            "_cls":"Product",
            "currency":"USD",
            "company_take":-1.0,
            "_sub_products":[],
            "business_rules":[],
            "price":100.0
         }
      }
   ],
   "note":"",
   "created":{"$date":1495264401233},
   "delivery_address":{
      "city":"Danielsen",
      "service":"google",
      "alias":"",
      "country":"Paraguay",
      "zip_code":"0556",
      "state":"Oslo",
      "street_name":"Iversenstien 7"
   },
   "total_quantity":120,
   "_id":{"$oid":"591ee163b70e2a10acb653a7"},
   "total_amount":1200.0,
   "stripe_refund_id":"",
   "override_company_take":-1.0,
   "user":{...},
   "delivery_time":{"$date":1497009600000}
}
```

Retrieves an Order with a given ID.

Argument | Type | Description
-------- | ---- | ------
**orderid** | `string` | ID of the queried order


## List all Orders

Retrieves a list of all Orders associated with the company's database.
It is also possible to filter the search using pagination.

> Definition

```
GET https://api.shareactor.io/orders
```

> Example request:

``` http
GET /orders HTTP/1.1
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
       "units":2,
       "delivery_status":"ACCEPTED",
       "currency":"NOK",
       "human_id":"51Q4LN",
       "payments":[],
       "modified":{"$date":1495196003948},
       "provider":{...},
       "top_up_amount":0.0,
       "stripe_charge_id":"",
       "order_status":"CREATED",
       "company":{"$oid":"591ee15db70e2a10acb65362"},
       "billing_address":{
          "city":"Danielsen",
          "service":"google",
          "alias":"",
          "country":"Paraguay",
          "zip_code":"0556",
          "state":"Oslo",
          "street_name":"Iversenstien 7"
       },
       "deliveries":[],
       "items":[
          {
             "quantity":60,
             "discount":0.9,
             "product":{
                "properties":{},
                "parents":[],
                "path":"/",
                "default_position":[-1, -1],
                "created":{"$date":1494479087000},
                "_id":{"$oid":"591ee15eb70e2a10acb65374"},
                "tags":[],
                "max_distance":0,
                "main_product":true,
                "modified":{"$date":1495195998969},
                "deleted":false,
                "name":"Ivar",
                "vat":0.0,
                "company":{"$oid":"591ee15db70e2a10acb65362"},
                "active":true,
                "_cls":"Product",
                "currency":"USD",
                "company_take":-1.0,
                "_sub_products":[],
                "business_rules":[],
                "price":100.0
             }
          }
       ],
       "note":"",
       "created":{"$date":1495264401233},
       "delivery_address":{
          "city":"Danielsen",
          "service":"google",
          "alias":"",
          "country":"Paraguay",
          "zip_code":"0556",
          "state":"Oslo",
          "street_name":"Iversenstien 7"
       },
       "total_quantity":120,
       "_id":{"$oid":"591ee163b70e2a10acb653a7"},
       "total_amount":1200.0,
       "stripe_refund_id":"",
       "override_company_take":-1.0,
       "user":{...},
       "delivery_time":{"$date":1497009600000}
    }
]
```


Arguments | Type | Description
--------- | ---- | -------
from_date | `number` | Start date, `timestamp` format _default is None_
to_date | `number` | End date, `timestamp` format _default is None_
date_filter | `string` | Date field used to filter results. _default is created_
size | `number` | Number of items to retrieve _default is 10_
page | `number` | Which page to retrieve _default is 0_
order_status | `string` | Return orders with a specific status
delivery_status | `string` | Return orders with a specific delivery status
sort | `string` | Field used for sorting results

### date_filter

Arguments | Description
--------- | ------
created | Shows orders by date of creation 
delivery | Shows orders by delivery time

### order_status

Arguments | Description
--------- | ------
created | Order is created
processing | Order is being processed
declined | Order is declined for some reason
failed | Order has failed for some reason
success | Order has succeeded
cancelled | Order is cancelled

### delivery_status

Arguments | Description
--------- | -----
CREATED | Delivery is created
PENDING | Delivery is pending
PROCESSING | Delivery is being processed
ASSIGNING | Delivery has been assigned to a provider
PICKUP_STARTED | Pickup from customer has been sent
PICKUP_ARRIVED | Pickup from customer is complete
DROPOFF_STARTED | Dropoff to customer has been sent
DROPOFF_ARRIVED | Dropoff to customer is complete
CANCELLED | Delivery is cancelled
DONE | Delivery task is done
UNKNOWN | Delivery is unknown
QUEUED | Delivery is in queue


## Search Orders

> Definition

```
GET https://api.shareactor.io/orders/search
```

> Example request:

``` http
GET /orders/search?query=51Q4LN HTTP/1.1
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
       "units":2,
       "delivery_status":"ACCEPTED",
       "currency":"NOK",
       "human_id":"51Q4LN",
       "payments":[],
       "modified":{"$date":1495196003948},
       "provider":{...},
       "top_up_amount":0.0,
       "stripe_charge_id":"",
       "order_status":"CREATED",
       "company":{"$oid":"591ee15db70e2a10acb65362"},
       "billing_address":{
          "city":"Danielsen",
          "service":"google",
          "alias":"",
          "country":"Paraguay",
          "zip_code":"0556",
          "state":"Oslo",
          "street_name":"Iversenstien 7"
       },
       "deliveries":[],
       "items":[
          {
             "quantity":60,
             "discount":0.9,
             "product":{
                "properties":{},
                "parents":[],
                "path":"/",
                "default_position":[-1, -1],
                "created":{"$date":1494479087000},
                "_id":{"$oid":"591ee15eb70e2a10acb65374"},
                "tags":[],
                "max_distance":0,
                "main_product":true,
                "modified":{"$date":1495195998969},
                "deleted":false,
                "name":"Ivar",
                "vat":0.0,
                "company":{"$oid":"591ee15db70e2a10acb65362"},
                "active":true,
                "_cls":"Product",
                "currency":"USD",
                "company_take":-1.0,
                "_sub_products":[],
                "business_rules":[],
                "price":100.0
             }
          }
       ],
       "note":"",
       "created":{"$date":1495264401233},
       "delivery_address":{
          "city":"Danielsen",
          "service":"google",
          "alias":"",
          "country":"Paraguay",
          "zip_code":"0556",
          "state":"Oslo",
          "street_name":"Iversenstien 7"
       },
       "total_quantity":120,
       "_id":{"$oid":"591ee163b70e2a10acb653a7"},
       "total_amount":1200.0,
       "stripe_refund_id":"",
       "override_company_take":-1.0,
       "user":{...},
       "delivery_time":{"$date":1497009600000}
    }
]
```

Retrieves a list of all Orders associated with the search parameters.
It is possible to search for id, human_id and first and last name for
Users and Providers associated with orders.

Arguments | Type | Description
--------- | ---- | -------
**query** | `string` | The search query. One of **human_id**, **user name**, **provider name**, or **id**
from_date | `number` | Start date, `timestamp` format. _default is None_
to_date | `number` | End date, `timestamp` format. _default is None_
date_filter | `string` | Date field used for filter results. _default is created_
size | `number` | Number of items to retrieve. _default is 10_
page | `number` | Which page to retrieve. _default is 0_
order_status | `string` | Return orders with specific status
delivery_status | `string` | Return orders with specific delivery status
sort | `string` | Field used for sorting results
