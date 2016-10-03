# Authentication

In order to create a User, you will have to authenticate yourself. We currently *only* allow login via Digits, the Twitter platform for logging in using a mobile phone - [Digits](https://get.digits.com/).

## Digits Authentication

### Mobile (iOS and Android)

Both iOS and Android support Digits login, but we need to verify on the server-side that the credentials being sent are correct. For that we need to get the same information Digits has sent your app. Documentation explains how to do this for both [Android](https://docs.fabric.io/android/digits/advanced-setup.html#verify-digits-user) and [iOS](https://docs.fabric.io/apple/digits/advanced-setup.html#verifying-a-user). For Android in specific, we built a test app to demonstrate how to do this: - [https://github.com/shareactorIO/digits-android](https://github.com/shareactorIO/digits-android).

### Web

``` http
POST /auth HTTP/1.1
Content-Type: application/json
Authorization: Bearer <shareactor-api-key>
X-Auth-Service-Provider: https://api.digits.com/1.1/...
X-Verify-Credentials-Authorization: OAuth oauth_consumer_key=... oauth_signature=... oauth_signature_method=...
Host: api.shareactor.io
```

If you're trying to authenticate through a website or any other way which does not have an SDK, you need to send us the same headers you get when talking to Digits:

* X-Auth-Service-Provider
* X-Verify-Credentials-Authorization
