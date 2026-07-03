# Configuration

This section documents all of the environment variables that are currently available

<style>
.config-table td:first-child,
.config-table th:first-child { white-space: nowrap; }
</style>

<div class="config-table">

| Env var                    | Description                                                                                   | Default                              | Values / Examples                |
| -------------------------- | --------------------------------------------------------------------------------------------- | ------------------------------------ | -------------------------------- |
| `LICENSE`                  | Termbox license                                                                               |                                      |                                  |
| `LOG_MIN_LEVEL`            |                                                                                               | `INFO`                               | `DEBUG`, `INFO`, `WARN`, `ERROR` |
| `LOG_FORMAT`               | Use json for structured logging                                                               | `text`                               | `text` \| `json`                 |
| `PG_PORT`                  | Postgres db port                                                                              | `5432`                               |                                  |
| `PG_USER`                  | Postgres user                                                                                 | `postgres`                           |                                  |
| `PG_PASSWORD`              | Postgres password                                                                             |                                      |                                  |
| `PG_HOST`                  | Postgres host                                                                                 | `localhost`                          |                                  |
| `PG_DATABASE`              | Name of the main database                                                                     | `termbox`                            |                                  |
| `PG_POOL_SIZE`             | Connection pool max size                                                                      | `20`                                 |                                  |
| `PG_MAINTENANCE_WORK_MEM`  | Postgres `maintenance_work_mem` used per session for load operations                          | `128MB`                              |                                  |
| `PG_WORK_MEM`              | Postgres `work_mem` used per session for load operations                                      | `128MB`                              |                                  |
| `HTTP_PORT`                | HTTP server port                                                                              | `3000`                               |                                  |
| `HTTP_MAX_BODY`            | Max body size in bytes                                                                        | `500000000`                          |                                  |
| `HTTP_BASE_URL`            | Base url where termbox will be hosted (useful for outgoing links)                             |                                      |                                  |
| `CACHE_CANONICAL_ENABLED`  | Whether caching of canonical resources is enabled                                             | `true`                               | `true` \| `false`                |
| `CACHE_CANONICAL_SIZE`     | Size (in items) of the canonical resource cache                                               | `200000`                             |                                  |
| `FHIR_TOTAL_BEHAVIOR`      | Controls whether FHIR responses include calculated totals by default                          | `none`                               | `none` \| `calculate`            |
| `FHIR_MULTI_INVOKE_BATCH_SIZE` | Number of patterns processed per database batch in `$x-multi-invoke` operations           | `6000`                               |                                  |
| `SNOMEDCT_DEFAULT_EDITION` | Default edition of SNOMED                                                                     | `900000000000207008` (international) | `83821000000107`: UK Edition     |
| `READONLY_MODE`            | Enables read-only mode: disables Admin API, UI ingest/delete routes, and FHIR write endpoints | `false`                              | `true` \| `false`                |
| `UI_ENABLED`               | Enables the browser UI under `/ui`                                                            | `true`                               | `true` \| `false`                |
| `ADMIN_API_ENABLED`        | Enables the Admin API under `/admin`                                                          | `true`                               | `true` \| `false`                |
| `FHIR_API_ENABLED`         | Enables all FHIR API routes under `/fhir`                                                     | `true`                               | `true` \| `false`                |
| `FHIR_API_R4_ENABLED`      | Enables FHIR R4 API routes under `/fhir/r4`                                                   | `true`                               | `true` \| `false`                |
| `FHIR_API_R4B_ENABLED`     | Enables FHIR R4b API routes under `/fhir/r4b`                                                 | `true`                               | `true` \| `false`                |
| `FHIR_API_R5_ENABLED`      | Enables FHIR R5 API routes under `/fhir/r5`                                                   | `true`                               | `true` \| `false`                |
| `FHIR_API_R6_ENABLED`      | Enables FHIR R6 API routes under `/fhir/r6`                                                   | `true`                               | `true` \| `false`                |

</div>
