# Search

Finding documents, records and model elements across projects, with results filtered by the
reader's permissions at query time.

Domain objects:

- **Search index entry**: searchable text and fields of a document version, record or entity.
- **Saved search**: a stored query, personal or shared, usable as a virtual folder.
- **Element index**: the per-version property index of model elements (HB-DATA-006).

## Search

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-SEARCH-001 | Global search | One search field covers document names and properties, sheet numbers and titles, issues, reviews, transmittals, construction records, items and production entities, in the current project or in all projects the user belongs to. Results are grouped by type. | T1 | 2 |
| HB-SEARCH-002 | Permission-correct results | Results, counts, facets and snippets include only content the user can read at query time; a dedicated test suite verifies that no result leaks the existence or content of unreadable resources. | T1 | 2 |
| HB-SEARCH-003 | Full-text content | Text from PDFs, Office documents, DWG and DXF text, Arc document annotations and record comments is indexed asynchronously, with a target lag of 60 seconds after derivative processing. Stemming is language-aware for English and Turkish. | T2 | 2 |
| HB-SEARCH-004 | Filters and facets | Searches filter by type, folder, property values, state, uploader, date range and file type, with counts per facet value. | T1 | 2 |
| HB-SEARCH-005 | Query syntax | A documented query grammar supports `field:value`, quoted phrases, AND, OR, NOT, ranges and relative dates. The same grammar is the filter language of the REST API (HB-API-003). | T2 | 2 |
| HB-SEARCH-006 | Saved searches | Users save searches privately or for the project, open them as virtual folders in the web client and desktop sync, and subscribe to them for notifications. | T2 | 2 |
| HB-SEARCH-007 | Element search | Users find model elements across the latest versions of a project or model set by property values, category and spatial container, for example all doors with a given fire rating, and open the results in the viewer. | T2 | 3 |
| HB-SEARCH-008 | Search in the viewer | Inside an open version, users search sheet text and element names and properties and jump to matches. | T1 | 2 |
| HB-SEARCH-009 | Recent and favorites | Users pin favorite folders, documents and records and see their recently opened items across projects. | T1 | 2 |
| HB-SEARCH-010 | Reindex | Server administrators rebuild the index of a project or organization in the background with visible progress; search stays available on the existing index meanwhile. | T2 | 2 |
| HB-SEARCH-011 | Semantic search | When an organization configures an embedding provider (HB-ASSIST-001), users can search document text in natural language. The feature is off by default and sends text only to the configured endpoint. | T3 | 5 |
