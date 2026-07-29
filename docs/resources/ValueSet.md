# ValueSet

A ValueSet describes a selectable set of codes, usually by referencing one or more CodeSystems. It can list codes explicitly or define rules that include concepts by hierarchy, properties, filters, or other terminology criteria.

In Termbox, ValueSets are used to expand a definition into concrete codes and to validate whether a code belongs to that set. This is the resource to use when an application needs a controlled list of allowed answers or values.

See the official FHIR specification for the ValueSet resource: [https://www.hl7.org/fhir/valueset.html](https://www.hl7.org/fhir/valueset.html).

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

## Delete

[Deleting](https://hl7.org/fhir/http.html#delete) a ValueSet removes the resource from the server.

```http
DELETE /ValueSet/a3f8e621-7c94-4b52-9d01-e2b345678901
```

```http
HTTP/1.1 204 No Content
```

## Supported Operations

Termbox supports the following FHIR operations for ValueSet resources:

| Operation | Description |
| --------- | ----------- |
| [`ValueSet/$expand`](../standard-operations/valueset-expand.md) | Expand a ValueSet into the concrete set of concepts selected by its compose rules. |
| [`ValueSet/$validate-code`](../standard-operations/valueset-validate-code.md) | Check whether a code, Coding, or CodeableConcept is a member of a ValueSet. |
