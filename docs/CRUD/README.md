# CRUD

Termbox supports the [FHIR REST API](https://hl7.org/fhir/http.html) for managing terminology resources. Full CRUD support is planned; the table below shows the current status.

## Capabilities

| Interaction | FHIR Spec                                                    | CodeSystem | ValueSet | ConceptMap |
| ----------- | ------------------------------------------------------------ | ---------- | -------- | ---------- |
| Create      | [POST   /[type]](https://hl7.org/fhir/http.html#create)      | ✅          | ✅        | ✅          |
| Read        | [GET    /[type]/[id]](https://hl7.org/fhir/http.html#read)   |            |          |            |
| Update      | [PUT    /[type]/[id]](https://hl7.org/fhir/http.html#update) |            |          |            |
| Delete      | [DELETE /[type]/[id]](https://hl7.org/fhir/http.html#delete) | ✅          | ✅        | ✅          |
| Search      | [GET    /[type]](https://hl7.org/fhir/http.html#search)      |            |          |            |
