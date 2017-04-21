# Phone Number Verification

Phone number verification is used to verify users, through
[Nexmo](https://www.nexmo.com/products/verify)'s sms verification API.

This method has two stages:

1. POST phone number. This returns a request_id.
The User will receive a code on their phone with SMS.
3. The User have to POST their validation code.


## Phone Number Verification:
Argument | Description
---------- | -------
phone_number | Phone number to associated User
request_id | Request id returned from Nexmo API
code | Verification code returned from Nexmo API

## 1. Send code to phone number
> Definition

```
POST https://api.shareactor.io/sms/verify
```

> Example request:

``` http
POST /invoices HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io

{
    "phone_number": "004712345678"
}
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
   "request_id":"2d426b2552e1446bb3d234e7298c21f9"
}
```

Argument | Description
---------- | -------
**phone_number** | Phone number to associated User

## 2. Send validation code to validate user

> Definition

```
POST https://api.shareactor.io/sms/verify
```

> Example request:

``` http
POST /invoices HTTP/1.1
Content-Type: application/json
Authorization: Bearer <jwt>
X-Share-Api-Key: <shareactor-api-key>
Host: api.shareactor.io

{
    "request_id": "2d426b2552e1446bb3d234e7298c21f9",
    "code": "1234"
}
```

``` http
HTTP/1.1 204 OK
Content-Type: application/json

```

Argument | Description
---------- | -------
**request_id** | Request id returned from Nexmo API
**code** | Code received by the User from Nexmo API

The validation is succsessfull if the status code in the responce is `204`

