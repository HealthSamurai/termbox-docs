# Query the Terminology Server

**Goal:** look up, validate, search and translate codes with Termbox's FHIR API.

**Base URL:** set `BASE` before running the calls below. Use the local Termbox:

```bash
BASE=http://localhost:3000/fhir
```

Or the hosted server, which needs no setup:

```bash
BASE=https://tx.health-samurai.io/fhir
```

To create a local Termbox, follow [Start Termbox Locally](start-local.md). The hosted server is [Termbox SaaS](../saas/README.md).

Never answer with codes from memory. Check them with these calls.

## Common calls

Get a code's display, version and properties:

```bash
curl -s -G "$BASE/CodeSystem/\$lookup" --data-urlencode 'system=http://loinc.org' --data-urlencode 'code=8867-4'
```

Check that a code exists:

```bash
curl -s -G "$BASE/CodeSystem/\$validate-code" --data-urlencode 'url=http://snomed.info/sct' --data-urlencode 'code=73211009'
```

Check that a code belongs to a value set:

```bash
curl -s -G "$BASE/ValueSet/\$validate-code" \
  --data-urlencode 'url=http://hl7.org/fhir/ValueSet/administrative-gender' \
  --data-urlencode 'system=http://hl7.org/fhir/administrative-gender' \
  --data-urlencode 'code=male'
```

List a value set's codes. `count` sets the page size and `offset` the start:

```bash
curl -s -G "$BASE/ValueSet/\$expand" --data-urlencode 'url=http://hl7.org/fhir/ValueSet/administrative-gender' --data-urlencode 'count=50'
```

Search a SNOMED CT hierarchy by text:

```bash
curl -s -G "$BASE/ValueSet/\$expand" \
  --data-urlencode 'url=http://snomed.info/sct?fhir_vs=isa/73211009' \
  --data-urlencode 'filter=type 2' \
  --data-urlencode 'count=10'
```

Test whether one SNOMED CT code includes another. `subsumes` means `codeA` is the broader concept:

```bash
curl -s -G "$BASE/CodeSystem/\$subsumes" \
  --data-urlencode 'system=http://snomed.info/sct' \
  --data-urlencode 'codeA=73211009' \
  --data-urlencode 'codeB=44054006'
```

Translate SNOMED CT to ICD-10:

```bash
curl -s -X POST "$BASE/ConceptMap/\$translate" -H 'Content-Type: application/fhir+json' -d '{
  "resourceType": "Parameters",
  "parameter": [
    {"name": "url", "valueUri": "http://snomed.info/sct?fhir_cm=447562003"},
    {"name": "sourceCode", "valueCode": "73211009"},
    {"name": "sourceSystem", "valueUri": "http://snomed.info/sct"}
  ]
}'
```

Find RxNorm drugs by ingredient, in this case the combinations of acetaminophen (`161`) and caffeine (`1886`). Write RxNorm relationship values with the `CUI:` prefix:

```bash
curl -s -X POST "$BASE/ValueSet/\$expand" -H 'Content-Type: application/fhir+json' -d '{
  "resourceType": "Parameters",
  "parameter": [
    {"name": "count", "valueInteger": 50},
    {"name": "valueSet", "resource": {
      "resourceType": "ValueSet",
      "status": "active",
      "compose": {"include": [{
        "system": "http://www.nlm.nih.gov/research/umls/rxnorm",
        "filter": [
          {"property": "tty", "op": "=", "value": "MIN"},
          {"property": "has_part", "op": "=", "value": "CUI:161"},
          {"property": "has_part", "op": "=", "value": "CUI:1886"}
        ]
      }]}
    }}
  ]
}'
```

## System URLs

| Terminology | System |
| --- | --- |
| SNOMED CT | `http://snomed.info/sct` |
| LOINC | `http://loinc.org` |
| RxNorm | `http://www.nlm.nih.gov/research/umls/rxnorm` |
| UCUM | `http://unitsofmeasure.org` |
| ICD-10 (WHO) | `http://hl7.org/fhir/sid/icd-10` |

## Good to know

- **Text search:** `filter` matches words in the display text and doesn't understand synonyms. If results look wrong, try another wording.
- **Ranking:** the best match isn't always first, so read all the results.
- **Inactive codes:** results can include them, marked `"inactive": true`. Prefer active codes unless the user asks otherwise.
- **Versions:** without a `version` parameter, the latest loaded version is used. For a specific SNOMED CT edition, pass `version=http://snomed.info/sct/<edition id>`.
- **FHIR versions:** `/fhir` is the default endpoint. To get responses in a specific FHIR version, use `/fhir/r4`, `/fhir/r4b`, `/fhir/r5` or `/fhir/r6` instead.

For all parameters and operations, see [Capabilities](../capabilities.md).
