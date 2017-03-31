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

In order to authorize a user on the API side we use [JWT tokens](https://jwt.io/) which have to be added to all requests using the following header:

`Authorization: Bearer <jwt>`

## Example Authorization (#TODO)

