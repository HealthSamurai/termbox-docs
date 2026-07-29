# Validate a code using an implicit value set

For this example we assume THO and SNOMED International are loaded.

Here we're validating that code `38341003` is valid in the implicit value set `http://snomed.info/sct?fhir_vs=isa/404684003`, which is the value set of all SNOMED concepts that _are a_ `404684003 | Clinical finding`.

```http
GET /ValueSet/$validate-code?url=http://snomed.info/sct?fhir_vs=isa/404684003&code=38341003&inferSystem=true
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
      "valueCode": "38341003"
    },
    {
      "name": "display",
      "valueString": "Hypertensive disorder, systemic arterial (disorder)"
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
