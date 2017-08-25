# Introduction

The ShareActor API is a standard JSON RESTful API. The API follows the HTTP standards as often as possible, but there might be some deviations from it in some specific scenarios.

All responses from the API, including errors, are returned as JSON objects.

The API is separated into 2 different environments: 


Environment | Endpoint
--------- | -------
**QA** | qa.shareactor.io
**PROD** | api.shareactor.io

Each environment has its separate API keys which you can manage through our Admin dashboard.

All the example API calls are using the PROD endpoint.


## Authentication

``` http
GET / HTTP/1.1
Content-Type: application/json
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
```

> Make sure to replace `<shareactor-api-key>` with your API key.

ShareActor uses API keys to allow access to the API. If you want access to our APIs, please get in touch through our [website](https://www.shareactor.io/contact).

All API requests should include this key in the headers*:

`X-Share-Api-Key: <shareactor-api-key>`

<aside class="notice">
*Note:  You must replace <code>shareactor-api-key</code> with your personal API key.
</aside>

## Errors

<aside class="notice">The errors below might change slightly between API calls</aside>

The ShareActor API uses the following error codes:


Error Code | Reason | Description
---------- | ------- | -------
400 | Bad Request | Your request is malformed or missing mandatory data.
401 | Unauthorized | Your API key is either wrong, missing or you have no permissions for this request.
403 | Forbidden | Your request is trying to access data it has no credentials for
404 | Not Found | The specified resource could not be found.
409 | Conflict | There was a conflict with processing your request. Try later or change the data being submitted
500 | Internal Server Error | We had a problem with our server. Please try again later.
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

Many response objects will contain IDs for related objects in the response. For example, an Order might have a User ID associated. If you want to expand the actual user information, you can use the `expand` query parameter.

You can also nest expand requests with the dot property. For example, requesting payments.payment_method on an order will expand the payments property into a list of Payment objects, and will then expand the payment method property into a full Payment Method object. 

You can expand multiple objects at once by identifying multiple items
in the expand array, separated with commas.

For example:

`GET /orders/<order-id>/?expand=user,provider,items.product`

## Common Object fields

Here's a list of fields which you will see often in the API responses:

Field | Description
---------- | -------
created  | Date when object was created
modified | Date when object was last modified
deleted  | Flag that shows if object has been deleted
active   | Flag that shows if object is active
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
X-Pagination-Total: 212
```

All top-level API resources have support for bulk fetches via "list" API methods. For instance you can list orders, users, providers, etc.
These "list" API methods share a common structure, taking at least these two parameters: size and page.

ShareActor uses pagination based on a page size (`size`) and current page (`page`). With these parameters you can retrieve the data the way you need and display it using your own pagination scheme.
The return of a response header contains the total count: `X-Pagination-Total: 212`


## Retrieve items

The models ([Users](#users), [Invoices](#invoices), [Issuers](#issuers), etc.)
describe below can be retrieve by two different ways, *searching* or an usual GET api call.

In both ways, you can use pagination, sorting, and filter the results by date to navigate big lists.
Additionally, the search method is using the argument `query`.
Each model that has the ability to do search will be explained more thoroughly their respective chapters,
but all the models have search by `ID` in common.

> Definition GET

```
GET https://api.shareactor.io/model
```

> Example GET request:

``` http
GET /model HTTP/1.1
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
        "_id":{"$oid":"a0b1c2d3e4d5c6b7a8b94242"},
        "company":{"$oid":"591ee15db70e2a10acb65362"},
    }
]

```


> Definition SEARCH

```
GET https://api.shareactor.io/model/search
```

> Example SEARCH request:

``` http
GET /model/search?query=a0b1c2d3e4d5c6b7a8b94242 HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io
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
like `user name` or `account_number`, without specifying that you are searching for
`user name` or `account_number`.

        | Invoices | Issuers | Orders | Payments | Products | Providers | Users | Resources
------- | -------- | ------- | ------ | -------- | -------- | --------- | ----- | -----
*Filter by date*  | x | x | x | x | x | x | x | x
*Pagination*      | x | x | x | x | x | x | x | x
*Sorting*         | x | x | x | x | x | x | x | x

# Authorization

Our API uses OAuth2 and JWT tokens for authorizing Users, through a service called [Auth0](https://auth0.com/). 

> OAuth 2.0 is the industry-standard protocol for authorization

> JSON Web Tokens are an open, industry standard RFC 7519 method for representing claims securely between two parties

Our API supports multiple login mechanisms such as the following:

* Facebook
* Google
* Email and password
* SAML, for example: BankID (Norwegian)
* SMS
* etc.

[Here](https://auth0.com/docs/identityproviders) is a full list of Auth0 supported integrations and [here](https://auth0.com/docs/quickstart/native)  you can see a guide on how to implement Auth0 with iOS, Android, Ionic, etc. 

To authorize a user on the API side, we use [JWT tokens](https://jwt.io/). These have to be added to all requests using the following header:

`Authorization: Bearer <jwt>`

## How-to 

There are just a couple of things you need to do to authorize with our APIs after we provide you with the necessary configuration settings:

1. Install your favourite OAuth2 SDK:
  * [Android](https://github.com/wuman/android-oauth-client)
  * [iOS](https://github.com/vsouza/awesome-ios#authentication)
  * [Ionic 2](https://auth0.com/authenticate/ionic2/oauth2)
  * [React Native](https://www.npmjs.com/package/react-native-oauth)
  * Manual - you can also use no SDK and just implement a typical OAuth2 flow
2. Configure the SDK with the settings given by us
3. Perform the authentication request, which will return you a JWT and a profile
4. Create or authenticate your user with our API - `POST /v2/users`

After this flow, you should use the JWT provided for all other API calls.

OAuth2 is a standard for authorization, so if you need more information on how to use/implement it, you can find various resources online. However, here are a few good starting points:

* [OAuth2 Spec - RFC6749](https://tools.ietf.org/html/rfc6749)
* [OAuth Bible](http://oauthbible.com/)
* [Auth0 - OAuth 2.0](https://auth0.com/docs/protocols/oauth2)
