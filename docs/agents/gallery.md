# Find Terminologies in the Gallery

**Goal:** find a terminology among the files Health Samurai publishes for Termbox, then write the matching `data.yaml` entry.

The files are in a public Google Cloud Storage bucket, `termbox-public`, in three folders:

- `gallery/` has large terminologies, such as SNOMED CT, LOINC and RxNorm, pre-indexed for fast loading.
- `resources/` has single FHIR resources, such as UCUM, ICD-10-CM, NDC, CVX and ISO country codes, as JSON files.
- `packages/` has FHIR packages that aren't on the public registries, as `.tgz` files.

None of them needs credentials or a running Termbox. Search the gallery first. If the terminology isn't there, search `resources/` and `packages/` in step 4.

## 1. List the gallery

```bash
curl -s 'https://storage.googleapis.com/storage/v1/b/termbox-public/o?prefix=gallery/&fields=items(name,size,updated,metadata),nextPageToken'
```

To list one terminology only, use a longer prefix:

| Terminology | Prefix |
| --- | --- |
| SNOMED CT | `gallery/snomed-info-sct/` |
| LOINC | `gallery/loinc-org/` |
| RxNorm | `gallery/www-nlm-nih-gov-research-umls-rxnorm/` |
| OMOP vocabularies | `gallery/fhir-terminology-ohdsi-org/` |
| NPU | `gallery/npu-terminology-org/` |

## 2. Read the result

Each entry in `items` is one version:

- `metadata.url` and `metadata.version` are the values to use in `data.yaml`. Copy them exactly.
- `size` is the download size in bytes.
- Skip entries with no `metadata`, because they are folder markers.
- If the response has `nextPageToken`, pass it back as `&pageToken=<token>` to get the next page.

Newest version, by terminology:

- **SNOMED CT:** the highest date at the end of the version, within one edition.
- **LOINC:** the highest number, such as `2.82`.
- **RxNorm:** the version format is `MMDDYYYY`, so compare the year first.

SNOMED CT editions, identified by the number in the version:

| Edition | ID |
| --- | --- |
| International | `900000000000207008` |
| International Spanish | `449081005` |
| United States | `731000124108` |
| United Kingdom | `83821000000107` |
| Canada | `20611000087101` |
| Germany | `11000274103` |
| Chile | `21000325107` |
| Denmark | `554471000005108` |
| Finland | `11000229106` |
| Norway | `51000202101` |
| Sweden | `45991000052106` |

## 3. Write the entry

Add it to the `# Gallery` group of `sources`, keeping the file's groups in place:

```yaml
sources:
  # Gallery
  # An exact release: reproducible, and the best choice for shared servers
  - type: gallery
    url: http://snomed.info/sct
    version: http://snomed.info/sct/900000000000207008/version/20261001
  # The newest release of one SNOMED CT edition
  - type: gallery
    url: http://snomed.info/sct
    version: http://snomed.info/sct/731000124108
  # The newest release of a terminology
  - type: gallery
    url: http://loinc.org
```

- Without an exact version, a restart can load a newer release.
- Always give SNOMED CT an edition.
- Always give RxNorm an exact version, because Termbox can otherwise pick an older release.
- Quote versions that look like numbers, such as `"2.82"` or `"06012026"`.

## 4. Search resources and packages

If the gallery doesn't have the terminology, list the other folders:

```bash
curl -s 'https://storage.googleapis.com/storage/v1/b/termbox-public/o?prefix=resources/&fields=items(name,size,metadata),nextPageToken'
curl -s 'https://storage.googleapis.com/storage/v1/b/termbox-public/o?prefix=packages/&fields=items(name,size,metadata),nextPageToken'
```

In `resources/`, `metadata.url` is the canonical URL of the CodeSystem or ValueSet inside, and `metadata.version` is its version. Search by URL, because file names can differ from it. For example, `http://hl7.org/fhir/sid/icd-10-cm` has versions `2025` and `2026`. `metadata.content` is `fragment` when the file has only part of the codes.

Termbox reads these files from disk, so download them into the `data` folder next to `docker-compose.yaml`:

```bash
mkdir -p data/resources
curl -fsSL -o data/resources/icd10cm-2026.json https://storage.googleapis.com/termbox-public/resources/icd10cm-2026.json
```

Then add the entry to the `# Resources` group:

```yaml
sources:
  # Resources
  - type: resource
    location: /data/resources/icd10cm-2026.json
```

For a file from `packages/`, download it to `data/packages/` and add `{type: package, location: /data/packages/<file>.tgz}` to the `# Packages` group.

If it isn't in any of the three folders, search the FHIR package registries with [Find FHIR Packages](packages.md).

## 5. Report to the user

List the matching versions with their sizes, and give the `data.yaml` entry and any download command. To load it, follow [Add Terminologies and Packages](add-content.md). Mention any license terms in the file's `copyright` element.
