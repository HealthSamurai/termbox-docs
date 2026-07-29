# Searching on an ad-hoc value set

This example uses `ValueSet/$expand` with an inline ValueSet to search SNOMED CT concepts filtered by pathological process, finding site, and the text `fever`.

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
      - "5448:5432"
    volumes:
      - termbox_example_search_adhoc_valueset_pgdata:/var/lib/postgresql
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
      - "3348:3000"
    environment:
      PG_USER: postgres
      PG_PASSWORD: postgres
      PG_HOST: postgres
      PG_DATABASE: termbox
      HTTP_BASE_URL: http://localhost:3348
      LICENSE: <your license>
      DATA_CONFIG_FILE: /data/data.yaml
    volumes:
      - ./data.yaml:/data/data.yaml
volumes:
  termbox_example_search_adhoc_valueset_pgdata: {}
```

Start the instance and wait until the SNOMED International import is complete:

```sh
docker compose up
```

Load jobs may be monitored via UI: [http://localhost:3348/ui/jobs](http://localhost:3348/ui/jobs).

## Example request

{% tabs %}
{% tab title="POST Request" %}
```http
POST /ValueSet/$expand
Content-Type: application/fhir+json
Accept: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [{
    "name": "valueSet",
    "resource": {
      "resourceType": "ValueSet",
      "compose": {
        "include": [{
          "system": "http://snomed.info/sct",
          "filter": [{
            "property": "370135005",
            "op": "=",
            "value": "441862004"
          }, {
            "property": "363698007",
            "op": "=",
            "value": "113255004"
          }]
        }]
      }
    }
  }, {
    "name": "filter",
    "valueString": "fever"
  }, {
    "name": "count",
    "valueInteger": 10
  }]
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
  "resourceType": "ValueSet",
  "expansion": {
    "identifier": "urn:uuid:394d6ec6-b750-4f14-9065-5387b654ea64",
    "timestamp": "2026-07-29T17:27:52Z",
    "total": 4,
    "parameter": [
      {
        "name": "count",
        "valueInteger": 10
      },
      {
        "name": "filter",
        "valueString": "fever"
      },
      {
        "name": "used-codesystem",
        "valueUri": "http://snomed.info/sct|http://snomed.info/sct/900000000000207008/version/20260701"
      }
    ],
    "contains": [
      {
        "code": "32286006",
        "system": "http://snomed.info/sct",
        "display": "Pneumonia in Q fever"
      },
      {
        "code": "45312009",
        "system": "http://snomed.info/sct",
        "display": "Pneumonia in typhoid fever"
      },
      {
        "code": "1208602000",
        "system": "http://snomed.info/sct",
        "display": "Pneumonia caused by Pseudomonas aeruginosa"
      },
      {
        "code": "763888005",
        "system": "http://snomed.info/sct",
        "display": "Necrotizing pneumonia caused by Panton-Valentine leukocidin producing Staphylococcus aureus"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

This request uses POST because the inline ValueSet carries the property filters in the request body.
