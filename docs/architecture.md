# Architecture

Termbox provides a FHIR terminology server with a web UI, a backend API, PostgreSQL persistence, and loaders for external terminology sources. It is designed to be easy to spin up from configuration, so teams can manage terminology content declaratively in the same spirit as infrastructure as code (terminology as code).

Termbox also exposes terminology capabilities at different levels of integration: interactive exploration through the UI, application integration through FHIR operations, and analytics workflows that need terminology data available from a relational database. The same core components can run in SaaS or on-prem deployments.

```mermaid
graph LR
    client(Client):::blue2

    subgraph deployment["SaaS/On-prem"]
        ui(Termbox UI):::green2
        backend(Termbox Backend):::red2
        pg[(PostgreSQL)]:::neutral2
    end

    subgraph sources["Terminology sources"]
        gallery(Termbox Gallery):::violet2
        npm(FHIR packages registry<br/>NPM):::violet2
        atom(Syndication<br/>Atom feed):::violet2
        fs(Local filesystem<br/>npm, bundle, resource):::yellow2
    end

    client -->|Explore| ui
    client -->|FHIR operations| backend
    client -->|Analytics SQL| pg

    ui --> backend
    backend --> pg

    gallery -->|Load| backend
    npm -->|Load| backend
    atom -->|Load| backend
    fs -->|Load| backend
```
