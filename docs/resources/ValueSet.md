# ValueSet

## Create

The example below creates a ValueSet that selects three concepts from an existing CodeSystem.

```http
POST /ValueSet
Content-Type: application/json

{
  "resourceType": "ValueSet",
  "url": "http://example.org/ValueSet/vs1",
  "version": "0.1.0",
  "name": "ExampleValueSet",
  "title": "Example Value Set",
  "status": "active",
  "experimental": false,
  "date": "2026-05-15",
  "compose": {
    "include": [
      {
        "system": "http://example.org/CodeSystem/cs1",
        "concept": [
          { "code": "code1" },
          { "code": "code2" },
          { "code": "code3" }
        ]
      }
    ]
  }
}
```

The response includes a `Location` header pointing to the newly created resource.

```http
HTTP/1.1 201 Created
Location: /ValueSet/a3f8e621-7c94-4b52-9d01-e2b345678901
Content-Type: application/json; charset=utf-8
```

```json
{
  "resourceType": "ValueSet",
  "id": "a3f8e621-7c94-4b52-9d01-e2b345678901",
  "url": "http://example.org/ValueSet/vs1",
  "version": "0.1.0",
  "name": "ExampleValueSet",
  "title": "Example Value Set",
  "status": "active",
  "experimental": false,
  "date": "2026-04-09",
  "compose": {
    "include": [
      {
        "system": "http://example.org/CodeSystem/cs1",
        "concept": [
          { "code": "code1" },
          { "code": "code2" },
          { "code": "code3" }
        ]
      }
    ]
  }
}
```

## Expand the ValueSet

Use [`ValueSet/$expand`](../standard-operations/valueset-expand.md) to resolve the compose rules and return the full enumerated set of concepts with their displays.

## Delete

[Deleting](https://hl7.org/fhir/http.html#delete) a ValueSet removes the resource from the server.

```http
DELETE /ValueSet/a3f8e621-7c94-4b52-9d01-e2b345678901
```

```http
HTTP/1.1 204 No Content
```
