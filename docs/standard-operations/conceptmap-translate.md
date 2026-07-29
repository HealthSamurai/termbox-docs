# ConceptMap/$translate

Translates a code from one terminology to another using a ConceptMap.

## FHIR Specification

See the official FHIR specification for supported request parameters and response parameters: [https://www.hl7.org/fhir/conceptmap-operation-translate.html](https://www.hl7.org/fhir/conceptmap-operation-translate.html).

## Response

Returns a FHIR `Parameters` resource. The `result` parameter indicates whether a translation was found. Each `match` part contains a target `concept` and its `relationship`.

## Example

This example translates a local code using the ConceptMap created in the ConceptMap CRUD guide.

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

## SNOMED CT to ICD-O-3 example

This example translates `10024003 | Upper lobe of lung` to ICD-O-3.

{% tabs %}
{% tab title="GET Request" %}
```http
GET /ConceptMap/$translate?sourceCode=10024003&sourceSystem=http://snomed.info/sct&targetSystem=http://terminology.hl7.org/CodeSystem/icd-o-3
Accept: application/fhir+json
```
{% endtab %}

{% tab title="POST Request" %}
```http
POST /ConceptMap/$translate
Content-Type: application/fhir+json
Accept: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "sourceCode", "valueCode": "10024003" },
    { "name": "sourceSystem", "valueUri": "http://snomed.info/sct" },
    { "name": "targetSystem", "valueUri": "http://terminology.hl7.org/CodeSystem/icd-o-3" }
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "result", "valueBoolean": true },
    {
      "name": "match",
      "part": [
        { "name": "concept", "valueCoding": { "system": "http://terminology.hl7.org/CodeSystem/icd-o-3", "code": "C34.3" } },
        { "name": "relationship", "valueCode": "equivalent" },
        {
          "name": "originMap",
          "valueUri": "http://snomed.info/sct?fhir_cm=446608001|http://snomed.info/sct/900000000000207008/version/20260701"
        }
      ]
    }
  ]
}
```
{% endtab %}
{% endtabs %}

The response includes `originMap`, which identifies the SNOMED CT implicit ConceptMap used for the translation.

## All known maps example

This example omits `targetSystem`, so Termbox returns matches from every SNOMED CT ConceptMap that covers `2681003 | Peripheral nerve of thigh`.

{% tabs %}
{% tab title="GET Request" %}
```http
GET /ConceptMap/$translate?sourceCode=2681003&sourceSystem=http://snomed.info/sct
Accept: application/fhir+json
```
{% endtab %}

{% tab title="POST Request" %}
```http
POST /ConceptMap/$translate
Content-Type: application/fhir+json
Accept: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "sourceCode", "valueCode": "2681003" },
    { "name": "sourceSystem", "valueUri": "http://snomed.info/sct" }
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "result", "valueBoolean": true },
    {
      "name": "match",
      "part": [
        { "name": "concept", "valueCoding": { "system": "http://terminology.hl7.org/CodeSystem/icd-o-3", "code": "C47.5" } },
        { "name": "relationship", "valueCode": "equivalent" },
        {
          "name": "originMap",
          "valueUri": "http://snomed.info/sct?fhir_cm=446608001|http://snomed.info/sct/900000000000207008/version/20260701"
        }
      ]
    },
    {
      "name": "match",
      "part": [
        { "name": "concept", "valueCoding": { "system": "http://snomed.info/sct", "code": "306800005" } },
        { "name": "relationship", "valueCode": "related-to" },
        {
          "name": "originMap",
          "valueUri": "http://snomed.info/sct?fhir_cm=734139008|http://snomed.info/sct/900000000000207008/version/20260701"
        }
      ]
    },
    {
      "name": "match",
      "part": [
        { "name": "concept", "valueCoding": { "system": "http://read.info/ctv3", "code": "XUAec" } },
        { "name": "relationship", "valueCode": "equivalent" },
        {
          "name": "originMap",
          "valueUri": "http://snomed.info/sct?fhir_cm=900000000000497000|http://snomed.info/sct/900000000000207008/version/20260701"
        }
      ]
    }
  ]
}
```
{% endtab %}
{% endtabs %}

Because no `targetSystem` is supplied, the response may include matches from multiple implicit ConceptMaps.

## Implicit ConceptMap example

This example uses the SNOMED CT implicit ConceptMap URL `http://snomed.info/sct?fhir_cm=900000000000497000` to target the CTV3 map explicitly.

{% tabs %}
{% tab title="GET Request" %}
```http
GET /ConceptMap/$translate?url=http://snomed.info/sct?fhir_cm=900000000000497000&sourceCode=154938001&sourceSystem=http://snomed.info/sct
Accept: application/fhir+json
```
{% endtab %}

{% tab title="POST Request" %}
```http
POST /ConceptMap/$translate
Content-Type: application/fhir+json
Accept: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "url", "valueUri": "http://snomed.info/sct?fhir_cm=900000000000497000" },
    { "name": "sourceCode", "valueCode": "154938001" },
    { "name": "sourceSystem", "valueUri": "http://snomed.info/sct" }
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "result", "valueBoolean": true },
    {
      "name": "match",
      "part": [
        { "name": "concept", "valueCoding": { "system": "http://read.info/ctv3", "code": ".E4D4" } },
        { "name": "relationship", "valueCode": "equivalent" },
        {
          "name": "originMap",
          "valueUri": "http://snomed.info/sct?fhir_cm=900000000000497000|http://snomed.info/sct/900000000000207008/version/20260701"
        }
      ]
    }
  ]
}
```
{% endtab %}
{% endtabs %}
