# Validate a code using an implicit value set

This example uses `ValueSet/$validate-code` to validate that `38341003` belongs to the SNOMED CT implicit ValueSet `http://snomed.info/sct?fhir_vs=isa/404684003`.

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
      - "5443:5432"
    volumes:
      - termbox_example_validate_code_implicit_valueset_pgdata:/var/lib/postgresql
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
      - "3343:3000"
    environment:
      PG_USER: postgres
      PG_PASSWORD: postgres
      PG_HOST: postgres
      PG_DATABASE: termbox
      HTTP_BASE_URL: http://localhost:3343
      LICENSE: <your license>
      DATA_CONFIG_FILE: /data/data.yaml
    volumes:
      - ./data.yaml:/data/data.yaml
volumes:
  termbox_example_validate_code_implicit_valueset_pgdata: {}
```

Start the instance and wait until the SNOMED International import is complete:

```sh
docker compose up
```

Load jobs may be monitored via UI: [http://localhost:3343/ui/jobs](http://localhost:3343/ui/jobs).

## Example request

{% tabs %}
{% tab title="GET Request" %}

```http
GET /ValueSet/$validate-code?url=http://snomed.info/sct?fhir_vs=isa/404684003&code=38341003&inferSystem=true
Accept: application/fhir+json
```
{% endtab %}

{% tab title="POST Request" %}
```http
POST /ValueSet/$validate-code
Content-Type: application/fhir+json
Accept: application/fhir+json

{
  "resourceType": "Parameters",
  "parameter": [
    { "name": "url", "valueUri": "http://snomed.info/sct?fhir_vs=isa/404684003" },
    { "name": "code", "valueCode": "38341003" },
    { "name": "inferSystem", "valueBoolean": true }
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
      "name": "result",
      "valueBoolean": true
    },
    {
      "name": "code",
      "valueCode": "38341003"
    },
    {
      "name": "display",
      "valueString": "Hypertensive disorder"
    },
    {
      "name": "system",
      "valueUri": "http://snomed.info/sct"
    },
    {
      "name": "version",
      "valueString": "http://snomed.info/sct/900000000000207008/version/20260701"
    }
  ]
}
```
{% endtab %}
{% endtabs %}

Using `inferSystem=true` lets the request omit `system` when the ValueSet URL identifies the SNOMED CT code system.
