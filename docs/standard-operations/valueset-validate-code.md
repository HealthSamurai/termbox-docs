# ValueSet/$validate-code

Checks whether a code, Coding, or CodeableConcept is a member of a ValueSet. When `display` is supplied, Termbox also validates the display text.

## FHIR Specification

See the official FHIR specification for supported request parameters and response parameters: [https://www.hl7.org/fhir/valueset-operation-validate-code.html](https://www.hl7.org/fhir/valueset-operation-validate-code.html).

## Response

Returns a FHIR `Parameters` resource. The `result` output parameter is `true` when the submitted code is in the ValueSet.

## Explicit SNOMED CT ValueSet example

This example validates a SNOMED CT code against an implicit ValueSet and supplies the code system explicitly.

{% tabs %}
{% tab title="GET Request" %}
```http
GET /ValueSet/$validate-code?url=http://snomed.info/sct?fhir_vs=isa/386661006&code=722892007&system=http://snomed.info/sct
Accept: application/fhir+json
```
{% endtab %}

{% tab title="POST Request" %}
```http
POST /ValueSet/$validate-code
Content-Type: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "url", "valueUri": "http://snomed.info/sct?fhir_vs=isa/386661006" },
    { "name": "system", "valueUri": "http://snomed.info/sct" },
    { "name": "code", "valueCode": "722892007" },
    { "name": "display", "valueString": "Fever due to infection (finding)" }
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
The SNOMED CT examples assume SNOMED CT is loaded in Termbox. The example checks whether `722892007` is in the implicit ValueSet for descendants of `386661006`.
{% endhint %}

## Implicit SNOMED CT ValueSet example

This example validates that `38341003` belongs to the SNOMED CT implicit ValueSet `http://snomed.info/sct?fhir_vs=isa/404684003`.

{% tabs %}
{% tab title="GET Request" %}
```http
GET /ValueSet/$validate-code?url=http://snomed.info/sct?fhir_vs=isa/404684003&code=38341003&inferSystem=true
Accept: application/fhir+json
```
{% endtab %}

{% tab title="POST Request" %}
```http
POST /ValueSet/$validate-code
Content-Type: application/fhir+json
Accept: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "url", "valueUri": "http://snomed.info/sct?fhir_vs=isa/404684003" },
    { "name": "code", "valueCode": "38341003" },
    { "name": "inferSystem", "valueBoolean": true }
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
    { "name": "code", "valueCode": "38341003" },
    { "name": "display", "valueString": "Hypertensive disorder" },
    { "name": "system", "valueUri": "http://snomed.info/sct" },
    { "name": "version", "valueString": "http://snomed.info/sct/900000000000207008/version/20260701" }
  ]
}
```
{% endtab %}
{% endtabs %}

Using `inferSystem=true` lets the request omit `system` when the ValueSet URL identifies the SNOMED CT code system.
