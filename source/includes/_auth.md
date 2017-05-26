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

In order to authorize a user on the API side, we use [JWT tokens](https://jwt.io/). These have to be added to all requests using the following header:

`Authorization: Bearer <jwt>`

## How-to 

In order to authorize with our APIs, there's just a couple of things you need to do, after we provide you with the necessary configuration settings:

1. Install your favourite OAuth2 SDK:
  * [Android](https://github.com/wuman/android-oauth-client)
  * [iOS](https://github.com/vsouza/awesome-ios#authentication)
  * [Ionic 2](https://auth0.com/authenticate/ionic2/oauth2)
  * [React Native](https://www.npmjs.com/package/react-native-oauth)
  * Manual - you can also use no SDK and just implement a typical OAuth2 flow
2. Configure the SDK with the settings given by us
3. Perform the authentication request which will return you a JWT and a profile
4. Create or authenticate your user with our API - `POST /v2/users`

After this flow, you should use the JWT provided for all other API calls.

OAuth2 is a standard for authorization, so if you need more information on how to use/implement it, you can find various resources online, but here are a few good starting points:

* [OAuth2 Spec - RFC6749](https://tools.ietf.org/html/rfc6749)
* [OAuth Bible](http://oauthbible.com/)
* [Auth0 - OAuth 2.0](https://auth0.com/docs/protocols/oauth2)
