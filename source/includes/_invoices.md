# Invoices

Invoices allow you to store invoice information for later payment.

## Invoice object

Attributes | Description
---------- | -------
user | User associated with Invoice
amount | Amount of invoice as a float
currency | ISO code of currency, for example: "EUR"
account_number | Account number to make payment of invoice
message | Message (KID) used for paying invoice. How it'll appear on the Issuer's bank
due_date | The due date of the invoice
issued_date | The date the invoice was generated
issuer | Issuer of invoice
deleted | Flag that sets Invoice object to deleted


## Create an Invoice

``` http
POST /invoices HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io

{
    "message": "some transaction",
    "amount": 123.45,
    "currency": "NOK",
    "account_number": "12345678903",
    "due_date": "2016-02-10T18:25:43.511Z"
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
   }
}
```

Creates a new Invoice.

Attribute | Description
---------- | -------
**message** | Message of transaction
**amount** | Amount in float
**currency** | ISO code of currency: "EUR", "USD", etc.
**account_number** | Account number to pay invoice to
**due_date** | Due date of invoice


## Retrieve an Invoice

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
   }
}
```

Retrieves Invoice with the given ID.

Argument | Description
---------- | -------
**invoice_id** | ID of the desired Invoice


## List all Invoices

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
       }
    }
]
```

Retrieves a list of all Invoices associated with the user.

Argument | Description
---------- | -------
size | number of items to retrieve
page | which page to retrieve. _default page size is 10_
order_by | field used for sorting results
