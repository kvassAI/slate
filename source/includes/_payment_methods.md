# Payment Methods

Payment Methods allows you to use multiple payment systems: Credit Card (Stripe, Braintree, etc.), PayPal, DIBS, etc. Furthermore, you can retrieve this information later for recurring payments and so on.

The user has a `default_payment_method` which can be updated in the user object. This default payment method corresponds to the last payment method used by the user to pay for an order.

## Payment Method object

A payment method object varies according to the system, however there are some fields which will always be present.

Attributes | Type | Description
---------- | ---- | ------
**user** | `object` | [`User`](#users) associated with Payment Method
**method** | `string` | Type of Payment Method. Currently acceptable methods: `stripe`, `dibs`, `paypal`, `single_paypal`, `future_paypal`
**name** | `string` | Name of the Payment Method - to be shown to final customer, if needed


## DIBS

DIBS is a Norwegian payments gateway. It works based on callbacks made via their servers to ours. This callback contains the information about the credit card that was created and we store that as a PaymentMethod object.

> Definition

```
POST https://api.shareactor.io/dibs/payment_method
```

> Example request:

``` http
POST /dibs/payment_method HTTP/1.1
Content-Type: application/json
Host: api.shareactor.io

{
  "payment_method": "dibs",
  "merchant": "<some-merchant-id>",
  "transact": "123123",
  "share-api-key-header": "<api-key>",
  "share-authorization-header": "Bearer <jwt>",
  "cardnomask": "XXXXXXXXXXXX0000",
  "cardprefix": "510010",
  "paytype": "MC",
  "orderid": "<some-order-id>"
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "company": {
    "$oid": "57ee9c71d76d431f8511142f"
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
    "$oid": "57ee9c72d76d431f85111432"
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

Attribute | Type | Description
---------- | ---- | ------
**payment_method** | `string` | must be "dibs" for triggering a DIBS payment method
**merchant** | `string` | merchant id of company
**transact** | `object` | id of card (ticket)
**orderid** | `string` | unique order id
**share-authorization-header** | `` | JWT used by user
**share-api-key-header** | `` | Bearer token used for API access
cardnomask | `string` | "XXXXXXXXXXXX1234"
cardprefix | `string` | first 5 numbers of the card (for type check: VISA, MasterCard, etc.)
paytype | `string` | type of credit card: MC, VISA, etc.
expdate | `string` | expiry date
cardholder_name | `string` | Card holder's name

## Paypal

Paypal allows for 2 types of payments: Single Payments or Future Payments. Single Payments is the most usual type of transaction and common for simple authotication and capturing of transactions. The Future Payments is mostly used for charging the user several times, after giving their consent.

## Single Payment (PayPal)

> Definition

```
POST https://api.shareactor.io/payment_methods
```

> Example request:

``` http
POST /payment_methods HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
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
    "$oid": "57ee9c71d76d431f8511142f"
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
    "$oid": "57ee9c72d76d431f85111432"
  },
  "method": "single_paypal",
  "name": "PayPal",
  "payment_id": "<some-payment-id>",
  "payer_id": "<some-payer-id>",
  "mode": "<sandbox-live>"
}
```

In this case, the website/app will create a Payment using their own SDK and then they send it to the API for verification and capture. The payment must be in the state: `approved` otherwise the API will return a `403` error.

Attribute | Type | Description
--------- | ---- | ------
**payment_id** | `string` | This is the ID of the payment. A verification is ran on this Payment to see its state is "approved", otherwise an exception is raised
**payer_id** | `string` | This is the ID of the creation of the payment, something which is associated with the company.
**method**| `string` | `single_paypal`


## Future Payments (PayPal)

> Definition

```
POST https://api.shareactor.io/payment_methods
```

> Example request:

``` http
POST /payment_methods HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
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
    "$oid": "57ee9c71d76d431f8511142f"
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
    "$oid": "57ee9c72d76d431f85111432"
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

Attribute | Type | Description
--------- | ---- | ------
**code** | `string` | This is an Authorization code sent by the mobile apps to be exchanged by a Refresh Token.
**method**| `string` | `future_paypal`


## Retrieve a Payment Method

> Definition

```
GET https://api.shareactor.io/payment_methods/<payment-method-id>
```

> Example request:

``` http
GET /payment_methods/<payment-method-id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "masked_card_number":"XXXXXXXXXXXX1234",
  "merchant":"some_merchant_id",
  "user":{
      "$oid":"57ee9c72d76d431f85111432"
  },
  "deleted":false,
  "method":"dibs",
  "company":{
      "$oid":"57ee9c71d76d431f8511142f"
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
      "$oid":"<payment-method-id>"
  }
}
```

Retrieves the payment method with a given ID.

Attribute | Type | Description
--------- | ---- | ------
**payment-method-id** | `string` | The payment method ID


## List all Payment Methods

> Definition

```
GET https://api.shareactor.io/payment_methods
```

> Example request:

``` http
GET /payment_methods HTTP/1.1
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
    "masked_card_number":"XXXXXXXXXXXX1234",
    "merchant":"some_merchant_id",
    "user":{
      "$oid":"57ee9c72d76d431f85111432"
    },
    "deleted":false,
    "method":"dibs",
    "company":{
      "$oid":"57ee9c71d76d431f8511142f"
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
      "$oid":"<payment-method-id>"
    }
  }
]
```

Retrieves a list of all Payment Methods associated with the user.

Attribute | Type | Description
--------- | ---- | ------
size | `integer` | number of items to retrieve
page | `integer` | which page to retrieve. _default page size is 10_
order_by | `string` | field used for sorting results

## Delete payment method

> Definition

```
DELETE https://api.shareactor.io/payment_methods/<payment_method_id>
```

> Example request:

``` http
DELETE /payment_methods/<payment_method_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

Delete the payment method associated with the user.