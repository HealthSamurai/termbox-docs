# Loading Data

Termbox does not ship with terminology content by default. Terminologies must be imported from external sources before they can be queried through the FHIR API.

Content can be loaded into Termbox through three main mechanisms:

- **User interface** – For a guided interactive loading
- **Admin API** – For scripted imports and automation
- **Syndication** – For automatic synchronization based on configuration

These mechanisms allow Termbox to retrieve data from a variety of source types:

| Source Type           | Description                                                                                                                                                | Current Status                          |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- |
| FHIR Packages         | Standard FHIR Packages based on NPM. See https://hl7.org/fhir/packages.html                                                                                | ✅ Currently supported via manual upload |
| FHIR Package registry | Ability to download packages (and dependencies) from registries such as: packages2, Simplifier, get-ig.org                                                 | ⚠️ Not yet supported.                   |
| Syndication Feeds     | As described in the [NCTS Syndication feed spec](https://www.healthterminologies.gov.au/specs/v3/conformant-server-apps/syndication-api/syndication-feed/) | ⚠️ Not yet supported.                   |
| Termbox Binaries      | Pre-indexed terminologies prepared for fast loading                                                                                                        | ✅ Currently supported via manual upload |
| Termbox Catalog       | Curated repository of terminologies                                                                                                                        | ⚠️ Not yet supported.                   |
| FHIR CRUD API         | Live authoring FHIR resources via API                                                                                                                      | ⚠️ Not yet supported.                   |

## Loading data via UI

Let's see an example of loading a FHIR Package into Termbox.

First, download THO from packages2:

1. Go to https://packages2.fhir.org/packages
2. Search for `hl7.terminology`
3. Click on the `hl7.terminology` link (currently version `7.0.1`)
4. It should download a file named `hl7.terminology-7.0.1.tgz`

Now, in Termbox UI, navigate to `http://localhost:3000/ui/content`, there should be an `Ingest` button. After clicking it, a modal should appear:

![UI Ingest](../assets/ui-ingest.avif)

Select the "FHIR Package file" option, and upload the file downloaded in the previous step. An _ingest_ job should start running and, after a short time, it should finalize and show the package loaded.

## Loading a pre-indexed binary file

Next, let's load SNOMED International using a pre-indexed binary file[^2].

First, download the binary file from https://storage.googleapis.com/termbox-public/snomed_int_20260201.bin

Next, follow the same steps as before but this time select the _Binary file_ option. After a brief period of time, SNOMED International should be loaded, indexed, and ready to be queried.

## Loading data via the Admin API

Let's load RxNorm via API. First, download the file from https://storage.googleapis.com/termbox-public/rxnorm-full-03022026.bin

Next, run this command:

```bash
curl -X POST "http://localhost:3000/admin/ingest" \  
-F "type=bin" \  
-F "file=@rxnorm-full-03022026.bin"
```

This should start a job and return a job-id and a status endpoint for the job, e.g.:

```json
{
  "job":"b5bd21a8-120c-4889-af87-42a618f32d21",
  "status":"http://localhost:3000/admin/ingest/b5bd21a8-120c-4889-af87-42a618f32d21/status"
}
```

And the status endpoint can be monitored for progress and completion:

```bash
curl "http://localhost:3000/admin/ingest/b5bd21a8-120c-4889-af87-42a618f32d21/status"
```

```json
{
  "key":"jobs.ingest/binary",
  "status":"started",
  "start-time":129350235595002,
  "progress":40
}
```

## Available pre-indexed files

In upcoming releases these files will be available via an online registry and we'll release a standalone cli[^3] to transform terminologies to FHIR and to index them. The current list of files is being published to an S3 bucket publicly available here: https://storage.googleapis.com/termbox-public.
