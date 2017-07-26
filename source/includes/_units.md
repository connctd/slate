# Units

A unit is a primitive container that gives developers the possibility to group things and to put them into a context. Since a unit can reference other units (children and parents) even complex structures of the real world can be modeled like for example a city with it's streets and buildings. The type property of a unit not just gives a clue about its meaning, it also defines which properties a unit needs to have in order to be valid. By enforcing these properties even generic applications know how to handle unknown units. This only counts as long as unit types are used that are predefined within our specification. If no predefined unit type fits the use case or context, the developer or unit creating app can choose its own unit type.

## Model 

```json
{
  "id": "<Generated id>",
  "name": "<Name of unit>",
  "type": "<Type of unit>",
  "things": [{"href":"https://api.connctd.io/api/v1/things/ab..."}],
  "properties": ["tuple(name,value)"],
  "children": [{"href":"https://api.connctd.io/api/v1/units/xy..."}],
  "parents": [{"href":"https://api.connctd.io/api/v1/units/qw..."}],
  "subjects": [{"href":"https://api.connctd.io/api/v1/subjects/98..."}],
  "owner": "65...."
}

```

On the right hand side you can see an exemplary unit. We also offer a [unit json schema](https://github.com/connctd/future-platform/blob/master/domain/unit-schema.json) which allows json object validation.

The unit **type** helps to interprete the meaning of a unit. Additionally it defines which properties need to be defined when creating the unit (only if it is predefined by specification). The following table lists all predefined types and their corresponding, mandatory properties. Keep in mind that a unit is not restricted to those properties. As long as the mandatory properties are defined any further properties can be attached to it. Using a type which is not listed below will disable the mandatory properties - check.

The values of the properties **parents**, **children**, **subjects** and **things** are lists holding references to other resources.

| Type  | Properties |
| ------------- | ------------- |
| CONTAINER | - |
| HOUSE | number |
| FLOOR | number |
| APARTMENT | number |
| ROOM | originX, originY, originZ, spanX, spanY, spanZ |

## Create unit

Creates an empty unit. Additional information like parents, children or subjects can be added by subsequent calls.

<aside class="notice">
Please note that for every request a valid access token is required and needs to be passed within the headers (Authorization:Bearer -your token-)
</aside>

Required scope: `connctd.units.admin`

> **Request:** *Method:* POST *Url:* https://api.connctd.io/api/v1/units *Content-Type:* application/json *Body:* see below

```json
{
  "name": "My House",
  "type": "HOUSE",
  "properties": [
    {
      "name":"number",
      "value":"25"
    }
  ]
}
```

> **Response:** *Code:* 201 *Body:* Id of newly created unit. See example below

```json
{
  "id": "e1de3bcf-7699-4838-97ec-635618fb3caa"
}
```

## Retrieve units

Retrieves a list of resource links to units the user belongs to

Required scope: `connctd.units.read`

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/units

```json
```

> **Response:** *Code:* 200 *Body:* Array of units. See example below

```json
[
  {
    "href": "https://api.connctd.io/api/v1/units/37cbbcfb-9a5c-4931-aa55-f73ff0af0e80"
  }, ...
]
    
```

## Retrieve unit

Retrieves a unit by id

Required scope: `connctd.units.read`

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/units/-unitId-

```json
```

> **Response:** *Code:* 200 *Body:* A single unit. See example below

```json
{
  "id": "e1de3bcf-7699-4838-97ec-635618fb3caa",
  "name": "My House",
  "type": "HOUSE",
  "things": [],
  "properties": [
    {
      "name": "number",
      "value": "25"
    }
  ],
  "children": [],
  "parents": [],
  "subjects": [],
  "owner": "1"
}   
```

## Delete unit

Removes a unit by its id.

Required scope: `connctd.units.admin`

> **Request:** *Method:* DELETE *Url:* https://api.connctd.io/api/v1/units/-unitId-

```json
```

> **Response:** *Code:* 200

## Get unit references

Retrives a list of unit references.

Required scope: `connctd.units.read`

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/units/-unitId-/-parents|children-

```json
```

> **Response:** *Code:* 200 *Body:* List of unit references. See example below

```json
[
  {
    "href": "/api/v1/units/1376f734-dbfc-486d-b264-f62d3ff88579"
  }, ...
]
```

## Add unit reference

Adds a reference to another unit. The reference also appears within the referenced unit either as parent or child reference.

Required scope: `connctd.units.admin`

> **Request:** *Method:* POST *Url:* https://api.connctd.io/api/v1/units/-unitId-/-parents|children- *Content-Type:* application/json *Body:* Id of parent/child unit. See example below

```json
{
  "id": "1376f734-dbfc-486d-b264-f62d3ff88579"
}
```

> **Response:** *Code:* 201

## Remove unit reference

Removes a reference to another unit. The reference also disappears from within the referenced unit.

Required scope: `connctd.units.admin`

> **Request:** *Method:* DELETE *Url:* https://api.connctd.io/api/v1/units/-unitId-/-parents|children- *Content-Type:* application/json *Body:* Id of parent/child unit. See example below

```json
{
  "id": "1376f734-dbfc-486d-b264-f62d3ff88579"
}
```

> **Response:** *Code:* 200

## Get subject references

Required scope: `connctd.units.read`

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/units/-unitId-/subjects

```json
```

> **Response:** *Code:* 200 *Body:* List of subject references. See example below

```json
[
  {
    "href": "https://api.connctd.io/api/v1/subjects/2"
  }
]
```

<aside class="note">
Subject references can be used for identification and organizational puproses. They are are currently NOT callable. Another reason why it makes sense to add a subject to this list is that the unit will appear within the list of unit resource links (GET /api/v1/units) of the added subject. But as long as there exists no policy (see section Policies) that regulates the access of that subject to the unit the addressed subject will be not able to resolve the unit
</aside>

## Add subject reference

Adds a reference to a subject.

Required scope: `connctd.units.admin`

> **Request:** *Method:* POST *Url:* https://api.connctd.io/api/v1/units/-unitId-/subjects *Content-Type:* application/json *Body:* Id of subject. See example below

```json
{
  "id": "2"
}
```

> **Response:** *Code:* 201

## Remove subject reference

Removes a reference to a subject.

Required scope: `connctd.units.admin`

> **Request:** *Method:* DELETE *Url:* https://api.connctd.io/api/v1/units/-unitId-/subjects *Content-Type:* application/json *Body:* Id of subject. See example below

```json
{
  "id": "2"
}
```

> **Response:** *Code:* 200

## Get properties

Retrieves a list of all properties

Required scope: `connctd.units.read`

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/units/-unitId-/properties

```json
```

> **Response:** *Code:* 200 *Body:* List of properties. See example below

```json
[
  {
    "name": "number",
    "value": "25"
  }
]
```


## Get property

Read a specific property

Required scope: `connctd.units.read`

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/units/-unitId-/properties/-propertyName-

```json
```

> **Response:** *Code:* 200 *Body:* A single property. See example below

```json
{
    "name": "number",
    "value": "25"
}
```

## Add property

Adds a new property.

Required scope: `connctd.units.admin`

> **Request:** *Method:* POST *Url:* https://api.connctd.io/api/v1/units/-unitId-/properties *Content-Type:* application/json *Body:* New property. See example below

```json
{
    "name": "MyNewProperty",
    "value": "123"
}
```

> **Response:** *Code:* 201

## Delete property

Removes a property from property set

Required scope: `connctd.units.admin`

> **Request:** *Method:* DELETE *Url:* https://api.connctd.io/api/v1/units/-unitId-/properties/-propertyName-

```json
```

> **Response:** *Code:* 200

```json
```

## Get thing references

Retrieves a list of all thing references

Required scope: `connctd.units.read`

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/units/-unitId-/things

```json
```

> **Response:** *Code:* 200 *Body:* List of thing references. See example below

```json
[
  {
    "href": "https://api.connctd.io/api/v1/things/591327fe-5c1a-4dbb-a96b-9bef4de0a273"
  }, ...
]
```

## Add thing reference

Adds a reference to a things.

Required scope: `connctd.units.admin`

> **Request:** *Method:* POST *Url:* https://api.connctd.io/api/v1/units/-unitId-/things *Content-Type:* application/json *Body:* Id of thing. See example below

```json
{
	"id": "591327fe-5c1a-4dbb-a96b-9bef4de0a273"
}
```

> **Response:** *Code:* 201

## Remove thing reference

Removes a reference to a thing.

Required scope: `connctd.units.admin`

> **Request:** *Method:* DELETE *Url:* https://api.connctd.io/api/v1/units/-unitId-/things *Content-Type:* application/json *Body:* Id of thing. See example below

```json
{
	"id": "591327fe-5c1a-4dbb-a96b-9bef4de0a273"
}
```

> **Response:** *Code:* 200