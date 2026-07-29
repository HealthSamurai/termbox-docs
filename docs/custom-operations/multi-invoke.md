# Multi-Invoke

`POST /fhir/$x-multi-invoke`

Termbox extension operation that invokes a supported FHIR operation over multiple entries in a single request. This operation allows for high-throughput batch invocation of supported operations.

## Supported Operations

| `operation` value          | Description                                                       |
| --------------------------- | ------------------------------------------------------------------ |
| `CodeSystem/$find-matches` | Match each entry's display text against concepts in a CodeSystem. |

## Request Parameters

The request body is a FHIR `Parameters` resource with the following parameters:

| Name        | Type     | Description                                                                                                                       |
| ----------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------|
| `operation` | `uri`    | The operation to invoke, e.g. `CodeSystem/$find-matches`.                                                                         |
| `shared`    | `part`   | Parameters shared across all entries. Parts: `system` (uri, required), `version` (string, optional).                             |
| `entry`     | `part[]` | One entry per lookup. Parts: `display` (string, required), `system` (uri, optional), `version` (string, optional). Entry-level `system` / `version` overrides the shared values. |

## Response

Returns a `Parameters` resource with one `entry` per input entry (in the same order). Each `entry.part` contains zero or more `match` elements. An entry with no matching concept returns an empty `part` array.

## Example

{% tabs %}
{% tab title="Request" %}
```http
POST /fhir/$x-multi-invoke
Content-Type: application/json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "operation", "valueUri": "CodeSystem/$find-matches" },
    {
      "name": "shared",
      "part": [
        { "name": "system",  "valueUri": "http://snomed.info/sct" },
        { "name": "version", "valueString": "http://snomed.info/sct/83821000000107/version/20260211" }
      ]
    },
    { "name": "entry", "part": [{ "name": "display", "valueString": "Acute appendicitis" }] },
    { "name": "entry", "part": [{ "name": "display", "valueString": "Type 2 diabetes mellitus" }] },
    { "name": "entry", "part": [{ "name": "display", "valueString": "this description does not exist" }] }
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
      "name": "entry",
      "part": [
        { "name": "match", "part": [{ "name": "code", "valueCode": "85189001" }] }
      ]
    },
    {
      "name": "entry",
      "part": [
        { "name": "match", "part": [{ "name": "code", "valueCode": "44054006" }] }
      ]
    },
    {
      "name": "entry",
      "part": []
    }
  ]
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Matching is exact and case-sensitive. Per-entry `system` and `version` parts can be used to look up across different code systems in a single request.
{% endhint %}
