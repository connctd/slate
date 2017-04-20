# OAuth 2.0

connctd implements the OAuth 2.0 specification. By doing so developers can write and host their own apps that can obtain limited access to resources on behalf of the user. 
Such an app (in OAuth terms it is called client) could for example list the units and things of an user or it could be used to administrate access permissions for resources. These apps do not just consume
 resources they might also deliver new resources like for example things. A common use case for connctd apps are apps that mediate between our platform and another, foreign technology. We often name those apps "connector" as they are translating from a remote domain model into our thing concept.
If you are unfamiliar with the OAuth 2.0 framework we highly recommend reading the [specification](https://tools.ietf.org/html/rfc6749). In case you are uncertain regarding some parameters we also recommend reading the documentation of [hydra](http://docs.hydra13.apiary.io/#reference/oauth2) - the framework we are using under the hood for OAuth 2.0 support.

## Register an app

Registers a new application

<aside class="notice">
Please note that for every request a valid access token is required and needs to be passed within the headers (Authorization:Bearer -your token-)
</aside>

**redirect_uris:** Array of redirect uris that are later on used in various flows

**scope:** Space separated list of scopes. The offline scope is necessary if a refresh token should be later on generated

**public:** If set to true the client_credentials flow can not be used

As a response you will get back a client_id (id) and a client_secret. The client_secret needs to be stored as it can't be retrieved again. 

> **Request:** *Method:* POST *Url:* https://api.connctd.io/api/v1/apps *Content-Type:* application/json *Body:* see below

```json
{
	"client_name":"My client application",
	"redirect_uris":["http://abc.com/auth/callback"],
	"policy_uri":"https://abc.com/policy",
	"tos_uri":"https://abc.com/tos",
	"client_uri":"https://abc.com/",
	"logo_uri":"https://abc.com/static/imgs/logo.png",
	"contacts":["abc@connctd.com"],
    "scope": "connctd.units.read connctd.units.admin connctd.connector connctd.things.read offline",
    "public":true,
    "response_types": [
      "code",
      "token",
      "id_token"
    ],
    "grant_types": [
      "authorization_code",
      "implicit",
      "refresh_token",
      "password",
      "client_credentials"
    ]
}
```

> **Response:** *Code:* 201 *Body:* Newly created application. See example below

```json
{
  "id": "1bd2f1a3-e72d-4606-b82b-86d1054f3bd4",
  "client_name": "My client application",
  "client_secret": "309*<qEU&c;B",
  ...
}
```

## Retrieve apps

Retrieves a list of all apps 

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/apps

```json
```

> **Response:** *Code:* 200 *Body:* List of apps. See example below

```json
{
  "1bd2f1a3-e72d-4606-b82b-86d1054f3bd4": {
    "id": "1bd2f1a3-e72d-4606-b82b-86d1054f3bd4",
    "client_name": "My client application",
    "redirect_uris": [...],
    ...
  }
```

## Delete an app

Removes an app by client_id

> **Request:** *Method:* DELETE *Url:* https://api.connctd.io/api/v1/apps/1bd2f1a3-e72d-4606-b82b-86d1054f3bd4

```json
```

> **Response:** *Code:* 204

```json
```

## Requesting Authorization

This request will redirect the user to the consent screen. You have to replace the example parameter values with the corresponding values of your app. A detailed parameter description can be found [here](https://tools.ietf.org/html/rfc6749#section-4.1.1).
The parameters may also vary depending on the used flow.

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

