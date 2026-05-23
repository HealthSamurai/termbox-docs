# ConceptMap

## Create

The example below creates a ConceptMap that maps two local condition codes to their SNOMED CT equivalents.

```http
POST http://localhost:3000/fhir/ConceptMap
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
Location: http://localhost:3000/fhir/ConceptMap/d528f194-091b-4ec7-ab21-dcfead860d2c
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

## Translate

[$translate](https://hl7.org/fhir/conceptmap-operation-translate.html) translates a code using the newly created ConceptMap. Pass the ConceptMap `url` to target it explicitly.

```http
POST http://localhost:3000/fhir/ConceptMap/$translate
Content-Type: application/json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "url",          "valueUri":  "http://example.org/ConceptMap/cm1" },
    { "name": "sourceCode",   "valueCode": "DM" },
    { "name": "sourceSystem", "valueUri":  "http://example.org/CodeSystem/local-conditions" }
  ]
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

```json
{
  "resourceType": "Parameters",
  "parameter": [
    {
      "name": "result",
      "valueBoolean": true
    },
    {
      "name": "match",
      "part": [
        {
          "name": "concept",
          "valueCoding": {
            "system": "http://snomed.info/sct",
            "code": "73211009"
          }
        },
        {
          "name": "relationship",
          "valueCode": "equivalent"
        }
      ]
    }
  ]
}
```

## Delete

[Deleting](https://hl7.org/fhir/http.html#delete) a ConceptMap removes the resource and all its mappings from the server.

```http
DELETE http://localhost:3000/fhir/ConceptMap/d528f194-091b-4ec7-ab21-dcfead860d2c
```

```http
HTTP/1.1 204 No Content
```
