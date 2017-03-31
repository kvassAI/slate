# Introduction

The ShareActor API is a standard JSON RESTful API. The API tries to follow the HTTP standards as often as possible, but there might be some deviations from it on some specific scenarios.

All responses from the API, including errors, are returned as JSON objects.

The API is separated into 2 different environments: 

* QA: [https://qa.shareactor.io](https://qa.shareactor.io)
* PROD: [https://api.shareactor.io](https://api.shareactor.io)

To separate the API usage for both environments, we'll issue two different API keys, one per environment.


## Authentication

``` http
GET / HTTP/1.1
Content-Type: application/json
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

> Make sure to replace `<shareactor-api-key>` with your API key.

ShareActor uses API keys to allow access to the API. If you want access to our APIs please get in touch through our [website](https://www.shareactor.io/contact).

All API requests should include this key in the headers like the following:

`X-Share-Api-Key: <shareactor-api-key>`

<aside class="notice">
You must replace <code>shareactor-api-key</code> with your personal API key.
</aside>

## Errors

<aside class="notice">The errors below might change slightly between API calls</aside>

The Shareactor API uses the following error codes:


Error Code | Reason | Description
---------- | ------- | -------
400 | Bad Request | Your request is malformed or missing mandatory data
401 | Unauthorized | Your API key is either wrong, missing or you have no permissions for such request
404 | Not Found | The specified resource could not be found
500 | Internal Server Error | We had a problem with our server. Try again later.
503 | Service Unavailable | We're temporarily offline for maintenance. Please try again later.

## Expanding objects

> Example request:

```http
GET /payment_methods/<payment-method-id>/?expand=user HTTP/1.1
Content-Type: application/json
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

> Example response:

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
   "masked_card_number":"XXXXXXXXXXXX1234",
   "user": {
        "first_name": "John",
        "last_name": "Doe"
    }
}
```

Many response objects will contain IDs for related objects in the response. For example, an Order might have a User ID associated. If you want to expand the actual user information you can use the `expand` query parameter for that.

You can also nest expand requests with the dot property. For example, requesting payments.payment_method on an order will expand the payments property into a list of Payment objects, and will then expand the payment method property into a full Payment Method object. 

You can expand multiple objects at once by identifying multiple items in the expand array, separated with commas.


## Common Object fields

Here's a list of fields which you will see often on the API responses:

Field | Description
---------- | -------
created  | Date when object was created
modified | Date when object was last modified
deleted  | Flag that shows if object has been deleted
active   | Flag that shows if object is active or not
company  | Company associated with object
user     | User associated with object


## Pagination

> Example request using `size` and `page`: 

``` http
GET /orders?size=10&page=1 HTTP/1.1
Content-Type: application/json
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

> Example request using only `size`:

``` http
GET /orders?size=100 HTTP/1.1
Content-Type: application/json
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

All top-level API resources have support for bulk fetches via "list" API methods. For instance you can list orders, list users, list providers, etc.
These list API methods share a common structure, taking at least these two parameters: size and page.

Shareactor uses a pagination based on a page size (`size`) and current page (`page`). With these parameters you can retrieve the data the way you need and display it using your own pagination scheme.

One of the missing features we are looking into implementing is the return of a response header with total count: `Share-Pagination-Total: 212`