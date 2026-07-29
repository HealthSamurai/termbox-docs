# Searching on an implicit value set

For this example we assume THO and SNOMED International are loaded.

Here we're searching for the words "heart failure risk" on the implicit value set from the previous example (findings), top 10 results.

```http
GET /ValueSet/$expand?url=http://snomed.info/sct?fhir_vs=isa/404684003&filter=heart+failure+risk&count=10
```

```json
{
  "resourceType": "ValueSet",
  // ...
    "contains": [
      {
        "code": "609386002",
        "display": "At increased risk for heart failure (finding)",
        "system": "http://snomed.info/sct"
      },
      {
        "code": "609388001",
        "display": "Stage A at high risk of heart failure (finding)",
        "system": "http://snomed.info/sct"
      },
  // ...
    ]
// ...
  }
}
```
