# CodeSystem

A CodeSystem defines a set of codes and what those codes mean. In Termbox, CodeSystems are the source of truth for concept displays, designations, properties, hierarchy relationships, and validation.

CodeSystems may represent large external terminologies such as SNOMED CT, or smaller custom code lists maintained by an implementation. Once loaded or created, their concepts can be looked up, validated, matched by display text, and compared by hierarchy.

## Create

The example below creates a CodeSystem with a three-level concept hierarchy, custom properties, and multi-language designations.

```http
POST /CodeSystem
Content-Type: application/json

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
Location: /CodeSystem/d528f194-091b-4ec7-ab21-dcfead860d2c
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

## Delete

[Deleting](https://hl7.org/fhir/http.html#delete) a CodeSystem removes the resource and all its concepts from the server.

```http
DELETE /CodeSystem/d528f194-091b-4ec7-ab21-dcfead860d2c
```

```http
HTTP/1.1 204 No Content
```

## Supported Operations

Termbox supports the following FHIR operations for CodeSystem resources:

| Operation | Description |
| --------- | ----------- |
| [`CodeSystem/$lookup`](../standard-operations/codesystem-lookup.md) | Resolve a code and return concept details such as display, designations, properties, abstract status, and version. |
| [`CodeSystem/$validate-code`](../standard-operations/codesystem-validate-code.md) | Check whether a code, Coding, or CodeableConcept is valid in a CodeSystem. |
| [`CodeSystem/$subsumes`](../standard-operations/codesystem-subsumes.md) | Test the subsumption relationship between two codes in a CodeSystem. |
| [`CodeSystem/$find-matches`](../standard-operations/codesystem-find-matches.md) | Return possible matching CodeSystem concepts for a submitted display string. |
