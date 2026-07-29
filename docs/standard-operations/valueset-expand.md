# ValueSet/$expand

Expands a ValueSet into the concrete set of concepts selected by its compose rules.

## FHIR Specification

See the official FHIR specification for supported request parameters and response parameters: [https://www.hl7.org/fhir/valueset-operation-expand.html](https://www.hl7.org/fhir/valueset-operation-expand.html).

## Response

Returns a FHIR `ValueSet` resource with an `expansion` element.

## Custom ValueSet expansion example

This example expands the ValueSet created in the ValueSet CRUD guide.

{% tabs %}
{% tab title="GET Request" %}
```http
GET /ValueSet/$expand?url=http://example.org/ValueSet/vs1
```
{% endtab %}

{% tab title="POST Request" %}
```http
POST /ValueSet/$expand
Content-Type: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "url", "valueUri": "http://example.org/ValueSet/vs1" }
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "resourceType": "ValueSet",
  "url": "http://example.org/ValueSet/vs1",
  "version": "0.1.0",
  "name": "ExampleValueSet",
  "title": "Example Value Set",
  "status": "active",
  "expansion": {
    "timestamp": "2026-04-09T00:00:00Z",
    "total": 3,
    "parameter": [
      {
        "name": "used-codesystem",
        "valueUri": "http://example.org/CodeSystem/cs1|0.1.0"
      }
    ],
    "contains": [
      {
        "system": "http://example.org/CodeSystem/cs1",
        "code": "code1",
        "display": "Display 1"
      },
      {
        "system": "http://example.org/CodeSystem/cs1",
        "code": "code2",
        "display": "Display 2"
      },
      {
        "system": "http://example.org/CodeSystem/cs1",
        "code": "code3",
        "display": "Display 3"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
This example uses the ValueSet created in the [ValueSet CRUD guide](../CRUD/ValueSet.md).
{% endhint %}

## Implicit SNOMED CT ValueSet example

This example searches for `heart failure risk` within the SNOMED CT implicit ValueSet `http://snomed.info/sct?fhir_vs=isa/404684003`.

{% tabs %}
{% tab title="GET Request" %}
```http
GET /ValueSet/$expand?url=http://snomed.info/sct?fhir_vs=isa/404684003&filter=heart+failure+risk&count=10
Accept: application/fhir+json
```
{% endtab %}

{% tab title="POST Request" %}
```http
POST /ValueSet/$expand
Content-Type: application/fhir+json
Accept: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "url", "valueUri": "http://snomed.info/sct?fhir_vs=isa/404684003" },
    { "name": "filter", "valueString": "heart failure risk" },
    { "name": "count", "valueInteger": 10 }
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "resourceType": "ValueSet",
  "url": "http://snomed.info/sct?fhir_vs=isa/404684003",
  "status": "active",
  "expansion": {
    "total": 8,
    "parameter": [
      { "name": "count", "valueInteger": 10 },
      { "name": "filter", "valueString": "heart failure risk" },
      {
        "name": "used-codesystem",
        "valueUri": "http://snomed.info/sct|http://snomed.info/sct/900000000000207008/version/20260701"
      }
    ],
    "contains": [
      { "code": "609389009", "system": "http://snomed.info/sct", "display": "Stage B at high risk of heart failure" },
      { "code": "1382259003", "system": "http://snomed.info/sct", "display": "Stage B at high risk of systolic heart failure" },
      { "code": "1382260008", "system": "http://snomed.info/sct", "display": "Stage B at high risk of diastolic heart failure" },
      { "code": "609387006", "system": "http://snomed.info/sct", "display": "At high risk for heart failure" },
      { "code": "609388001", "system": "http://snomed.info/sct", "display": "Stage A at high risk of heart failure" },
      { "code": "609386002", "system": "http://snomed.info/sct", "display": "At increased risk for heart failure" },
      { "code": "1382261007", "system": "http://snomed.info/sct", "display": "Stage B at high risk of systolic heart failure due to ischemic cardiomyopathy" },
      { "code": "1303865002", "system": "http://snomed.info/sct", "display": "Marfanoid habitus, facial dysmorphism, skeletal abnormality, heart defect syndrome" }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

## Inline ValueSet example

This example expands an inline ValueSet filtered by pathological process, finding site, and the text `fever`.

{% tabs %}
{% tab title="POST Request" %}
```http
POST /ValueSet/$expand
Content-Type: application/fhir+json
Accept: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [{
    "name": "valueSet",
    "resource": {
      "resourceType": "ValueSet",
      "compose": {
        "include": [{
          "system": "http://snomed.info/sct",
          "filter": [{
            "property": "370135005",
            "op": "=",
            "value": "441862004"
          }, {
            "property": "363698007",
            "op": "=",
            "value": "113255004"
          }]
        }]
      }
    }
  }, {
    "name": "filter",
    "valueString": "fever"
  }, {
    "name": "count",
    "valueInteger": 10
  }]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "resourceType": "ValueSet",
  "expansion": {
    "total": 4,
    "parameter": [
      { "name": "count", "valueInteger": 10 },
      { "name": "filter", "valueString": "fever" },
      {
        "name": "used-codesystem",
        "valueUri": "http://snomed.info/sct|http://snomed.info/sct/900000000000207008/version/20260701"
      }
    ],
    "contains": [
      { "code": "32286006", "system": "http://snomed.info/sct", "display": "Pneumonia in Q fever" },
      { "code": "45312009", "system": "http://snomed.info/sct", "display": "Pneumonia in typhoid fever" },
      { "code": "1208602000", "system": "http://snomed.info/sct", "display": "Pneumonia caused by Pseudomonas aeruginosa" },
      { "code": "763888005", "system": "http://snomed.info/sct", "display": "Necrotizing pneumonia caused by Panton-Valentine leukocidin producing Staphylococcus aureus" }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

This request uses POST because the inline ValueSet carries the property filters in the request body.
