---
title: API Reference

#language_tabs:
#  - shell

toc_footers:
  - <a href='https://api.connctd.io/register'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
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

## OAuth 2

In most cases you will interact with our API in the role of an OAuth2 authenticated application.
`TODO add OAuth2 documentation`.

# Units

Our API allows you to create Units to logically group Things and Users. Units are simple containers
which have a type and which can contain children. Units also have properties. Some of which are required
depending upon the Unit type.

Unit types:

* CONTAINER

`TODO add documentation about Unit API`

# Things

Things in our API are abstract representations of specific IoT related devices. They can be created, deleted
and managed with our API.


