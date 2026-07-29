# FHIR Operations

Termbox supports the following standard FHIR terminology operations and extensions.

## Standard Operations

| Operation                                                                       | Description                                                                                                                                                   |
| ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`CodeSystem/$lookup`](standard-operations/codesystem-lookup.md)                | Given a code/system, get additional details about the concept. See [spec](https://www.hl7.org/fhir/codesystem-operation-lookup.html)                          |
| [`CodeSystem/$validate-code`](standard-operations/codesystem-validate-code.md)  | Validates a coded value. See [spec](https://www.hl7.org/fhir/codesystem-operation-validate-code.html)                                                         |
| [`CodeSystem/$subsumes`](standard-operations/codesystem-subsumes.md)            | Test the subsumption relationship between A and B. See [spec](https://www.hl7.org/fhir/codesystem-operation-subsumes.html)                                    |
| [`CodeSystem/$find-matches`](standard-operations/codesystem-find-matches.md)    | Given a set of properties (and text), return one or more possible matching codes. See [spec](https://www.hl7.org/fhir/codesystem-operation-find-matches.html) |
| [`ValueSet/$expand`](standard-operations/valueset-expand.md)                    | Returns an expansion of concepts according to the value set definition. See [spec](https://www.hl7.org/fhir/valueset-operation-expand.html)                   |
| [`ValueSet/$validate-code`](standard-operations/valueset-validate-code.md)      | Validate that a coded value is in the set of codes allowed by a value set. See [spec](https://www.hl7.org/fhir/valueset-operation-validate-code.html)         |
| [`ConceptMap/$translate`](standard-operations/conceptmap-translate.md)          | Translate a code from one terminology to another. See [spec](https://www.hl7.org/fhir/conceptmap-operation-translate.html)                                    |

## Extensions

| Operation                                                 | Description                                                          |
| --------------------------------------------------------- | -------------------------------------------------------------------- |
| [`/$x-multi-invoke`](extensions/multi-invoke.md)   | Invoke a supported operation over multiple entries in a single request. |
