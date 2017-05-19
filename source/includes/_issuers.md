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
      "$oid":"58de5a46b70e2a8840ea4aa6"
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
      "$oid":"58de5a47b70e2a8840ea4aae"
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
    "name": "Telenor Norge AS",
    "company": {
        "$oid": "58e104d5b70e2a76c0e94c83"
    }, 
    "deleted": false,
    "verified": false,
    "_id": {
        "$oid": "58e104d5b70e2a76c0e94c85"
    },
    "account_number": "12345678903",
    "logo_url": "http://logok.org/wp-content/uploads/2014/10/Telenor_logo-and-wordmark.png",
    "address": {
        "street_name": "Fornebuveien 6",
        "alias": "",
        "service": "google",
        "city": "Oslo",
        "zip_code": "0556",
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
         "$oid":"58e10791b70e2a516c376a8c"
      },
      "logo_url":"http://logok.org/wp-content/uploads/2014/10/Telenor_logo-and-wordmark.png",
      "address": {
         "country":"NOR",
         "street_name":"Fornebuveien 6",
         "city":"Oslo",
         "zip_code":"0556",
         "service":"google",
         "alias":""
      },
      "created": {
         "$date":1491142545321
      },
      "modified": {
         "$date":1491142545323
      },
      "verified":false,
      "deleted":false,
      "name":"Telenor Norge AS",
      "company": {
         "$oid":"58e10791b70e2a516c376a8a"
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
      "$oid":"58e112acb70e2a326cf1f955"
   },
   "name":"Telenor updated",
   "modified":{
      "$date":1491145388209
   },
   "is_message_mandatory":false,
   "_id":{
      "$oid":"58e112acb70e2a326cf1f957"
   },
   "created":{
      "$date":1491145388109
   },
   "deleted":false,
   "logo_url":"http://logok.org/wp-content/uploads/2014/10/Telenor_logo-and-wordmark.png",
   "address":{
      "service":"google",
      "street_name":"Fornebuveien 6",
      "city":"Oslo",
      "zip_code":"0556",
      "country":"NOR",
      "alias":""
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

``` http
GET /issuers/search?query=tele HTTP/1.1
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
      "name":"Telenor Norge AS",
      "account_number":"12345678903",
      "is_message_mandatory":false,
      "address":{
         "service":"google",
         "street_name":"Fornebuveien 6",
         "zip_code":"0556",
         "alias":"",
         "city":"Oslo",
         "country":"NOR"
      },
      "deleted":false,
      "blacklisted":false,
      "logo_url":"http://logok.org/wp-content/uploads/2014/10/Telenor_logo-and-wordmark.png",
      "active":true,
      "created":{
         "$date":1491565328234
      },
      "modified":{
         "$date":1491565328235
      },
      "_id":{
         "$oid":"58e77b10b70e2a7650ea6521"
      },
      "company":{
         "$oid":"58e77b10b70e2a7650ea651f"
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
      "$oid":"58e798d0b70e2a901ca32396"
   },
   "name":"Telenor Norge AS",
   "account_number":"12345678903",
   "verified":false,
   "deleted":true,
   "_id":{  
      "$oid":"58e798d0b70e2a901ca32398"
   },
   "blacklisted":false,
   "is_message_mandatory":false,
   "address":{  
      "service":"google",
      "alias":"",
      "zip_code":"0556",
      "street_name":"Fornebuveien 6",
      "country":"NOR",
      "city":"Oslo"
   },
   "created":{  
      "$date":1491572944202
   },
   "modified":{  
      "$date":1491572947166
   },
   "logo_url":"http://logok.org/wp-content/uploads/2014/10/Telenor_logo-and-wordmark.png"
}
```

Deletes an Issuer

Argument | Description
-------- | -----------
issuer_id | ID of the Issuer to delete.

