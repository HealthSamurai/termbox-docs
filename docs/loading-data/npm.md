# NPM

Loads a FHIR package from a package registry or from a local `.tgz` archive. Either `package` or `location` must be specified.

| Field          | Optional | Description                                             |
| -------------- | -------- | ------------------------------------------------------- |
| `package`      | no       | Package name                                            |
| `version`      | yes      | Package version; omit to use the latest                 |
| `dependencies` | yes      | Controls transitive dependency loading (see below)      |

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

## Dependency control

By default, Termbox recursively loads all transitive dependencies of a package. The `dependencies` field lets you opt out entirely or exclude specific packages.

### Skip all dependencies

Set `load: false` to load only the package itself, without pulling any of its dependencies:

```yaml
sources:
  - type: npm
    package: hl7.fhir.us.davinci-pdex
    version: 2.0.0
    dependencies:
      load: false
```

### Exclude specific packages

Use `exclude` to block specific packages from the dependency tree. Each entry requires a `package` name; `version` is optional — omitting it excludes that package regardless of version.

```yaml
sources:
  - type: npm
    package: hl7.fhir.us.core
    dependencies:
      exclude:
        - package: hl7.fhir.uv.smart-app-launch
          version: 2.2.0        # exclude this version only
        - package: us.cdc.phinvads
                                # no version — excludes all versions
```

| Field     | Optional | Description                                              |
| --------- | -------- | -------------------------------------------------------- |
| `package` | no       | Package name to exclude                                  |
| `version` | yes      | Version to exclude; omit to exclude all versions         |

