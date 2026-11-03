# Add Terminologies and Packages

**Goal:** load more content into a running Termbox and confirm that it arrived.

**You need:** the Termbox folder with `data/data.yaml`, and the base URL, which is `http://localhost:3000` by default. To create them, follow [Start Termbox Locally](start-local.md). The `./data` folder appears inside the container as `/data`.

## 1. Add the source to `data/data.yaml`

Add each entry to the end of its group in `sources`, and keep the existing entries. The groups always come in this order, each with a comment header: `# Gallery` (`type: gallery`), `# Resources` (`type: resource` and `type: bundle`), `# NPM registry` (`type: npm`) and `# Packages` (`type: package`). If a group is missing, add it in its place. If the file has `sync: true`, content you remove from the file gets deleted.

| Content | Entry |
| --- | --- |
| Gallery terminology, see [Find Terminologies in the Gallery](gallery.md) | `{type: gallery, url: http://loinc.org}` |
| FHIR package from the registry, with its dependencies | `{type: npm, package: hl7.fhir.us.core, version: 9.0.0}` |
| Local FHIR package, without its dependencies | `{type: package, location: /data/packages/my-package-1.0.0.tgz}` |
| One CodeSystem, ValueSet or ConceptMap in a JSON file | `{type: resource, location: /data/resources/my-codesystem.json}` |
| FHIR Bundle in a JSON file | `{type: bundle, location: /data/bundle.json}` |

To find a package in `packages2.fhir.org`, `packages.fhir.org` or `get-ig.org`, follow [Find FHIR Packages](packages.md).

To skip part of a package's dependency tree:

```yaml
  - type: npm
    package: hl7.fhir.us.core
    version: 9.0.0
    dependencies:
      exclude:
        - package: us.nlm.vsac
```

Health Samurai also publishes single resources, such as ICD-10-CM, NDC, CVX and ISO country codes, and some packages. To find one and download it into `data/`, follow [Find Terminologies in the Gallery](gallery.md).

## 2. Reload

```bash
curl -s -X POST http://localhost:3000/admin/loader/reload
```

The response has a `status` URL. Poll it every 15 seconds until `status` is `finished` or `failed`. If a `failed` response has an `error`, report it.

```bash
curl -s http://localhost:3000/admin/jobs/<job-id>/status
```

## 3. Verify the content

A `finished` job doesn't prove the content loaded, because a source missing from the gallery only logs a warning. Check the content itself:

```bash
curl -s -G 'http://localhost:3000/fhir/CodeSystem' --data-urlencode 'url=<code system url>' --data-urlencode '_elements=url,version,content'
```

The source you added should have `content: complete` or `fragment`. `not-present` is a definition with no codes. For a ValueSet, use `ValueSet` in the path. If nothing comes back, run `docker compose logs termbox | grep -i 'not found'`, then see [Troubleshoot Termbox](troubleshoot.md).

## 4. Report to the user

List each source you added, its loaded version, and any that failed. Mention the source's own license terms if it has any.
