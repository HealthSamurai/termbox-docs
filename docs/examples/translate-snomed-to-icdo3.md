# Translating a SNOMED code to ICD-O-3

For this example we assume THO and SNOMED International are loaded.

Translating `10024003 | Upper lobe of lung` to its ICD-O-3 equivalent:

```http
POST /ConceptMap/$translate
Content-Type: application/json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "sourceCode",   "valueCode": "10024003" },
    { "name": "sourceSystem", "valueUri":  "http://snomed.info/sct" },
    { "name": "targetSystem", "valueUri":  "http://terminology.hl7.org/CodeSystem/icd-o-3" }
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
        {
          "name": "relationship",
          "valueCode": "equivalent"
        },
        {
          "name": "concept",
          "valueCoding": {
            "system": "http://terminology.hl7.org/CodeSystem/icd-o-3",
            "code": "C34.3",
            "display": "Upper lobe, bronchus or lung"
          }
        }
      ]
    }
  ]
}
```
