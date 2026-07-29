# Translating across all known maps

This example uses `ConceptMap/$translate` without `targetSystem`, so Termbox returns matches from every SNOMED CT ConceptMap that covers `2681003 | Peripheral nerve of thigh`.

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
      - "5446:5432"
    volumes:
      - termbox_example_translate_all_known_maps_pgdata:/var/lib/postgresql
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
      - "3346:3000"
    environment:
      PG_USER: postgres
      PG_PASSWORD: postgres
      PG_HOST: postgres
      PG_DATABASE: termbox
      HTTP_BASE_URL: http://localhost:3346
      LICENSE: <your license>
      DATA_CONFIG_FILE: /data/data.yaml
    volumes:
      - ./data.yaml:/data/data.yaml
volumes:
  termbox_example_translate_all_known_maps_pgdata: {}
```

Start the instance and wait until the SNOMED International import is complete:

```sh
docker compose up
```

Load jobs may be monitored via UI: [http://localhost:3346/ui/jobs](http://localhost:3346/ui/jobs).

## Example request

{% tabs %}
{% tab title="GET Request" %}

```http
GET /ConceptMap/$translate?sourceCode=2681003&sourceSystem=http://snomed.info/sct
Accept: application/fhir+json
```
{% endtab %}

{% tab title="POST Request" %}
```http
POST /ConceptMap/$translate
Content-Type: application/fhir+json
Accept: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "sourceCode", "valueCode": "2681003" },
    { "name": "sourceSystem", "valueUri": "http://snomed.info/sct" }
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```http
HTTP/1.1 200 OK
Content-Type: application/fhir+json; charset=utf-8
```

```json
{
  "resourceType": "Parameters",
  "parameter": [
    {
      "name": "result",
      "valueBoolean": true
    },
    {
      "name": "match",
      "part": [
        {
          "name": "concept",
          "valueCoding": {
            "system": "http://terminology.hl7.org/CodeSystem/icd-o-3",
            "code": "C47.5"
          }
        },
        {
          "name": "relationship",
          "valueCode": "equivalent"
        },
        {
          "name": "originMap",
          "valueUri": "http://snomed.info/sct?fhir_cm=446608001|http://snomed.info/sct/900000000000207008/version/20260701"
        }
      ]
    },
    {
      "name": "match",
      "part": [
        {
          "name": "concept",
          "valueCoding": {
            "system": "http://snomed.info/sct",
            "code": "306800005"
          }
        },
        {
          "name": "relationship",
          "valueCode": "related-to"
        },
        {
          "name": "originMap",
          "valueUri": "http://snomed.info/sct?fhir_cm=734139008|http://snomed.info/sct/900000000000207008/version/20260701"
        }
      ]
    },
    {
      "name": "match",
      "part": [
        {
          "name": "concept",
          "valueCoding": {
            "system": "http://read.info/ctv3",
            "code": "XUAec"
          }
        },
        {
          "name": "relationship",
          "valueCode": "equivalent"
        },
        {
          "name": "originMap",
          "valueUri": "http://snomed.info/sct?fhir_cm=900000000000497000|http://snomed.info/sct/900000000000207008/version/20260701"
        }
      ]
    }
  ]
}
```
{% endtab %}
{% endtabs %}

Because no `targetSystem` is supplied, the response may include matches from multiple implicit ConceptMaps.
