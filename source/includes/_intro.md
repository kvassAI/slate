# Introduction

The KVASS API is a standard JSON RESTful API. The API follows the HTTP standards as often as possible, but there might be some deviations from it in some specific scenarios.

All responses from the API, including errors, are returned as JSON objects.

The API is separated into 2 different environments:


Environment | Endpoint
--------- | -------
**QA** | qa.kvass.ai
**PROD** | api.kvass.ai

Each environment has its separate API keys which you can manage through our Admin dashboard.

All the example API calls are using the PROD endpoint.


## Authentication

``` http
GET / HTTP/1.1
Content-Type: application/json
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

> Make sure to replace `<kvass-api-key>` with your API key.

KVASS uses API keys to allow access to the API. If you want access to our APIs, please get in touch through our [website](https://www.kvass.ai/).

All API requests should include this key in the headers*:

`X-Kvass-Api-Key: <kvass-api-key>`

<aside class="notice">
*Note:  You must replace <code>kvass-api-key</code> with your personal API key.
</aside>

## Errors

<aside class="notice">The errors below might change slightly between API calls</aside>

The KVASS API uses the following error codes:


Error Code | Reason | Description
---------- | ------- | -------
400 | Bad Request | Your request is malformed or missing mandatory data.
401 | Unauthorized | Your API key is either wrong, missing or you have no permissions for this request.
403 | Forbidden | Your request is trying to access data it has no credentials for
404 | Not Found | The specified resource could not be found.
409 | Conflict | There was a conflict with processing your request. Try later or change the data being submitted
415 | Unsupported Media Type | The requests' payload is in a format that is not supported by this method on the target resource.
500 | Internal Server Error | We had a problem with our server. Please try again later.
503 | Service Unavailable | We're temporarily offline for maintenance. Please try again later.

## Expanding objects

> Example request:

```http
GET /payment_methods/<payment-method-id>/?expand=user HTTP/1.1
Content-Type: application/json
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
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

Many response objects will contain IDs for related objects in the response. For example, an Order might have a User ID associated. If you want to expand the actual user information, you can use the `expand` query parameter.

You can also nest expand requests with the dot property. For example, requesting payments.payment_method on an order will expand the payments property into a list of Payment objects, and will then expand the payment method property into a full Payment Method object.

You can expand multiple objects at once by identifying multiple items
in the URL query string param expand that defines which fields that you want to be expanded, separated with commas.

For example:

`GET /orders/<order-id>/?expand=user,provider,items.product`

## Common Object fields

Here's a list of fields which you will see often in the API responses:

Field | Description
---------- | -------
created  | Date when the object was created
modified | Date when the object was last modified
deleted  | Flag that shows if the object has been deleted
active   | Flag that shows if the object is active
company  | Company associated with the object
user     | User associated with the object


## Pagination

> Example request using `size` and `page`:

``` http
GET /orders?size=10&page=1 HTTP/1.1
Content-Type: application/json
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

> Example request using only `size`:

``` http
GET /orders?size=100 HTTP/1.1
Content-Type: application/json
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
X-Pagination-Total: 212
```

All top-level API resources have support for bulk fetches via "list" API methods. For instance, you can list orders, users, providers, etc.
These "list" API methods share a common structure, taking at least these two parameters: size and page.

KVASS uses pagination based on a page size (`size`) and current page (`page`). With these parameters, you can retrieve the data the way you need and display it using your own pagination scheme.
The return of a response header contains the total count: `X-Pagination-Total: 212`


## Retrieve items

The models ([Users](#users), [Invoices](#invoices), [Issuers](#issuers), etc.)
describe below can be retrieved by two different ways, *searching* or a usual GET API call.

In both ways, you can use pagination, sorting, and filter the results by date to navigate big lists.
Additionally, the search method is using the argument `query`.
Each model that has the ability to do a search will be explained more thoroughly their respective chapters,
but all the models have a search by `ID` in common.

> Definition GET

```
GET https://api.kvass.ai/model
```

> Example GET request:

``` http
GET /model HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "_id":{"$oid":"a0b1c2d3e4d5c6b7a8b94242"},
        "company":{"$oid":"591ee15db70e2a10acb65362"},
    }
]

```


> Definition SEARCH

```
GET https://api.kvass.ai/model/search
```

> Example SEARCH request:

``` http
GET /model/search?query=a0b1c2d3e4d5c6b7a8b94242 HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Kvass-Api-Key: <kvass-api-key>
Host: api.kvass.ai
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "_id":{"$oid":"a0b1c2d3e4d5c6b7a8b94242"},
    "company":{"$oid":"591ee15db70e2a10acb65362"},
}
```

        | Types | Description
------- | ----- | -----------
**Filter by date** | |
from_date | `number` | Start date, `timestamp` format. _default is None_
to_date | `number` | End date, `timestamp` format. _default is None_
date_filter | `string` | Date field used for filter results. _default is created_
**Pagination** | |
size | `number` | Number of items to retrieve. _default is 10_
page | `number` | Which page to retrieve. _default is 0_
**Sorting** | |
sort | `string` |  Model specific attributes for sorting results

### Getting list of models

The `Get` method allows you to retrieve all items from a model in a list.
Some models give you additional arguments for filter the requests (e.g: status filter).

        | Invoices | Issuers | Orders | Payments | Products | Providers | Users | Resources
------- | -------- | ------- | ------ | -------- | -------- | --------- | ----- | -----
*Filter by date*  | x | x | x | x | x | x | x | x
*Pagination*      | x | x | x | x | x | x | x | x
*Sorting*         | x | x | x | x | x | x | x | x

### Search on model

The `Search` method is an advanced type of the `Get` request.
This is because the results depend on the query.
This method allows you to retrieve a list of items matching with a search query,
like `user_name` or `account_number`, without specifying that you are searching for
`user_name` or `account_number`.

        | Invoices | Issuers | Orders | Payments | Products | Providers | Users | Resources
------- | -------- | ------- | ------ | -------- | -------- | --------- | ----- | -----
*Filter by date*  | x | x | x | x | x | x | x | x
*Pagination*      | x | x | x | x | x | x | x | x
*Sorting*         | x | x | x | x | x | x | x | x


### Search model by tags

Some `GET` methods allowing you to search models by *tags*.
Here follows an explanation on how to query by tags.

There are 3 ways of getting models by tags:

1. Get all models that contain a specific tag.

*Code: `?tags=tag_1`*

2. Get models that match multiple tags by separating the different tags in the query by `+`.

*Code: `?tags=tag_1+tag_2`*

3. Get models that contain only 1 specific tag.

*Code: `?tags=tag_1+`*


###Tag Query Examples

Under follows some examples on how to query the models by tags.

The basic structure when querying a model by tags is `some-route?tags=some-tag`. 

It is also possible to query by multiple tags, like `some-route?tags=tag_1,tag_2`. This will return every document that matches either `tag1_` or `tag_2`, or both.

If you only want models that match both `tag1_` and `tag_2`, separate the tags by `+`. Like `some-route?tags=tag_1+tag_2`.


#### Get model by multiple tags

- `GET some-route?tags=tag_1,tag_2`

Here we have two expressions: `tag_1` and `tag_2`, which would you returns all models contain `tag_1` or `tag_2`

- `GET some-route?tags=tag_2+tag_3,tag_1,tag_4+tag_5+tag_3`

Here we have three expressions: `tag_2+tag_3`, `tag_1` and, `tag_4+tag_5`, which would you returns all models contain `tag_1` and `tag_2`, models contain `tag_1` and, models contain `tag_4`, `tag_5` and, `tag_3`.

- `Get some-route?tags=tag_2+`

In the query above, the query is 'tag_2+'. This query will return models that match the tag 'tag_2'

<aside class="warning">
The "+" used in the expression must be encoded: "%2B" else you will get a space between your tags.
</aside>

Let's see how it works with some examples:

Assuming we have five models with their tags:

*Model_1 has tags `tag_1` and `tag_2`, etc.*

        | tag_1 | tag_2 | tag_3 | tag_4
------- | ----- | ----- | ----- | -----
model_1 |   x   |   x   |       |
model_2 |   x   |       |   x   |
model_3 |       |   x   |       |
model_4 |       |       |       |   x
model_5 |       |   x   |   x   |   x


Under follows a table with different queries and their corresponding results:

                                        | model_1 | model_2 | model_3 | model_4 | model_5
--------------------------------------- | ------- | ------- | ------- | ------- | -------
`?tags=tag_1`                           |   x     |   x     |         |         |
`?tags=tag_1, tag_2`                    |   x     |   x     |   x     |         |   x
`?tags=tag_1+tag_2`                     |   x     |         |         |         |
`?tags=tag_2+tag_3+tag_4`               |   x     |         |         |         |   x
`?tags=tag_1+tag_2, tag_4`              |   x     |         |         |   x     |   x
`?tags=tag_2+`                          |         |         |   x     |         |
`?tags=tag_2+, tag_1+tag_3`             |         |    x    |   x     |         |



# Authorization

Our API uses OAuth2 and JWT tokens for authorizing Users.

> OAuth 2.0 is the industry-standard protocol for authorization

> JSON Web Tokens are an open, industry standard RFC 7519 method for representing claims securely between two parties

We have support for a variety of authentication providers, including [Auth0](https://auth0.com/), [Google Firebase](https://firebase.google.com/docs/auth/), [Amazon's Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html) or even your own authentication mechanism. Each of these provides different Identity Providers such as Facebook, Google, Email and password, etc.

We use the JWT in the request to authenticate your service, regardless of which authentication provider you use. The [JWT tokens](https://jwt.io/) has to be added to all request where user-specific actions or admin tasks are done.

`Authorization: Bearer <jwt>`

OAuth2 is an industry-standard protocol for authorization, so if you need more information on how to use/implement it, you can find various resources online. However, here are a few good starting points:

* [OAuth2 Spec - RFC6749](https://tools.ietf.org/html/rfc6749)
* [OAuth Bible](http://oauthbible.com/)
* [Auth0 - OAuth 2.0](https://auth0.com/docs/protocols/oauth2)
