# Searching on an ad-hoc value set

For this example we assume THO and SNOMED International are loaded.

We'll search now based on two property filters:

- `370135005 | pathological process` = `441862004 | Infectious process`
- `363698007 | findign site` = `113255004 | lung`
- and the text `fever`

```http
POST /ValueSet/$expand
Content-Type: application/json

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

```json
{
  "resourceType": "ValueSet",
  "expansion": {
  // ...
    ],
    "contains": [
      {
        "code": "32286006",
        "display": "Pneumonia in Q fever (disorder)",
        "system": "http://snomed.info/sct"
      },
      {
        "code": "45312009",
        "display": "Pneumonia in typhoid fever (disorder)",
        "system": "http://snomed.info/sct"
      },
      {
        "code": "763888005",
        "display": "Necrotizing pneumonia caused by Panton-Valentine leukocidin producing Staphylococcus aureus (disorder)",
        "system": "http://snomed.info/sct"
      },
      {
        "code": "1208602000",
        "display": "Pneumonia caused by Pseudomonas aeruginosa (disorder)",
        "system": "http://snomed.info/sct"
      }
    ]
  }
}
```
