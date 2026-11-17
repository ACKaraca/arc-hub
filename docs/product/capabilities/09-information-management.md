# Information management

Properties and metadata, naming standards and the information container states of ISO 19650. One
property engine serves documents, product data items, records and production entities.

Domain objects:

- **Property definition**: a typed, validated field definition owned by an organization or project.
- **Property set**: the properties a folder or record type applies, each required or optional.
- **Naming standard**: ordered fields, separators and dictionaries that a file name must match.
- **Container state**: work in progress, shared, published or archived, held by each version.
- **Suitability code** and **revision code**: status values assigned when a version changes state.
- **Delivery plan entry**: an expected deliverable with responsible company, milestone and target state.

## Properties and metadata

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-INFO-001 | Property definitions | Organizations and projects define properties with key, localized label, type (text, multi-line text, integer, decimal with unit, boolean, date, single or multiple choice list, user, company, URL, link to record), validation (pattern, range), default value and help text. The same definitions apply to documents, items, records and production entities. | T1 | 2 |
| HB-INFO-002 | Folder property sets | A folder declares which properties apply to its documents and which are required; subfolders inherit the set and can add properties. | T1 | 2 |
| HB-INFO-003 | Version property values | Property values are entered at upload and edited later with a history per value. An upload missing a required property is refused, unless the folder allows incomplete uploads, in which case the version is flagged incomplete in lists and checks. | T1 | 2 |
| HB-INFO-004 | System properties | Name, version number, size, digest, uploader, dates, container or lifecycle state, revision, review status and lock holder are read-only properties available in lists, filters and exports. | T1 | 2 |
| HB-INFO-005 | Batch edit and import | Users edit properties of many documents at once or import values from CSV matched by document identifier or name, with a preview of every change; the import applies all rows or, if chosen, all valid rows with a per-row error list. | T1 | 2 |
| HB-INFO-006 | Properties from file content | Mapping rules copy values from file metadata into properties at upload: PDF document information, Arc document properties, IFC project data and DWG drawing properties and title-block attributes. Writing values back into files is a product data capability (HB-PDM-019). | T2 | 3 |

## Naming standards

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-INFO-007 | Naming standards | A project defines naming standards as ordered fields with separators. Each field is a dictionary code, free text matching a pattern, a zero-padded number, the project code or a company code from the directory, for example project, originator, volume, level, type, role and number. | T1 | 2 |
| HB-INFO-008 | Name validation | Every upload into a folder with a naming standard is validated field by field. The folder either rejects non-compliant names or accepts them flagged as non-compliant; Arc applications validate before saving to the Hub. | T1 | 2 |
| HB-INFO-009 | Name builder | Upload forms offer a picker per field to compose a compliant name and propose the next free number for the chosen field combination. | T2 | 2 |
| HB-INFO-010 | Name fields to properties | Fields parsed from a compliant name populate mapped properties (for example originator, role, type) automatically. | T2 | 2 |
| HB-INFO-011 | Compliance report | A report lists non-compliant documents per folder with the failing field. After a standard changes, documents are revalidated and flagged; existing documents are never renamed automatically. | T2 | 2 |
| HB-INFO-012 | Dictionaries | Dictionary codes have a description and can be deprecated: existing documents with a deprecated code stay valid, new uploads using it are refused. | T2 | 2 |

## Container states

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-INFO-013 | Container states | A project can enable information management, in which every version holds one state: work in progress, shared, published or archived. Each state defines permission caps (HB-ACCESS-020), for example work in progress readable only by the originating team and information managers. | T1 | 2 |
| HB-INFO-014 | State transitions | Transitions from work in progress to shared and from shared to published run through a gate review (HB-REVIEW-004) or, if the project allows, a direct transition by an authorized role. Transitions never change content, record actor, time and reason, and either keep the version in place or move it into the folder assigned to the target state, as the project chooses. | T1 | 2 |
| HB-INFO-015 | Suitability codes | The project keeps a list of suitability codes, each allowed in particular states. A code is required when a version enters the shared or published state and is validated against the target state. | T1 | 2 |
| HB-INFO-016 | Revision codes | A project revision scheme assigns preliminary and contractual revision codes with optional minor increments at state transitions. The scheme engine is shared with product data revision schemes (HB-PDM-009). | T1 | 2 |
| HB-INFO-017 | Supersede on publish | Publishing a newer revision of a document marks the earlier published revision as superseded and, if the policy says so, moves it to archived. | T1 | 2 |
| HB-INFO-018 | Task team areas | Work-in-progress areas are created per task team (company or team) with access for that team and the information manager role only. | T2 | 2 |
| HB-INFO-019 | Container register | A register lists every container with state, suitability, revision, originator and dates and exports as CSV or XLSX. | T1 | 2 |

## Requirements and delivery

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-INFO-020 | Information requirements | Projects attach exchange requirements as IDS files and as required property sets per document type. A job validates each new IFC version against the applicable IDS and reports pass or fail per requirement and element. | T2 | 3 |
| HB-INFO-021 | Delivery plan | A delivery plan lists expected deliverables with expected name, responsible company, milestone and target state. An upload whose name matches an entry links to it; the plan report shows delivered, late and missing entries. | T2 | 3 |
| HB-INFO-022 | Classification references | Classification properties store references to buildingSMART Data Dictionary entries (URI, code, label), with locally cached dictionaries so lookups work without outbound connections. | T3 | 4 |
