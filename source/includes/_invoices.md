# Invoices

Invoices allow you to store invoice information for later payment.

## Invoice object

Attributes | Description
---------- | -------
user | User associated with Invoice
amount | Amount of invoice as a float.
currency | ISO code of currency, for example: "EUR"
account_number | Account number to issuer, to whom the payment of invoice is done to
message | Message (KID) used for paying invoice. How it will appear in the Issuer's bank
due_date | The due date of the invoice
issued_date | The date the invoice was generated
issuer | Issuer of invoice
issuer_alias | regardless of the issuer existing or not, the user may want to use an alias for the issuer
deleted | Flag that sets Invoice object to deleted
image_url | The url for the invoice image which is returned from the ocr
status | The status of the Invoice. Default is CREATED. Additionally there is: "SCHEDULED", "DONE", "FAILED", "CANCELLED"
created| The date and time the invoice was created
updated | The last time the invoice was updated


## Create an Invoice

> Definition

```
POST https://api.shareactor.io/invoices
```

> Example request:

``` http
POST /invoices HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
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
   "issuer":{
      "$oid":"57ee9c72d76d431f85111433"
   },
   "deleted":false,
   "due_date":{
      "$date":1476551410009
   },
   "message":"12313123",
   "account_number":"12345678903",
   "user":{
      "$oid":"57ee9c72d76d431f85111432"
   },
   "company":{
      "$oid":"57ee9c71d76d431f8511142f"
   },
   "amount":123.45,
   "_id":{
      "$oid":"57ee9c72d76d431f85111434"
   },
   "image_url": "<image-id>/<image-name>.jpg"
}
```

Creates a new Invoice.

Attribute | Description
---------- | -------
**amount** | Amount in float. Required field
**currency** | ISO code of currency: "EUR", "USD", "NOK" etc. Required field
**account_number** | Account number to pay invoice to. Required field
**due_date** | Due date of invoice. If missing, the due_date is today.
image_url | The url for the invoice image
issued_date| The date the Invoice was issued. Not mandatory
issuer_alias| Issuer alias that User could set. Not mandatory
message | Message of transaction. e.g. KID number

## Retrieve an Invoice

> Definition

```
GET https://api.shareactor.io/invoices/<invoice_id>
```

> Example request:

``` http
GET /invoices/<invoice_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
   "currency":"NOK",
   "issuer":{
      "$oid":"57ee9c72d76d431f85111433"
   },
   "deleted":false,
   "due_date":{
      "$date":1476551410009
   },
   "message":"12313123",
   "account_number":"12345678903",
   "user":{
      "$oid":"57ee9c72d76d431f85111432"
   },
   "company":{
      "$oid":"57ee9c71d76d431f8511142f"
   },
   "amount":123.45,
   "_id":{
      "$oid":"57ee9c72d76d431f85111434"
   },
   "image_url": "<image-id>/<image-name>.jpg"
}
```

Retrieves an Invoice with a given ID.

Argument | Description
---------- | -------
**invoice_id** | ID of the queried Invoice


## List all Invoices

> Definition

```
GET https://api.shareactor.io/invoices
```

> Example request:

``` http
GET /invoices HTTP/1.1
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
       "currency":"NOK",
       "issuer":{
          "$oid":"57ee9c72d76d431f85111433"
       },
       "deleted":false,
       "due_date":{
          "$date":1476551410009
       },
       "message":"12313123",
       "account_number":"12345678903",
       "user":{
          "$oid":"57ee9c72d76d431f85111432"
       },
       "company":{
          "$oid":"57ee9c71d76d431f8511142f"
       },
       "amount":123.45,
       "_id":{
          "$oid":"57ee9c72d76d431f85111434"
       },
       "image_url": "<image-id>/<image-name>.jpg"
    }
]
```

Retrieves a list of all Invoices associated with the user.

Argument | Description
---------- | -------
size | number of items to retrieve
page | which page to retrieve. _default page size is 10_
order_by | field used for sorting results. If missing default is "-due_date". Could be other parameters in the Invoice model, like "-created" or "status=DONE".

## Update an invoice
> Definition

```
PUT https://api.shareactor.io/invoices/<invoice_id>
```

> Example request:

``` http
PUT /invoices/<invoice_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io
```

Updates an Invoice with a given ID. For example:

Argument | Description
---------- | -------
**invoice_id** | ID of the queried Invoice

## Delete invoice
> Definition

```
DELETE https://api.shareactor.io/invoices/<invoice_id>
```

> Example request:

``` http
DELETE /invoices/<invoice_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io
```

Deletes an invoice with a give ID.