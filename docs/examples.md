# Examples

For these examples we assume THO and SNOMED International are loaded.

## Lookup a code in a code system

```http
GET /fhir/CodeSystem/$lookup?system=http://snomed.info/sct&code=73211009
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

## Validate a code in a code system

Validating that `73211009 | Diabetes mellitus` is valid for SNOMED

```http
GET /fhir/CodeSystem/$validate-code?url=http://snomed.info/sct&code=73211009
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

## Validate a code using an implicit value set

Here we're validating that code `38341003` is valid in the implicit value set `http://snomed.info/sct?fhir_vs=isa/404684003`, which is, the value set of all SNOMED concepts that _are a_ `404684003 | Clinical finding`

```http
GET /fhir/ValueSet/$validate-code?url=http://snomed.info/sct?fhir_vs=isa/404684003&code=38341003&inferSystem=true
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

## Searching on an implicit value set

Here we're searching for the words "heart failure risk" on the implicit value set from the previous example (findings), top 10 results.

```http
GET /fhir/ValueSet/$expand?url=http://snomed.info/sct?fhir_vs=isa/404684003&filter=heart+failure+risk&count=10
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

## Translating a SNOMED code to ICD-O-3

Translating `10024003 | Upper lobe of lung` to its ICD-O-3 equivalent:

```http
POST /fhir/ConceptMap/$translate
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

## Translating across all known maps

Omitting `targetSystem` returns matches from every ConceptMap that covers the source code. Here `2681003 | Peripheral nerve of thigh` maps to both CTV3 and ICD-O-3:

```http
POST /fhir/ConceptMap/$translate
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

## Translating using an implicit ConceptMap

Pass the `url` parameter with a SNOMED implicit ConceptMap URL to target a specific map. Here we use the CTV3 map (`fhir_cm=900000000000497000`):

```http
POST /fhir/ConceptMap/$translate
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

## Searching on an ad-hoc value set

We'll search now based on two property filters:

- `370135005 | pathological process` = `441862004 | Infectious process`
- `363698007 | findign site` = `113255004 | lung`
- and the text `fever`

```http
POST /fhir/ValueSet/$expand
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
