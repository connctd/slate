# OAuth 2.0

The connctd platform acts as a oauth2 provider implementing all flows described in https://tools.ietf.org/html/rfc6749#section-4 except the "Resource Owner Password Credentials Grant". The following two sections only give examples based on the Authorization Code Grant Flow as there already exist hundreds of good oauth2 tutorials which can be read in case you need another flow.

## Requesting Authorization

This request will redirect the user to the consent screen. You have to replace the example parameter values with the corresponding values of your app. A detailed parameter description can be found [here](https://tools.ietf.org/html/rfc6749#section-4.1.1).

> **Request:** *Method:* GET *Url:* https://api.connctd.io/oauth2/auth?state=abcdefghiklmnop&client_id=1bd2f1a3-e72d-4606-b82b-86d1054f3bd4&redirect_uri=https%3A%2F%2Fabc%2Fauth%2Fcallback&response_type=code&scope=connctd.units.read+connctd.things.read+connctd.connector+offline

```json
```

> **Response:** *Code:* 301

```json
```

## Requesting a token

This request requires the authentication of the app by specifying an Authorization header with value `Basic base64_encode(client_id:client_secret)`
The body will contain a url encoded form with the fields *grant_type*, *code*, *redirect_uri* and *client_id*  

> **Request:** *Method:* POST *Url:* https://api.connctd.io/oauth2/token *Content-Type:* application/x-www-form-urlencoded

```java
grant_type=authorization_code&code=-auth code from reply of prev request-&
redirect_uri=http%3A%2F%2Fabc.com%%2Fauth%2Fcallback&client_id=1bd2f1a3-e72d-4606-b82b-86d1054f3bd4
```

> **Response:** *Code:* 200

```json
{
    "access_token":"...",
    "refresh_token":"...",
    "scope":"...",
}
```