# Lookup a code in a code system

This example uses `CodeSystem/$lookup` to resolve the SNOMED CT code `73211009` and return its display and system metadata.

## Setup

Create `data.yaml` with SNOMED International as the loaded source:

```yaml
sources:
  - type: gallery
    url: http://snomed.info/sct
    version: http://snomed.info/sct/900000000000207008
```

Create `docker-compose.yaml`:

```yaml
services:
  postgres:
    image: postgres:18
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: termbox
    ports:
      - "5441:5432"
    volumes:
      - termbox_example_lookup_code_pgdata:/var/lib/postgresql
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 5s
      retries: 5
    mem_limit: 512m
  termbox:
    depends_on:
      postgres:
        condition: service_healthy
    image: healthsamurai/termbox:edge
    pull_policy: always
    ports:
      - "3341:3000"
    environment:
      PG_USER: postgres
      PG_PASSWORD: postgres
      PG_HOST: postgres
      PG_DATABASE: termbox
      HTTP_BASE_URL: http://localhost:3341
      LICENSE: <your license>
      DATA_CONFIG_FILE: /data/data.yaml
    volumes:
      - ./data.yaml:/data/data.yaml
volumes:
  termbox_example_lookup_code_pgdata: {}
```

Start the instance and wait until the SNOMED International import is complete:

```sh
docker compose up
```

Load jobs may be monitored via UI: [http://localhost:3341/ui/jobs](http://localhost:3341/ui/jobs).

## Example request

{% tabs %}
{% tab title="GET Request" %}

```http
GET /CodeSystem/$lookup?system=http://snomed.info/sct&code=73211009
Accept: application/fhir+json
```
{% endtab %}

{% tab title="POST Request" %}
```http
POST /CodeSystem/$lookup
Content-Type: application/fhir+json
Accept: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "system", "valueUri": "http://snomed.info/sct" },
    { "name": "code", "valueCode": "73211009" }
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

```json
{
  "resourceType": "Parameters",
  "parameter": [
    {
      "name": "code",
      "valueCode": "73211009"
    },
    {
      "name": "display",
      "valueString": "Diabetes mellitus"
    },
    {
      "name": "name",
      "valueString": "snomed-ct"
    },
    {
      "name": "system",
      "valueUri": "http://snomed.info/sct"
    },
    {
      "name": "abstract",
      "valueBoolean": false
    },
    {
      "name": "version",
      "valueString": "http://snomed.info/sct/900000000000207008/version/20260701"
    },
    {
      "name": "designation",
      "part": [
        {
          "name": "language",
          "valueCode": "en"
        },
        {
          "name": "use",
          "valueCoding": {
            "system": "http://terminology.hl7.org/CodeSystem/hl7TermMaintInfra",
            "code": "preferredForLanguage",
            "display": "Preferred For Language"
          }
        },
        {
          "name": "value",
          "valueString": "Diabetes mellitus"
        }
      ]
    },
    {
      "name": "designation",
      "part": [
        {
          "name": "language",
          "valueCode": "en"
        },
        {
          "name": "use",
          "valueCoding": {
            "system": "http://snomed.info/sct",
            "code": "900000000000013009",
            "display": "Synonym"
          }
        },
        {
          "name": "value",
          "valueString": "Diabetes mellitus"
        }
      ]
    },
    {
      "name": "designation",
      "part": [
        {
          "name": "language",
          "valueCode": "en"
        },
        {
          "name": "use",
          "valueCoding": {
            "system": "http://snomed.info/sct",
            "code": "900000000000003001",
            "display": "Fully Specified Name"
          }
        },
        {
          "name": "value",
          "valueString": "Diabetes mellitus (disorder)"
        }
      ]
    },
    {
      "name": "designation",
      "part": [
        {
          "name": "language",
          "valueCode": "en"
        },
        {
          "name": "use",
          "valueCoding": {
            "system": "http://snomed.info/sct",
            "code": "900000000000013009",
            "display": "Synonym"
          }
        },
        {
          "name": "value",
          "valueString": "DM - Diabetes mellitus"
        }
      ]
    },
    {
      "name": "property",
      "part": [
        {
          "name": "code",
          "valueCode": "inactive"
        },
        {
          "name": "value",
          "valueBoolean": false
        }
      ]
    }
  ]
}
```
{% endtab %}
{% endtabs %}
