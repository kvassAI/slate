# Payments

When you want to pay for an Order or Invoice (single or multiple items) you need to use the Payment object. Together with a
[Payment Method](#payment-methods), amount and currency, among other fields, you'll be able to create and process payments, as well
as listing all available payments.

Some customers prefer to create the payment object before the actualy paying moment, to keep track of all states. For that case we allow the user to create a Payment object and they process it later. If you do not need this, you can just capture the payment by passing the payment details directly on that API call.

## Payment object

A payment object contains the following attributes.

Attributes | Description
---------- | -------
**user** | User associated with Payment
**amount** | Amount as an Integer. Do not use decimal points for separating units from decimal points, for example: 10EUR becomes amount: `1000`; 22.53EUR becomes `2253`.
**currency** | 3 letter currency code as defined by [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217).
**human_id** | Human readable ID.
**payment_date** | Date to execute payment. Defaults to now.
**invoices** | List with invoices to be paid. Either this or **orders** must be set.
**orders** | List with orders to be paid. Either this or **invoices** must be set.
**current_state** | Current state of operation. Can be one of: `created`, `processing`, `succeeded`, `failed` and `cancelled`.
payment_method | ID of already created Payment Method.
billing_address | Address object with billing details.
description | Some additional description, if wanted.
metadata | Some additional information to Payment.
status_code | In case the payment fails, this will have some code from the chosen payment method backend. *(if existing)*
error_message | In case the payment fails, this will have the reason in a textual way. *(if existing)*

## Create a Payment

Creates a new payment object. All payments should have the following attributes:

``` http
POST /payments HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io

{
    "amount": 10.00,
    "currency": "NOK",
    "invoices": ["<invoice-id>"]
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
  "orders": [],
  "user": {
    "$oid": "<user-id>"
  },
  "amount": 450.2,
  "metadata": {},
  "current_state": "created",
  "active": true,
  "human_id": "51rQxLN",
  "currency": "NOK",
  "payment_date": {
    "$date": 1476118043580
  },
  "invoices": [
    {
      "$oid": "<invoice-id>"
    }
  ]
}



```

Attribute | Description
---------- | -------
**amount** | Amount as an Integer. Do not use decimal points for separating units from decimal points, for example: 10EUR becomes amount: `1000`; 22.53EUR becomes `2253`. The final amount must be the same as the subjects being paid for (orders or invoices).
**currency** | 3 letter currency code as defined by [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217).
**invoices** | List with invoices to be paid. Either this or **orders** must be set.
**orders** | List with orders to be paid. Either this or **invoices** must be set.


## Capture Created Payment

Capture already created Payment.

``` http
POST /payments/<payment-id>/pay HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io

{
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
    "$date": 1476118623795
  },
  "deleted": false,
  "orders": [],
  "user": {
    "$oid": "<user-id>"
  },
  "amount": 450.2,
  "metadata": {},
  "current_state": "succeeded",
  "active": true,
  "human_id": "51rQxLN",
  "currency": "NOK",
  "payment_date": {
    "$date": 1476118043580
  },
  "invoices": [
    {
      "$oid": "<invoice-id>"
    }
  ]
}
```

Attribute | Description
---------- | -------
**payment-id** | The Payment ID to capture
**payment_method** | ID of already existing and valid Payment Method.
**payment_date** | Date to execute payment. Defaults to now.

## Capture New Payment

Capture a new Payment. For this case, we will create a Payment object ourselves and then capture that same payment. This is seamless for the user.

``` http
POST /payments/pay HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io

{
  "payment_method": "<payment-method-id>",
  "amount": 450.2,
  "currency": "NOK",
  "invoices": ["<invoice-id>"]
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
    "$date": 1476118623795
  },
  "deleted": false,
  "orders": [],
  "user": {
    "$oid": "<user-id>"
  },
  "amount": 450.2,
  "metadata": {},
  "current_state": "succeeded",
  "active": true,
  "human_id": "51rQxLN",
  "currency": "NOK",
  "payment_date": {
    "$date": 1476118043580
  },
  "invoices": [
    {
      "$oid": "<invoice-id>"
    }
  ]
}
```

Attribute | Description
---------- | -------
**payment_method** | ID of already existing and valid Payment Method.
**payment_date** | Date to execute payment. Defaults to now.
**amount** | Amount as an Integer. Do not use decimal points for separating units from decimal points, for example: 10EUR becomes amount: `1000`; 22.53EUR becomes `2253`. The final amount must be the same as the subjects being paid for (orders or invoices).
**currency** | 3 letter currency code as defined by [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217).
**invoices** | List with invoices to be paid. Either this or **orders** must be set.
**orders** | List with orders to be paid. Either this or **invoices** must be set.



