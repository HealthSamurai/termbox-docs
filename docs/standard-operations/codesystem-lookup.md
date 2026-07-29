# CodeSystem/$lookup

Resolves a code in a CodeSystem and returns concept details such as display, designations, properties, abstract status, and version.

## FHIR Specification

See the official FHIR specification for supported request parameters and response parameters: [https://www.hl7.org/fhir/codesystem-operation-lookup.html](https://www.hl7.org/fhir/codesystem-operation-lookup.html).

## Response

Returns a FHIR `Parameters` resource. Common output parameters include `code`, `display`, `name`, `system`, `abstract`, `version`, `designation`, and `property`.

## Custom CodeSystem property example

This example resolves a code from the CodeSystem created in the CodeSystem resource guide and asks for the custom `prop` property.

{% tabs %}
{% tab title="GET Request" %}
```http
GET /CodeSystem/$lookup?system=http://example.org/CodeSystem/cs1&code=code1&property=prop
```
{% endtab %}

{% tab title="POST Request" %}
```http
POST /CodeSystem/$lookup
Content-Type: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "system", "valueUri": "http://example.org/CodeSystem/cs1" },
    { "name": "code", "valueCode": "code1" },
    { "name": "property", "valueCode": "prop" }
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "code", "valueCode": "code1" },
    { "name": "display", "valueString": "Display 1" },
    { "name": "name", "valueString": "ExampleCodeSystem" },
    { "name": "system", "valueUri": "http://example.org/CodeSystem/cs1" },
    { "name": "abstract", "valueBoolean": false },
    { "name": "version", "valueString": "0.1.0" },
    {
      "name": "designation",
      "part": [
        { "name": "language", "valueCode": "en" },
        {
          "name": "use",
          "valueCoding": {
            "system": "http://terminology.hl7.org/CodeSystem/hl7TermMaintInfra",
            "code": "preferredForLanguage"
          }
        },
        { "name": "value", "valueString": "Display 1" }
      ]
    },
    {
      "name": "designation",
      "part": [
        { "name": "language", "valueCode": "de-DE" },
        { "name": "value", "valueString": "Anzeige 1" }
      ]
    },
    {
      "name": "property",
      "part": [
        { "name": "code", "valueCode": "prop" },
        { "name": "value", "valueCode": "old" }
      ]
    },
    {
      "name": "property",
      "part": [
        { "name": "code", "valueCode": "inactive" },
        { "name": "value", "valueBoolean": false }
      ]
    }
  ]
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
This example uses the CodeSystem created in the [CodeSystem resource guide](../resources/CodeSystem.md).
{% endhint %}

## SNOMED CT lookup example

This example resolves the SNOMED CT code `73211009` and returns its display and system metadata.

{% tabs %}
{% tab title="GET Request" %}
```http
GET /CodeSystem/$lookup?system=http://snomed.info/sct&code=73211009
Accept: application/fhir+json
```
{% endtab %}

{% tab title="POST Request" %}
```http
POST /CodeSystem/$lookup
Content-Type: application/fhir+json
Accept: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "system", "valueUri": "http://snomed.info/sct" },
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
    { "name": "code", "valueCode": "73211009" },
    { "name": "display", "valueString": "Diabetes mellitus" },
    { "name": "name", "valueString": "snomed-ct" },
    { "name": "system", "valueUri": "http://snomed.info/sct" },
    { "name": "abstract", "valueBoolean": false },
    { "name": "version", "valueString": "http://snomed.info/sct/900000000000207008/version/20260701" },
    {
      "name": "property",
      "part": [
        { "name": "code", "valueCode": "inactive" },
        { "name": "value", "valueBoolean": false }
      ]
    }
  ]
}
```
{% endtab %}
{% endtabs %}
