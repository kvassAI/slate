# Payments

When you want to pay for an Order or Invoice (single or multiple items) you need to use the Payment object. Together with a
[Payment Method](#payment-methods), among other fields, you'll be able to create and process payments, as well
as listing all available payments.

When paying for an [Invoice](#invoices) you can also provide the API with a `payment_date` which allows you to schedule payments
for later. If no `payment_date` is provided, the Invoice's `due_date` will become the `payment_date`.

## Payment object

A payment object contains the following attributes.

Attribute | Type | Description
--------- | ---- | ------
**user** | `object` | [`User`](#Users) associated with Payment
**amount** | `number` | Amount as a Float with decimal points (`.`). _Example: 10.23 NOK_
**currency** | `string` | 3 letter ISO currency code as defined by [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217)
**human_id** | `string` | Human readable ID. 6 character long
**subject** | `string` | Either an Invoice or Order reference
**current_state** | `string` | Current state of operation. Can be one of: `created`, `processing`, `captured`, `succeeded`, `failed` and `cancelled
payment_date | `object` | Date (`timestamp` format) for scheduling payment of invoices. Defaults to the due_date of each invoice
payment_method | `string` | ID of already created Payment Method
billing_address | `object` | [`Address`](#address) object with billing details
description | `string` | Some additional description, if wanted
metadata | `array` |  Additional information on the payment
status_code | `string` | In case the payment fails, this will have some code from the chosen payment method backend. *(if existing)*
error_message | `string` | In case the payment fails, this will have the reason in a textual way. *(if existing)*

## Process a Payment

Creates and process a new Payment. Can't process both invoices and orders at the same time.
All payments should have the following attributes:

> Definition

```
POST https://api.kvass.ai/payments
```

> Example request:

``` http
POST /payments HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "invoices": ["<invoice-id>"],
    "payment_method": "<payment-method-id>"
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
    "$oid": "<payment-id>"
  },
  "modified": {
    "$date": 1476118043580
  },
  "deleted": false,
  "user": {
    "$oid": "57ee9c72d76d431f85111432"
  },
  "amount": 450.2,
  "metadata": {},
  "current_state": "created",
  "active": true,
  "human_id": "51Q4LN",
  "currency": "NOK",
  "subject": "<invoice-id>",
  "payment_method": "<payment-method-id>",
  "payment_date": {
    "$date": 147998016470
  }
}
```

Arguments | Type | Description
--------- | ---- | ------
**invoices** | `array` | List with [`Invoices`](#invoices) to be paid. Either this or **orders** must be set.
**orders** | `array` | List with [`Orders`](#orders) to be paid. Either this or **invoices** must be set.
**payment_method** | `string` | Selected payment method id to use for paying **invoices** or **orders**.
payment_date | `number` | Date (`timestamp` format) for scheduling payment of **invoices**. Defaults to the `due_date` of each invoice or now.


## Retrieve a Payment
> Definition

```
GET https://api.kvass.ai/payments/<payment-id>
```

> Example request:

``` http
GET /payments/<payment-id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
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
    "$oid": "<payment-id>"
  },
  "modified": {
    "$date": 1476118043580
  },
  "deleted": false,
  "user": {
    "$oid": "57ee9c72d76d431f85111432"
  },
  "amount": 450.2,
  "metadata": {},
  "current_state": "created",
  "active": true,
  "human_id": "51rQxLN",
  "currency": "NOK",
  "subject": "<invoice-id>",
  "payment_method": "<payment-method-id>",
  "payment_date": {
    "$date": 147998016470
  }
}
```

Retrieves the payment with a given ID.

Argument | Type | Description
-------- | ---- | ------
**payment-id** | `string` | ID of the queried payment

## List all Payments

> Definition

```
GET https://api.kvass.ai/payments/
```

> Example request:

``` http
GET /payments HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "company": {
      "$oid": "57ee9c71d76d431f8511142f"
    },
    "created": {
      "$date": 1476118043580
    },
    "_id": {
      "$oid": "<payment-id>"
    },
    "modified": {
      "$date": 1476118043580
    },
    "deleted": false,
    "user": {
      "$oid": "57ee9c72d76d431f85111432"
    },
    "amount": 450.2,
    "metadata": {},
    "current_state": "created",
    "active": true,
    "human_id": "51Q4LN",
    "currency": "NOK",
    "subject": "57ee9c72d76d431f85111434",
    "payment_method": "<payment-method-id>",
    "payment_date": {
      "$date": 147998016470
    }
  }
]
```

Retrieves a list of all Payments associated with the user.

Arguments | Type | Description
--------- | ---- | ------
size | `number` | Number of items to retrieve. _default is 10_
page | `number` | Which page to retrieve. _default is 0_
sort | `string` | Field used for sorting results. _default is -modified_
date_filter | `string` | Date field used for filter results.
current_state | `string` | Status of Payments to filter by

## Update payment

> Definition

```
PUT https://api.kvass.ai/payments/<payment_id>
```

> Example request:

``` http
PUT /payments/<payment_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

```
HTTP/1.1 200 OK
Content-Type: application/json

```

Update a payment. Editable fields:

Arguments | Type | Description
--------- | ---- | ------
active | `boolean` | Boolean field. _default is `True`_
payment_method | `string` | Id for payment method
billing_address | `string` | Address to whom the payment is to
payment_date | `number` | Update payment date, `timestamp` format. Default payment date is `due_date` for invoices. This will override the `due_date`
