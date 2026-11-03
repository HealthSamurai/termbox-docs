# Find FHIR Packages

**Goal:** find the FHIR package the user needs, such as an implementation guide or a terminology package, and write the matching `data.yaml` entry.

The three public FHIR package registries need no credentials:

| Registry | Search |
| --- | --- |
| `packages2.fhir.org` | `https://packages2.fhir.org/packages/catalog?name=<text>` |
| `packages.fhir.org` (Simplifier) | `https://packages.fhir.org/catalog?name=<text>` |
| `get-ig.org` | `https://get-ig.org/-/v1/search?text=<text>` |

Termbox downloads `type: npm` sources from `packages2.fhir.org`. Packages found only in the other two are downloaded as local files in step 4.

## 1. Search

Search all three with a word from the package name, such as `us.core`, `ips` or `de.basisprofil`:

```bash
curl -s 'https://packages2.fhir.org/packages/catalog?name=ips&fhirversion=R4'
curl -s 'https://packages.fhir.org/catalog?name=ips&fhirversion=R4'
curl -s 'https://get-ig.org/-/v1/search?text=ips'
```

To search `packages2.fhir.org` by the guide's canonical URL instead, use `catalog?canonical=http://hl7.org/fhir/uv/ips`. Prefer packages that match the server's FHIR version, R4 by default. Packages often have one name per FHIR version, such as `.r4` and `.r5`.

## 2. Pick the version

```bash
curl -s https://packages2.fhir.org/packages/hl7.fhir.uv.ips
```

`dist-tags.latest` is the newest version, and `versions` lists all of them. Use the version the user asks for, or the latest. Avoid `-ballot`, `-draft` and `-snapshot` versions unless the user wants them.

## 3. If packages2 has it, add an npm entry

If the package and version appear in step 2, add the entry to the `# NPM registry` group. Termbox downloads it and its dependencies:

```yaml
  # NPM registry
  - type: npm
    package: hl7.fhir.uv.ips
    version: 2.0.1
```

To skip some dependencies, add `dependencies.exclude`, as [Add Terminologies and Packages](add-content.md) shows.

## 4. If packages2 doesn't have it, add a local package

Download the package into the `data/packages` folder next to `docker-compose.yaml`. Use the registry where you found it:

```bash
mkdir -p data/packages
# packages.fhir.org
curl -fsSL -o data/packages/<name>-<version>.tgz https://packages.simplifier.net/<name>/<version>
# get-ig.org: the file is at versions.<version>.dist.tarball
curl -s -H 'Accept: application/json' https://get-ig.org/<name>
curl -fsSL -o data/packages/<name>-<version>.tgz <tarball url>
```

Add it to the `# Packages` group:

```yaml
  # Packages
  - type: package
    location: /data/packages/<name>-<version>.tgz
```

Termbox doesn't load the dependencies of local packages. List them:

```bash
tar -xzOf data/packages/<name>-<version>.tgz package/package.json
```

Add each package in `dependencies` that isn't loaded yet. Use an npm entry if packages2 has it, or repeat this step if it doesn't.

## 5. Report to the user

Give the package, version, registry and `data.yaml` entry, and any dependencies you added. To load it, follow [Add Terminologies and Packages](add-content.md).
