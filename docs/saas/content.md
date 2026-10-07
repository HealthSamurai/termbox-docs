# Content

Termbox SaaS comes preloaded with the terminologies most commonly needed in clinical, administrative and research FHIR workloads. You don't need to download, license-check or load anything, the content is ready to use.

## Always up to date

We track the official release cycles of every major terminology and load new releases as soon as they are published. When a new release is available, it becomes the default version returned by the API, while previous releases stay available for clients that pin a specific version.

| Terminology | Publisher release cycle |
| --- | --- |
| SNOMED CT International Edition | Monthly |
| SNOMED CT US Edition | March and September |
| SNOMED CT UK Edition | Several times a year |
| SNOMED CT Canadian Edition | Twice a year |
| SNOMED CT German Edition | Twice a year |
| LOINC | February and August |
| RxNorm | Monthly |
| ICD-10-CM | Yearly (October) |

{% hint style="info" %}
To see exactly which versions are loaded at any time, search the `CodeSystem` resources by URL, for example:

`GET https://tx.health-samurai.io/fhir/CodeSystem?url=http://loinc.org&_elements=url,version,title`
{% endhint %}

## Key terminologies

### SNOMED CT

Termbox SaaS includes several editions:

| Edition | Module URI |
| --- | --- |
| International Edition | `http://snomed.info/sct/900000000000207008` |
| US Edition | `http://snomed.info/sct/731000124108` |
| UK Edition | `http://snomed.info/sct/83821000000107` |
| Canadian Edition | `http://snomed.info/sct/20611000087101` |
| German Edition | `http://snomed.info/sct/11000274103` |

For the International and US editions, several historical releases are also available. Implicit value sets (`?fhir_vs=isa/...`, `?fhir_vs=ecl/...`, etc.), subsumption and a subset of ECL are supported. See the [SNOMED guide](../guides/snomed.md) and [ECL support](../capabilities.md#snomed-ct-ecl) for details.

### LOINC

The full LOINC release is loaded, along with the German (de-DE) linguistic variant.

### RxNorm

The full monthly release is loaded. See the [RxNorm guide](../guides/rxnorm.md).

### ICD family

| Code system | Canonical URL | Versions |
| --- | --- | --- |
| ICD-10-CM (US diagnoses) | `http://hl7.org/fhir/sid/icd-10-cm` | 2025, 2026 |
| ICD-10-PCS (US procedures) | `http://www.cms.gov/Medicare/Coding/ICD10` | 2.0.1 |
| ICD-10 (WHO) | `http://hl7.org/fhir/sid/icd-10` | 2019 |
| ICD-10-GM (Germany) | `http://fhir.de/CodeSystem/bfarm/icd-10-gm` | 2026 |
| ICD-9-CM | `http://hl7.org/fhir/sid/icd-9-cm` | 2024 |
| ICD-O-3 (oncology) | `http://terminology.hl7.org/CodeSystem/icd-o-3` | 3.2 |

## Other code systems

### US clinical and administrative

- **CPT** (fragment — the full CPT set is licensed by the AMA)
- **HCPCS Level II**
- **NDC** — National Drug Codes
- **CVX** — CDC vaccine codes
- **CMS HIPPS** — Health Insurance Prospective Payment System
- **NUCC Provider Taxonomy**
- **CDC Race and Ethnicity**
- **USPS** state codes

### Germany

Official terminologies published by BfArM, gematik, IFA and KBV:

- **OPS** — procedure classification
- **ATC-GM** — anatomical therapeutic chemical classification
- **ICF** — International Classification of Functioning, Disability and Health
- **Orphacodes** — rare diseases
- **gematik HDDT**, **IFA medication** and **KBV allergy** terminologies

### International and general purpose

- **UCUM** — units of measure
- **ISO 3166** (countries and subdivisions) and **ISO 4217** (currencies)
- **IETF BCP 47** (languages) and **BCP 13** (media types)
- **ISO/IEEE 11073 MDC** — medical device nomenclature
- **HGNC** — gene names and gene groups
- **Sequence Ontology**
- **Australian Immunisation Register** vaccine codes

## FHIR and implementation guide content

All code systems, value sets and concept maps from the following packages are loaded, so value sets referenced by these specifications can be expanded and validated directly:

| Package | Description |
| --- | --- |
| `hl7.fhir.r4.core` 4.0.1 | FHIR R4 |
| `hl7.fhir.r4b.core` 4.3.0 | FHIR R4B |
| `hl7.terminology.r4` | HL7 Terminology (THO) |
| `hl7.fhir.us.core` 9.0.0 | US Core |
| `hl7.fhir.us.davinci-pdex` | Da Vinci Payer Data Exchange |
| `hl7.fhir.us.vrdr` | Vital Records Death Reporting |
| `hl7.fhir.uv.ips` | International Patient Summary |
| `hl7.fhir.uv.genomics-reporting.r4` | Genomics Reporting |
| `ihe.iti.balp` | IHE Basic Audit Log Patterns |
| `hl7.fhir.cl.clcore` | Chile Core |

## Need something else?

If you need a terminology, edition or implementation guide that is not listed here, contact us. We regularly add content at customer request.
