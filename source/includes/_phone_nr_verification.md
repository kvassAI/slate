# Phone Number Verification

The phone number verification API allows you to verify a user's phone numbers, using
[Nexmo](https://www.nexmo.com/products/verify)'s sms verification service.

This process is divided into two stages:

1. Sending the phone number and generating a PIN code and Request ID. An SMS is sent to the user with that same PIN code.
2. Validating the PIN code input given by the user.

## 1. Send PIN code to User

> Definition

```
POST https://api.shareactor.io/sms/verify
```

> Example request:

``` http
POST /sms/verify HTTP/1.1
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

Given a phone number, the API will send a verification PIN code to the user and you should store the `request_id` resulting from this call.

Argument | Type | Description
-------- | ---- | ------
**phone_number** | `string` | Phone number of the associated user

## 2. Validate PIN code and phone number

> Definition

```
POST https://api.shareactor.io/sms/verify
```

> Example request:

``` http
POST /sms/verify HTTP/1.1
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
HTTP/1.1 204 No Content
```

Using the `request_id` stored from the API call above, together with the PIN code the user has input, this call will verify that the code is correct.

The validation is successful if the status code of the response is `204`.

Arguments | Type | Description
--------- | ---- | ------
**request_id** | `string` | Request ID returned from the API
**code** | `string` | PIN code received by the user



