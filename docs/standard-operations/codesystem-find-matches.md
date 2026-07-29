# CodeSystem/$find-matches

Returns possible matching CodeSystem concepts for a submitted display string. Termbox currently supports exact, case-sensitive matching against concept designations and display text.

## FHIR Specification

See the official FHIR specification for supported request parameters and response parameters: [https://www.hl7.org/fhir/codesystem-operation-find-matches.html](https://www.hl7.org/fhir/codesystem-operation-find-matches.html).

## Response

Returns a FHIR `Parameters` resource with zero or more `match` parameters. Each `match` contains a `code` part.

## Example

{% tabs %}
{% tab title="Request" %}
```http
POST /CodeSystem/$find-matches
Content-Type: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "system", "valueUri": "http://example.org/CodeSystem/cs-colors" },
    { "name": "display", "valueString": "Azure" }
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "resourceType": "Parameters",
  "parameter": [
    {
      "name": "match",
      "part": [
        { "name": "code", "valueCode": "blue" }
      ]
    }
  ]
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
For high-throughput matching over many display strings, use [$x-multi-invoke](../custom-operations/multi-invoke.md) with `operation` set to `CodeSystem/$find-matches`.
{% endhint %}
