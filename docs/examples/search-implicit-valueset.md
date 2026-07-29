# Searching on an implicit value set

This example uses `ValueSet/$expand` to search for `heart failure risk` within the SNOMED CT implicit ValueSet `http://snomed.info/sct?fhir_vs=isa/404684003`.

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
      - "5444:5432"
    volumes:
      - termbox_example_search_implicit_valueset_pgdata:/var/lib/postgresql
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
      - "3344:3000"
    environment:
      PG_USER: postgres
      PG_PASSWORD: postgres
      PG_HOST: postgres
      PG_DATABASE: termbox
      HTTP_BASE_URL: http://localhost:3344
      LICENSE: <your license>
      DATA_CONFIG_FILE: /data/data.yaml
    volumes:
      - ./data.yaml:/data/data.yaml
volumes:
  termbox_example_search_implicit_valueset_pgdata: {}
```

Start the instance and wait until the SNOMED International import is complete:

```sh
docker compose up
```

Load jobs may be monitored via UI: [http://localhost:3344/ui/jobs](http://localhost:3344/ui/jobs).

## Example request

{% tabs %}
{% tab title="GET Request" %}

```http
GET /ValueSet/$expand?url=http://snomed.info/sct?fhir_vs=isa/404684003&filter=heart+failure+risk&count=10
Accept: application/fhir+json
```
{% endtab %}

{% tab title="POST Request" %}
```http
POST /ValueSet/$expand
Content-Type: application/fhir+json
Accept: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "url", "valueUri": "http://snomed.info/sct?fhir_vs=isa/404684003" },
    { "name": "filter", "valueString": "heart failure risk" },
    { "name": "count", "valueInteger": 10 }
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
  "resourceType": "ValueSet",
  "url": "http://snomed.info/sct?fhir_vs=isa/404684003",
  "status": "active",
  "expansion": {
    "identifier": "urn:uuid:73d72771-ad46-41ac-b350-f55864f6a9ef",
    "timestamp": "2026-07-29T16:16:55Z",
    "total": 8,
    "parameter": [
      {
        "name": "count",
        "valueInteger": 10
      },
      {
        "name": "filter",
        "valueString": "heart failure risk"
      },
      {
        "name": "used-codesystem",
        "valueUri": "http://snomed.info/sct|http://snomed.info/sct/900000000000207008/version/20260701"
      }
    ],
    "contains": [
      {
        "code": "609389009",
        "system": "http://snomed.info/sct",
        "display": "Stage B at high risk of heart failure"
      },
      {
        "code": "1382259003",
        "system": "http://snomed.info/sct",
        "display": "Stage B at high risk of systolic heart failure"
      },
      {
        "code": "1382260008",
        "system": "http://snomed.info/sct",
        "display": "Stage B at high risk of diastolic heart failure"
      },
      {
        "code": "609387006",
        "system": "http://snomed.info/sct",
        "display": "At high risk for heart failure"
      },
      {
        "code": "609388001",
        "system": "http://snomed.info/sct",
        "display": "Stage A at high risk of heart failure"
      },
      {
        "code": "609386002",
        "system": "http://snomed.info/sct",
        "display": "At increased risk for heart failure"
      },
      {
        "code": "1382261007",
        "system": "http://snomed.info/sct",
        "display": "Stage B at high risk of systolic heart failure due to ischemic cardiomyopathy"
      },
      {
        "code": "1303865002",
        "system": "http://snomed.info/sct",
        "display": "Marfanoid habitus, facial dysmorphism, skeletal abnormality, heart defect syndrome"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}
