# Package

Loads a FHIR package from a local `.tgz` archive.

| Field      | Optional | Description                            |
| ---------- | -------- | -------------------------------------- |
| `location` |          | Path to a local `.tgz` package archive |

Relative paths are resolved from the Termbox root. Dependencies are not resolved automatically; each must be listed as a separate source entry.

```yaml
sources:
  - type: package
    location: /data/tx-content/hl7.fhir.r4.core-4.0.1.tgz
```
