# Issuers


# Issuers object
Attributes | Description
---------- | -------
created | The date the Issuer was generated.
updated | The date the Issuer was updated.
active | Flag that sets Issuer object to active.
deleted | Flag that set Issuer object to deleted.
verified | Flag that set Issuer object to verified. Change to true when AML check it.
blacklisted | Flag that set Issuer object to blacklisted. Change to true if AML result value is `Potential Match`, `True Positive` or `True Positive`. Change to true if AML result value is `No Match`, `False Positive` or `True Positive - Reject`.
comments | Comments used for add a comment about Issuer.
account_number | Account number make sure used same account number between Issuer and Invoice.
name | Name of Issuer
logo_url | Path of logo for Issuer.
company | The company which belong to Issuer.
address | Address corresponding to payment address.
is_message_mandatory | Flag that determines if `message` field is mandatory for invoices issued by this issuer.


## Create an Issuer

``` http
POST /issuers HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io

{
    'account_number': '12345678903',
    'name': 'Create_issuer',
    'logo_url': "https://www.shareactor.io/hs-fs/hubfs/Shareactor_Dec2016_Theme/img/Shareactor_logo.png?t=1490176701994&width=200&name=Shareactor_logo.png",
    'comments': "It is a comment"
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{  
   'created':{  
      '$date':1490967111428
   },
   'modified':{  
      '$date':1490967111446
   },
   'active':True,
   'name':'Create_issuer',
   'company':{  
      '$oid':'58de5a46b70e2a8840ea4aa6'
   },
   'comments':'It is a comment',
   'address':{  
      'service':'google',
      'alias':''
   },
   'blacklisted':False,
   'is_message_mandatory':False,
   'verified':False,
   'deleted':False,
   'logo_url':'http://logok.org/wp-content/uploads/2014/10/Telenor_logo-and-wordmark.png',
   'account_number':'12345678903',
   '_id':{  
      '$oid':'58de5a47b70e2a8840ea4aae'
   }
}
```

Creates a new Issuer.

Attribute | Description
--------- | -----------
**created** | Created is date generated when Issuer created. Default value `now date`: `timestamp`.
**modified** | Modified is date generated when Issuer updated. Default value `now date`: `timestamp`.
**active** | Active is `boolean`. Default `True`.
**deleted** | Deleted is `boolean`. Default `False`.
**verified** | Verified is `boolean`. Default `False`.
**blacklisted** | Blacklisted is `boolean`. Default `False`.
**account_number** | Account_number is `string`, must contain 11 digit.
comments | Comments is a `string`.
name | Name of Issuer and is `string`.
logo_url | Logo_url is a logo's path of Issuer. It's `string`.
address | Address belong payment. It's an [Address object](#_addresses)
is_messaging_mandatory | Determines if *message* field is mandatory for invoices issued by this issuer. It's a `boolean`. Default value `False`.
company | Company belong to Issuer. It's a [Company object](#_companies)

## Retrieve an Issuer

``` http
GET /issuers/<issuer_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
x-Share-Session: <session-id>
Host: api.shareactor.io
```
