# Translating a SNOMED code to ICD-O-3

This example uses `ConceptMap/$translate` to translate `10024003 | Upper lobe of lung` to its ICD-O-3 equivalent.

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
      - "5445:5432"
    volumes:
      - termbox_example_translate_snomed_to_icdo3_pgdata:/var/lib/postgresql
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
      - "3345:3000"
    environment:
      PG_USER: postgres
      PG_PASSWORD: postgres
      PG_HOST: postgres
      PG_DATABASE: termbox
      HTTP_BASE_URL: http://localhost:3345
      LICENSE: <your license>
      DATA_CONFIG_FILE: /data/data.yaml
    volumes:
      - ./data.yaml:/data/data.yaml
volumes:
  termbox_example_translate_snomed_to_icdo3_pgdata: {}
```

Start the instance and wait until the SNOMED International import is complete:

```sh
docker compose up
```

Load jobs may be monitored via UI: [http://localhost:3345/ui/jobs](http://localhost:3345/ui/jobs).

## Example request

{% tabs %}
{% tab title="GET Request" %}

```http
GET /ConceptMap/$translate?sourceCode=10024003&sourceSystem=http://snomed.info/sct&targetSystem=http://terminology.hl7.org/CodeSystem/icd-o-3
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
    { "name": "sourceCode", "valueCode": "10024003" },
    { "name": "sourceSystem", "valueUri": "http://snomed.info/sct" },
    { "name": "targetSystem", "valueUri": "http://terminology.hl7.org/CodeSystem/icd-o-3" }
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
            "code": "C34.3"
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
    }
  ]
}
```
{% endtab %}
{% endtabs %}

The response includes `originMap`, which identifies the SNOMED CT implicit ConceptMap used for the translation.
