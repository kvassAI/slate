# Introduction

Welcome to the Shareactor API! Our APIs intend to provide businesses with the capability of porting their backends into our platform and start worrying with other things rather than tech!

We have language bindings in Shell and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

... Some other text here ...

## Authorization

``` http
GET / HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
Host: api.shareactor.io
```

```python
import requests

headers = {'Authorization': 'Bearer <shareactor-api-key>'}

requests.get('https://api.shareactor.io/...', headers=headers)
```

> Make sure to replace `<shareactor-api-key>` with your API key.

Shareactor uses API keys to allow access to the API. If you want access to our APIs please get in touch on our [website](http://www.shareactor.io).

All API requests should include this key in the headers like the following:

`Authorization: Bearer <shareactor-api-key>`

<aside class="notice">
You must replace <code>shareactor-api-key</code> with your personal API key.
</aside>

## Sessions

``` http
GET / HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io
```

Some API calls will require a user session. In that case you should use the following additional header:

`X-Share-Session: <session_id>`

Check the section on [Digits Authentication](#users) for more information on how to get this header.

<aside class="notice">
You must replace <code>session-id</code> with your Session id.
</aside>

## Errors

<aside class="notice">The errors below might change slightly between API calls</aside>

The Shareactor API uses the following error codes:


Error Code | Reason | Description
---------- | ------- | -------
400 | Bad Request | Your request is malformed or missing mandatory data
401 | Unauthorized | Your API key is either wrong, missing or missing session information (X-Share-Session)
404 | Not Found | The specified resource could not be found
500 | Internal Server Error | We had a problem with our server. Try again later.
503 | Service Unavailable | We're temporarially offline for maintanance. Please try again later.

## Expanding objects

> Without the `expand` parameter:

```http
GET /payment_methods/<payment_method_id> HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
   "masked_card_number":"XXXXXXXXXXXX1234",
   "merchant":"some_merchant_id",
   "user":{
      "$oid":"57ee9705d76d431e56769b38"
   },
   "_id":{
      "$oid":"57ee9705d76d431e56769b39"
   }
}
```

> With the `expand` parameter:

```http
GET /payment_methods/<payment_method_id>/?expand=user HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
   "masked_card_number":"XXXXXXXXXXXX1234",
   "merchant":"some_merchant_id",
   "user": {
        "first_name": "John",
        "last_name": "Doe",
        "mobile_phone_number": "+4791234567",
        "email": "john@email.com"
    },
   "_id":{
      "$oid":"57ee9705d76d431e56769b39"
   }
}
```

Many response objects will contain IDs for related objects in the response. For example, an Order might have a customer id associated. If you want to expand the actual customer information you can use the `expand` query parameter.


## Common Object fields

Here's a list of fields which you will see very often on the API response:

Field | Description
---------- | -------
created  | Date when object was created
modified | Date when object was last modified
deleted  | Flag that shows if object has been deleted
active   | Flag that shows if object is active or not
company  | Company associated with object

## Pagination

``` http
GET /orders?size=10&page=1 HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io
```

``` http
GET /orders?size=100 HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Share-Session: <session-id>
Host: api.shareactor.io
```

All top-level API resources have support for bulk fetches via "list" API methods. For instance you can list orders, list users, list providers, etc.
These list API methods share a common structure, taking at least these two parameters: size and page.

Shareactor uses a pagination based on a page size (`size`) and current page (`page`). With these parameters you can retrieve the data the way you need and display it
using your own pagination scheme.

