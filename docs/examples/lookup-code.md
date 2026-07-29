# Lookup a code in a code system

For this example we assume THO and SNOMED International are loaded.

```http
GET /CodeSystem/$lookup?system=http://snomed.info/sct&code=73211009
```

```json
{
  "resourceType": "Parameters",
  "parameter": [
    {
      "name": "code",
      "valueCode": "73211009"
    },
    {
      "name": "display",
      "valueString": "Diabetes mellitus (disorder)"
    },
    {
      "name": "name",
      "valueString": "snomed-ct"
    },
    {
      "name": "system",
      "valueUri": "http://snomed.info/sct"
    },
    // ...
}
```
