# Capabilities

Termbox supports all R4, R5, and R6 normative operations on ValueSets and CodeSystems.

{% hint style="info" %}
**Support Legend**
- ✅ Full support - Complete implementation with all parameters
- 🏗️ In development - Under development, expect partial support in the meantime
- ❌ No support - In our roadmap
- 🚫 Won't support - Not in our roadmap
{% endhint %}

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

## CRUD

Termbox supports the [FHIR REST API](https://hl7.org/fhir/http.html) for managing terminology resources. Full CRUD support is planned; the table below shows the current status.

| Interaction | FHIR Spec                                                    | CodeSystem | ValueSet | ConceptMap |
| ----------- | ------------------------------------------------------------ | ---------- | -------- | ---------- |
| Create      | [POST   /[type]](https://hl7.org/fhir/http.html#create)      | ✅          | ✅        | ✅          |
| Read        | [GET    /[type]/[id]](https://hl7.org/fhir/http.html#read)   |            |          |            |
| Update      | [PUT    /[type]/[id]](https://hl7.org/fhir/http.html#update) |            |          |            |
| Delete      | [DELETE /[type]/[id]](https://hl7.org/fhir/http.html#delete) | ✅          | ✅        | ✅          |
| Search      | [GET    /[type]](https://hl7.org/fhir/http.html#search)      |            |          |            |

## Operations

| Operation                   | Description                                                       | Support |
| --------------------------- | ----------------------------------------------------------------- | ------- |
| `CodeSystem/$lookup`        | Given a code/system, get additional details about the concept.    | ✅       |
| `CodeSystem/$validate-code` | Validates a coded value.                                          | ✅       |
| `CodeSystem/$subsumes`      | Test the subsumption relationship between A and B.                | ✅       |
| `CodeSystem/$find-matches`  | Given a set of properties (and text), return matching codes.      | ✅       |
| `ValueSet/$expand`          | Returns an expansion according to the value set definition.       | ✅       |
| `ValueSet/$validate-code`   | Validate that a coded value is in the allowed value set codes.    | ✅       |
| `ConceptMap/$translate`     | Translate a code from one terminology to another.                 | ✅       |
| `ConceptMap/$closure`       | Maintenance of a client-side transitive closure table.            | ❌       |
| `/$x-multi-invoke`          | Invoke a supported operation over multiple entries in one request. | ✅       |

## FHIR Versions

Termbox runs one endpoint for each major FHIR version and a default endpoint (currently R5, users will be able to configure this in upcoming releases)

- Default: `/fhir/`
- Version specific: `/fhir/:version/`

Examples:
- R4: `GET /fhir/r4/metadata`
- R6: `GET /fhir/r6/ValueSet/$expand`
- R4B: `GET /fhir/r4b/metadata`
