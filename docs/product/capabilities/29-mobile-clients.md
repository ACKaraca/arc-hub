# Mobile clients

Native clients for phones and tablets: documents, sheets and models available offline, issues,
markups, forms and checklists, photos, reviews and approvals, push notifications and barcode
lookup of items. The clients are built from the same Rust crates as the desktop applications
(suite [ADR-0018](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0018-web-and-mobile-targets.md))
and talk to the server only through the public API. The approach is recorded in
[ADR-0007](../../adr/0007-native-mobile-clients.md).

Domain objects:

- **Mobile device**: a registered installation of a mobile client, bound to one user and one server.
- **Offline set**: the folders, documents, sheets, saved views and records a user keeps on a device.
- **Outbox**: the ordered queue of changes made on a device and not yet accepted by the server.
- **Field capture**: a photo, form entry, markup or issue created on a device.
- **Code label**: a printable QR code or barcode that identifies an item, location or document.

## Clients and devices

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-MOBILE-001 | Native mobile clients | iOS and Android clients are built from the shared Rust crates (`arc-hub-api` client, document readers, UI kit) as targets of the same code, not as separate products. A client works with one or more Hub servers and uses only operations of the public REST API. | T3 | 5 |
| HB-MOBILE-002 | Sign-in and device registration | Users sign in through the system browser with OpenID Connect and PKCE. Each installation registers as a device with name, platform and client version; users list and remove their devices, and administrators list and revoke devices per user. Revoking a device invalidates its tokens. | T3 | 5 |
| HB-MOBILE-003 | Offline sets | Users mark folders, documents, sheets, saved views, forms and issue lists as available offline. The device downloads versions with their derivatives, verifies digests, refreshes the set when new versions appear and shows its size; the user can set a storage limit and remove items. | T3 | 5 |
| HB-MOBILE-004 | Offline documents and sheets | PDF, images, text and sheets of an offline set open without a connection with pan, zoom, page and sheet navigation, text search and layer visibility. A banner states when a newer version exists on the server. | T3 | 5 |
| HB-MOBILE-005 | Offline models | Model viewables of an offline set are viewed with navigation, element selection, property tables from the property index, sections, measurement and saved views, using the GPU of the device. A model whose viewable exceeds the device limit opens at a reduced level of detail with a notice. | T3 | 5 |
| HB-MOBILE-006 | Deep links | Permalinks (HB-FILE-013) open in the native client when it is installed and in the browser otherwise; a link to content in an offline set opens the offline copy. | T3 | 5 |
| HB-MOBILE-017 | Accessibility and localization | Clients follow the platform's screen-reader, text-scaling and contrast settings, name every control, and use the same localized message catalogues as the server. | T3 | 5 |

## Field work

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-MOBILE-007 | Issues | Users create, comment on, assign and change the status of issues, pin them to sheet positions or model elements with viewpoints, and attach photos, with or without a connection. | T3 | 5 |
| HB-MOBILE-008 | Markups | Users draw, type and measure on sheets, photos and model views and save the result as a markup set (HB-MARKUP-001); markups made offline join the outbox. | T3 | 5 |
| HB-MOBILE-009 | Forms and checklists | Users complete form templates (HB-SITE-007) with all field types including signature, photo and location, see conditional fields and scores, and submit; a submitted form becomes immutable as on the server (HB-SITE-008). A failed check item creates its linked issue. | T3 | 5 |
| HB-MOBILE-010 | Photos | The camera captures photos with time and location, which users tag, put into albums and link to issues, forms, RFIs and locations. Uploads run in the background, resumable and in order, over the networks the user allows. | T3 | 5 |
| HB-MOBILE-011 | Reviews and approvals | Users see the review steps assigned to them, open the documents under review, comment and decide. A decision made offline is held in the outbox, shown as pending and recorded with the server's time on acceptance; it is refused with an explanation if the step has changed in the meantime. | T3 | 5 |
| HB-MOBILE-012 | Push notifications | Assignments, mentions, review requests, status changes and due dates reach devices through the platform push services when an administrator has configured the provider credentials; without them, clients poll when open. Payloads carry an event type and a link, never file content or record text, and users choose events per device. | T3 | 5 |
| HB-MOBILE-013 | Barcode and QR lookup | The camera scans QR codes and common linear barcodes and opens the matching item (HB-ITEM-001), document, production entity, equipment record or project location, resolved on the server with the user's permissions; offline, codes of items in an offline set resolve locally. | T3 | 5 |
| HB-MOBILE-014 | Code labels | Users generate printable label sheets (PDF) with QR codes for items, documents, entities and locations. A code encodes a permalink, so it keeps working after renames and moves. | T3 | 5 |

## Synchronization and device security

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-MOBILE-015 | Outbox and synchronization | Changes made on a device are queued in order and sent when a connection is available. Each operation is idempotent (HB-API-004); a rejected operation stays visible with its reason and can be edited or discarded by the user. When a record changed on the server in the meantime, both states are kept as a conflict record (the rule of HB-SESSION-008) and the user chooses the result; nothing is overwritten silently. | T3 | 5 |
| HB-MOBILE-016 | Device security | The offline store is encrypted with a key held in the platform's secure storage. Users can require biometric or passcode unlock; an administrator can set a maximum offline age after which a device must reconnect, and revoking a device (HB-MOBILE-002) removes its offline store at the next contact. | T3 | 5 |
