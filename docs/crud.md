# CRUD

Full support for [CRUD](https://hl7.org/fhir/R4/http.html) is planned. Currently, only [creation](https://hl7.org/fhir/R4/http.html#create) of CodeSystems and ValueSets is supported.

## Create a CodeSystem

The example below creates a simple code system.

```http
POST /fhir/CodeSystem
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
  "date": "2026-04-08",
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
          "property": [{"code": "prop", "valueCode": "new"}],
          "concept": [
            {
              "code": "code2aI",
              "display": "Display 2aI",
              "definition": "First third level concept",
              "property": [{"code": "prop", "valueCode": "old"}]
            },
            {
              "code": "code2aII",
              "display": "Display 2aII",
              "definition": "Second third level concept",
              "property": [{"code": "prop", "valueCode": "new"}]
            }
          ]
        },
        {
          "code": "code2b",
          "display": "Display 2b",
          "definition": "Second second level code",
          "property": [{"code": "prop", "valueCode": "old"}]
        }
      ]
    },
    {
      "code": "code3",
      "display": "Display 3",
      "property": [{"code": "prop", "valueCode": "old"}]
    }
  ]
}
```

The **response** includes a `Location` header pointing to the newly created CodeSystem, and the resource body with the `concept` array excised.

```http
HTTP/1.1 201 Created
Location: /fhir/CodeSystem/9191fce8-f7b0-4008-8387-bb140ae063f0
Content-Type: application/json; charset=utf-8
```

```json
{
  "date": "2026-04-08",
  "content": "complete",
  "property": [
    {
      "type": "code",
      "code": "prop",
      "uri": "http://hl7.org/fhir/test/CodeSystem/properties#prop"
    },
    {
      "type": "code",
      "code": "status",
      "uri": "http://hl7.org/fhir/concept-properties#status"
    },
    {
      "type": "boolean",
      "code": "notSelectable",
      "uri": "http://hl7.org/fhir/concept-properties#notSelectable"
    }
  ],
  "name": "ExampleCodeSystem",
  "experimental": false,
  "resourceType": "CodeSystem",
  "title": "Example Code System",
  "extension": [
    {
      "extension": [
        {
          "valueString": "concept",
          "url": "path"
        },
        {
          "url": "count",
          "valueInteger": 3
        }
      ],
      "url": "http://health-samurai.io/extensions/excised-data"
    }
  ],
  "status": "active",
  "language": "en",
  "hierarchyMeaning": "is-a",
  "id": "9191fce8-f7b0-4008-8387-bb140ae063f0",
  "url": "http://example.org/CodeSystem/cs1",
  "caseSensitive": true,
  "version": "0.1.0"
}
```

## Lookup a Concept

After creating a CodeSystem, you can start using [terminology operations](https://hl7.org/fhir/terminology-module.html).

```http
GET /fhir/CodeSystem/$lookup?system=http://example.org/CodeSystem/cs1&code=code1
```
**Response**

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
      "name": "display"
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
      "name": "version",
      "valueString": "0.1.0"
    },
    {
      "name": "abstract",
      "valueBoolean": false
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
          "name": "value"
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
          "name": "value"
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
