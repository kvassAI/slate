# Issuers

Issuers receive the payment for an [`invoice`](#invoices).

## Issuers object

Attributes | Type | Description
---------- | ---- | -------
**verified** | `boolean` | Flag that sets issuer object to verified. Changes to true after Anti-Money Laundering (AML) check up.
**blacklisted** | `boolean` | Flag that sets issuer object to blacklisted. `false` if AML status is either `potential_match`, `true_positive` or `true_positive_approved`. `true` if AML status value is either `no_match`, `false_positive` or `true_positive_reject`.
comments | `string` | Add a comment about issuer.
account_number | `string` | Account number of issuer, used when using invoices.
name | `string` | Name of issuer.
logo_url | `string` | Path to logo for issuer.
address | `object` | Billing [`address`](#address) for the issuer.
is_message_mandatory | `boolean` | Flag that determines if `message` field is mandatory for invoices issued by this issuer, e.g. KID number required.


## Create an Issuer

Create a new issuer.

> Definition

````
POST https://api.kvass.ai/issuers
````

> Example request:

``` http
POST /issuers HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai

{
    "account_number": "12345678903",
    "name": "issuer name",
    "logo_url": "<logo-url.png>",
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
   "name":"issuer name",
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
   "logo_url":"<logo-url.png>",
   "account_number":"12345678903",
   "_id":{
      "$oid":"57ee9c72d76d431f85111433"
   }
}
```


Arguments | Type | Description
--------- | ---- | ------
**verified** | `boolean` | Flag that sets issuer object to verified. _default `false`_
**blacklisted** | `boolean` | Flag that sets issuer object to blacklisted. _default `false`_
**account_number** | `string` | Account number to issuer, must contain 11 digits.
comments | `string` | Comments regarding the issuer.
name | `string` | Name of Issuer.
logo_url | `string` | The URL of the issuer's logo.
address | `object` | Billing [`address`](#address) for the issuer.
is_messaging_mandatory | `boolean` | Determines if *message* field is mandatory for invoices issued by this issuer, e.g. KID number required. _default `false`_

## Retrieve an Issuer

Retrieves issuer with a particular ID.

> Definition

```
GET https://api.kvass.ai/issuers/<issuer-id>
```

> Example request:

``` http
GET /issuers/<issuer-id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
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


Argument | Type | Description
-------- | ---- | -------
**issuer_id** | `string` | ID of the queried issuer

## List all Issuers

Retrieves a list of all issuers.

> Definition

```
GET https://api.kvass.ai/issuers
```

> Example request:

``` http
GET /issuers HTTP/1.1
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


Arguments | Type | Description
--------- | ---- | -------
active | `boolean` | Active issuers are also listed
verified | `boolean` | Verified issuers are also listed
blacklisted | `boolean` | Blacklisted issuers are also listed
size | `number` | Number of items to retrieve _default 10_
page | `number` | Which page to retrieve _default 0_
sort | `string` | Field used for sorting results
from_date | `number` | Start date, `timestamp` format
to_date | `number` | End date, `timestamp` format
date_filter | `string` | Date field used for filter results _default `created`_
include_deleted | `boolean` | If `true`, deleted issuers are also listed _default `false`_

### date_filter
Arguments | Description
--------- | -----------
created | Date generated when issuer is created
modified | Date generated when issuer is updated


## Update Issuer

Updates an issuer.

> Definition

```
PUT https://api.kvass.ai/issuers/<issuer-id>
```

> Example request:

``` http
PUT /issuers/<issuer-id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
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


Arguments | Type | Descriptions
--------- | ---- | ------
**issuer_id** | `string` |  ID of the desired issuer.

## Issuer Search

Retrieves a list of Issuers associate with search.

> Definition

```
GET https://api.kvass.ai/issuers/search
```

> Example request:

``` http
GET /issuers/search?query=Big HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
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


Arguments | Type | Description
--------- | ---- | -------
**query** | `string` | What you want to search for, such as **name**, **account_number** or **id**
filters | `string` | Use filters for more accurate results
active | `boolean` | Active issuers are also listed
verified | `boolean` | Verified issuers are also listed
blacklisted | `boolean` | Blacklisted issuers are also listed
sort | `string` | Field used for sorting results _default `-created`_
from_date | `number` | Start date, `timestamp` format _default None_
to_date | `number` | End date, `timestamp` format _default None_
date_filter | `string` | Date field used for filter results _default `created`_
include_deleted | `boolean` | If `true`, deleted issuers are also listed _default `false`_

### Active, Verified & Blacklisted

For all `boolean` flags it is possible to put:

- True: `1`, `'1'` or `'true'`

- False: `0`, `'0'`or `'false'`


## Delete Issuer

Deletes an issuer.

> Definition

```
DELETE https://api.kvass.ai/issuers/<issuer-id>
```

> Example request:

``` http
DELETE /issuers/<issuer-id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <kvass-api-key>
Host: api.kvass.ai
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


Argument | Type | Description
-------- | ---- | -------
**issuer_id** | `string` | ID of the issuer to delete

