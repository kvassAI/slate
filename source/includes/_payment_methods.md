# Payment Methods

Payment Methods allows you to use multiple payment systems: Credit Card (Stripe, Braintree, etc.), PayPal, DIBS, etc. Furthermore,
you can retrieve this information later for recurring payments and so on.

## Payment Method object

A payment method object varies according to the system, however there are some fields which will always be present.

Attributes | Description
---------- | -------
**user** | User associated with Payment Method
**method** | Type of Payment Method. Currently acceptable methods: `stripe`, `dibs`, `paypal`, etc.
**name** | Name of Payment Method - to be shown to final customer, if needed

## Create a Payment Method

Creates a new payment method. Different methods will have different attributes needed. Below you can find a list of the attributes needed per method:

### DIBS

``` http
POST /payment_methods HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io

{
  "method": "dibs",
  "merchant": "<some-merchant-id>",
  "ticket": "123123"
}
```

Attribute | Description
---------- | -------
**merchant** | merchant id of company
**ticket** | id of card (ticket)
masked_card_number | "XXXXXXXXXXXX1234"
card_prefix | first 5 numbers of the card (for type check: VISA, MasterCard, etc.)

### Paypal

``` http
POST /payment_methods HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io

{
  "method": "paypal",
  "some_token": "..."
}
```

Attribute | Description
---------- | -------
**some_token** | ...


## Retrieve a Payment Method

``` http
GET /payment_methods/<payment-method-id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "masked_card_number":"XXXXXXXXXXXX1234",
  "merchant":"some_merchant_id",
  "user":{
      "$oid":"57ee9705d76d431e56769b38"
  },
  "deleted":false,
  "method":"dibs",
  "company":{
      "$oid":"57ee9705d76d431e56769b35"
  },
  "created":{
      "$date":1475254021326
  },
  "ticket":"some_ticket_id",
  "modified":{
      "$date":1475254021326
  },
  "active":true,
  "_id":{
      "$oid":"57ee9705d76d431e56769b39"
  }
}
```

Retrieves the payment method with the given ID.

Argument | Description
---------- | -------
**payment-method-id** | ID of the desired payment method


## List all Payment Methods

``` http
GET /payment_methods HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "masked_card_number":"XXXXXXXXXXXX1234",
    "merchant":"some_merchant_id",
    "user":{
      "$oid":"57ee9705d76d431e56769b38"
    },
    "deleted":false,
    "method":"dibs",
    "company":{
      "$oid":"57ee9705d76d431e56769b35"
    },
    "created":{
      "$date":1475254021326
    },
    "ticket":"some_ticket_id",
    "modified":{
      "$date":1475254021326
    },
    "active":true,
    "_id":{
      "$oid":"57ee9705d76d431e56769b39"
    }
  }
]
```

Retrieves a list of all Payment Methods associated with the user.

Argument | Description
---------- | -------
size | number of items to retrieve
page | which page to retrieve. _default page size is 10_
order_by | field used for sorting results
