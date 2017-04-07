# Payments

When you want to pay for an Order or Invoice (single or multiple items) you need to use the Payment object. Together with a
[Payment Method](#payment-methods), among other fields, you'll be able to create and process payments, as well
as listing all available payments.

When paying for an [Invoice](#invoices) you can also provide the API with a `payment_date` which allows you to schedule payments
for later. If no `payment_date` is provided, the Invoice's `due_date` will become the `payment_date`.

## Payment object

A payment object contains the following attributes.

Attributes | Description
---------- | -------
**user** | User associated with Payment
**amount** | Amount as a Float with decimal points (`.`). Example: 10.23 NOK.
**currency** | 3 letter ISO currency code as defined by [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217).
**human_id** | Human readable ID. 6 character long.
**subject** | Either an Invoice or Order reference.
**current_state** | Current state of operation. Can be one of: `created`, `processing`, `succeeded`, `failed` and `cancelled`.
created | Date the payment was created.
modified | Date if the payment was modified
payment_date | Date for scheduling payment of invoices. Defaults to the due_date of each invoice.
payment_method | ID of already created Payment Method.
billing_address | Address object with billing details.
description | Some additional description, if wanted.
metadata | Some additional information to Payment.
status_code | In case the payment fails, this will have some code from the chosen payment method backend. *(if existing)*
error_message | In case the payment fails, this will have the reason in a textual way. *(if existing)*

## Process a Payment

Creates and process a new Payment. Can't process both invoices and orders at the same time.
All payments should have the following attributes:

> Definition

```
POST https://api.shareactor.io/payments
```

> Example request:

``` http
POST /payments HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io

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
    "$oid": "<company-id>"
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
    "$oid": "<user-id>"
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

Attribute | Description
---------- | -------
**invoices** | List with invoices to be paid. Either this or **orders** must be set.
**orders** | List with orders to be paid. Either this or **invoices** must be set.
**payment_method** | Selected payment method id to use for paying **invoices** or **orders**.
**payment_date** | Date for scheduling payment of **invoices**. Defaults to the `due_date` of each invoice.


## Retrieve a Payment
> Definition

```
GET https://api.shareactor.io/payments/<payment-id>
```

> Example request:

``` http
GET /payments/<payment-id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
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
    "$oid": "<payment-id>"
  },
  "modified": {
    "$date": 1476118043580
  },
  "deleted": false,
  "user": {
    "$oid": "<user-id>"
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

Argument | Description
---------- | -------
**payment-id** | ID of the queried payment

## List all Payments

> Definition

```
GET https://api.shareactor.io/payments/
```

> Example request:

``` http
GET /payments HTTP/1.1
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
    "company": {
      "$oid": "<company-id>"
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
      "$oid": "<user-id>"
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
]
```

Retrieves a list of all Payments associated with the user.

Argument | Description
---------- | -------
size | Number of items to retrieve
page | Which page to retrieve. _default page size is 10_
order_by | Field used for sorting results
status | Status of Payments to filter by

## Update payment

> Definition

```
PUT https://api.shareactor.io/payments/<payment_id>
```

> Example request:

``` http
PUT /payments/<payment_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

```
HTTP/1.1 200 OK
Content-Type: application/json

```

Update a payment. Editable fields:

Argument | Description
---------- | -------
active | Boolean field. Default is `True`
payment_method | Id for payment method.
billing_address | String of address to whom the payment is to.
payment_date | Update payment date. Default payment date is `due_date` for invoices. This will override the `due_date`.
