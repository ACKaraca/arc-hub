# ADR-0007: Native mobile clients from the shared Rust crates

Status: accepted

## Context

Field staff, inspectors and site managers work on phones and tablets, often without a connection.
The web client serves tablets that are online (HB-VIEW-012), but offline sheets and models,
camera capture, background upload, push notifications and barcode scanning need a native client.
Suite [ADR-0018](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0018-web-and-mobile-targets.md)
allows iOS and Android builds of the suite's Rust crates as targets of the same code. The Hub
server must stay the single authority for permissions and records, and a device is a place where
data can be lost or stolen.

## Decision

- The mobile clients are built from the shared Rust crates: the `arc-hub-api` client, the document
  readers needed to open sheets and model viewables, the UI kit and the viewer renderer. The
  application wiring lives in `apps/arc-hub-mobile`; platform shells contain only what the operating
  system requires (lifecycle, camera, push registration, secure storage).
- A client uses only operations of the public REST API and the push channels; it has no private
  endpoints. Server behavior for mobile is therefore testable with any API client.
- Offline use is explicit: the user marks an offline set, the device stores derivatives by digest,
  and changes are queued in an ordered outbox of idempotent operations.
- A conflict never discards work: when the server changed a record in the meantime, both states are
  kept as a conflict record and the user decides, as in multi-user editing.
- Offline stores are encrypted with a key in the platform's secure storage. Revoking a device
  removes its tokens at once and its offline store at the next contact.
- Push notifications use the platform services only when an administrator configures credentials,
  and payloads carry an event type and a link, never content.

## Consequences

- One implementation of formats and viewing for desktop, browser and mobile; fixes reach all.
- Field capture (photos, forms, issues) works on site without coverage, and what was entered is
  sent in order afterwards.
- The server gains device registration and revocation, a push sender and label generation, and
  must keep API operations idempotent and resumable.
- Store distribution, device test matrices and release signing for two mobile platforms add
  release work that the desktop products do not have.
- Models must fit device memory; the client reduces level of detail and says so.

## Alternatives considered

- **Tablet browser only**: no work in the offline case, no background upload, no scanning.
- **A cross-platform framework with a JavaScript UI**: a second language and UI stack, duplicated
  validation, and a break with the suite's single UI kit.
- **Separate native codebases per platform**: duplicated viewing and API logic and two sets of bugs.
- **A web view shell around the web client**: inherits the connection requirement and gives no
  secure offline store.

## Reopen when

Model viewing cannot meet its frame-rate or memory targets on mainstream devices with the shared
renderer, or a platform requirement cannot be met without a separate native UI layer.
