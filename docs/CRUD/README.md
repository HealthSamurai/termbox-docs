# CRUD

Termbox supports the [FHIR REST API](https://hl7.org/fhir/http.html) for managing terminology resources. Full CRUD support is planned; the table below shows the current status.

## Capabilities

| Interaction | FHIR Spec                                                         | CodeSystem | ValueSet |
| ----------- | ----------------------------------------------------------------- | ---------- | -------- |
| Create      | [POST   /fhir/[type]](https://hl7.org/fhir/http.html#create)      | ✅          | ✅        |
| Read        | [GET    /fhir/[type]/[id]](https://hl7.org/fhir/http.html#read)   |            |          |
| Update      | [PUT    /fhir/[type]/[id]](https://hl7.org/fhir/http.html#update) |            |          |
| Delete      | [DELETE /fhir/[type]/[id]](https://hl7.org/fhir/http.html#delete) | ✅          | ✅        |
| Search      | [GET    /fhir/[type]](https://hl7.org/fhir/http.html#search)      |            |          |
