# Things

Things in our API are abstract representations of specific IoT related devices. They can be created, deleted
and managed with our API. Depending on the requested action the supplied token must have certain scopes.

Available scopes for Things:

* connctd.connector
* connctd.things.read

## Create Thing

To add a Thing you need to acquire a token with the scope `connctd.connector` through the OAuth2
process. After this you are able to create Things with the following request

> **Request:** *Method:* POST *Url:* https://api.connctd.io/api/v1/things *Content-Type:* application/json *Body:* see below

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

> **Response:** *Code:* 201 *Body:* Id of newly created thing. See example below

```json
{
  "Id": "f6088362-a864-4834-89eb-d9e4dafbaefe"
}
```

Required scope: `connctd.connector`

<aside class="notice">The Thing-ID needs to be unique across all Things in our API. Therefore it is
possible that the Thing-ID is changed by our backend. The actual Thing-ID is returned in case of 
success.</aside>


## Retrieve things

Retrieves a list of all things the subject is owner of. Be aware that this list does NOT contain things that are shared via policies with the subject.

Required scope: `connctd.things.read or connctd.core`

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/things

```json
```

> **Response:** *Code:* 200 *Body:* List of thing references. See example below

```json
[
  {
    "href": "https://api.connctd.io/api/v1/things/1376f734-dbfc-486d-b264-f62d3ff88579"
  }, ...
]
```

## Retrieve thing

Retrieves a single thing.

Required scope: `connctd.things.read or connctd.core`

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/things/-thingid-

```json
```

> **Response:** *Code:* 200 *Body:* Thing. See example below

```json
{
    "id": "177e3f1a-ef98-4898-b277-b6b392720be9",
    "name": "TutorialDummyThing",
    "manufacturer": "Tutorial app",
    "displayType": "LAMP",
    "mainComponentId": "lamp",
    "componentLinks": [
        {
            "href": "https://api.connctd.io/api/v1/things/177e3f1a-ef98-4898-b277-b6b392720be9/components/lamp"
        }
    ],
    "attributes": null
}
```

## Retrieve thing component

Retrieves the component of a thing.

Required scope: `connctd.things.read or connctd.core`

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/things/-thingid-/components/-componentid-

```json
```

> **Response:** *Code:* 200 *Body:* Thing component. See example below

```json
{
    "id": "lamp",
    "name": "Lamp",
    "componentType": "LAMP",
    "capabilities": [
        "SWITCHABLE"
    ],
    "propertyLinks": [
        {
            "href": "https://api.connctd.io/api/v1/things/177e3f1a-ef98-4898-b277-b6b392720be9/components/lamp/properties/on"
        }
    ],
    "actions": [
        {
            "name": "setOn",
            "parameters": [
                {
                    "name": "on",
                    "type": "BOOLEAN"
                }
            ]
        }
    ]
}
```

## Retrieve component property

Retrieves the property of a component.

Required scope: `connctd.things.read or connctd.core`

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/things/-thingid-/components/-componentid-/properties/-propertyName-

```json
```

> **Response:** *Code:* 200 *Body:* Component property. See example below

```json
{
    "name": "on",
    "value": "false",
    "unit": "ONOFF",
    "type": "BOOLEAN",
    "lastUpdate": "2017-08-23T14:10:53Z"
}
```

## Trigger action request

Used to trigger actions offered by a component. The content of the body depends on the action parameters which can be found in the action description of the corresponding thing component. Action requests will be sent to the callback urls corresponding to the connector/app that has created the app (*see Apps->Register a callback url*).

Required scope: `connctd.things.action`

> **Request:** *Method:* POST *Url:* https://api.connctd.io/api/v1/things/-thingid-/components/-componentid-/actions/-actionName-

```json
{
  "on":"true"
}
```

> **Response:** *Code:* 201 *Body:* Thing component. See example below

```json
{
    "id": "ebfc9fa6-5eb6-4217-bde5-3439797c4577",
    "status": "COMPLETED",
    "deadline": "2017-08-30T15:00:08.941380198Z"
}
```

## Update thing property

This endpoint lets you update a property of a Thing you created. The supplied value must be a valid representation of the property type of this property.

Required scope: `connctd.connector`


> **Request:** *Method:* PUT *Url:* https://api.connctd.io/api/v1/things/-thingid-/components/-componentid-/properties/-propertyname-

```json
{
    "value": "xyz"
}
```

> **Response:** *Code:* 200 *Body:* Empty body

## Delete thing

If the supplied bearer token has the scope `connctd.connector` and the Thing with the specified
id was created by the application belongig to the bearer token, this request will delete the 
specified Thing.

Required scope: `connctd.connector`

> **Request:** *Method:* DELETE *Url:* https://api.connctd.io/api/v1/things/-thingid-

```json
```

> **Response:** *Code:* 200 *Body:* Empty body

