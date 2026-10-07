# SLA

This page describes the service levels Health Samurai provides for Termbox SaaS.

## Availability

| Metric | Commitment |
| --- | --- |
| Monthly uptime | 99.9% |
| Planned maintenance | Announced at least 48 hours in advance, scheduled outside peak hours |

Uptime is measured as the percentage of time the FHIR API at `https://tx.health-samurai.io/fhir` responds successfully to requests, excluding announced maintenance windows.

## Performance

Termbox is built for high-throughput terminology workloads. Typical response times for single-code operations such as `$lookup` and `$validate-code` are in the low milliseconds. Large `$expand` requests depend on the size of the value set and the requested page size. See [Performance](../performance.md) for benchmarks.

## Content freshness

| Terminology | New release available in the SaaS |
| --- | --- |
| SNOMED CT (all editions) | Within 7 days of official publication |
| LOINC | Within 7 days of official publication |
| RxNorm | Within 7 days of official publication |
| ICD-10-CM and other ICD code systems | Before the official effective date |
| HL7 FHIR and implementation guide packages | Within 7 days of publication |

Previous releases remain available so that clients pinning a specific version are not affected by updates.

## Support

| Severity | Description | First response |
| --- | --- | --- |
| Critical | Service unavailable or returning errors for most requests | 1 hour |
| High | Major functionality degraded | 4 business hours |
| Normal | Questions, content requests, minor issues | 1 business day |

Report issues to Health Samurai support through your usual support channel.
