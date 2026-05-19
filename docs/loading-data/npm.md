# NPM

Loads a FHIR package from a package registry or from a local `.tgz` archive. Either `package` or `location` must be specified.

| Field      | Optional | Description                                             |
| ---------- | -------- | ------------------------------------------------------- |
| `package`  | yes      | Package name (required when not using `location`)       |
| `version`  | yes      | Package version; omit to use the latest (registry only) |
| `location` | yes      | Path to a local `.tgz` package archive (required when not using `package`) |

## Registry

Download a package by coordinates from the registry.

```yaml
sources:
  - type: npm
    package: hl7.terminology
  - type: npm
    package: hl7.fhir.r4.core
    version: 4.0.1
```

## Local archive

Load a package from a local `.tgz` file. Relative paths are resolved from the Termbox root. Dependencies are not resolved automatically; each must be listed as a separate source entry.

```yaml
sources:
  - type: npm
    location: ../.tx-content/hl7.fhir.r4.core-4.0.1.tgz
```
