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

## Live data streams

Data streams link live readings, such as sensor values, to model elements so that a model shows
current conditions. The Hub stores and shows the readings; it does not control equipment or manage
maintenance work.

Domain objects:

- **Data stream**: a source of timestamped readings with unit, expected rate and retention.
- **Stream link**: the binding of a stream to an element identifier of a model.

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-DATA-015 | Data streams | A stream is registered with name, unit, value type, expected rate and retention. Readings arrive over an authenticated ingest operation of the API or an MQTT subscription that an administrator configures, are validated against the stream definition and rate-limited per source. | T3 | 5 |
| HB-DATA-016 | Stream links | A stream links to elements by source identifier in a document; a CSV mapping imports many links at once. Links resolve against the latest version, and the Hub lists links whose element is missing from a newer version. | T3 | 5 |
| HB-DATA-017 | Live values in the viewer | The viewer shows the latest value, its age and a time-series chart for a selected linked element, and can color elements by value range. Values update over the push channel (HB-API-020) and are shown only to users who may read the stream. | T3 | 5 |
| HB-DATA-018 | Stream thresholds | A threshold on a stream, with hysteresis and a hold time, fires an automation rule (HB-JOB-013) that can notify, create an issue linked to the element, or start a job; repeated crossings update one open issue. | T3 | 5 |
| HB-DATA-019 | Reading storage and export | Readings are kept in time-partitioned tables with downsampled rollups and per-stream retention. Users export a range as CSV or Parquet. Reading volume is reported in the usage summary (HB-ORG-005). | T3 | 5 |
