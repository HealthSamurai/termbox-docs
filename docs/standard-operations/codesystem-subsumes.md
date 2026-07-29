# CodeSystem/$subsumes

Tests the subsumption relationship between two codes in a CodeSystem.

## FHIR Specification

See the official FHIR specification for supported request parameters and response parameters: [https://www.hl7.org/fhir/codesystem-operation-subsumes.html](https://www.hl7.org/fhir/codesystem-operation-subsumes.html).

## Response

Returns a FHIR `Parameters` resource with one `outcome` parameter. Possible values are `equivalent`, `subsumes`, `subsumed-by`, and `not-subsumed`.

## SNOMED CT subsumption example

{% tabs %}
{% tab title="GET Request" %}
```http
GET /CodeSystem/$subsumes?system=http://snomed.info/sct&codeA=73211009&codeB=44054006
Accept: application/fhir+json
```
{% endtab %}

{% tab title="POST Request" %}
```http
POST /CodeSystem/$subsumes
Content-Type: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "system", "valueUri": "http://snomed.info/sct" },
    {
      "name": "codingA",
      "valueCoding": { "system": "http://snomed.info/sct", "code": "73211009" }
    },
    {
      "name": "codingB",
      "valueCoding": { "system": "http://snomed.info/sct", "code": "44054006" }
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
    { "name": "outcome", "valueCode": "subsumes" }
  ]
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
In this example, `73211009` (Diabetes mellitus) subsumes `44054006` (Type 2 diabetes mellitus). The SNOMED CT examples assume SNOMED CT is loaded in Termbox.
{% endhint %}
