# Termbox Gallery

Loads a terminology from the Termbox Gallery by its canonical URL.

| Field     | Optional | Description                      |
| --------- | -------- | -------------------------------- |
| `url`     |          | Canonical URL of the terminology |
| `version` | yes      | Specific version or edition      |

```yaml
sources:
  - type: gallery
    url: http://loinc.org
  - type: gallery
    url: http://snomed.info/sct
    version: http://snomed.info/sct/900000000000207008/version/20260201
```
