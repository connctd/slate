---
title: API Reference

toc_footers:
  - <a href='https://api.connctd.io/register'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - oauth2
  - apps
  - units
  - things
  - connectors
  - scopes
  - errors

search: true
---

# Introduction

Welcome to the connctd.io API. This REST based API provides functionality to create, manage and update Things and
other resources in the world of IoT.

# Authentication

To authenticate against our API you need verify your credentials. In return you receive a token identifying you as
the resource owner (user), granting you a certain amount of permissions.

## Login

> **Request**<br>
> POST https://api.connctd.io/api/v1/auth/login<br>
> *Headers:*<br>
> &nbsp;Content-Type:application/json<br>
> *Body:* see below<br>

```json
{
  "email":"yourmail",
  "password":"yourpassword"
}
```

> **Response**<br>
> *Code:* 200<br>
> *Body:* Token und further information. See example below

```json
{
  "access_token": "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCJ9.eyJhY2NpZCI6I.....",
  "user_id": "546",
  "user_uuid": "4bbb4e40-99ea-43c6-9447-a4c67f563e1c"
}
```

The response will either have the status code 200 in case of success or 401 in case the 
supplied credentials can't be verified. In error cases a default error object is
returned in the body.

## Logout

> **Request**<br>
> POST *Url:* https://api.connctd.io/api/v1/auth/logout<br>
> *Headers:*<br>
> &nbsp;Content-Type:application/json<br>
> &nbsp;Authorization:YOUR TOKEN<br>
> *Body:* empty<br>

```json
```

> **Response**<br>
> *Code:* 200<br>
> *Body:* Empty

Invalidates the access token. You have to specify your access token within *Authorization* header field.
