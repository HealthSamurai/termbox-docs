# Troubleshoot Termbox

**Goal:** find out why Termbox isn't working, and fix it.

**Start here:**

```bash
docker compose ps
docker compose logs --tail 200 termbox
curl -s -o /dev/null -w '%{http_code}\n' http://localhost:3000/fhir/metadata
```

## Find the symptom

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| Connection refused | Termbox isn't running, or uses another port | Run `docker compose up -d`, then check `docker compose ps` and the logs |
| `bind: address already in use` | Port 3000 is taken | Change the published port in `docker-compose.yaml` and use it in every URL |
| `403` with `License required` | `LICENSE` is missing or invalid | Copy the `LICENSE` value from [Start Termbox Locally](start-local.md), then run `docker compose up -d` |
| `503` from `/fhir/metadata` | Termbox can't reach PostgreSQL | Check that `postgres` is running and that the `PG_*` values are correct |
| A code or `CodeSystem` is missing right after startup | Content is still loading | Wait, and keep checking. The default content took about 20 minutes in our test |
| Code still missing with no activity in the logs | The source failed or isn't in the gallery | Search the logs as shown below, then check the source with [Find Terminologies in the Gallery](gallery.md) or [Find FHIR Packages](packages.md) |
| Admin job has `status: failed` | The load failed | Read `error` in `/admin/jobs/<job-id>/status`, then fix the source in `data/data.yaml` and reload |
| `404` on `/admin/...` | `ADMIN_API_ENABLED=false` | Change content with a restart, see [Roll Out to Kubernetes](kubernetes.md) |
| `422` with `too-costly` | The expansion is too large | Use a smaller `count`, page with `offset`, or narrow with `filter` |
| YAML error, or a version isn't found | A version was parsed as a number | Quote it, such as `version: "2.82"` |
| Too much disk used, or the disk is full | Each loaded version takes space | Remove sources you don't need with `sync: true` in `data/data.yaml`, then reload |

## Search the logs for failures

```bash
docker compose logs termbox | grep -i -E 'not found|unable|error'
```

## Check what is loaded

```bash
curl -s -G 'http://localhost:3000/fhir/CodeSystem' --data-urlencode 'url=http://loinc.org' --data-urlencode '_elements=url,version,content'
```

`content: not-present` is a definition with no codes, which FHIR packages include for external terminologies. The codes come from a different source, such as the gallery.

## Check a running job

`POST /admin/loader/reload` returns a job ID:

```bash
curl -s http://localhost:3000/admin/jobs/<job-id>/status
```

## Start over

This deletes all loaded content. Ask the user first:

```bash
docker compose down -v
docker compose up -d
```

## Report to the user

Give the symptom, the cause, what you changed, and the result of the final check. If it's still failing, include the relevant log lines and the image version from `docker compose images`.
