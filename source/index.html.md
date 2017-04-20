---
title: API Reference

toc_footers:
  - <a href='https://api.connctd.io/register'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - oauth2
  - units
  - things
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

### HTTP Requests


`POST https://api.connctd.io/api/v1/auth/login`

> Request body

```json
{
  "Email": "test@example.com",
  "Password": "pikaboo"
}
```

### HTTP Response

The response with either have the status code 200 in case of success or 401 in case the 
supplied credentials can't be verified. In error cases a default error object is
returned in the body.

> Response body

```json
{
 "Token": "xyz"
}
```

## Logout

### HTTP Request

`GET https://api.connctd.io/api/v1/auth/logout`
