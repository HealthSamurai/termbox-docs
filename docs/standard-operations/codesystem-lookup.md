# CodeSystem/$lookup

`GET /CodeSystem/$lookup`

`POST /CodeSystem/$lookup`

Resolves a code in a CodeSystem and returns concept details such as display, designations, properties, abstract status, and version.

## FHIR Specification

See the official FHIR specification for supported request parameters and response parameters: [https://www.hl7.org/fhir/codesystem-operation-lookup.html](https://www.hl7.org/fhir/codesystem-operation-lookup.html).

## Response

Returns a FHIR `Parameters` resource. Common output parameters include `code`, `display`, `name`, `system`, `abstract`, `version`, `designation`, and `property`.

## Example

{% tabs %}
{% tab title="Request" %}
```http
GET /CodeSystem/$lookup?system=http://example.org/CodeSystem/cs1&code=code1&property=prop
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
This example uses the CodeSystem created in the [CodeSystem CRUD guide](../CRUD/CodeSystem.md).
{% endhint %}
