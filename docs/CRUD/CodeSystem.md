# CodeSystem

## Create

The example below creates a CodeSystem with a three-level concept hierarchy, custom properties, and multi-language designations.

```http
POST http://localhost:3000/fhir/CodeSystem
Content-Type: application/json
```

```json
{
  "resourceType": "CodeSystem",
  "language": "en",
  "url": "http://example.org/CodeSystem/cs1",
  "version": "0.1.0",
  "name": "ExampleCodeSystem",
  "title": "Example Code System",
  "status": "active",
  "experimental": false,
  "date": "2026-05-15",
  "caseSensitive": true,
  "hierarchyMeaning": "is-a",
  "content": "complete",
  "property": [
    {
      "code": "prop",
      "uri": "http://hl7.org/fhir/test/CodeSystem/properties#prop",
      "type": "code"
    },
    {
      "code": "status",
      "uri": "http://hl7.org/fhir/concept-properties#status",
      "type": "code"
    },
    {
      "code": "notSelectable",
      "uri": "http://hl7.org/fhir/concept-properties#notSelectable",
      "type": "boolean"
    }
  ],
  "concept": [
    {
      "code": "code1",
      "display": "Display 1",
      "designation": [{ "value": "Anzeige 1", "language": "de-DE" }],
      "property": [{ "code": "prop", "valueCode": "old" }]
    },
    {
      "code": "code2",
      "display": "Display 2",
      "definition": "Second concept, with children",
      "property": [
        { "code": "prop", "valueCode": "new" },
        { "code": "notSelectable", "valueBoolean": true },
        { "code": "status", "valueCode": "retired" }
      ],
      "concept": [
        {
          "code": "code2a",
          "display": "Display 2a",
          "definition": "First second level concept",
          "property": [{ "code": "prop", "valueCode": "new" }],
          "concept": [
            {
              "code": "code2aI",
              "display": "Display 2aI",
              "definition": "First third level concept",
              "property": [{ "code": "prop", "valueCode": "old" }]
            },
            {
              "code": "code2aII",
              "display": "Display 2aII",
              "definition": "Second third level concept",
              "property": [{ "code": "prop", "valueCode": "new" }]
            }
          ]
        },
        {
          "code": "code2b",
          "display": "Display 2b",
          "definition": "Second second level code",
          "property": [{ "code": "prop", "valueCode": "old" }]
        }
      ]
    },
    {
      "code": "code3",
      "display": "Display 3",
      "property": [{ "code": "prop", "valueCode": "old" }]
    }
  ]
}
```

The response includes a `Location` header pointing to the newly created resource. The `concept` array is excised from the response body.

```http
HTTP/1.1 201 Created
Location: http://localhost:3000/fhir/CodeSystem/d528f194-091b-4ec7-ab21-dcfead860d2c
Content-Type: application/json; charset=utf-8
```

```json
{
  "resourceType": "CodeSystem",
  "id": "60e25275-2eed-43b1-b3d7-14c1291fc1b6",
  "url": "http://example.org/CodeSystem/cs1",
  "version": "0.1.0",
  "name": "ExampleCodeSystem",
  "title": "Example Code System",
  "language": "en",
  "status": "active",
  "experimental": false,
  "date": "2026-05-15",
  "caseSensitive": true,
  "hierarchyMeaning": "is-a",
  "content": "complete",
  "property": [
    {
      "code": "prop",
      "uri": "http://hl7.org/fhir/test/CodeSystem/properties#prop",
      "type": "code"
    },
    {
      "code": "status",
      "uri": "http://hl7.org/fhir/concept-properties#status",
      "type": "code"
    },
    {
      "code": "notSelectable",
      "uri": "http://hl7.org/fhir/concept-properties#notSelectable",
      "type": "boolean"
    }
  ],
  "extension": [
    {
      "url": "http://health-samurai.io/extensions/excised-data",
      "extension": [
        { "url": "path", "valueString": "concept" },
        { "url": "count", "valueInteger": 3 }
      ]
    }
  ]
}
```

## Lookup a Concept

[$lookup](https://hl7.org/fhir/codesystem-operation-lookup.html) resolves a code against the newly created CodeSystem and returns its display, designations, and properties.

```http
GET http://localhost:3000/fhir/CodeSystem/$lookup?system=http://example.org/CodeSystem/cs1&code=code1&property=prop
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
      "name": "code",
      "valueCode": "code1"
    },
    {
      "name": "display",
      "valueString": "Display 1"
    },
    {
      "name": "name",
      "valueString": "ExampleCodeSystem"
    },
    {
      "name": "system",
      "valueUri": "http://example.org/CodeSystem/cs1"
    },
    {
      "name": "abstract",
      "valueBoolean": false
    },
    {
      "name": "version",
      "valueString": "0.1.0"
    },
    {
      "name": "designation",
      "part": [
        {
          "name": "language",
          "valueCode": "en"
        },
        {
          "name": "use",
          "valueCoding": {
            "system": "http://terminology.hl7.org/CodeSystem/hl7TermMaintInfra",
            "code": "preferredForLanguage"
          }
        },
        {
          "name": "value",
          "valueString": "Display 1"
        }
      ]
    },
    {
      "name": "designation",
      "part": [
        {
          "name": "language",
          "valueCode": "de-DE"
        },
        {
          "name": "value",
          "valueString": "Anzeige 1"
        }
      ]
    },
    {
      "name": "property",
      "part": [
        {
          "name": "code",
          "valueCode": "prop"
        },
        {
          "name": "value",
          "valueCode": "old"
        }
      ]
    },
    {
      "name": "property",
      "part": [
        {
          "name": "code",
          "valueCode": "inactive"
        },
        {
          "name": "value",
          "valueBoolean": false
        }
      ]
    }
  ]
}
```

## Delete

[Deleting](https://hl7.org/fhir/http.html#delete) a CodeSystem removes the resource and all its concepts from the server.

```http
DELETE http://localhost:3000/fhir/CodeSystem/d528f194-091b-4ec7-ab21-dcfead860d2c
```

```http
HTTP/1.1 204 No Content
```
