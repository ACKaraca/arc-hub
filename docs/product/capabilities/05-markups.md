# Markups

Graphic annotation of drawings, sheets and models in the browser and in Arc applications. Markups
belong to a version, are private until published, and can become issues.

Domain objects:

- **Markup**: a graphic or measurement annotation in document coordinates of one version.
- **Markup set**: a named group of markups on a version, with an owner and visibility.
- **Stamp**: a reusable image or text template with fields.

## Drawing markups

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-MARKUP-001 | 2D markup tools | Users with Comment draw pen strokes, lines, arrows, rectangles, ellipses, polylines, polygons, clouds, text, callouts and highlights with color, width, fill, opacity and text size. Markups are stored in page coordinates so they stay aligned at every zoom level. | T1 | 2 |
| HB-MARKUP-002 | 3D markups | In models, users draw on a saved viewpoint and place pins at points or elements; opening the markup restores the viewpoint and highlights the elements by identifier. | T1 | 2 |
| HB-MARKUP-003 | Measurement markups | Linear, polyline, area, count and angle markups store their value with unit and the calibration used; if the calibration changes, the markup shows both the stored and the recalculated value. | T2 | 2 |
| HB-MARKUP-004 | Stamps | Organizations and projects keep stamp libraries of images or text templates with fields (user, date, status) that are filled when placed. | T2 | 2 |
| HB-MARKUP-005 | Markup attachments | A markup can carry photos or files, uploaded or referenced from project documents. | T2 | 2 |

## Publishing and organization

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-MARKUP-006 | Draft and published markups | New markups are private drafts of their author. Publishing makes them visible to everyone who can read the version; editing a published markup creates a new markup revision and keeps the earlier one in its history. | T1 | 2 |
| HB-MARKUP-007 | Markup sets | Markups are grouped into named sets per version, toggled on and off, and filtered by author, company, date and status. | T1 | 2 |
| HB-MARKUP-008 | Markup comments and status | Each markup has a comment thread and a status of open, addressed or void; the markup list of a version shows status counts. | T2 | 2 |
| HB-MARKUP-009 | Markups in reviews | Markups made inside a review belong to the review and are visible to its participants; when the review completes, the initiator can publish them to the versions. | T1 | 2 |
| HB-MARKUP-010 | Markup permissions | Creating markups requires Comment; deleting other users' published markups requires Manage. Users can add markups to versions in published states because the state cap for Comment is set per state. | T1 | 2 |
| HB-MARKUP-011 | Carry markups forward | When a new version is added, users carry selected markups forward: 2D markups to the same page and position, 3D markups by element identifier. Carried markups are labeled with their source version. | T2 | 3 |

## Conversion and exchange

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-MARKUP-012 | Markup to issue | A markup converts into an issue that keeps the markup, its pin and a snapshot image, and links back to it. | T1 | 2 |
| HB-MARKUP-013 | Live markup updates | When several users have the same version open, published markups and status changes appear for all of them within 2 seconds without reloading. Only the author edits a markup. | T2 | 3 |
| HB-MARKUP-014 | Markup export | Markups export into a PDF either flattened into the page or as PDF annotations; 3D markups export as BCF viewpoints. | T2 | 2 |
| HB-MARKUP-015 | Markup import | PDF annotations in an uploaded version import into a markup set, labeled as imported with their original author text. | T3 | 3 |
| HB-MARKUP-016 | Markup register | A table of markups per version or per review shows author, status and comments and exports as CSV. | T2 | 2 |
