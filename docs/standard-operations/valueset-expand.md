# ValueSet/$expand

`GET /ValueSet/$expand`

`POST /ValueSet/$expand`

Expands a ValueSet into the concrete set of concepts selected by its compose rules.

## FHIR Specification

See the official FHIR specification for supported request parameters and response parameters: [https://www.hl7.org/fhir/valueset-operation-expand.html](https://www.hl7.org/fhir/valueset-operation-expand.html).

## Response

Returns a FHIR `ValueSet` resource with an `expansion` element.

## Example

{% tabs %}
{% tab title="Request" %}
```http
GET /ValueSet/$expand?url=http://example.org/ValueSet/vs1
```
{% endtab %}

{% tab title="Response" %}
```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

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
