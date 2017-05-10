# Things

Things in our API are abstract representations of specific IoT related devices. They can be created, deleted
and managed with our API. Depending on the requested action the supplied token must have certain scopes.

Available scopes for Things:

* connctd.connector
* connctd.things.read

## Add a Thing

Required scope: `connctd.connector`

To add a Thing you need to acquire a token with the scope `connctd.connector` through the OAuth2
process. After this you are able to create Things with the following request

`POST https://api.connctd.io/api/v1/things`

<aside class="notice">The Thing-ID needs to be unique across all Things in our API. Therefore it is
possible that the Thing-ID is changed by our backend. The actual Thing-ID is returned in case of 
success.</aside>

> Request body

```json
{
  "id": "<unique-thing-id>",
  "name": "<human readable name>",
  "manufacturer": "<human readbale manufacturer name>",
  "displaytype": "<hint for display purposes>",
  "maincomponentid": "<id of the main component>",
  "components": [
    {
      "id": "<thing unique id of component>",
      "name": "<human readable name of component>",
      "componenttype": "<type of the component>",
      "capabilities": ["<one ore more capabaliies>"],
      "properties": [
        {
          "name": "<component unique name of property>",
          "value": "<string representation of the value>",
          "unit": "<short name of unit of measurement>",
          "type": "<data type of value (NUMBER,STRING,BOOLEAN)>"
        }
      ],
      "actions": [
        {
          "name": "<component unique name of action>",
          "parameters": [
            {
              "name": "<parameter name>",
              "type": "<type of parameter(NUMBER,STRING or BOOLEAN>"
            }
          ]
        }   
      ]
    }
  ]
}
```

> Response body

```json
{
  "Id": "actual-thing-id"
}
```

## Delete a Thing

Required scope: `connctd.connector`

`DELETE https://api.connctd.io/api/v1/things/<thing-id>`

If the supplied bearer token has the scope `connctd.connector` and the Thing with the specified
id was created by the application belongig to the bearer token, this request will delete the 
specified Thing.


## Update a Thing property

Required scope: `connctd.connector`

This endpoint lets you update a property of a Thing you created.

`PUT https://api.connctd.io/api/v1/things/<thing-id>/component/<component-id>/property/<property-name>`

> Request body

```text
value
```

The supplied value must be a valid representation of the property type of this property.

## Register a callback for ActionRequests

Required scope: `connctd.connector`

This endpoint lets application register an endpoint to receive action requests. 
<aside class="notice">The token used to call this endpoint should be a client (application)
specific token. The token needs to be acquired via the Client credentials grant, specified
on Section 4.4 of RFC6749</aside>
<aside class="notice">The callback URL needs to a HTTPS url and needs to have path in its URL.
HTTP urls or root paths are not permitted</aside>

`POST https://api.connctd.io/api/v1/actions/callback/register`

> Request body

```json
{
  "callbackUrl": "https://example.com/callbacks/actions"
}

```

A succesful response will have an empty body and a status code `200`.

## Create ActionRequests

Required scope: `connctd.things.action`

This endpoint lets users create ActionRequests which are send via the specified callbacks
to the applications implementing the necessary logic to handle these requests.

The request body contains a string map representing the parameters to call this action.

The response contains a JSON representation of the created ActionRequest. The `error` field
is optional and is only set of the status is `FAILED`. Valid values for status are

* `PENDING`
* `COMPLETED`
* `CANCELED`
* `FAILED`

If the remote backend responds fast enough and is able to compeplete the ActionRequest the
status may already be `COMPLETED`.

<aside class="notice">ActionRequests are deleted after roughly 24 hours after their deadline.</aside>

`POST http://api.connctd.io/api/v1/things/{thingId}/component/{componentId}/action/{actionName}`

> Request body

```json 
{
  "param1": "value1",
  "param2": "value2"
}
```

> Response body

```json
{
  "id": "foobar",
  "status": "PENDING",
  "deadline": "2017-05-08T15:46:06.801064-02:00",
  "error": "Description of error in case status is FAILED"
}
```

