# NPM

Downloads a FHIR package from a package registry.

| Field     | Optional | Description                             |
| --------- | -------- | --------------------------------------- |
| `package` |          | Package name                            |
| `version` | yes      | Package version; omit to use the latest |

```yaml
sources:
  - type: npm
    package: hl7.terminology
  - type: npm
    package: hl7.fhir.r4.core
    version: 4.0.1
```
