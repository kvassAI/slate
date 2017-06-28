# Issuers


## Issuers object

Attributes | Type | Description
---------- | ---- | -------
**verified** | `boolean` | Flag that set Issuer object to verified. Changes to true after AML check up.
**blacklisted** | `boolean` | Flag that set Issuer object to blacklisted. `false` if AML status is either `Potential Match`, `true Positive` or `true Positive`. `true` if AML status value is either `No Match`, `false Positive` or `true Positive - Reject`.
comments | `string` | Comments used for add a comment about Issuer.
account_number | `string` | Account number to Issuer. Used when handling Invoice's
name | `string` | Name of Issuer
logo_url | `string` | Path for logo for Issuer.
address | `object` of [`Address`](#address) | Address corresponding to payment address.
is_message_mandatory | `boolean` | Flag that determines if `message` field is mandatory for invoices issued by this issuer.


## Create an Issuer

> Definition

POST https://api.shareactor.io/issuers

> Example request:

``` http
POST /issuers HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io

{
    "account_number": "12345678903",
    "name": "Create_issuer",
    "logo_url": "https://www.shareactor.io/hs-fs/hubfs/Shareactor_Dec2016_Theme/img/Shareactor_logo.png?t=1490176701994&width=200&name=Shareactor_logo.png",
    "comments": "It is a comment"
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{  
   "created":{
      "$date":1490967111428
   },
   "modified":{
      "$date":1490967111446
   },
   "active":true,
   "name":"Create_issuer",
   "company":{
      "$oid":"57ee9c71d76d431f8511142f"
   },
   "comments":"It is a comment",
   "address":{
      "service":"google",
      "alias":""
   },
   "blacklisted":false,
   "is_message_mandatory":false,
   "verified":false,
   "deleted":false,
   "logo_url":"http://logok.org/wp-content/uploads/2014/10/Telenor_logo-and-wordmark.png",
   "account_number":"12345678903",
   "_id":{
      "$oid":"57ee9c72d76d431f85111433"
   }
}
```

Create a new Issuer.

Arguments | Type | Description
--------- | ---- | ------
**created** | `integer` | Created is date generated when Issuer created, `timestamp` format. _default value `now date`_
**modified** | `integer` | Date generated when Issuer is updated, `timestamp` format. _default value `now date`_
**active** | `boolean` | Flag that sets Issuer object to active. _default `true`_
**deleted** | `boolean` | Flag that sets Issuer object to deleted. _default `false`_
**verified** | `boolean` | Flag that sets Issuer object to verified. _default `false`_
**blacklisted** | `boolean` | Flag that sets Issuer object to blacklisted. _default `false`_
**account_number** | `string` | Account number to Issuer, must contain 11 digits.
comments | `string` | Comments.
name | `string` | Name of Issuer.
logo_url | `string` | The URL of the Issuer's logo.
address | [`object`](#address) | Address corresponding to payment address. 
is_messaging_mandatory | `boolean` | Determines if *message* field is mandatory for invoices issued by this issuer. _default value `false`_

## Retrieve an Issuer

> Definition

GET https://api.shareactor.io/issuers/<issuer-id>

> Example request:

``` http
GET /issuers/<issuer-id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "blacklisted": false,
    "name": "Big Company Inc",
    "company": {
        "$oid": "57ee9c71d76d431f8511142f"
    }, 
    "deleted": false,
    "verified": false,
    "_id": {
        "$oid": "57ee9c72d76d431f85111433"
    },
    "account_number": "12345678903",
    "logo_url": "<logo-url.png>",
    "address": {
        "street_name": "Money Lane 99",
        "alias": "",
        "service": "google",
        "city": "Oslo",
        "zip_code": "1234",
        "country": "NOR"
    }, 
    "is_message_mandatory": false,
    "modified": {
        "$date": 1491141845656
    }, 
    "created": {
        "$date": 1491141845653
    }, 
    "active": true
}
```

Retrieves Issuer with ID.

Argument | Type | Description
-------- | ---- | -------
**issuer_id** | `string` | ID of the queried Issuer.

## List all Issuers

> Definition

GET https://api.shareactor.io/issuers

> Example request:

``` http
GET /issuers HTTP/1.1
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
      "is_message_mandatory":false,
      "account_number":"12345678903",
      "blacklisted":false,
      "active":true,
      "_id": {
         "$oid":"57ee9c72d76d431f85111433"
      },
      "logo_url": "<logo-url.png>",
      "address": { 
            "street_name": "Money Lane 99", 
            "alias": "", 
            "service": "google", 
            "city": "Oslo", 
            "zip_code": "1234", 
            "country": "NOR" 
      },
      "created": {
         "$date":1491142545321
      },
      "modified": {
         "$date":1491142545323
      },
      "verified":false,
      "deleted":false,
      "name":"Big Company Inc",
      "company": {
         "$oid":"57ee9c71d76d431f8511142f"
      }
    }
]
```

Retrieves a list of all Issuers.

Arguments | Type | Description
--------- | ---- | -------
size | `integer` | Number of items to retrieve _default is 10_
page | `integer` | Which page to retrieve _default is 0_
from_date | `integer` | Start date, `timestamp` format.
to_date | `integer` | End date, `timestamp` format.
date_filter | `string` | Date field used for filter results. _default is `created`_
deleted | `boolean` | If `true`, deleted issuer are also listed. _default is `false`_
sorting | `string` | Field used for sorting results

### date_filter
Arguments | Description
--------- | -----------
created | Date generated when Issuer is created.
modified | Date generated when Issuer is updated.


## Update Issuer

> Definition

PUT https://api.shareactor.io/issuers/<issuer-id>

> Example request:

``` http
PUT /issuers/<issuer-id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{  
   "company":{
      "$oid":"57ee9c71d76d431f8511142f"
   },
   "name":"Big Company Ltd",
   "modified":{
      "$date":1491145388209
   },
   "is_message_mandatory":false,
   "_id":{
      "$oid":"57ee9c72d76d431f85111433"
   },
   "created":{
      "$date":1491145388109
   },
   "deleted":false,
   "logo_url":"<logo-url.png>",
    "address": {
        "street_name": "Money Lane 99",
        "alias": "",
        "service": "google",
        "city": "Oslo",
        "zip_code": "1234",
        "country": "NOR"
   },
   "verified":false,
   "account_number":"12345678903",
   "blacklisted":false,
   "active":true
}
```

Updates an Issuer

Arguments | Type | Descriptions
--------- | ---- | ------
**issuer_id** | `string` |  ID of the desired Issuer.

## Issuer Search

> Definition

GET https://api.shareactor.io/issuers/search

> Example request:

``` http
GET /issuers/search?query=Big HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-type: application/json

[  
   {  
      "verified":false,
      "name":"Big Company Inc",
      "account_number":"12345678903",
      "is_message_mandatory":false,
      "logo_url":"<logo-url.png>",
      "address": {
          "street_name": "Money Lane 99",
          "alias": "",
          "service": "google",
          "city": "Oslo",
          "zip_code": "1234",
          "country": "NOR"
      },
      "deleted":false,
      "blacklisted":false,
      "active":true,
      "created":{
         "$date":1491565328234
      },
      "modified":{
         "$date":1491565328235
      },
      "_id":{
         "$oid":"57ee9c72d76d431f85111433"
      },
      "company":{
         "$oid":"57ee9c71d76d431f8511142f"
      }
   }
]
```

Retrieves a list of Issuers associate with search.

Arguments | Type | Description
--------- | -----------
**query** | `string` | What you want to search, like **name** or **account_number**
filters | `string` | Use filters for more accurate results
active | `boolean` | Active issuer are also listed
verified | `boolean` | Verified issuer are also listed
blacklisted | `boolean` | Blacklisted are also listed

### Active, Verified & Blacklisted

For all `Boolean` flags it is possible to put:

- True: `1`, `'1'` or `'true'`

- False: `0`, `'0'`or `'false'`


## Delete Issuer

> Definition

DELETE https://api.shareactor.io/issuers/<issuer-id>

> Example request:

``` http
DELETE /issuers/<issuer-id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{  
   "active":false,
   "company":{  
      "$oid":"57ee9c71d76d431f8511142f"
   },
   "name":"Big Company Inc",
   "account_number":"12345678903",
   "verified":false,
   "deleted":true,
   "_id":{  
      "$oid":"57ee9c72d76d431f85111433"
   },
   "blacklisted":false,
   "is_message_mandatory":false,
   "address": {
       "street_name": "Money Lane 99",
       "alias": "",
       "service": "google",
       "city": "Oslo",
       "zip_code": "1234",
       "country": "NOR"
   },
   "created":{
      "$date":1491572944202
   },
   "modified":{  
      "$date":1491572947166
   },
   "logo_url": "<logo-url.png>"
}
```

Deletes an Issuer

Argument | Type | Description
-------- | ---- | -------
**issuer_id** | `string` | ID of the Issuer to delete.

