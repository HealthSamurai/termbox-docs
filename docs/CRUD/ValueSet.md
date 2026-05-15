# ValueSet

## Create

The example below creates a ValueSet that selects three concepts from an existing CodeSystem.

```http
POST http://localhost:3000/fhir/ValueSet
Content-Type: application/json
```

```json
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
Location: http://localhost:3000/fhir/ValueSet/a3f8e621-7c94-4b52-9d01-e2b345678901
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

[$expand](https://hl7.org/fhir/valueset-operation-expand.html) resolves the compose rules and returns the full enumerated set of concepts with their displays, sourced from the referenced CodeSystem.

```http
GET /fhir/ValueSet/$expand?url=http://example.org/ValueSet/vs1
```

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

```json
{
  "resourceType": "ValueSet",
  "url": "http://example.org/ValueSet/vs1",
  "version": "0.1.0",
  "name": "ExampleValueSet",
  "title": "Example Value Set",
  "status": "active",
  "expansion": {
    "timestamp": "2026-04-09T00:00:00Z",
    "total": 3,
    "parameter": [
      {
        "name": "used-codesystem",
        "valueUri": "http://example.org/CodeSystem/cs1|0.1.0"
      }
    ],
    "contains": [
      {
        "system": "http://example.org/CodeSystem/cs1",
        "code": "code1",
        "display": "Display 1"
      },
      {
        "system": "http://example.org/CodeSystem/cs1",
        "code": "code2",
        "display": "Display 2"
      },
      {
        "system": "http://example.org/CodeSystem/cs1",
        "code": "code3",
        "display": "Display 3"
      }
    ]
  }
}
```

## Delete

[Deleting](https://hl7.org/fhir/http.html#delete) a ValueSet removes the resource from the server.

```http
DELETE http://localhost:3000/fhir/ValueSet/a3f8e621-7c94-4b52-9d01-e2b345678901
```

```http
HTTP/1.1 204 No Content
```
