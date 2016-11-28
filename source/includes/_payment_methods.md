# Payment Methods

Payment Methods allows you to use multiple payment systems: Credit Card (Stripe, Braintree, etc.), PayPal, DIBS, etc. Furthermore, you can retrieve this information later for recurring payments and so on.

The user has a `default_payment_method` which can be updated in the user object. This default payment method correspond to the last payment method used by the user to pay for an order.

## Payment Method object

A payment method object varies according to the system, however there are some fields which will always be present.

Attributes | Description
---------- | -------
**user** | User associated with Payment Method
**method** | Type of Payment Method. Currently acceptable methods: `stripe`, `dibs`, `paypal`, etc.
**name** | Name of Payment Method - to be shown to final customer, if needed

## DIBS

DIBS is a Norwegian payments gateway. It works based on callbacks made via their servers to ours. This callback contains the information about the credit card that was created and we store that as a PaymentMethod object.

``` http
POST /dibs/payment_method HTTP/1.1
Content-Type: application/json
Host: api.shareactor.io

{
  "method": "dibs",
  "merchant": "<some-merchant-id>",
  "ticket": "123123"
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "company": {
    "$oid": "<company-id>"
  },
  "created": {
    "$date": 1476118043580
  },
  "_id": {
    "$oid": "<payment-method-id>"
  },
  "modified": {
    "$date": 1476118043580
  },
  "deleted": false,
  "user": {
    "$oid": "<user-id>"
  },
  "method": "dibs",
  "name": "DIBS",
  "merchant": "<some-merchant>",
  "ticket": "<some-ticket-id>",
  "purchase_id": "<some-order-id>",
  "masked_card_number": "XXXXXXXXXXXX1234",
  "card_prefix": "12345",
  "card_type": "VISA",
  "expiry_date": "06/16",
  "cardholder": "John Doe",
  "callback_data": {}
}
```

Attribute | Description
---------- | -------
**merchant** | merchant id of company
**transact** | id of card (ticket)
**orderid** | unique order id
cardnomask | "XXXXXXXXXXXX1234"
cardprefix | first 5 numbers of the card (for type check: VISA, MasterCard, etc.)
paytype | type of credit card: MC, VISA, etc.
expdate | expiry date
cardholder_name | Card holder's name

## Paypal

Paypal allows for 2 types of payments: Single Payments or Future Payments. Single Payments is the usual transaction and it's pretty common for simples authorize/capture transactions. The Future Payments are more used for being able to charge the user over and over again after it has given its consent.

## Single Payments (PayPal)

``` http
POST /payment_methods HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io

{
  "method": "single_paypal",
  "payment_id": "<payment-id>",
  "payer_id": "<payer-id>"
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "company": {
    "$oid": "<company-id>"
  },
  "created": {
    "$date": 1476118043580
  },
  "_id": {
    "$oid": "<payment-method-id>"
  },
  "modified": {
    "$date": 1476118043580
  },
  "deleted": false,
  "user": {
    "$oid": "<user-id>"
  },
  "method": "single_paypal",
  "name": "PayPal",
  "payment_id": "<some-payment-id>",
  "payer_id": "<some-payer-id>",
  "mode": "<sandbox-live>"
}
```

In this case, the website/app will create a Payment using their own SDK and then they send it to the API for verification and capture. The payment must be in the state: `approved` otherwise the API will return a `403` error.

Attribute | Description
---------- | -------
**payment_id** | This is the ID of the payment. A verification is ran on this Payment to see its state is "approved", otherwise an exception is raised
**payer_id** | This is the ID of the creation of the payment, something which is associated with the company.


## Future Payments (PayPal)


``` http
POST /payment_methods HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io

{
  "method": "future_paypal",
  "code": "<some-code>"
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "company": {
    "$oid": "<company-id>"
  },
  "created": {
    "$date": 1476118043580
  },
  "_id": {
    "$oid": "<payment-method-id>"
  },
  "modified": {
    "$date": 1476118043580
  },
  "deleted": false,
  "user": {
    "$oid": "<user-id>"
  },
  "method": "future_paypal",
  "name": "PayPal",
  "authorization_code": "<some-auth-code>",
  "refresh_token": "<some-refresh-token>",
  "correlation_id": "Device ID",
  "mode": "<sandbox-live>"
}
```

This feature is only available for the mobile SDK and for using it, the app needs to send the API an authorization code which the API then changed for a Refresh Token which is also stored in the object. When paying for an invoice, you should also add the `PayPal-Client-Metadata-Id` header to the API `/payments` in [Payments](#payments).

Attribute | Description
---------- | -------
**code** | This is an Authorization code sent by the mobile apps to be exchanged by a Refresh Token.


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
