# Model data services

Derivatives, property and element indexes, granular queries and data exchange between
applications. These services turn stored files into viewable, searchable and queryable model data
without requiring the authoring application.

Domain objects:

- **Derivative**: a product of a version (viewable, thumbnail, text, property index, references, conversion), keyed by content digest, kind and converter version.
- **Element record**: an element of a model version with identifier, classification, container and property values.
- **Saved query**: a stored element query with filters and aggregations.
- **Exchange**: a named, versioned subset of elements of a source model offered to other applications.
- **Parameter library**: an organization's shared property definitions for authoring applications.

## Derivatives

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-DATA-001 | Derivative pipeline | Each new version of a supported format gets a thumbnail, a streaming viewable, extracted text, a property index and a reference list, produced by the headless Arc Review application. Derivatives are keyed by content digest, kind and converter version, so identical content is processed once. | T1 | 2 |
| HB-DATA-002 | Derivative status | Each version shows derivative status per kind (pending, ready, failed, unsupported); failures carry the converter's loss report and message and can be retried. | T1 | 2 |
| HB-DATA-003 | Derivative regeneration | When a converter version changes, administrators regenerate derivatives in the background, most recently viewed versions first, while existing derivatives stay in use. | T2 | 2 |
| HB-DATA-004 | Format conversion | Users request conversions by headless Arc applications to IFC, glTF, STEP, DXF, DWG, PDF, OBJ or STL as far as the source supports, stored as a derivative or saved as a new document. | T2 | 3 |
| HB-DATA-005 | Client-supplied derivatives | Arc applications upload viewables they computed locally; the server validates the format and binds them to the version digest. | T2 | 3 |

## Elements and queries

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-DATA-006 | Property index | Model versions are indexed into element records with stable identifier, class and type, name, spatial container and property sets with typed values and units. | T1 | 2 |
| HB-DATA-007 | Element tree API | The object tree and element properties of a version are available through paged API operations. | T1 | 2 |
| HB-DATA-008 | Granular queries | Users and clients query elements across versions, documents, model sets and projects by property filters, class and spatial container, with aggregations (count, sum, group by) that respect units. Results are paged and queries can be saved. | T2 | 3 |
| HB-DATA-009 | Element history | For an element identifier, the server lists the versions that changed it and how, based on version comparisons (HB-VIEW-023). | T2 | 3 |
| HB-DATA-010 | Quantities | Element quantities (length, area, volume, count) are exposed where the source provides them and aggregate with unit conversion. | T2 | 3 |
| HB-DATA-011 | Query export | Query results export as CSV, XLSX and JSON and can be scheduled as data extracts (HB-ADMIN-008). | T2 | 3 |

## Exchange between applications

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-DATA-012 | Parameter library | Organizations keep a versioned library of shared property definitions (names, data types, units, classification references) that Arc applications import so models use consistent properties. | T2 | 3 |
| HB-DATA-013 | Exchanges | An exchange is a named, versioned subset of elements selected by a query from a source model, materialized as an IFC subset or Arc document fragment with a property table. Consumers in other Arc applications link the exchange. | T3 | 4 |
| HB-DATA-014 | Exchange updates | When the source publishes a new version, the exchange is recomputed; consumers are notified, choose to update and receive a mapping report of added, removed and changed elements. | T3 | 4 |
