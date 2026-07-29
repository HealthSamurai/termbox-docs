# Translating across all known maps

For this example we assume THO and SNOMED International are loaded.

Omitting `targetSystem` returns matches from every ConceptMap that covers the source code. Here `2681003 | Peripheral nerve of thigh` maps to both CTV3 and ICD-O-3:

```http
POST /ConceptMap/$translate
Content-Type: application/json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "sourceCode",   "valueCode": "2681003" },
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
        { "name": "concept", "valueCoding": { "system": "http://read.info/ctv3", "code": "XUAec" } }
      ]
    },
    {
      "name": "match",
      "part": [
        { "name": "relationship", "valueCode": "equivalent" },
        { "name": "concept", "valueCoding": { "system": "http://terminology.hl7.org/CodeSystem/icd-o-3", "code": "C47.5" } }
      ]
    }
  ]
}
```
