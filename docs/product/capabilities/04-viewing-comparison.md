# Viewing and comparison

Browser viewing of drawings, sheets and models, sheet registers, document sets and the comparison
of versions. Viewing uses the Arc Review viewer compiled to WebAssembly and the derivatives that
the job worker produces.

Domain objects:

- **Viewable**: a streaming derivative of a version that the browser viewer loads.
- **Saved view**: camera, sections and visibility stored against a version.
- **Calibration**: a scale for a 2D page derived from two points and a known length.
- **Sheet**: a numbered drawing extracted from a version, stacked by sheet number.
- **Document set**: an immutable, named list of versions.
- **Comparison**: a computed difference between two versions or two sets.

## Browser viewer

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-VIEW-001 | Embedded viewer | Opening a version loads its viewable into the Arc Review viewer running as WebAssembly in the page, using WebGPU where available and WebGL 2 otherwise. On browsers without either, the page shows the thumbnail, properties and download actions. | T1 | 2 |
| HB-VIEW-002 | 2D viewing | PDF, DWG, DXF and raster images open with pan, zoom, page thumbnails, layer and optional-content visibility, text search in vector text and followed hyperlinks. | T1 | 2 |
| HB-VIEW-003 | 3D viewing | IFC, Arc native documents, STEP, glTF, OBJ and OpenUSD viewables open with orbit and walk navigation, section planes and section box, isolate and hide, model tree and properties panel. | T1 | 2 |
| HB-VIEW-004 | Measurement | Users measure distances, angles and areas: in 3D with snapping to geometry, in 2D using the document scale or a calibration set by two points and a known length. Calibrations are stored per version, private or shared. | T1 | 2 |
| HB-VIEW-005 | Saved views | Users save the camera, sections and visibility against a version, privately or shared. A link to a saved view opens the version in that state. | T1 | 2 |
| HB-VIEW-006 | Element properties | Selecting an element shows its identifier, type and property sets from the property index (HB-DATA-006), with units in the user's display units. | T1 | 2 |
| HB-VIEW-007 | Federated viewing | Users open several versions together, either as a model set (HB-COORD-001) or as an ad hoc selection, with transforms taken from shared coordinates or set manually. | T2 | 3 |
| HB-VIEW-008 | Streaming large models | Viewables stream progressively by spatial and screen-size priority, so navigation starts before the whole model is loaded; see the performance targets in the architecture overview. | T2 | 2 |
| HB-VIEW-009 | Office and text documents | Office documents, plain text and Markdown are viewed through a PDF derivative produced by an isolated converter process. When no converter is configured the page offers download only. | T2 | 3 |
| HB-VIEW-010 | Point clouds | E57 and LAS/LAZ versions are viewed through a streaming level-of-detail derivative with point size and color-by options. | T3 | 4 |
| HB-VIEW-011 | Accessible viewer page | The model tree, properties, sheets list and viewer commands are fully keyboard operable with accessible names, and every element shown in the canvas is also reachable through the tree and property table. | T1 | 2 |
| HB-VIEW-012 | Tablet browsers | The web client adapts to tablet screen sizes for viewing, markup, issues and form filling, using touch input. It requires a connection. | T2 | 3 |
| HB-VIEW-013 | Embeddable viewer | Administrators allow a view of a version to be embedded in an external web page through a scoped, expiring token and an allowlist of embedding origins. | T3 | 4 |

## Sheets

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-VIEW-014 | Sheet extraction | Uploading a multi-page PDF into a sheet-enabled folder starts a job that splits pages into sheets and reads sheet number, title, revision and discipline from Arc sheet metadata, PDF page labels or title-block regions. The uploader reviews and corrects the values before the sheets are published. | T1 | 2 |
| HB-VIEW-015 | Title-block regions | Administrators define named rectangular regions per title-block layout (sheet number, title, revision, date) matched by page size; extraction reads the text inside each region. | T2 | 2 |
| HB-VIEW-016 | Sheet register | The project sheet register lists every sheet with its current revision and stack of earlier revisions; a new sheet with an existing number becomes the next revision of that sheet. | T1 | 2 |
| HB-VIEW-017 | Sheet cross-links | Sheet numbers that appear in sheet text are detected and turned into links to the current revision of the referenced sheet. | T3 | 3 |
| HB-VIEW-018 | Publish from Arc applications | Arc desktop applications publish sheet sets with sheet metadata directly, without extraction; each sheet keeps a link to the model version it was produced from. | T1 | 2 |

## Document sets

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-VIEW-019 | Document sets | Users with Edit capture a named, dated, immutable set of selected document versions or sheet revisions, for example a tender issue. Later versions never change a set. | T1 | 2 |
| HB-VIEW-020 | Set comparison | Comparing two sets, or a set with the current state, lists added, removed and revised documents with their version and revision labels. | T2 | 2 |
| HB-VIEW-021 | Set download | A set downloads as a ZIP preserving folder paths with a CSV manifest, or for sheets as one merged PDF with bookmarks per sheet. | T2 | 2 |

## Comparison

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-VIEW-022 | 2D overlay comparison | Any two 2D versions are compared as a color-coded overlay (content only in the first, only in the second, unchanged) or side by side with synchronized navigation. Users align by two point pairs when page origins differ. | T1 | 2 |
| HB-VIEW-023 | 3D element comparison | Two model versions are compared by stable element identifier (Arc identifiers, IFC GlobalId): elements are classified as added, removed, geometry changed or properties changed, shown in color with a property difference table. | T2 | 2 |
| HB-VIEW-024 | Journal comparison | For Arc native documents, the comparison between two versions can also list the recorded commands between their journal positions with author and time, giving an exact account of what changed. | T2 | 3 |
| HB-VIEW-025 | Comparison export | 2D comparisons export as PDF; 3D comparisons export as CSV and PDF change lists with element identifiers. | T2 | 3 |
| HB-VIEW-026 | Change summary on new versions | When a sheet or model receives a new version, a job computes a summary of changes against the previous version and shows it in the history and in notifications to followers. | T2 | 3 |
