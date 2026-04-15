# Getting Started

This section describes how to start Termbox for local development using Docker Compose with a PostgreSQL instance alongside it. Production deployments will typically use an external PostgreSQL instance.

## Requirements

Before starting, make sure the following software is available:

- Docker
- Docker Compose

## Quick Start

Create a `docker-compose.yaml` file with the following contents

```yaml
services:
  postgres:
    image: postgres:18
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: termbox
    volumes:
    - postgres_data:/var/lib/postgresql
  termbox:
    depends_on:
      - postgres
    image: ghcr.io/healthsamurai/termbox:latest
    pull_policy: always
    ports:
      - "3000:3000"
    environment:
      PG_USER: postgres
      PG_PASSWORD: postgres
      PG_HOST: postgres
volumes:
  postgres_data: {}
```

Then run these commands:

```bash
docker compose up -d               # start the services
docker compose logs -f termbox     # view logs
```

Once you see a log like

```log
termbox.http-server[99,5] HTTP Server listening on  3000
```

It means Termbox is successfully running. Before you can start using the Termbox FHIR API, you need to acquire a license. See [Licensing](licensing.md) for available license types and how to activate one. After that, you can start using the Termbox FHIR API. To check the FHIR API is working run this command:

```bash
curl http://localhost:3000/fhir/metadata
```

It should return a JSON resource of type `CapabilityStatement`

The UI should be available at `http://localhost:3000/ui`
