# Configuration

This section documents all of the environment variables that are currently available

| Env var                    | Description                                                       | Default                              | Values / Examples                |
| -------------------------- | ----------------------------------------------------------------- | ------------------------------------ | -------------------------------- |
| `LOG_MIN_LEVEL`            |                                                                   | `INFO`                               | `DEBUG`, `INFO`, `WARN`, `ERROR` |
| `LOG_FORMAT`               | Use json for structured logging                                   | `text`                               | `text` \| `json`                 |
| `PG_PORT`                  | Postgres db port                                                  | `5432`                               |                                  |
| `PG_USER`                  | Postgres user                                                     | `postgres`                           |                                  |
| `PG_PASSWORD`              | Postgres password                                                 |                                      |                                  |
| `PG_HOST`                  | Postgres host                                                     | `localhost`                          |                                  |
| `PG_DATABASE`              | Name of the main database                                         | `termbox`                            |                                  |
| `PG_POOL_SIZE`             | Connection pool max size                                          | `20`                                 |                                  |
| `HTTP_PORT`                | HTTP server port                                                  | `3000`                               |                                  |
| `HTTP_MAX_BODY`            | Max body size in bytes                                            | `500000000`                          |                                  |
| `HTTP_BASE_URL`            | Base url where termbox will be hosted (useful for outgoing links) |                                      |                                  |
| `CACHE_CANONICAL_ENABLED`  | Whether caching of canonical resources is enabled                 | `true`                               | `true` \| `false`                |
| `CACHE_CANONICAL_SIZE`     | Size (in items) of the canonical resource cache                   | `200000`                             |                                  |
| `SNOMEDCT_DEFAULT_EDITION` | Default edition of SNOMED                                         | `900000000000207008` (international) | `83821000000107`: UK Edition     |
