# Configuration

Termbox is configured through environment variables. When an environment variable changes, Termbox must be restarted for the change to take effect. The tables below list the available variables and their defaults.

<style>
.config-table td:first-child,
.config-table th:first-child { white-space: nowrap; }
</style>

<div class="config-table">

## Termbox server settings

| Env var         | Description                                                       | Default     | Values / Examples                |
| --------------- | ----------------------------------------------------------------- | ----------- | -------------------------------- |
| `LICENSE`       | Termbox license                                                   |             |                                  |
| `LOG_MIN_LEVEL` |                                                                   | `INFO`      | `DEBUG`, `INFO`, `WARN`, `ERROR` |
| `LOG_FORMAT`    | Use json for structured logging                                   | `text`      | `text` \| `json`                 |
| `HTTP_PORT`     | HTTP server port                                                  | `3000`      |                                  |
| `HTTP_MAX_BODY` | Max body size in bytes                                            | `500000000` |                                  |
| `HTTP_BASE_URL` | Base url where termbox will be hosted (useful for outgoing links) | `http://localhost:3000` |                                  |
| `HTTP_THREADS`    | Number of HTTP worker threads                                     | `16`        |                                  |
| `HTTP_QUEUE_SIZE` | Max number of requests queued while all worker threads are busy   | `40960`     |                                  |
| `METRICS`         | Whether HTTP request metrics are collected                        | `true`      | `true` \| `false`                |
| `DATA_CONFIG_FILE` | Path to the `data.yaml` file with terminologies to load on startup. See [Loading Data](loading-data/README.md) |  | `/data/data.yaml` |
| `PACKAGE_REGISTRY` | FHIR package registry URL used to download FHIR packages        | `https://packages2.fhir.org/packages` |               |
| `INGESTION_BATCH_SIZE` | Number of rows inserted per database batch during CodeSystem ingestion | `5000` |                              |
| `INGESTION_ANALYZE_THRESHOLD` | Min number of ingested concepts that triggers a Postgres `ANALYZE` after ingestion | `10000` |                |
| `FHIR_DEFAULT_COUNT`           | Default page size for FHIR search and `ValueSet/$expand` when `_count`/`count` is not specified | `1000`          |                                  |
| `FHIR_MAX_EXCLUDED_COUNT`      | Max number of codes a single `ValueSet.compose.exclude` may resolve to during expansion; larger excludes fail with `too-costly` | `20000` |                    |
| `CACHE_CANONICAL_SIZE`         | Size (in items) of the canonical resource cache                                 | `200000`                             |                                  |
| `FHIR_TOTAL_BEHAVIOR`          | Controls whether FHIR responses include calculated totals by default            | `none`                               | `none` \| `calculate`            |
| `FHIR_READ_ITEMS_LIMIT`        | Max number of concepts (CodeSystem, ValueSet) or mappings (ConceptMap) returned by a FHIR read interaction; larger resources are returned truncated and tagged `SUBSETTED` | `1000` |                                  |
| `FHIR_MULTI_INVOKE_BATCH_SIZE` | Number of patterns processed per database batch in `$x-multi-invoke` operations | `6000`                               |                                  |
| `SNOMED_DEFAULT_EDITION`       | Default edition of SNOMED                                                       | `900000000000207008` (international) | `83821000000107`: UK Edition     |

## Database settings

| Env var                   | Description                                                          | Default     | Values / Examples |
| ------------------------- | -------------------------------------------------------------------- | ----------- | ----------------- |
| `PG_PORT`                 | Postgres db port                                                     | `5432`      |                   |
| `PG_USER`                 | Postgres user                                                        | `postgres`  |                   |
| `PG_PASSWORD`             | Postgres password                                                    | `password`  |                   |
| `PG_HOST`                 | Postgres host                                                        | `localhost` |                   |
| `PG_DATABASE`             | Name of the main database                                            | `termbox`   |                   |
| `PG_POOL_SIZE`            | Connection pool max size                                             | `20`        |                   |
| `PG_MAINTENANCE_WORK_MEM` | Postgres `maintenance_work_mem` used per session for load operations | `128MB`     |                   |
| `PG_WORK_MEM`             | Postgres `work_mem` used per session for load operations             | `128MB`     |                   |
| `PG_MAX_LIFETIME`         | Max lifetime of a pooled connection, in ms                           | `3300000` (55min)   |                   |

## Databricks settings

See [Running Termbox on Managed PostgreSQL](managed-postgresql.md#databricks-lakebase-postgres) for setup instructions.

| Env var                        | Description                                | Default | Values / Examples |
| ------------------------------ | ------------------------------------------ | ------- | ----------------- |
| `PG_DATABRICKS_HOST`           | Databricks workspace URL                   |         |                   |
| `PG_DATABRICKS_PROJECT`        | Lakebase project ID                        |         |                   |
| `PG_DATABRICKS_BRANCH`         | Lakebase branch ID                         |         |                   |
| `PG_DATABRICKS_ENDPOINT`       | Lakebase endpoint ID                       |         |                   |
| `PG_DATABRICKS_CLIENT_ID`      | Databricks service principal client ID     |         |                   |
| `PG_DATABRICKS_CLIENT_SECRET`  | Databricks service principal client secret |         |                   |

## Feature toggles

| Env var                   | Description                                                                                                  | Default | Values / Examples |
| ------------------------- | ------------------------------------------------------------------------------------------------------------ | ------- | ----------------- |
| `CACHE_CANONICAL_ENABLED` | Whether caching of canonical resources is enabled                                                            | `true`  | `true` \| `false` |
| `FUZZY_SEARCH_ENABLED`    | Enables typo-tolerant full text search for `ValueSet/$expand` filters; set to `false` for prefix-only search | `true`  | `true` \| `false` |
| `READONLY_MODE`           | Enables read-only mode: disables Admin API, UI ingest/delete routes, and FHIR write endpoints                | `false` | `true` \| `false` |
| `UI_ENABLED`              | Enables the browser UI under `/ui`                                                                           | `true`  | `true` \| `false` |
| `ADMIN_API_ENABLED`       | Enables the Admin API under `/admin`                                                                         | `true`  | `true` \| `false` |
| `FHIR_API_ENABLED`        | Enables all FHIR API routes under `/fhir`                                                                    | `true`  | `true` \| `false` |
| `FHIR_API_R4_ENABLED`     | Enables FHIR R4 API routes under `/fhir/r4`                                                                  | `true`  | `true` \| `false` |
| `FHIR_API_R4B_ENABLED`    | Enables FHIR R4b API routes under `/fhir/r4b`                                                                | `true`  | `true` \| `false` |
| `FHIR_API_R5_ENABLED`     | Enables FHIR R5 API routes under `/fhir/r5`                                                                  | `true`  | `true` \| `false` |
| `FHIR_API_R6_ENABLED`     | Enables FHIR R6 API routes under `/fhir/r6`                                                                  | `true`  | `true` \| `false` |

</div>
