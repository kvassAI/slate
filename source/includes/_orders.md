# Orders

Orders are the result of a User buying a given set of Products and it's what connects Users and Providers.

## Order object

Attributes | Description
---------- | -------
**created** | The date the Order was generated.
**modified** | The date the Order was last modified.
**user** | User associated with Order
human_id | Human readable ID that identifies the Order easily
provider | Provider assigned to Order
items | List of Items associated with an Order
units | Amount of unique Items associated with the Order
total_quantity | Total number of Products in Order
**total_amount** | Amount as a Float with decimal points (`.`). Example: 10.23 NOK.
**currency** | 3 letter ISO currency code as defined by [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217).
top_up_amount | Extra amount to fulfill company's minimum Order value 
delivery_time | Time expected for delivery
delivery_address | Address used for delivery
deliveries | If the Order has multiple deliveries, this is used for storing that Delivery history
billing_address | Address used for billing purposes
payments | List of Payment objects associated with Order
note | Field used to provide extra notes between User and Provider
status | The status of the Invoice. Default is CREATED. Additionally there is: "SCHEDULED", "DONE", "FAILED", "CANCELLED"
**order_status** | Status of the Order. Default is CREATED. Additionally there are: PROCESSING, SUCCESS, CANCELLED, FAILED
**delivery_status** | Status of the Delivery. Default is PENDING. Additionally there are: ACCEPTED, REJECTED, ON_THE_WAY, ARRIVED, DONE, READY_FOR_DELIVERY

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
  "created": {
    "$date": 1488475795244
  },
  "company": {
    "$oid": "586faf7445ed910006373513"
  },
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
  "_id": {
    "$oid": "58b85693d76d439fdacea41b"
  },
  "total_amount": 40.0,
  "delivery_time": {
    "$date": 1472022000000
  },
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
  "user": {
    "$oid": "57ee9c72d76d431f85111432"
  },
  "units": 2,
  "delivery_status": "PENDING",
  "modified": {
    "$date": 1488475795244
  },
  "currency": "NOK"
}
```

Creates a new Order.

Argument | Description
---------- | -------
user | Id of the User who's creating the Order
items | List of Order's items
delivery_time | Time expected for delivery 
delivery_address | Address used for delivery
billing_address | Address used for billing purposes
currency | 3 letter ISO currency code as defined by [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217)
note | Field used to provide extra notes between User and Provider
