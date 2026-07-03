# Release Notes

## 0.14.0

- CodeSystem/$find-matches, basic support
- Multi-invoke support for CodeSystem/$find-matches
- FHIR search support for `name:contains` param
- ConceptMap/$translate support for codeableConcept
- ValueSet/$expand multi version (R4/R5)
- Canonicals resolution improvements
- Bug fixes and general enhancements

## 0.13.1

- Readonly mode fixes.
- Include supplement bug fixes.

## 0.13.0

- Readonly mode and other toggles (ui, admin, fhir). See [docs](https://www.health-samurai.io/docs/termbox/configuration)
- Several FHIR API and content conformance and alignment improvements.
- Maybe fixes and enhancements.

## 0.12.2

- Performance improvements on pagination
- Fix memory leak on large data.yaml

## 0.12.1

- Multibranch valuesets pagination bug fixes

## 0.12.0

- $lookup: Support for accept-language HTTP header
- data.yaml: New source type: resource
- npm loader: Dependency control, see docs
- paging: Configurable total behavior
- Bug fixes and other enhancements

## 0.11.1

- Metrics endpoint and status codes fixes

## 0.11.0

- Support for `ConceptMap` and `ConceptMap/$translate`
- Support for SNOMED implicit concept maps
- Atom: support for `client_id_env` as well as `client_id_file` and `client_secret_file`
- Data.yaml: support for local npm packages
- Bug fixes and other enhancements

## 0.10.0

- JSON file ingestion improvements: Streaming JSON for lower memory consumption
- Prometheus metrics support (/metrics)
- Admin reset endpoint (/admin/reset)
- Some performance improvements
- Bug fixes and other enhancements

## 0.9.0

- FHIR Ops interface: Ability to run FHIR Operations from Termbox UI
- Resource details interface: Detailed view for CodeSystems and ValueSets
- Atom: Filter support
- Data Loader: sync: true option for content synchronization
- Bug fixes and other enhancements

## 0.8.0

- Support for syndication feed
- Support for batch request
- Data loading fixes

## 0.7.0

- Content preload from config file
- Support for gallery source
- Support for bundle source
- Support for supplements (data only)
- FHIR packages loading fixes
- Security patch for ValueSet operations

## 0.6.0

- FHIR package registry integration
- Licensing
- Ability to control ingestion resources
- Bug fixes

## 0.5.0

- Support for deleting CodeSystem and ValueSet via FHIR API
- Conformance bug fixes

## 0.4.0

- Performance improvements
- Binary ingestion improvements
- Support for the creation of CodeSystem and ValueSet via FHIR API
- Conformance bug fixes
