# FHIR API

Termbox supports all R4, R5, and R6 normative operations on ValueSets and CodeSystems[^4]

{% hint style="info" %}
**Support Legend**
- ✅ Full support - Complete implementation with all parameters
- 🏗️ In development - Under development, expect partial support in the meantime
- ❌ No support - In our roadmap
- 🚫 Won't support - Not in our roadmap
{% endhint %}

## Operations

<style>
.operations-table td:first-child,
.operations-table th:first-child { white-space: nowrap; }
</style>

<div class="operations-table">

| Operation                   | Description                                                                                                                                                   | Support |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| [`CodeSystem/$lookup`](standard-operations/codesystem-lookup.md)               | Given a code/system, get additional details about the concept. See [spec](https://www.hl7.org/fhir/codesystem-operation-lookup.html)                          | ✅       |
| [`CodeSystem/$validate-code`](standard-operations/codesystem-validate-code.md) | Validates a coded value. See [spec](https://www.hl7.org/fhir/codesystem-operation-validate-code.html)                                                         | ✅       |
| [`CodeSystem/$subsumes`](standard-operations/codesystem-subsumes.md)           | Test the subsumption relationship between A and B. See [spec](https://www.hl7.org/fhir/codesystem-operation-subsumes.html)                                    | ✅       |
| [`CodeSystem/$find-matches`](standard-operations/codesystem-find-matches.md)   | Given a set of properties (and text), return one or more possible matching codes. See [spec](https://www.hl7.org/fhir/codesystem-operation-find-matches.html) | ✅       |
| [`ValueSet/$expand`](standard-operations/valueset-expand.md)                   | Returns an expansion of concepts according to the value set definition. See [spec](https://www.hl7.org/fhir/valueset-operation-expand.html)                   | ✅       |
| [`ValueSet/$validate-code`](standard-operations/valueset-validate-code.md)     | Validate that a coded value is in the set of codes allowed by a value set. See [spec](https://www.hl7.org/fhir/valueset-operation-validate-code.html)         | ✅       |
| [`ConceptMap/$translate`](standard-operations/conceptmap-translate.md)         | Translate a code from one terminology to another. See [spec](https://www.hl7.org/fhir/conceptmap-operation-translate.html)                                    | ✅       |
| `ConceptMap/$closure`       | Maintenance of a client-side transitive closure table. See [spec](https://www.hl7.org/fhir/conceptmap-operation-closure.html)                                 | ❌       |
| [`/$x-multi-invoke`](custom-operations/multi-invoke.md)                       | Termbox extension: invoke a supported operation over multiple entries in a single request.                                                                    | ✅       |

</div>

## Features

| Feature                                      | Description                                                     | Support |
| -------------------------------------------- | --------------------------------------------------------------- | ------- |
| Capability Statements                        | + TerminologyCapabilities                                       | ✅       |
| CRUD of Terminology resources                | Create, Read, Update, Delete operations                         | 🏗️       |
| Pre-coordinated codes                        | Standard coded concepts                                         | ✅       |
| Post-coordinated codes                       | Complex expressions not yet supported                           | ❌       |
| Intensional ValueSets                        | Filter-based ValueSet definitions                               | ✅       |
| Extensional ValueSets                        | Explicit concept enumeration                                    | ✅       |
| ValueSet expansion                           | Full expansion with pagination                                  | ✅       |
| ValueSet validation                          | Code membership validation                                      | ✅       |
| ValueSet inclusion/exclusion                 | Deep set operations support                                     | ✅       |
| Lookup displays, designation, and properties | All concept attributes                                          | ✅       |
| Full text search filter                      | Prefix, stemming, phrase matching                               | ✅       |
| Fuzzy search                                 | Typo-tolerant text, enabled by default                          | ✅       |
| FTS ranking                                  | Full text search ranking based on relevance                     | ✅       |
| Property filters                             | Property-based filtering: `=`, `in`, `regex`, etc               | ✅       |
| Subsumption filtering                        | `is-a`, `generalizes`, `ancestors`, etc                         | ✅       |
| Active/Inactive filtering                    | Via `status`, `inactive`, `notSelectable`, etc                  | ✅       |
| Multi-language support                       | Translations via displaylanguage, HTTP header, designation, etc | ✅       |
| Hierarchy via properties                     | e.g.: `parent`, `child`, `PAR`, `CHD`, etc                      | ✅       |
| Nested concepts                              | Hierarchy via `concept.concept`                                 | ✅       |
| Supplemental CodeSystems                     | Additional concept properties and designations                  | 🏗️       |
| Implicit ValueSets                           | System-generated ValueSets                                      | ✅       |
| ConceptMap translations                      | Code mapping between terminology systems                        | ✅       |
| Multiple ConceptMap matches                  | Returns all applicable mappings for source code                 | ✅       |
| Transitive closure table                     | `$closure`                                                      | ❌       |
| Syntax-based code systems                    | UCUM, BCP-47, HGVS, etc                                         | ❌       |
| `tx-resource` parameter                      | Inline resource definitions                                     | ✅       |
| Ad-hoc ValueSets                             | ValueSet as a `Parameter`                                       | ✅       |
| R4/R5/R6 format conversion                   | One server, one database, multiple FHIR versions                | ❌       |
| Batch validation                             | Validation of many codes in one request                         | 🏗️       |

## FHIR Versions

Termbox runs one endpoint for each major FHIR version[^5] and a default endpoint (currently R5, users will be able to configure this in upcoming releases)

- Default: `/fhir/`
- Version specific: `/fhir/:version/`

Examples:
- R6: `GET /fhir/r6/ValueSet/$expand`
- R4B: `GET /fhir/r4b/metadata`
