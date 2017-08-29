# Payment Methods

Payment Methods allow you to use multiple payment systems:
Credit Card (Stripe, Braintree, etc.), PayPal, DIBS and more.
Other integrations available on request.

You can also retrieve this information later for recurring payments or
however you may require.

The user has a `default_payment_method` which can be updated in the user object. This default payment method corresponds to the last payment method used by the user to pay for an order.

## Payment Method object

A payment method object varies according to the system,
however there are some fields which will always be present.

Attributes | Type | Description
---------- | ---- | ------
**user** | `object` | [`User`](#users) associated with Payment Method.
**method** | `string` | Type of Payment Method. Currently acceptable methods: `stripe`, `dibs`, `paypal`, `single_paypal`, `future_paypal`.
**name** | `string` | Name of the Payment Method - to be shown to final customer, if desired.


## DIBS

DIBS is a Norwegian payments gateway. It works based on callbacks made via their servers to ours. This callback contains the information about the credit card that was created and we store that as a Payment Method object.

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
**payment_method** | `string` | Must be "dibs" for triggering a DIBS payment method
**merchant** | `string` | Merchant id of company
**transact** | `object` | ID of card (ticket)
**orderid** | `string` | Unique order ID
**share-authorization-header** | `string` | JWT token used for authentication the user
**share-api-key-header** | `string` | Bearer token used for API access
cardnomask | `string` | The last four numbers of the users credit card, e.g., "XXXXXXXXXXXX1234"
cardprefix | `string` | First 5 numbers of the card (for type check: VISA, MasterCard, etc.)
paytype | `string` | Type of credit card: MC, VISA, etc.
expdate | `string` | Expiration date on the credit card
cardholder_name | `string` | The name of the card holder

## Paypal

Paypal allows for 2 types of payments: Single Payments or Future Payments. A Single Payment is the most usual type of transaction and common for simple authentication and capturing of transactions. Future Payments are mostly used for charging the user several times, after giving their consent.

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

In this case, the website/app will create a Payment using their own SDK and then send it to the API for verification and capture. The payment must be in the state `approved`, otherwise the API will return a `403` error.

Attribute | Type | Description
--------- | ---- | ------
**payment_id** | `string` | This is the ID of the payment. The payment recieves a verification to see if its state is "approved", otherwise an exception is raised, `403`.
**payer_id** | `string` | The payment ID created by PayPal.
**method**| `string` | This must be `single_paypal`.


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

This feature is only available for the mobile SDK.  To use it, the app sends the API an authorization code. The API is then exchanged for a Refresh Token, which is also stored in the object. When paying for an invoice, you should also add the `PayPal-Client-Metadata-Id` header to the API `/payments` in [Payments](#payments).

Attribute | Type | Description
--------- | ---- | ------
**code** | `string` | This is an authorization code sent by the mobile app to be exchanged for a Refresh Token.
**method**| `string` | This must be `future_paypal`.


## Retrieve a Payment Method

Retrieves the payment method with a given ID.

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


Attribute | Type | Description
--------- | ---- | ------
**payment-method-id** | `string` | This is the unique ID of the queried payment method.


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
size | `number` | number of items to retrieve
page | `number` | which page to retrieve _default page size is 10_
sort | `string` | field used for sorting results

## Delete payment method

Delete the payment method associated with the user.

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


Argument | Type | Description
--------- | ---- | ------
**payment-method-id** | `string` | This is the unique ID of the payment method.

