# Interoperability

Formats and protocols Arc Hub reads and writes, by direction and fidelity, and how losses are
reported. Fidelity levels follow the suite policy
([ADR-0009](https://github.com/ACKaraca/arc-suite/tree/main/docs/adr)): **view** (geometry and
appearance), **data** (semantics, properties, relationships) and **edit** (native editing).

## Storage principle

Arc Hub stores every file unchanged as a versioned document, whatever its format. Fidelity levels
below describe what the server can additionally *do* with a format: show it, index its data, or
change it. The original bytes are always the version; derivatives never replace them.

## Model and drawing formats

Processing is performed by headless Arc applications in the job worker. "Edit" means a server job
can produce new versions through the owning Arc application (property write-back, reference
repair, copy design, merges).

| Format | Read | Write | Notes |
|---|---|---|---|
| Arc native packages (`.archb`, `.draftb`, `.civilb`, `.solidb`, `.meshb`, `.renderb`, `.arcrev`) | view, data, edit | edit | Properties, references, journals and thumbnails read with `arc-doc`; change sets and merges for multi-user editing. |
| IFC 2x3, IFC 4, IFC 4.3 (STEP and ZIP encodings) | view, data | data (subsets for exchanges) | Element identity by GlobalId; property sets and quantities indexed; IDS validation. |
| DWG and DXF | view, data | edit (properties, external reference paths) | Layers, text and title-block attributes indexed; external references resolved. |
| PDF | view, data | data (cover sheets, reports, stamps) | Sheet extraction, text search, markups as annotations or flattened. |
| STEP AP203, AP214, AP242 | view, data | data (conversion target) | Assembly structure for BOM extraction; header properties read. |
| glTF 2.0, OBJ, STL, PLY, 3MF | view | view (conversion target) | |
| OpenUSD | view | none | Production review of scenes. |
| E57, LAS, LAZ | view | none | Streaming level-of-detail derivative. |
| LandXML | view, data | none | Through CivilBuilder's command-line tool. |
| Raster images (PNG, JPEG, TIFF, WebP) | view | none | Thumbnails; EXIF time and location for photos. |
| OpenEXR, DPX, image sequences | view | none | Proxies through OpenColorIO configuration. |
| Video (containers and codecs subject to [OQ-11](../product/open-questions.md)) | view | proxy only | Browser proxies produced by jobs. |
| Office documents (DOCX, XLSX, PPTX, ODF) | view, data (text) | edit through WOPI host | Viewing through PDF derivative produced by an isolated converter process. |

## Data exchange formats

| Format | Read | Write | Use |
|---|---|---|---|
| BCF 2.1 and 3.0 archives | data, edit | data | Issue import and export with viewpoints. |
| IDS 1.0 | data | none | Information requirements and model checks. |
| CSV, XLSX | data | data | Properties, members, companies, items, BOMs, entities, registers, reports. |
| JSON (documented schemas) | data | data | Templates, organization export, clash results, BOMs, manifests. |
| Parquet | none | data | Data extracts for analytics. |
| OpenTimelineIO, CMX 3600 EDL | data | none | Shot lists and editorial cuts. |
| Microsoft Project XML, Primavera P6 XML | data | data (Project XML) | Construction and production schedules. |
| iCalendar | none | data | Feeds for due dates and milestones. |
| buildingSMART Data Dictionary references | data | data | Classification properties. |

## Protocols

| Protocol | Role | Notes |
|---|---|---|
| HTTP REST with OpenAPI 3.1 | server | Primary API; see [`api.md`](api.md). |
| openCDE Foundation API, BCF API 3.0, Documents API | server | Interoperation with other common data environments and BCF tools. |
| OpenID Connect, OAuth 2.0 (PKCE, device authorization), SAML 2.0 | client and server | Sign-in through external providers; authorization server for clients. |
| SCIM 2.0 | server | User and group provisioning. |
| WebDAV (RFC 4918) | server | Operating-system mounts. |
| tus 1.0 | server | Resumable uploads. |
| CloudEvents 1.0 | producer | Event and webhook format. |
| WOPI | host | Browser editing by a separately deployed office suite server. |
| S3 API | client | Blob storage. |
| SMTP | client | Email delivery. |
| OpenMetrics, OTLP, syslog | producer | Observability and audit streaming. |
| Model Context Protocol | server | Agent access to Hub tools. |

## Mapping principles

- **Identifiers are carried, not invented.** Element identity uses the source identifier; BCF
  GUIDs stay the identity of issues; imported documents keep their original names and times.
- **Hub metadata never enters files implicitly.** Property values reach file content only through
  explicit property mapping with write direction (HB-PDM-019), producing a new version.
- **Every import produces a report.** CSV, BCF, manifest and template imports list created,
  updated, skipped and rejected rows with reasons before or after applying, depending on the
  import mode.
- **Derivative losses are visible.** A derivative job that cannot represent something (unknown IFC
  entity, unsupported DWG object, missing reference) stores a loss report with the derivative and
  shows it in the version's derivative status (HB-DATA-002).
- **Round trips are tested.** BCF export followed by import into a fresh project reproduces
  issues, comments and viewpoints; organization export followed by import reproduces identifiers,
  histories and audit chains.

## Validation

- BCF archives and API responses are validated against the buildingSMART schemas.
- openCDE API compliance uses the published test suites where available.
- IDS evaluation results are compared against the buildingSMART IDS test cases through the Arc
  Review command-line tool.
- Exports with published schemas (BCF, IFC subsets, iCalendar, Microsoft Project XML) are read back
  by an independent reader in continuous integration.
- Documented Hub JSON schemas are versioned and published with the server; exports validate
  against them before being offered for download.

## Deliberately unsupported

| Content | Reason | How it is exchanged instead |
|---|---|---|
| Viewing or indexing proprietary model formats without a published specification (for example RVT and NWD) | The suite never reverse engineers formats. | Stored and versioned as files; shared for viewing as IFC, DWG or PDF exports. |
| Proprietary cloud model-derivative formats | Not published. | Arc Hub produces its own viewables from open or Arc formats. |
| Native mobile synchronization protocols of other services | Out of scope. | Tablet browsers (HB-VIEW-012) and the REST API. |
| Writing Office documents on the server | Requires an office suite. | WOPI host integration with a separately deployed office server (HB-INTEG-006). |
