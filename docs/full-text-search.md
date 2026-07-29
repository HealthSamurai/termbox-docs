# Full Text Search

Termbox supports text filtering in `ValueSet/$expand` through the standard `filter` parameter. The filter is applied to concept displays and designations within the requested value set.

Full text search is intended for UI search boxes and autocomplete workflows. Typo-tolerant fuzzy search is enabled by default and includes prefix matching behavior. You can disable fuzzy search to use prefix-only matching.

## Prefix Search

Prefix search treats each search word as a prefix. This makes partial user input work naturally while the user is still typing.

For example, this request can match `Left knee fracture` even though the final word is incomplete:

```http
GET /ValueSet/$expand?url=http://snomed.info/sct?fhir_vs&filter=left+knee+fra&count=10
```

A response can include:

```json
{
  "resourceType": "ValueSet",
  "url": "http://snomed.info/sct?fhir_vs",
  "status": "active",
  "expansion": {
    "total": 2,
    "parameter": [
      { "name": "count", "valueInteger": 10 }, 
      { "name": "filter", "valueString": "left knee fra" },
      { "name": "used-codesystem", "valueUri": "http://snomed.info/sct|http://snomed.info/sct/900000000000207008/version/20260701" }
    ],
    "contains": [
      {
        "code": "54331000087107",
        "system": "http://snomed.info/sct",
        "display": "Fracture of left knee"
      },
      {
        "code": "12217721000119105",
        "system": "http://snomed.info/sct",
        "display": "Fracture of bone adjacent to left knee joint prosthesis"
      }
    ]
  }
}
```

Prefix search is useful for inputs like:

| Filter                  | Intended match example        |
| ----------------------- | ----------------------------- |
| `left knee fra`         | Left knee fracture            |
| `left shoulder repla`   | Left shoulder replacement     |
| `general anx`           | Generalized anxiety disorder  |

## Fuzzy Search

Fuzzy search extends prefix search with typo correction. Termbox automatically fix misspelled query words before running the text search. For example, this request can match a concept containing `spinal` even though the input contains `soinal`:

```http
GET /ValueSet/$expand?url=http://snomed.info/sct?fhir_vs&filter=Lumbar+soinal+fusion&count=5
```

A response can include:

```json
{
  "resourceType": "ValueSet",
  "url": "http://snomed.info/sct?fhir_vs",
  "status": "active",
  "expansion": {
    "parameter": [
      { "name": "count", "valueInteger": 5 },
      { "name": "filter", "valueString": "Lumbar soinal fusion" },
      { "name": "used-codesystem", "valueUri": "http://snomed.info/sct|http://snomed.info/sct/900000000000207008/version/20260701" }
    ],
    "contains": [
      {
        "code": "50172003",
        "system": "http://snomed.info/sct",
        "display": "Lumbar spinal fusion"
      },
      {
        "code": "178643002",
        "system": "http://snomed.info/sct",
        "display": "Spinal fusion: [primary posterior lumbar] or [Bosworth] or [Hibbs]",
        "property": [{ "code": "inactive", "valueBoolean": true }],
        "inactive": true
      },
      {
        "code": "178644008",
        "system": "http://snomed.info/sct",
        "display": "Posterior spinal fusion (& lumbar (& [primary] or [primary NEC] or [named variants])",
        "property": [{ "code": "inactive", "valueBoolean": true }],
        "inactive": true
      },
      {
        "code": "278659001",
        "system": "http://snomed.info/sct",
        "display": "Lumbar spinal fusion for pseudoarthrosis"
      },
      {
        "code": "178642007",
        "system": "http://snomed.info/sct",
        "display": "Fusion spinal joint: [other primary lumbar] or [Moe's posterior]",
        "property": [{ "code": "inactive", "valueBoolean": true }],
        "inactive": true
      }
    ]
  }
}
```

Fuzzy search is useful for common typo shapes:

| Filter                  | Intended match example                         |
| ----------------------- | ---------------------------------------------- |
| `Lumbar soinal fusion`  | Lumbar spinal fusion                           |
| `diangostic`            | Diagnostic procedure                           |
| `trazadone`             | Trazodone hydrochloride 50 MG Oral Tablet      |
| `vomitting`             | Vomiting                                       |

Fuzzy search keeps the original search tokens as prefix alternatives. This means valid prefix searches continue to work in the default fuzzy search mode.

## Configuration

Fuzzy search is controlled by the `FUZZY_SEARCH_ENABLED` environment variable. It is enabled by default. Fuzzy queries carry a small performance penalty compared with prefix-only search because Termbox also checks candidate typo corrections before running the text search.

```
FUZZY_SEARCH_ENABLED=true|false
```

Set it to `false` only when you want prefix-only full text search:

See [Feature toggles](configuration.md#feature-flags-and-boolean-switches) for the full environment variable reference.
