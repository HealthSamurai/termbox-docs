# Resource

Loads a single FHIR resource (CodeSystem, ValueSet, or ConceptMap) from a local JSON file.

| Field      | Optional | Description                                    |
| ---------- | -------- | ---------------------------------------------- |
| `location` |          | Path to a local `.json` file with the resource |

Relative paths are resolved from the Termbox root. Dependencies are not resolved automatically; each resource must be listed as a separate source entry.

```yaml
sources:
  - type: resource
    location: /data/my-codesystem.json
```
