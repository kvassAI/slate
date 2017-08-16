# Invoices

Invoices allow you to store invoice information for later payment.

## Invoice object

Attributes | Type | Description
---------- | ---- | ------
**user** | `object` | [`User`](#users) associated with invoice
**amount** | `number` | The invoice amount. Two decimals.
**currency** | `string` | ISO 4217 code of currency, for example: "EUR"
**account_number** | `string` | Account number of the issuer that will receive the payment.
due_date | `object` | The due date of the invoice, `timestamp` format
issued_date | `object` | The date the invoice was generated, `timestamp` format
message | `string` | Message (KID) used for paying the invoice as it will appear in the issuer's bank
issuer | `object`| [`Issuer`](#issuers) of the invoice
issuer_alias | `string` | Regardless of the issuer existing or not, the user may want to use an alias for the issuer
image_url | `string` | The url for the image of the invoice that is returned from the ocr
status | `string` | The status of the Invoice. Default is CREATED. Additionally there are: "SCHEDULED", "DONE", "FAILED" and "CANCELLED"


## Create an Invoice

> Definition

```
POST https://api.shareactor.io/invoices
```

> Example request:

``` http
POST /invoices HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io

{
    "message": "some transaction or KID number",
    "amount": 123.45,
    "currency": "NOK",
    "account_number": "12345678903",
    "due_date": "2016-02-10T18:25:43.511Z",
    "image_url": "<image-id>/<image-name>.jpg",
    "issuer": "Big Important Firm AS",
    "issuer_alias": "issuer alias for Issuer"
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
   "image_url": "<image-id>/<image-name>.jpg"
}
```

Creates a new Invoice.

Attribute | Type | Description
---------- | --- | -------
**amount** | `number` | The amount of invoice _Required field_
**currency** | `string` | ISO 4217 code of currency: "EUR", "USD", "NOK" etc. _Required field_
**account_number** | `string` | Account number to which payment should be submitted _Required field_
due_date | `number` | Due date of invoice, `timestamp` format. _Default is current date if missing_
image_url | `string` | The url for the invoice image
issued_date| `number` | The date the Invoice was issued, `timestamp` format. _Not a required field_
issuer_alias| `string` | Issuer alias that User could set. _Not a required field_
message | `string` | Message included on the invoice, e.g. KID number to be included with payment


## Retrieve an Invoice

> Definition

```
GET https://api.shareactor.io/invoices/<invoice_id>
```

> Example request:

``` http
GET /invoices/<invoice_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
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
   "image_url": "<image-id>/<image-name>.jpg"
}
```

Retrieves an Invoice with a given ID.

Argument | Type | Description
-------- | ---- | ------
**invoice_id** | `string` | ID of the queried invoice


## List all Invoices

> Definition

```
GET https://api.shareactor.io/invoices
```

> Example request:

``` http
GET /invoices HTTP/1.1
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
       "image_url": "<image-id>/<image-name>.jpg"
    }
]
```

Retrieves a list of all Invoices associated with the user.

Arguments | Type | Description
--------- | ---- | ------
size | `number` | number of items to retrieve
page | `number` | which page to retrieve. _default page size is 10_
sort | `string` | field used for sorting results. If missing default is "-due_date". Could be other parameters in the Invoice model, like "-created" or "status=DONE".
status | `string` | Status of Invoice. By default there is no value, the API returns invoices regardless the status.Additionally there are : CREATED, SCHEDULED, DONE, FAILED, CANCELLED
from_date | `number` | Start date, `timestamp` format. _default is None_
to_date | `number` | End date, `timestamp` format. _default is None_
date_filter | `string` | Date field used for filter results. _default is `created`_


## Invoices Search

Retrieves a list of Invoices associate with search.

> Definition

```
GET https://api.shareactor.io/invoices/search
```

> Example request by account_number:

``` http
GET /invoices/search/query=12345678903 HTTP/1.1
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
       "image_url": "<image-id>/<image-name>.jpg"
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

## Update an invoice
> Definition

```
PUT https://api.shareactor.io/invoices/<invoice_id>
```

> Example request:

``` http
PUT /invoices/<invoice_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

Updates an Invoice with a given ID. 

Argument | Type | Description
-------- | ---- | -----
**invoice_id** | `string` | ID of the queried Invoice

## Delete invoice

Deletes an invoice with a give ID.

> Definition

```
DELETE https://api.shareactor.io/invoices/<invoice_id>
```

> Example request:

``` http
DELETE /invoices/<invoice_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```
