# Users

In order to create a User, you also need to authenticate yourself. We currently *only* allow login via Digits, the Twitter platform for logging in using a mobile phone - [Digits](https://get.digits.com/).

## Digits Authentication

### Mobile (iOS and Android)

Both iOS and Android support Digits login, but we need to verify on the server-side that the credentials being sent are correct. For that we need to get the same information Digits has sent your app. Documentation explains how to do this for both [Android](https://docs.fabric.io/android/digits/advanced-setup.html#verify-digits-user) and [iOS](https://docs.fabric.io/apple/digits/advanced-setup.html#verifying-a-user). For Android in specific, we built a test app to demonstrate how to do this: - [https://github.com/shareactorIO/digits-android](https://github.com/shareactorIO/digits-android).

### Web

If you're trying to authenticate through a website or any other way which does not have an SDK, you need to send us the same headers you get when talking to Digits:

* X-Auth-Service-Provider
* X-Verify-Credentials-Authorization


```shell
curl "https://api.shareactor.io/auth"
  -H "Authorization: Bearer <shareactor-api-key>"
  -H "X-Auth-Service-Provider: https://api.digits.com/1.1/..."
  -H "X-Verify-Credentials-Authorization: OAuth oauth_consumer_key=\"...\"
      oauth_signature=\"...\" oauth_signature_method=..."
```

```python
import requests

headers = {'Authorization': 'Bearer <shareactor-api-key>',
           'X-Auth-Service-Provider': 'https://api.digits.com/1.1/...',
           'X-Verify-Credentials-Authorization': 'OAuth oauth_consumer_key="..."
              oauth_signature="..." oauth_signature_method=...'}

requests.post('https://api.shareactor.io/auth', headers=headers)
```

## Create a User

```shell
curl "https://api.shareactor.io/users"
  -H "Authorization: Bearer <shareactor-api-key>"
  -X POST
  -d "{\"user\":{\"first_name\":\"John\",\"last_name\":\"Doe\"}}"
```

```python
import requests

headers = {'Authorization': 'Bearer <shareactor-api-key>'}
user = { "user": { "first_name": "John", "last_name": "Doe" }}

requests.post('https://api.shareactor.io/auth', json=user, headers=headers)
```

> The above command returns JSON structured like this:

```json
{
  "modified": {
    "$date": "..."
  },
  "_cls": "User",
  "company": {
    "$oid": "..."
  },
  "created": {
    "$date": "..."
  },
  "roles": [
    "user"
  ],
  "last_name": "Doe",
  "billing_address": {},
  "_id": {
    "$oid": "ID"
  },
  "addresses": [],
  "first_name": "John"
}
```

This endpoint creates a new user.

### HTTP Request

`POST https://api.shareactor.io/users`

### Request Body

User in JSON format: `{ "user": {"first_name": "John", "last_name": "Doe"}}`


## Get a specific User

```shell
curl "https://api.shareactor.io/users/ID"
  -H "Authorization: Bearer <shareactor-api-key>"
```

```python
import requests

headers = {'Authorization': 'Bearer <shareactor-api-key>'}

requests.get('https://api.shareactor.io/users/ID', headers=headers)
```

> The above command returns JSON structured like this:

```json
{
  "modified": {
    "$date": "..."
  },
  "_cls": "User",
  "company": {
    "$oid": "..."
  },
  "created": {
    "$date": "..."
  },
  "roles": [
    "user"
  ],
  "last_name": "Doe",
  "billing_address": {},
  "_id": {
    "$oid": "ID"
  },
  "addresses": [],
  "first_name": "John"
}
```

This endpoint retrieves a specific user.

### HTTP Request

`GET https://api.shareactor.io/users/ID`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the user to retrieve
