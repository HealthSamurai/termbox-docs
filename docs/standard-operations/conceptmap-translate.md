# ConceptMap/$translate

`GET /ConceptMap/$translate`

`POST /ConceptMap/$translate`

Translates a code from one terminology to another using a ConceptMap.

## FHIR Specification

See the official FHIR specification for supported request parameters and response parameters: [https://www.hl7.org/fhir/conceptmap-operation-translate.html](https://www.hl7.org/fhir/conceptmap-operation-translate.html).

## Response

Returns a FHIR `Parameters` resource. The `result` parameter indicates whether a translation was found. Each `match` part contains a target `concept` and its `relationship`.

## Example

{% tabs %}
{% tab title="Request" %}
```http
POST /ConceptMap/$translate
Content-Type: application/json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "url", "valueUri": "http://example.org/ConceptMap/cm1" },
    { "name": "sourceCode", "valueCode": "DM" },
    { "name": "sourceSystem", "valueUri": "http://example.org/CodeSystem/local-conditions" }
  ]
}
```
{% endtab %}

{% tab title="Response" %}
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
{% endtab %}
{% endtabs %}

{% hint style="info" %}
This example uses the ConceptMap created in the [ConceptMap CRUD guide](../CRUD/ConceptMap.md).
{% endhint %}
