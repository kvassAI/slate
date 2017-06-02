# Issuers


## Issuers object
Attributes | Description
---------- | -------
created | The date the Issuer was generated.
updated | The date the Issuer was last updated.
active | Flag that sets Issuer object to active.
deleted | Flag that set Issuer object to deleted.
verified | Flag that set Issuer object to verified. Changes to true after AML check up.
blacklisted | Flag that set Issuer object to blacklisted. `false` if AML status is either `Potential Match`, `true Positive` or `true Positive`. `true` if AML status value is either `No Match`, `false Positive` or `true Positive - Reject`.
comments | Comments used for add a comment about Issuer.
account_number | Account number to Issuer. Used when handling Invoice's
name | Name of Issuer
logo_url | Path for logo for Issuer.
address | Address corresponding to payment address.
is_message_mandatory | Flag that determines if `message` field is mandatory for invoices issued by this issuer.


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

Attribute | Description
--------- | -----------
**created** | Created is date generated when Issuer created. Default value `now date`: `timestamp`.
**modified** | Date generated when Issuer is updated. Default value `now date`: `timestamp`.
**active** | Active is `boolean`. Default `true`.
**deleted** | Deleted is `boolean`. Default `false`.
**verified** | Verified is `boolean`. Default `false`.
**blacklisted** | Blacklisted is `boolean`. Default `false`.
**account_number** | Account_number is `string`, must contain 11 digits.
comments | Comments. `string`.
name | Name of Issuer. `string`.
logo_url | The URL of the Issuer's logo. `string`.
address | Issuer address. As an [`Address`](#address) object.
is_messaging_mandatory | Determines if *message* field is mandatory for invoices issued by this issuer. `boolean`. Default value `false`.

## Retrieve an Issuer

> Definition

GET https://api.shareactor.io/issuers/<issuer_id>

> Example request:

``` http
GET /issuers/<issuer_id> HTTP/1.1
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

Argument | Description
-------- | -----------
**issuer_id** | ID of the queried Issuer.

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

Arguments | Description
--------- | -----------
size | Number of items to retrieve
page | Which page to retrieve. _default page size is 10_
from_date | Start date. Type value `timestamp`.
to_date | End date. Type value `timestamp`.
date_filter | Date field used for filter results.
deleted | It's a `boolean`. If `true` issuer is deleted.
sorting | Field used for sorting results

### date_filter
Arguments | Description
--------- | -----------
created | Date generated when Issuer is created.
modified | Date generated when Issuer is updated.


## Update Issuer

> Definition

PUT https://api.shareactor.io/issuers/issuer_id

> Example request:

``` http
PUT /issuers/<issuer_id> HTTP/1.1
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

Arguments | Descriptions
--------- | ----------
**issuer_id** | ID of the desired Issuer.

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

Arguments | Description
--------- | -----------
**query** | What you want to search, like **name** or **account_number**. `string`
filters | In search you can use filters for more accurate search.


### Filters

For all `Boolean` flags it is possible to put:

- True: `1`, `'1'` or `'true'`

- False: `0`, `'0'`or `'false'`

Arguments | Descriptions
--------- | ---------
active | Active is `Boolean`.
verified | Verified is `Boolean`.
blacklisted | Blacklisted is `Boolean`.

## Delete Issuer

> Definition

DELETE https://api.shareactor.io/issuers/<issuer_id>

> Example request:

``` http
DELETE /issuers/<issuer_id> HTTP/1.1
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

Argument | Description
-------- | -----------
issuer_id | ID of the Issuer to delete.

