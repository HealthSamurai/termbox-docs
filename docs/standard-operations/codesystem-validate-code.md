# CodeSystem/$validate-code

Checks whether a code, Coding, or CodeableConcept is valid in a CodeSystem. When `display` is supplied, Termbox also validates the display text.

## FHIR Specification

See the official FHIR specification for supported request parameters and response parameters: [https://www.hl7.org/fhir/codesystem-operation-validate-code.html](https://www.hl7.org/fhir/codesystem-operation-validate-code.html).

## Response

Returns a FHIR `Parameters` resource. The `result` output parameter is `true` when the submitted code is valid.

## SNOMED CT Coding example

This example validates a SNOMED CT Coding, including the submitted display text.

{% tabs %}
{% tab title="GET Request" %}
```http
GET /CodeSystem/$validate-code?url=http://snomed.info/sct&code=722892007&display=Fever%20due%20to%20infection%20(finding)
Accept: application/fhir+json
```
{% endtab %}

{% tab title="POST Request" %}
```http
POST /CodeSystem/$validate-code
Content-Type: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [
    {
      "name": "coding",
      "valueCoding": {
        "system": "http://snomed.info/sct",
        "code": "722892007",
        "display": "Fever due to infection (finding)"
      }
    }
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
    { "name": "code", "valueCode": "722892007" },
    { "name": "display", "valueString": "Fever due to infection (finding)" },
    { "name": "system", "valueUri": "http://snomed.info/sct" }
  ]
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
The SNOMED CT examples assume SNOMED CT is loaded in Termbox.
{% endhint %}

## SNOMED CT code example

This example checks that `73211009 | Diabetes mellitus` is a valid SNOMED CT code.

{% tabs %}
{% tab title="GET Request" %}
```http
GET /CodeSystem/$validate-code?url=http://snomed.info/sct&code=73211009
Accept: application/fhir+json
```
{% endtab %}

{% tab title="POST Request" %}
```http
POST /CodeSystem/$validate-code
Content-Type: application/fhir+json
Accept: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "url", "valueUri": "http://snomed.info/sct" },
    { "name": "code", "valueCode": "73211009" }
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
    { "name": "code", "valueCode": "73211009" },
    { "name": "display", "valueString": "Diabetes mellitus" },
    { "name": "system", "valueUri": "http://snomed.info/sct" },
    { "name": "version", "valueString": "http://snomed.info/sct/900000000000207008/version/20260701" }
  ]
}
```
{% endtab %}
{% endtabs %}
