# Table of contents

* [Introduction](introduction.md)

## Getting Started

* [Getting Started](getting-started.md)
* [Loading Data](loading-data/README.md)
  * [NPM](loading-data/npm.md)
  * [Package](loading-data/package.md)
  * [Resource](loading-data/resource.md)
  * [Termbox Gallery](loading-data/gallery.md)
  * [FHIR Bundle](loading-data/bundle.md)
  * [Atom Feed](loading-data/atom.md)

## Architecture

* [Architecture](architecture.md)
* [Model](model.md)

## Reference

* [Capabilities](capabilities.md)
* [FHIR Operations](fhir-operations.md)
  * [CodeSystem/$lookup](standard-operations/codesystem-lookup.md)
  * [CodeSystem/$validate-code](standard-operations/codesystem-validate-code.md)
  * [CodeSystem/$subsumes](standard-operations/codesystem-subsumes.md)
  * [CodeSystem/$find-matches](standard-operations/codesystem-find-matches.md)
  * [ValueSet/$expand](standard-operations/valueset-expand.md)
  * [ValueSet/$validate-code](standard-operations/valueset-validate-code.md)
  * [ConceptMap/$translate](standard-operations/conceptmap-translate.md)
  * [Extensions](extensions/README.md)
    * [$x-multi-invoke](extensions/multi-invoke.md)
* [CodeSystem](CRUD/CodeSystem.md)
* [ValueSet](CRUD/ValueSet.md)
* [ConceptMap](CRUD/ConceptMap.md)
* [Full Text Search](full-text-search.md)
* [Running Termbox on Managed PostgreSQL](managed-postgresql.md)
* [Authentication and Authorization](auth.md)
* [Configuration](configuration.md)
* [Licensing](licensing.md)
* [Troubleshooting](troubleshooting.md)

## Misc

* [Performance](performance.md)
* [Release Notes](release-notes.md)

## Guides

* [Lookup a code in a code system](examples/lookup-code.md)
* [Validate a code in a code system](examples/validate-code.md)
* [Validate a code using an implicit value set](examples/validate-code-implicit-valueset.md)
* [Searching on an implicit value set](examples/search-implicit-valueset.md)
* [Translating a SNOMED code to ICD-O-3](examples/translate-snomed-to-icdo3.md)
* [Translating across all known maps](examples/translate-all-known-maps.md)
* [Translating using an implicit ConceptMap](examples/translate-implicit-conceptmap.md)
* [Searching on an ad-hoc value set](examples/search-adhoc-valueset.md)
* [RxNorm Guide](guides/rxnorm.md)
* [SNOMED guide](guides/snomed.md)
