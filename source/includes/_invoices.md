# Invoices

Invoices allow you to store invoice information for later payment or accounting information sent by a provider 
of a product or service to the [`User`](#users). 

## Invoice object

Attributes | Type | Description
---------- | ---- | ------
**user** | `object` | [`User`](#users) associated with invoice
**amount** | `float` | The invoice amount. Two decimals.
**currency** | `string` | ISO 4217 code of currency, for example: "EUR"
**account_number** | `string` | Account number of the issuer that will receive the payment.
**user** | `string` | User who is being invoiced, referenced by `user_id`
due_date | `object` | The due date of the invoice, `timestamp` format
issued_date | `object` | The date the invoice was generated, `timestamp` format
message | `string` | Message (KID) used for paying the invoice as it will appear in the issuer's bank
issuer | `object`| [`Issuer`](#issuers) of the invoice
issuer_alias | `string` | The user may want to use an alias for the [`issuer`](#issuers)
image_url | `string` | The url for the image of the invoice that is returned from the ocr
status | `string` | The status of the Invoice. Default is CREATED. Additionally there are: "SCHEDULED", "DONE", "FAILED" and "CANCELLED"
payments | `array` | List of [`Payment`](#payments) objects associated with invoice.
invoice_lines | `array` | An array of [`invoice lines`](#invoice-lines)
fee | `float` | Additional fee that will be added to the `amount`
kind | `string` | The type or kind of invoice.
deleted | `boolean` | Whether the invoice is deleted or not. _Default is `false`_
accounting_reference | `string` | A reference ID from your Accounting system.
external_reference | `string` | Field to set your own reference or if you are using a third party.
If your company is using an Account system supported by KVASS this external_reference field will automatically be setup.

## Invoice Lines

Invoice lines are additional information that can be appended to an invoice. 
Each line has the format presented below.

Attributes | Type | Description
---------- | ---- | ------
**description** | `string` | Description of unit. 
**quantity** | `number` | Quantity of unit `product` or `resource`. _Default is 1_
**amount** | `float`| Unit price. 
**currency** | `string` | Currency of the unit. ISO 4217 currency code. For example "EUR" 
product | `object` | [`Product`](#products) in unit
resource | `object` | [`Resource`](#resources) in unit
vat | `float` | VAT of unit. _From 0.0 to 1.0. Default is 0_
discount | `float` | Discount value. _From 0.0 to 1.0. Default is 0_


## Create an Invoice

> Definition

```
POST https://api.kvass.ai/invoices
```

> Example request:

``` http
POST /invoices HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "user": "57ee9c72d76d431f85111432",
    "amount": 123.45,
    "currency": "NOK",
    "account_number": "12345678903",
    "message": "some transaction or KID number",
    "due_date": "2016-02-10T18:25:43.511Z",
    "image_url": "<image-id>/<image-name>.jpg",
    "issuer": "Big Important Firm AS",
    "issuer_alias": "issuer alias for Issuer",
    "kind": "test"
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
   "currency":"NOK",
   "issuer":{"$oid":"57ee9c72d76d431f85111433"},
   "deleted":false,
   "due_date":{"$date":1476551410009},
   "message":"Some message to the Issuer",
   "account_number":"12345678903",
   "user":{"$oid":"57ee9c72d76d431f85111432"},
   "company":{"$oid":"57ee9c71d76d431f8511142f"},
   "amount":123.45,
   "_id":{"$oid":"57ee9c72d76d431f85111434"},
   "image_url": "<image-id>/<image-name>.jpg",
   "invoice_lines": [],
   "kind": "test"
}
```

Creates a new Invoice.

Attribute | Type | Description
---------- | --- | -------
**user** | `string` | User who is being invoiced, referenced by `user_id`
**amount** | `number` | The amount of invoice _Required field_
**currency** | `string` | ISO 4217 code of currency: "EUR", "USD", "NOK" etc. 
**account_number** | `string` | Account number to which payment should be submitted
due_date | `number` | Due date of invoice, `timestamp` format. _Default is current date if missing_
image_url | `string` | The url for the invoice image
issued_date| `number` | The date the Invoice was issued, `timestamp` format. _Not a required field_
issuer_alias| `string` | Issuer alias that User could set. _Not a required field_
message | `string` | Message included on the invoice, e.g. KID number to be included with payment
invoice_lines | `array` | An array of [`invoice lines`](#invoice-lines)
fee | `float` | Fee added to the amount
kind | `string` | The type or kind of invoice created


## Create an Invoice with Invoice Line

 
> Definition

```
POST https://api.kvass.ai/invoices
```

> Example request:

``` http
POST /invoices HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "user": "5a211ae347e31c001312124e",
    "amount": 123.45,
    "currency": "NOK",
    "account_number": "12345678903",
    "message": "some transaction or KID number",
    "due_date": "2016-02-10T18:25:43.511Z",
    "image_url": "<image-id>/<image-name>.jpg",
    "issuer": "Big Important Firm AS",
    "issuer_alias": "issuer alias for Issuer",
    "invoice_lines": [
        {
            "resource": "596c643ed57ba203be2cf1c9",
            "amount": 200.00,
            "currency": "NOK",
            "description": "A resource",
            "vat": 0.25,
            "discount": 0.1
        },
        {
            "amount": 20.00,
            "currency": "NOK",
            "description": "Standard Shipping"
        }
   ]
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
   "currency":"NOK",
   "issuer":{"$oid":"57ee9c72d76d431f85111433"},
   "deleted":false,
   "due_date":{"$date":1476551410009},
   "message":"Some message to the Issuer",
   "account_number":"12345678903",
   "user":{"$oid":"57ee9c72d76d431f85111432"},
   "company":{"$oid":"57ee9c71d76d431f8511142f"},
   "amount":123.45,
   "_id":{"$oid":"57ee9c72d76d431f85111434"},
   "image_url": "<image-id>/<image-name>.jpg",
   "invoice_lines": [
        {
            "amount": 200.00,
            "currency": "NOK",
            "description": "Standard Shipping",
            "description": "A resource",
            "vat": 0.25,
            "discount": 0.1
        },
        {
            "amount": 20.00,
            "currency": "NOK",
            "description": "Standard Shipping"
        }
   ]
}
```
Creates a new Invoice including invoice lines.
The following example shows you how to create different invoice lines in an Invoice.
The first invoice line contains a [`Resource`](#Resources) but it works the same way with a [`Product`](#Products).

## Retrieve an Invoice

> Definition

```
GET https://api.kvass.ai/invoices/<invoice_id>
```

> Example request:

``` http
GET /invoices/<invoice_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
   "currency":"NOK",
   "issuer":{"$oid":"57ee9c72d76d431f85111433"},
   "deleted":false,
   "due_date":{"$date":1476551410009},
   "message":"12313123",
   "account_number":"12345678903",
   "user":{"$oid":"57ee9c72d76d431f85111432"},
   "company":{"$oid":"57ee9c71d76d431f8511142f"},
   "amount":123.45,
   "_id":{"$oid":"57ee9c72d76d431f85111434"},
   "image_url": "<image-id>/<image-name>.jpg",
   "invoice_lines": []
}
```

Retrieves an Invoice with a given ID.

Argument | Type | Description
-------- | ---- | ------
**invoice_id** | `string` | ID of the queried invoice


## List all Invoices

> Definition

```
GET https://api.kvass.ai/invoices
```

> Example request:

``` http
GET /invoices HTTP/1.1
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
       "currency":"NOK",
       "issuer":{"$oid":"57ee9c72d76d431f85111433"},
       "deleted":false,
       "due_date":{"$date":1476551410009},
       "message":"12313123",
       "account_number":"12345678903",
       "user":{"$oid":"57ee9c72d76d431f85111432"},
       "company":{"$oid":"57ee9c71d76d431f8511142f"},
       "amount":123.45,
       "_id":{"$oid":"57ee9c72d76d431f85111434"},
       "image_url": "<image-id>/<image-name>.jpg",
       "invoice_lines": []
    }
]
```

Retrieves a list of all Invoices associated with the user.

Arguments | Type | Description
--------- | ---- | ------
size | `number` | Number of items to retrieve
page | `number` | Which page to retrieve. _default is 10_
sort | `string` | Field used for sorting results. _default sorting is `-due_date`_.
status | `string` | Status of Invoice. By default there is no value, the API returns invoices regardless the status.Additionally there are : CREATED, SCHEDULED, DONE, FAILED, CANCELLED
from_date | `number` | Start date, `timestamp` format. _default is None_
to_date | `number` | End date, `timestamp` format. _default is None_
date_filter | `string` | Date field used for filter results. _default is `created`_


## Invoices Search

Retrieves a list of Invoices associated with search.

> Definition

```
GET https://api.kvass.ai/invoices/search
```

> Example request by account_number:

``` http
GET /invoices/search?query=12345678903 HTTP/1.1
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
       "currency":"NOK",
       "issuer":{"$oid":"57ee9c72d76d431f85111433"},
       "deleted":false,
       "due_date":{"$date":1476551410009},
       "message":"12313123",
       "account_number":"12345678903",
       "user":{"$oid":"57ee9c72d76d431f85111432"},
       "company":{"$oid":"57ee9c71d76d431f8511142f"},
       "amount":123.45,
       "_id":{"$oid":"57ee9c72d76d431f85111434"},
       "image_url": "<image-id>/<image-name>.jpg",
       "invoice_lines": []
    }
]
```

Arguments | Type | Description
--------- | ---- | -------
**query** | `string` | What you want to search, like **id**, **account_number**, **issuer name**, or **user name**
size | `number` | number of items to retrieve
page | `number` | which page to retrieve. _default page size is 10_
sort | `string` | field used for sorting results. If missing default is "-created". Could be other parameters in the Invoice model, like "-created" or "status".
from_date | `number` | Start date, `timestamp` format. _default is None_
to_date | `number` | End date, `timestamp` format. _default is None_
date_filter | `string` | Date field used for filter results. _default is `created`_
include_deleted | `boolean` | If `true`, deleted invoices are also listed. _default is `false`_

## Update an Invoice

Updates an Invoice with a given ID.

> Definition

```
PUT https://api.kvass.ai/invoices/<invoice_id>
```

> Example request:

``` http
PUT /invoices/<invoice_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```


Argument | Type | Description
-------- | ---- | -----
**invoice_id** | `string` | ID of the Invoice

## Delete invoice

Deletes an invoice with a give ID.

> Definition

```
DELETE https://api.kvass.ai/invoices/<invoice_id>
```

> Example request:

``` http
DELETE /invoices/<invoice_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

Argument | Type | Description
-------- | ---- | -----
**invoice_id** | `string` | ID of the Invoice