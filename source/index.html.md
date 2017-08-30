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
  - policies
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

The response will either have the status code 200 in case of success or 401 in case the 
supplied credentials can't be verified. In error cases a default error object is
returned in the body.


> **Request:** *Method:* POST *Url:* https://api.connctd.io/api/v1/auth/login *Content-Type:* application/json *Body:* see below

```json
{
  "email":"yourmail",
  "password":"yourpassword"
}
```

> **Response:** *Code:* 200 *Body:* Token und further information. See example below

```json
{
  "access_token": "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCJ9.eyJhY2NpZCI6IjJhYWE0ZTQwLTg5ZWEtNDVmNS05NDQ3LWU1YzY4ZjU2N2QyYiIsImV4cCI6MTUwNDEwOTkyNSwiaXNzIjoiYXBpLmNvbm5jdGQuaW8iLCJuYmYiOjE1MDQxMDYzMjUsInN1YiI6InNlYmFzdGlhbi5nYXJuQHBvc3Rlby5kZSJ9.eXyWVlwb8rG70rrxE4_sglCExZwKyT9hqUdVN-InG0CB3_XbeL7PddO79E5wsLa9WfxSZJxWbrBFiwc5lBk2pA",
  "user_id": "546",
  "user_uuid": "4bbb4e40-99ea-43c6-9447-a4c67f563e1c"
}
```

## Logout

Invalidates the access token. You have to specify your access token within *Authorization* header field.

> **Request:** *Method:* POST *Url:* https://api.connctd.io/api/v1/auth/logout *Content-Type:* application/json *Body:* empty

```json
```

> **Response:** *Code:* 200 *Body:* Empty
