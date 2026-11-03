# Start Termbox Locally

**Goal:** run Termbox, a FHIR terminology server, on this machine with SNOMED CT International, FHIR R4 core, HL7 Terminology (THO), LOINC, RxNorm and UCUM loaded.

**You need:** Docker with Docker Compose, `curl`, internet access, about 15 GB of free disk, and port 3000 free. Loading took about 20 minutes in our test. If port 3000 is taken, change the published port below and use it in every URL.

## 1. Create the files

Create a folder and add the UCUM file:

```bash
mkdir -p termbox-local/data/resources
cd termbox-local
curl -fsSL -o data/resources/UCUM.json https://storage.googleapis.com/termbox-public/resources/UCUM.json
```

Create `docker-compose.yaml`:

```yaml
name: termbox-local
services:
  postgres:
    image: postgres:18
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: termbox
    volumes:
      - postgres_data:/var/lib/postgresql
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 5s
      retries: 5
  termbox:
    image: healthsamurai/termbox:latest
    pull_policy: always
    depends_on:
      postgres:
        condition: service_healthy
    ports:
      - "127.0.0.1:3000:3000"
    environment:
      PG_HOST: postgres
      PG_USER: postgres
      PG_PASSWORD: postgres
      PG_DATABASE: termbox
      DATA_CONFIG_FILE: /data/data.yaml
      LICENSE: eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJvZmZsaW5lIjpmYWxzZSwiY3JlYXRvciI6eyJpZCI6IjMxNTU4MWUzLWYxMGYtNGU5ZS05NWJiLWExYmY1ZWM1ZmM3OCIsInJlc291cmNlVHlwZSI6IlVzZXIifSwia2V5Ijoib25saW5lLTIwMjYwNDEzLTE0MzY0NyIsInN1YiI6IjMxNTU4MWUzLWYxMGYtNGU5ZS05NWJiLWExYmY1ZWM1ZmM3OCIsImlzcyI6Imh0dHBzOi8vYWlkYm94LmFwcCIsIm5hbWUiOiJUZXJtYm94IElBIEFnZW50cyIsImV4cGlyYXRpb24iOiIyMTI1LTEwLTA5VDE2OjQyOjI3LjAyMVoiLCJraWQiOiJvbmxpbmUtMjAyNjA0MTMtMTQzNjQ3IiwiZXhwIjo0OTE1NzAxNzQ3LCJ0eXBlIjoiZGV2ZWxvcG1lbnQiLCJjcmVhdGVkIjoiMjAyNi0xMS0wMlQxNjo0MjoyNy4wMjFaIiwibWF4LWluc3RhbmNlcyI6MiwibWF4LWRiLXNpemUiOjUzNjg3MDkxMjAsInByb2R1Y3QiOiJ0ZXJtYm94IiwicHJvamVjdCI6eyJpZCI6IjY5ZWFlNTAyLWI0ZmQtNDlhOC05NzQ2LWEyZGE5MTY3YTExMCIsInJlc291cmNlVHlwZSI6IlByb2plY3QifSwic3RhdHVzIjoiYWN0aXZlIiwiaWQiOiI4Y2NlODU0NS04MmE0LTRiOTItYjE5OS0xNjQ4MjBiODQ2MDAiLCJpbmZvIjp7ImdvYWwiOiJldmFsdWF0aW9uIiwiaG9zdGluZyI6InNlbGYtaG9zdGVkIn0sImlzc3VlciI6Imh0dHBzOi8vYWlkYm94LmFwcCIsImlhdCI6MTc5MzYzNzc0N30.pPTzyAhLJeBryKVVmG4XIhaVQ7IAOLQgHIfv-jy82h8StIUowUo081BmXxBNc9ux4kgDFjKO_jqtj2EHxfUIuJ0PHTbM3Fb19MWcnl7Bespt_4xfzH-y3Obx5ZL062eO9ssBelyO0gr4dXyKwUXP6qXCnMtXU8ctAfR9P1h3Zp7UacKRKLKuS-MsInc9EkT8DxmeC5b_xvVUWfMHUf6Jumf26Afm36JOq6CrXwSzOab-qwdXw08FCYwXWZDQm8JQ3gSPBP6oPfEpPB5RtJViWU6VccTYVAejfEP6L4w4hF1JrzPeV93IWm9O8-HoaRwCVAjBpxObralg911aZDXVUg
    volumes:
      - ./data:/data
volumes:
  postgres_data: {}
```

This is a shared development license. It needs no sign-up and is for development and evaluation only. Production use needs a [production license](../licensing.md).

Create `data/data.yaml`:

```yaml
sync: true
sources:
  # Gallery
  - type: gallery
    url: http://snomed.info/sct
    version: http://snomed.info/sct/900000000000207008
  - type: gallery
    url: http://loinc.org
  - type: gallery
    url: http://www.nlm.nih.gov/research/umls/rxnorm
    version: "06012026"

  # Resources
  - type: resource
    location: /data/resources/UCUM.json

  # NPM registry
  - type: npm
    package: hl7.fhir.r4.core
    version: 4.0.1
  - type: npm
    package: hl7.terminology.r4
  - type: npm
    package: hl7.fhir.uv.extensions.r4
  - type: npm
    package: fhir.tx.support.r4
  - type: npm
    package: hl7.fhir.uv.ips

  # Packages
  # (none yet: local .tgz files in /data/packages/ go here)
```

Keep the four groups in this order whenever you edit the file, and add each new source to the end of its group.

## 2. Start Termbox

```bash
docker compose up -d
```

Loading continues in the background after the server starts.

## 3. Wait until the content is loaded

The server answers before loading finishes, so check the content itself. Run this script. It tests one code from each terminology and exits once all of them pass:

```bash
BASE=http://localhost:3000/fhir
check() { curl -s -G "$BASE/CodeSystem/\$validate-code" --data-urlencode "url=$1" --data-urlencode "code=$2" | grep -q '"valueBoolean":true'; }
for i in $(seq 1 240); do
  missing=""
  check http://snomed.info/sct 73211009 || missing="$missing SNOMED"
  check http://loinc.org 8867-4 || missing="$missing LOINC"
  check http://www.nlm.nih.gov/research/umls/rxnorm 161 || missing="$missing RxNorm"
  check http://unitsofmeasure.org mg || missing="$missing UCUM"
  check http://hl7.org/fhir/administrative-gender male || missing="$missing FHIR-core"
  check http://terminology.hl7.org/CodeSystem/v3-ActCode AMB || missing="$missing THO"
  [ -z "$missing" ] && echo "All terminologies loaded" && break
  echo "Still loading:$missing"
  sleep 15
done
```

It gives up after one hour. If it stops without printing `All terminologies loaded`, read the logs with `docker compose logs termbox` and follow [Troubleshoot Termbox](troubleshoot.md).

## 4. Report to the user

Tell the user:

- The FHIR API is at `http://localhost:3000/fhir` and the browser UI is at `http://localhost:3000/ui`.
- UCUM is a fragment of 1,346 common codes. Other valid UCUM expressions may not validate.
- `/admin` has no authentication, so keep port 3000 bound to `127.0.0.1`.
- SNOMED CT, LOINC and RxNorm have their own license terms, and the user is responsible for following them.
- To stop Termbox and keep the data, run `docker compose down`. To delete the data as well, run `docker compose down -v`.
