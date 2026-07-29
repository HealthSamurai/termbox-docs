# Validate a code in a code system

For this example we assume THO and SNOMED International are loaded.

Validating that `73211009 | Diabetes mellitus` is valid for SNOMED.

```http
GET /CodeSystem/$validate-code?url=http://snomed.info/sct&code=73211009
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
      "name": "code",
      "valueCode": "73211009"
    },
    {
      "name": "display",
      "valueString": "Diabetes mellitus (disorder)"
    },
    {
      "name": "system",
      "valueUri": "http://snomed.info/sct"
    },
    {
      "name": "version",
      "valueString": "http://snomed.info/sct/900000000000207008/version/20260201"
    }
  ]
}
```
