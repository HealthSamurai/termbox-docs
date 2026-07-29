# ConceptMap

## Create

The example below creates a ConceptMap that maps two local condition codes to their SNOMED CT equivalents.

```http
POST /ConceptMap
Content-Type: application/json

{
  "resourceType": "ConceptMap",
  "url": "http://example.org/ConceptMap/cm1",
  "version": "0.1.0",
  "name": "ExampleConceptMap",
  "title": "Example Concept Map",
  "status": "active",
  "group": [
    {
      "source": "http://example.org/CodeSystem/local-conditions",
      "target": "http://snomed.info/sct",
      "element": [
        {
          "code": "DM",
          "display": "Diabetes",
          "target": [
            {
              "code": "73211009",
              "display": "Diabetes mellitus (disorder)",
              "relationship": "equivalent"
            }
          ]
        },
        {
          "code": "HTN",
          "display": "Hypertension",
          "target": [
            {
              "code": "38341003",
              "display": "Hypertensive disorder, systemic arterial (disorder)",
              "relationship": "equivalent"
            }
          ]
        }
      ]
    }
  ]
}
```

The response includes a `Location` header pointing to the newly created resource.

```http
HTTP/1.1 201 Created
Location: /ConceptMap/d528f194-091b-4ec7-ab21-dcfead860d2c
Content-Type: application/json; charset=utf-8
```

```json
{
  "resourceType": "ConceptMap",
  "id": "d528f194-091b-4ec7-ab21-dcfead860d2c",
  "url": "http://example.org/ConceptMap/cm1",
  "version": "0.1.0",
  "name": "ExampleConceptMap",
  "title": "Example Concept Map",
  "status": "active"
}
```

## Delete

[Deleting](https://hl7.org/fhir/http.html#delete) a ConceptMap removes the resource and all its mappings from the server.

```http
DELETE /ConceptMap/d528f194-091b-4ec7-ab21-dcfead860d2c
```

```http
HTTP/1.1 204 No Content
```

## Supported Operations

Termbox supports the following FHIR operations for ConceptMap resources:

| Operation | Description |
| --------- | ----------- |
| [`ConceptMap/$translate`](../standard-operations/conceptmap-translate.md) | Translate a code from one terminology to another using a ConceptMap. |
