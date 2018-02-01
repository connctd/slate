# Apps

connctd implements the OAuth 2.0 specification. By doing so developers can write and host their own apps that can obtain limited access to resources on behalf of the user. 
Such an app (in OAuth terms it is called client) could for example list the units or things of an user. These apps do not just consume resources they might also deliver new resources like things. A common use case for connctd apps are apps that mediate between our platform and another, foreign technology. We often name those apps "connector" as they are translating from a remote domain model into our thing concept.
If you are unfamiliar with the OAuth 2.0 framework we highly recommend reading the [specification](https://tools.ietf.org/html/rfc6749). In case you are uncertain regarding some parameters we also recommend reading the documentation of [hydra](http://docs.hydra13.apiary.io/#reference/oauth2) - the framework we are using under the hood for OAuth 2.0 support.

## Register an app


> **Request**<br>
> POST https://api.connctd.io/api/v1/apps<br>
> *Headers:*<br>
> &nbsp;Content-Type:application/json<br>
> &nbsp;Authorization:YOUR TOKEN<br>
> *Body:* see below<br>

```json
{
  "name":"Name of application",
  "description":"Any description",
  "homepage":"https://abc.com",
  "redirecturis":["https://abc.com/auth/callback"],
  "policyuri":"https://abc.com/policy",
  "tosuri":"https://abc.com/tos",
  "logouri":"https://abc.com/logo.png",
  "scopes": ["connctd.units.read","connctd.units.admin","connctd.connector","connctd.things.read","offline"],
  "public":true,
  "visible":true,
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

> **Response**<br>
> *Code:* 201<br>
> *Body:* Newly created application. See example below. 

```json
{
  "id": "f62794f6-659d-439a-b19b-45e6cf705cef",
  "client_id": "f62794f6-659d-439a-b19b-45e6cf705cef",
  "client_secret": "NWx.W-B>G/!u"
}
```

Registers a new application

**redirect_uris:** Array of redirect uris that are later on used in various flows

**scope:** Space separated list of scopes. The offline scope is necessary if a refresh token should be later on generated. You can see in a list of all available scopes within the *scopes* section

**public:** You have to set this to true if there exists the possibility of client secret disclosure (e.g. Android app might be decompiled). Apps with setting public=true will not be able to register action callback urls since the client_credentials grant type flow is required for that but which is deactivated due to security concerns with public apps.

**visible:** If set to true any user can see your app (e.g. in an app store). If set to false only you can see the app.

**tags:** Used to search for apps with specific meanings (e.g. Smart Home, Netatmo, Assisted living)

As a response you will get back a client_id, id and a client_secret. The client_secret needs to be stored as it can't be retrieved again. Right now the id of the created client (used for managing clients) is equal to the oauth2 client_id (required for OAuth2 flow).

Required scope: `connctd.core`

## Retrieve apps

> **Request**<br>
> GET https://api.connctd.io/api/v1/apps<br>
> *Headers:*<br>
> &nbsp;Authorization:YOUR TOKEN<br>
> *Body:* empty<br>

```json
```

> **Response**<br>
> *Code:* 200<br>
> *Body:* List of apps. See example below

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

Retrieves a list of all apps 

Required scope: `connctd.core`

## Retrieve an app

> **Request**<br>
> GET https://api.connctd.io/api/v1/apps/f62794f6-659d-439a-b19b-45e6cf705cef<br>
> *Headers:*<br>
> &nbsp;Authorization:YOUR TOKEN<br>
> *Body:* empty<br>

```json
```

> **Response**<br>
> *Code:* 200<br>
> *Body:* Single app description. See example below

```json
{
  "id": "f62794f6-659d-439a-b19b-45e6cf705cef",
  "name":"Name of application",
  "description":"Any description",
  ...
}
```

Retrieves a specific app

Required scope: `connctd.core`

## Add tag

> **Request**<br>
> POST https://api.connctd.io/api/v1/apps/f62794f6-659d-439a-b19b-45e6cf705cef/tags<br>
> *Headers:*<br>
> &nbsp;Content-Type:application/json<br>
> &nbsp;Authorization:YOUR TOKEN<br>
> *Body:* see below<br>

```json
{
  "name":"My new tag"
}
```

> **Response**<br>
> *Code:* 201

Adds a new tag to the tag list of an app

Required scope: `connctd.core`

## Delete a tag

> **Request**<br>
> DELETE https://api.connctd.io/api/v1/apps/1bd2f1a3-e72d-4606-b82b-86d1054f3bd4/tags/TAGNAME<br>
> *Headers:*<br>
> &nbsp;Authorization:YOUR TOKEN<br>
> *Body:* empty<br>

```json
```

> **Response**<br>
> *Code:* 204

```json
```

Removes a tag

## Delete an app

> **Request**<br>
> DELETE https://api.connctd.io/api/v1/apps/1bd2f1a3-e72d-4606-b82b-86d1054f3bd4<br>
> *Headers:*<br>
> &nbsp;Authorization:YOUR TOKEN<br>
> *Body:* empty<br>

```json
```

> **Response**<br>
> *Code:* 204

```json
```

Removes an app by client_id

Required scope: `connctd.core`

## Register a callback url

> **Request**<br>
> POST https://api.connctd.io/api/v1/actions/callback/register<br>
> *Headers:*<br>
> &nbsp;Content-Type:application/json<br>
> &nbsp;Authorization:APP TOKEN<br>
> *Body:* see below<br> 

```json
{
  "CallbackUrl":"https://remote-service-url"
}
```

> **Response**<br>
> *Code:* 200

```json
```

This request stores a system wide callback url for all action requests that are related to this app. This is important whenever an app creates things that contain actions. As soon a user triggers any of these actions by performing an action request the request is forwarded to the given callback url. 
The header requires a valid app token. See section oauth2 -> Register an app token for more information about gaining app tokens
