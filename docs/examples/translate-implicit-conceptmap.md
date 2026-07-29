# Translating using an implicit ConceptMap

For this example we assume THO and SNOMED International are loaded.

Pass the `url` parameter with a SNOMED implicit ConceptMap URL to target a specific map. Here we use the CTV3 map (`fhir_cm=900000000000497000`):

```http
POST /ConceptMap/$translate
Content-Type: application/json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "url",          "valueUri":  "http://snomed.info/sct?fhir_cm=900000000000497000" },
    { "name": "sourceCode",   "valueCode": "154938001" },
    { "name": "sourceSystem", "valueUri":  "http://snomed.info/sct" }
  ]
}
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
      "name": "match",
      "part": [
        { "name": "relationship", "valueCode": "equivalent" },
        { "name": "concept", "valueCoding": { "system": "http://read.info/ctv3", "code": ".E4D4" } }
      ]
    }
  ]
}
```
