# Running Termbox on Managed PostgreSQL

Termbox can connect to any PostgreSQL-compatible database, both self-hosted and managed. Database connections are configured with the [`PG_*`](configuration.md#pg) environment variables. Connecting to a typical PostgreSQL instance may require the following setup:

```shell
PG_HOST=<host>
PG_PORT=5432
PG_USER=termbox
PG_PASSWORD=termboxpass
PG_DATABASE=termbox
```

Some managed PostgreSQL instances require special authentication methods.

## Databricks Lakebase Postgres

[Lakebase](https://docs.databricks.com/aws/en/oltp/) is Databricks' managed, autoscaling Postgres offering, organized into projects, branches, and endpoints. Instead of a static password, Termbox authenticates to Lakebase as a [service principal](https://docs.databricks.com/aws/en/admin/users-groups/service-principals) using short-lived OAuth-issued database credentials.

{% hint style="info" %}
Termbox currently supports Lakebase **Autoscaling** projects (`project` / `branch` / `endpoint`). Since March 2026, new Lakebase instances are created as **Autoscaling**, and existing [*Provisioned*](https://docs.databricks.com/aws/en/oltp/instances/) instances are being upgraded to **Autoscaling** starting June 2026 — for that reason, **Provisioned** instances are not supported in Termbox.
{% endhint %}

When [`PG_DATABRICKS_HOST`, `PG_DATABRICKS_CLIENT_ID`, and `PG_DATABRICKS_CLIENT_SECRET`](configuration.md#pg_databricks) are set, Termbox resolves database credentials dynamically instead of using `PG_USER` / `PG_PASSWORD`. The resolved credential is cached and reused for new pooled connections until `PG_MAX_LIFETIME` elapses (default `3300000` ms / 55 minutes), at which point it's transparently refreshed. `PG_MAX_LIFETIME` should be kept below Databricks' token expiry (1 hour).

{% stepper %}
{% step %}
**Create a Lakebase project, branch, and endpoint**

In the Databricks workspace, set up a Lakebase Postgres project with a branch and endpoint. Note their IDs — you'll need them as `PG_DATABRICKS_PROJECT`, `PG_DATABRICKS_BRANCH`, and `PG_DATABRICKS_ENDPOINT`. See [Connect an external application](https://docs.databricks.com/aws/en/oltp/projects/external-apps-connect#3-get-connection-details) for how to find these, along with the Postgres host, port, and database name.
{% endstep %}
{% step %}
**Create a service principal**

Create a [service principal](https://docs.databricks.com/aws/en/admin/users-groups/service-principals) with an OAuth secret. Note its client ID and client secret. And register the service principal as a Postgres role.
{% endstep %}
{% step %}
**Configure termbox**

```shell
PG_HOST=<instance>.database.cloud.databricks.com
PG_PORT=5432
PG_DATABASE=databricks_postgres
PG_DATABRICKS_HOST=https://<workspace>.cloud.databricks.com
PG_DATABRICKS_PROJECT=termbox
PG_DATABRICKS_BRANCH=production
PG_DATABRICKS_ENDPOINT=primary
PG_DATABRICKS_CLIENT_ID=<service-principal-client-id>
PG_DATABRICKS_CLIENT_SECRET=<service-principal-client-secret>
```
{% endstep %}
{% endstepper %}



{% hint style="info" %}
`PG_USER` and `PG_PASSWORD` are ignored once the `PG_DATABRICKS_*` variables above are set — termbox falls back to them only when Databricks credentials aren't configured.
{% endhint %}
