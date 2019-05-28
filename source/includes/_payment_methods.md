# Payment Methods

Payment Methods allow you to use multiple payment systems. We currently support Stripe credit card payments.
We are always welcome to incorporating other integrations, so please let us know if you have any suggestions.

You can also retrieve this information for recurring payments or for other required uses.

The user has a `default_payment_method` which can be updated in the user object. This default payment method corresponds to the last payment method used by the user to pay for an order.

## Payment Method object

A payment method object varies according to the type of payment method,
however there are some fields which are present for all payment methods.

Attributes | Type | Description
---------- | ---- | ------
**payment_method** | `string` | Type of Payment Method. Currently acceptable methods: `stripe`, `dibs`, `bank_transfer`, `single_paypal`, `future_paypal`.
**name** | `string` | Name of the Payment Method - to be shown to final customer, if desired.
user | `object` | [`User`](#users) associated with Payment Method.


## Stripe


> Definition

```
POST https://api.kvass.ai/payment_methods
```

<aside class="warning">
Note: If your customers can add a payment method or update their current payment method, ensure you update the KVASS.AI API as well.  When done directly through Stripe, our APIs won’t be notified and the payment will not be processed or tracked on our system.
</aside>


> Example request:

``` http
POST /payment_methods HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
  "payment_method": "stripe",
  "token": "tok_1Bv5yfEeeXxFpLJtpWYeGYuy"
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
  "method": "stripe",
  "name": "Stripe",
  "customer_id": "cus_CJjTlT4Wci2P0u",
  "token": "tok_1Bv5yfEeeXxFpLJtpWYeGYuy",
  "card": {
    "object": "card",
    "address_state": null,
    "fingerprint": "npPw68vQg1usKUWb",
    "metadata": {},
    "exp_year": 2019,
    "country": "US",
    "last4": "4242",
    "address_zip_check": null,
    "address_zip": null,
    "funding": "credit",
    "cvc_check": "unchecked",
    "id": "card_1Bv5yfEeeXxFpLJtPBVfQ1wZ",
    "tokenization_method": null,
    "address_line1": null,
    "exp_month": 12,
    "brand": "Visa",
    "dynamic_last4": null,
    "address_country": null,
    "address_line2": null,
    "address_line1_check": null,
    "name": null,
    "address_city": null
  }
}
```

Attribute | Type | Description
--------- | ---- | ------
**payment_method** | `string` | Must be "stripe" for triggering a STRIPE payment method
**token** | `string` | Token created with the card details (number, expiration month, expiration year, cvc)


## Retrieve a Payment Method

Retrieves the payment method with a given ID.

> Definition

```
GET https://api.kvass.ai/payment_methods/<payment-method-id>
```

> Example request:

``` http
GET /payment_methods/<payment-method-id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
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
GET https://api.kvass.ai/payment_methods
```

> Example request:

``` http
GET /payment_methods HTTP/1.1
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
DELETE https://api.kvass.ai/payment_methods/<payment_method_id>
```

> Example request:

``` http
DELETE /payment_methods/<payment_method_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```


Argument | Type | Description
--------- | ---- | ------
**payment-method-id** | `string` | This is the unique ID of the payment method.

