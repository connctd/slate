# Apps

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

**public:** If set to true any user can see your app (e.g. in an app store). If set to false only you can see the app.

**tags:** Used to search for apps with specific meanings (e.g. Smart Home, Netatmo, Assisted living)

As a response you will get back a client_id, id and a client_secret. The client_secret needs to be stored as it can't be retrieved again. Right now the id of the created client (used for managing clients) is equal to the oauth2 client_id (required for OAuth2 flow).

> **Request:** *Method:* POST *Url:* https://api.connctd.io/api/v1/apps *Content-Type:* application/json *Body:* see below

```json
{
  "name":"Name of application",
  "description":"Any description",
  "homepage":"https://abc.com",
  "redirecturis":["https://abc.com/auth/callback"],
  "policyuri":"https://abc.com/policy",
  "tosuri":"https://abc.com/tos",
  "logouri":"https://abc.com/logo.png",
  "scopes": ["connctd.units.read","connctd.connector","connctd.things.read","offline","openid"],
  "public":true,
  "tags":[{"name":"Smart Home"}],
  "contacts":["your.mail@mail.com"],
  "responsetypes": [
    "code",
    "token",
    "id_token"
  ],
  "granttypes": [
    "authorization_code",
    "implicit",
    "refresh_token",
    "password",
    "client_credentials"
  ]
}

```

> **Response:** *Code:* 201 *Body:* Newly created application. See example below. 

```json
{
  "id": "f62794f6-659d-439a-b19b-45e6cf705cef",
  "client_id": "f62794f6-659d-439a-b19b-45e6cf705cef",
  "client_secret": "NWx.W-B>G/!u"
}
```

## Retrieve apps

Retrieves a list of all apps 

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/apps

```json
```

> **Response:** *Code:* 200 *Body:* List of apps. See example below

```json
[
  {
      "id": "f62794f6-659d-439a-b19b-45e6cf705cef",
      "name":"Name of application",
      "description":"Any description",
      "public": true,
      "added": 1497866109,
      "owner": "7",
      "tags": [
          {
              "name": "Smart Home"
          }
      ],
      ...
  },
  ...
]

```

## Retrieve an app

Retrieves a specific app

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/apps/f62794f6-659d-439a-b19b-45e6cf705cef

```json
```

> **Response:** *Code:* 200 *Body:* Single app description. See example below

```json
{
  "id": "f62794f6-659d-439a-b19b-45e6cf705cef",
  "name":"Name of application",
  "description":"Any description",
  ...
}
```

## Add tag

Adds a new tag to the tag list of an app

> **Request:** *Method:* POST *Url:* https://api.connctd.io/api/v1/apps/f62794f6-659d-439a-b19b-45e6cf705cef/tags *Body:* see below

```json
{
  "name":"My new tag"
}
```

> **Response:** *Code:* 200

## Delete a tag

Removes a tag

> **Request:** *Method:* DELETE *Url:* https://api.connctd.io/api/v1/apps/1bd2f1a3-e72d-4606-b82b-86d1054f3bd4/tags/TAGNAME

```json
```

> **Response:** *Code:* 200

```json
```

## Delete an app

Removes an app by client_id

> **Request:** *Method:* DELETE *Url:* https://api.connctd.io/api/v1/apps/1bd2f1a3-e72d-4606-b82b-86d1054f3bd4

```json
```

> **Response:** *Code:* 204

```json
```


