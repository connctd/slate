# Units

A unit is a primitive container that gives developers the possibility to group things and to put them into a context. Since a unit can reference other units (children and parents) even complex structures of the real world can be modeled like for example a city with it's streets and buildings.

## Model 

```json
{
  "id": "<Generated id>",
  "name": "<Name of unit>",
  "type": "<Type of unit>",
  "things": [{"href":"https://api.connctd.io/api/v1/things/ab..."}],
  "properties": ["tuple(name,json object)"],
  "children": [{"href":"https://api.connctd.io/api/v1/units/xy..."}],
  "parents": [{"href":"https://api.connctd.io/api/v1/units/qw..."}],
  "subjects": [{"href":"https://api.connctd.io/api/v1/subjects/98..."}],
  "owner": "65...."
}

```

On the right hand side you can see an exemplary unit. We also offer a [unit json schema](https://github.com/connctd/future-platform/blob/master/domain/unit-schema.json) which allows json object validation.

The unit **type** helps to interprete the meaning of a unit. The **properties** can hold arbitrary information since json objects can be passed. Even json ld is allowed to semantically enrich the properties. A json ld processor takes care of object validation.

The values of the properties **parents**, **children**, **subjects** and **things** are lists holding references to other resources.

## Create unit

```json
{
  "name": "My House",
  "type": "HOUSE",
  "properties": [
    {
      "name":"address",
      "value":{
      	"number":"25",
      	"street":"..."
      }
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

Creates an empty unit. Additional information like parents, children or subjects can be added by subsequent calls.

Required scope: `connctd.units.admin or connctd.core`

> **Request:** *Method:* POST *Url:* https://api.connctd.io/api/v1/units *Content-Type:* application/json *Body:* see below

## Retrieve units

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

Retrieves a list of resource links to units the user belongs to

Required scope: `connctd.units.read or connctd.core`

## Retrieve unit

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

Retrieves a unit by id

Required scope: `connctd.units.read or connctd.core`

## Delete unit

> **Request:** *Method:* DELETE *Url:* https://api.connctd.io/api/v1/units/-unitId-

```json
```

> **Response:** *Code:* 204

Removes a unit by its id.

Required scope: `connctd.units.admin or connctd.core`

## Get unit references

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/units/-unitId-/-parents|children-

```json
```

> **Response:** *Code:* 200 *Body:* List of unit references. See example below

```json
[
  {
    "href": "https://api.connctd.io/api/v1/units/1376f734-dbfc-486d-b264-f62d3ff88579"
  }, ...
]
```

Retrieves a list of unit references.

Required scope: `connctd.units.read or connctd.core`

## Add unit reference

> **Request:** *Method:* POST *Url:* https://api.connctd.io/api/v1/units/-unitId-/-parents|children- *Content-Type:* application/json *Body:* Id of parent/child unit. See example below

```json
{
  "id": "1376f734-dbfc-486d-b264-f62d3ff88579"
}
```

> **Response:** *Code:* 201

Adds a reference to another unit. The reference also appears within the referenced unit either as parent or child reference.

Required scope: `connctd.units.admin or connctd.core`

## Remove unit reference

> **Request:** *Method:* DELETE *Url:* https://api.connctd.io/api/v1/units/-unitId-/-parents|children- *Content-Type:* application/json *Body:* Id of parent/child unit. See example below

```json
{
  "id": "1376f734-dbfc-486d-b264-f62d3ff88579"
}
```

> **Response:** *Code:* 204

Removes a reference to another unit. The reference also disappears from within the referenced unit.

Required scope: `connctd.units.admin or connctd.core`

## Get subject references

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/units/-unitId-/subjects

```json
```

> **Response:** *Code:* 200 *Body:* List of subject references. See example below

```json
[
  {
    "href": "https://api.connctd.io/api/v1/subjects/123-55664-aab-2456"
  }
]
```

Retrieves list of subject references

Required scope: `connctd.units.read or connctd.core`

<aside class="note">
Subject references can be used for identification and organizational puproses. They are are currently NOT callable. Another reason why it makes sense to add a subject to this list is that the unit will appear within the list of unit resource links (GET /api/v1/units) of the added subject. But as long as there exists no policy (see section Policies) that regulates the access of that subject to the unit the addressed subject will be not able to resolve the unit
</aside>

## Add subject reference

> **Request:** *Method:* POST *Url:* https://api.connctd.io/api/v1/units/-unitId-/subjects *Content-Type:* application/json *Body:* Id of subject. See example below

```json
{
  "id": "123-55664-aab-2456"
}
```

> **Response:** *Code:* 201

Adds a reference to a subject.

Required scope: `connctd.units.admin or connctd.core`

## Remove subject reference

> **Request:** *Method:* DELETE *Url:* https://api.connctd.io/api/v1/units/-unitId-/subjects *Content-Type:* application/json *Body:* Id of subject. See example below

```json
{
  "id": "123-55664-aab-2456"
}
```

> **Response:** *Code:* 204

Removes a reference to a subject.

Required scope: `connctd.units.admin or connctd.core`

## Get properties

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/units/-unitId-/properties

```json
```

> **Response:** *Code:* 200 *Body:* List of properties. See example below

```json
[
  {
    "name": "address",
    "value": {...}
  }
]
```

Retrieves a list of all properties

Required scope: `connctd.units.read or connctd.core`

## Get property

> **Request:** *Method:* GET *Url:* https://api.connctd.io/api/v1/units/-unitId-/properties/-propertyName-

```json
```

> **Response:** *Code:* 200 *Body:* A single property. See example below

```json
{
    "name": "number",
    "value": {...}
}
```

Read a specific property

Required scope: `connctd.units.read or connctd.core`

## Add property

> **Request:** *Method:* POST *Url:* https://api.connctd.io/api/v1/units/-unitId-/properties *Content-Type:* application/json *Body:* New property. See example below

```json
{
    "name": "MyNewProperty",
    "value": -any json(-ld)-
}
```

> **Response:** *Code:* 201

Adds a new property.

Required scope: `connctd.units.admin or connctd.core`

## Update property

> **Request:** *Method:* PUT *Url:* https://api.connctd.io/api/v1/units/-unitId-/properties *Content-Type:* application/json *Body:* New property value. See example below

```json
{
    "name": "MyNewProperty",
    "value": {...}
}
```

> **Response:** *Code:* 204

Updates a property.

Required scope: `connctd.units.admin or connctd.core`

## Delete property

> **Request:** *Method:* DELETE *Url:* https://api.connctd.io/api/v1/units/-unitId-/properties/-propertyName-

```json
```

> **Response:** *Code:* 200

```json
```

Removes a property from property set

Required scope: `connctd.units.admin or connctd.core`

## Get thing references

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

Retrieves a list of all thing references

Required scope: `connctd.units.read or connctd.core`

## Add thing reference

> **Request:** *Method:* POST *Url:* https://api.connctd.io/api/v1/units/-unitId-/things *Content-Type:* application/json *Body:* Id of thing. See example below

```json
{
	"id": "591327fe-5c1a-4dbb-a96b-9bef4de0a273"
}
```

> **Response:** *Code:* 201

Adds a reference to a things.

Required scope: `connctd.units.admin or connctd.core`

## Remove thing reference

> **Request:** *Method:* DELETE *Url:* https://api.connctd.io/api/v1/units/-unitId-/things *Content-Type:* application/json *Body:* Id of thing. See example below

```json
{
	"id": "591327fe-5c1a-4dbb-a96b-9bef4de0a273"
}
```

> **Response:** *Code:* 204

Removes a reference to a thing.

Required scope: `connctd.units.admin or connctd.core`
