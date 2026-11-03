# Update Terminology Versions

**Goal:** move a terminology in a running Termbox to another version, with no restart and no downtime.

**You need:** the Termbox folder with `data/data.yaml`, and the base URL, which is `http://localhost:3000` by default. If `/admin` returns `404` because `ADMIN_API_ENABLED=false`, follow [Roll Out to Kubernetes](kubernetes.md) instead.

The old version keeps serving requests while the new one loads. With `sync: true`, it's removed only after that.

## 1. Find the current and new versions

Get the version in use now:

```bash
curl -s -G 'http://localhost:3000/fhir/CodeSystem/$lookup' --data-urlencode 'system=http://www.nlm.nih.gov/research/umls/rxnorm' --data-urlencode 'code=161'
```

Read `version` in the response. For another terminology, use its system and any code it has. To find the available versions, follow [Find Terminologies in the Gallery](gallery.md).

## 2. Pin the new version in `data/data.yaml`

Use the exact version from the gallery. Pinned versions keep updates reproducible and let `sync` remove the old release.

```yaml
sync: true
sources:
  # Gallery
  - type: gallery
    url: http://snomed.info/sct
    version: http://snomed.info/sct/900000000000207008/version/20261001
  - type: gallery
    url: http://www.nlm.nih.gov/research/umls/rxnorm
    version: "06012026"
```

To keep both versions, list both entries. Clients can request either one with the `version` parameter. Check which one is the default with the lookup in step 1, because for RxNorm Termbox can choose the older release.

## 3. Reload and wait

```bash
curl -s -X POST http://localhost:3000/admin/loader/reload
```

The response has a `status` URL. Poll it every 15 seconds until `status` is `finished` or `failed`:

```bash
curl -s http://localhost:3000/admin/jobs/<job-id>/status
```

## 4. Verify

Repeat the lookup from step 1 and confirm that `version` is the new one. Then list what remains loaded, ignoring entries with `content: not-present`, which are definitions with no codes:

```bash
curl -s -G 'http://localhost:3000/fhir/CodeSystem' --data-urlencode 'url=http://www.nlm.nih.gov/research/umls/rxnorm' --data-urlencode '_elements=url,version,content'
```

A failed or missing version doesn't affect the old one, which stays in use. For diagnosis, see [Troubleshoot Termbox](troubleshoot.md). To roll back, restore the previous version in `data/data.yaml` and reload.

## 5. Report to the user

Give the old and new version of each terminology, and say whether the old release was removed.
