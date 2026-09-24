# ConceptMap

A ConceptMap defines relationships between codes from one terminology and codes from another. It is used when systems need to translate or align data across different coding systems.

In Termbox, ConceptMaps power `$translate` operations. A single source code may map to one or more target codes, and each mapping includes a relationship such as `equivalent` or `related-to`.

See the official FHIR specification for the ConceptMap resource: [https://www.hl7.org/fhir/conceptmap.html](https://www.hl7.org/fhir/conceptmap.html).

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

## Read

[Reading](https://hl7.org/fhir/http.html#read) a ConceptMap returns the resource with its mapping groups and elements.

```http
GET /ConceptMap/d528f194-091b-4ec7-ab21-dcfead860d2c
```

```http
HTTP/1.1 200 OK
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

{% hint style="info" %}
To keep responses bounded, a read returns at most `FHIR_READ_ITEMS_LIMIT` mappings (source–target pairs) (default `1000`, see [Configuration](../configuration.md)). When the resource has more, the response is truncated and tagged with `SUBSETTED` in `meta.tag`. A group whose elements were cut off keeps the `http://health-samurai.io/extensions/excised-data` extension, so a client can tell which groups are incomplete.
{% endhint %}

If the ConceptMap does not exist, the server responds with `404 Not Found` and an `OperationOutcome` with the `not-found` issue code.

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
