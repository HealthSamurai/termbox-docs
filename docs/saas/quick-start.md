# Quick Start

This page shows how to start using Termbox SaaS with a few basic requests. No installation or setup is required.

{% hint style="warning" %}
**Authentication is coming soon.** The SaaS API is currently open, but we plan to move to an authenticated API very soon. Once this happens, clients will need to authenticate to use the SaaS. The examples below will then require credentials.
{% endhint %}

## Base URL

All FHIR requests go to:

```
https://tx.health-samurai.io/fhir
```

Check that the server is reachable by requesting its `CapabilityStatement`:

```bash
curl https://tx.health-samurai.io/fhir/metadata
```

## Supported operations

Termbox SaaS supports the standard FHIR terminology operations:

| Operation | Purpose |
| --- | --- |
| [CodeSystem/$lookup](../standard-operations/codesystem-lookup.md) | Get details for a code: display, designations, properties |
| [CodeSystem/$validate-code](../standard-operations/codesystem-validate-code.md) | Check whether a code exists in a code system |
| [CodeSystem/$subsumes](../standard-operations/codesystem-subsumes.md) | Test the hierarchical relationship between two codes |
| [CodeSystem/$find-matches](../standard-operations/codesystem-find-matches.md) | Find codes matching a set of properties |
| [ValueSet/$expand](../standard-operations/valueset-expand.md) | List the codes in a value set, optionally filtered by text |
| [ValueSet/$validate-code](../standard-operations/valueset-validate-code.md) | Check whether a code belongs to a value set |
| [ConceptMap/$translate](../standard-operations/conceptmap-translate.md) | Translate a code from one code system to another |

Each operation can be called with `GET` and query parameters, or with `POST` and a FHIR `Parameters` resource. The examples below use `GET` for brevity.

## Examples

### Look up a SNOMED CT code

```bash
curl "https://tx.health-samurai.io/fhir/CodeSystem/\$lookup?system=http://snomed.info/sct&code=73211009"
```

The response is a `Parameters` resource with the display (`Diabetes mellitus`), the edition and version used, designations and properties.

### Look up an RxNorm code

```bash
curl "https://tx.health-samurai.io/fhir/CodeSystem/\$lookup?system=http://www.nlm.nih.gov/research/umls/rxnorm&code=1049502"
```

### Validate a LOINC code

```bash
curl "https://tx.health-samurai.io/fhir/CodeSystem/\$validate-code?url=http://loinc.org&code=8867-4"
```

```json
{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "result", "valueBoolean": true },
    { "name": "code", "valueCode": "8867-4" },
    { "name": "display", "valueString": "Heart rate" },
    { "name": "system", "valueUri": "http://loinc.org" },
    { "name": "version", "valueString": "2.82" }
  ]
}
```

### Validate an ICD-10-CM code

```bash
curl "https://tx.health-samurai.io/fhir/CodeSystem/\$validate-code?url=http://hl7.org/fhir/sid/icd-10-cm&code=E11.9"
```

### Check subsumption

Is *Type 2 diabetes mellitus* (`44054006`) a kind of *Diabetes mellitus* (`73211009`)?

```bash
curl "https://tx.health-samurai.io/fhir/CodeSystem/\$subsumes?system=http://snomed.info/sct&codeA=73211009&codeB=44054006"
```

```json
{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "outcome", "valueCode": "subsumes" }
  ]
}
```

### Expand a value set

```bash
curl "https://tx.health-samurai.io/fhir/ValueSet/\$expand?url=http://hl7.org/fhir/ValueSet/administrative-gender"
```

### Search within a SNOMED CT hierarchy

Expand an implicit SNOMED CT value set (all descendants of *Diabetes mellitus*) and filter by text, a typical type-ahead use case:

```bash
curl -G "https://tx.health-samurai.io/fhir/ValueSet/\$expand" \
  --data-urlencode "url=http://snomed.info/sct?fhir_vs=isa/73211009" \
  --data-urlencode "filter=type" \
  --data-urlencode "count=5"
```

### Validate a code against a value set

Check that a SNOMED CT code is allowed by the US Core Condition Code value set:

```bash
curl -G "https://tx.health-samurai.io/fhir/ValueSet/\$validate-code" \
  --data-urlencode "url=http://hl7.org/fhir/us/core/ValueSet/us-core-condition-code" \
  --data-urlencode "system=http://snomed.info/sct" \
  --data-urlencode "code=44054006"
```

### Translate a code

```bash
curl -G "https://tx.health-samurai.io/fhir/ConceptMap/\$translate" \
  --data-urlencode "url=http://hl7.org/fhir/ConceptMap/cm-administrative-gender-v2" \
  --data-urlencode "system=http://hl7.org/fhir/administrative-gender" \
  --data-urlencode "code=male"
```

## Selecting an edition or version

When no version is given, Termbox uses the latest available release. To target a specific SNOMED CT edition, pass its module URI as `version` — for example, the US Edition:

```bash
curl -G "https://tx.health-samurai.io/fhir/CodeSystem/\$lookup" \
  --data-urlencode "system=http://snomed.info/sct" \
  --data-urlencode "code=73211009" \
  --data-urlencode "version=http://snomed.info/sct/731000124108"
```

To pin an exact release, use the full version URI, e.g. `http://snomed.info/sct/731000124108/version/20260301`. See [Content](content.md) for the available editions.

## Use it from a FHIR server or validator

Termbox SaaS can be used as the external terminology server of FHIR servers, validators and other tools that support one. Point them at `https://tx.health-samurai.io/fhir`. For example, with the HL7 FHIR Validator:

```bash
java -jar validator_cli.jar my-resource.json -version 4.0.1 -tx https://tx.health-samurai.io/fhir
```
