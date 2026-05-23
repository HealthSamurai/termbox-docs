# SNOMED guide

## ConceptMaps

SNOMED CT ships with refsets that describe mappings between SNOMED concepts and other code systems. The FHIR specification covers how these are represented in [Using SNOMED CT with HL7 Standards](https://terminology.hl7.org/en/SNOMEDCT.html). There are three types:

**Simple map refsets** map each SNOMED concept to a single target code in another system — for example, ICD-O-3 and CTV3.

**Association refsets** record historical relationships between active and inactive SNOMED concepts. When a concept is retired, association refsets point to its replacement(s): REPLACED BY, SAME AS, POSSIBLY EQUIVALENT TO, or ALTERNATIVE.

**Extended map refsets** support complex mapping scenarios such as ICD-10, where a single SNOMED concept may require multiple target codes (map groups), ordered conditional alternatives (map priorities), and IFA rules that depend on patient context such as age or sex.

### Implicit ConceptMaps

Each SNOMED CT mapping refset is available as an implicit ConceptMap using the `fhir_cm` query parameter:

```
http://snomed.info/sct?fhir_cm={refsetId}
```

The following ConceptMaps are loaded with SNOMED CT International:

| Implicit URL | Target System | Type |
| --- | --- | --- |
| `http://snomed.info/sct?fhir_cm=446608001` | `http://terminology.hl7.org/CodeSystem/icd-o-3` | Simple map |
| `http://snomed.info/sct?fhir_cm=447562003` | `http://hl7.org/fhir/sid/icd-10` | Extended map |
| `http://snomed.info/sct?fhir_cm=900000000000497000` | `http://read.info/ctv3` | Simple map |
| `http://snomed.info/sct?fhir_cm=900000000000526001` | `http://snomed.info/sct` | Association (REPLACED BY) |
| `http://snomed.info/sct?fhir_cm=900000000000527005` | `http://snomed.info/sct` | Association (SAME AS) |
| `http://snomed.info/sct?fhir_cm=900000000000523009` | `http://snomed.info/sct` | Association (POSSIBLY EQUIVALENT TO) |
| `http://snomed.info/sct?fhir_cm=900000000000530003` | `http://snomed.info/sct` | Association (ALTERNATIVE) |

Use the `url` parameter in `$translate` to target a specific map:

```http
POST /fhir/ConceptMap/$translate
Content-Type: application/json
```

```json
{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "url",          "valueUri":  "http://snomed.info/sct?fhir_cm=447562003" },
    { "name": "sourceCode",   "valueCode": "73211009" },
    { "name": "sourceSystem", "valueUri":  "http://snomed.info/sct" }
  ]
}
```

Omit `url` and pass only `sourceSystem` + `targetSystem` to let the server find the right map automatically:

```http
POST /fhir/ConceptMap/$translate
Content-Type: application/json
```

```json
{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "sourceCode",   "valueCode": "73211009" },
    { "name": "sourceSystem", "valueUri":  "http://snomed.info/sct" },
    { "name": "targetSystem", "valueUri":  "http://hl7.org/fhir/sid/icd-10" }
  ]
}
```
