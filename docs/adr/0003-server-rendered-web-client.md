# ADR-0003: Server-rendered web client with the WebAssembly viewer

Status: accepted

## Context

Arc Hub needs a browser client for people who do not run Arc desktop applications: reviewers,
document controllers, clients, site staff and producers. The suite builds desktop applications in
Rust only, without web views or JavaScript; the browser client is the single documented exception,
and it should keep that exception as small as possible. The client must be accessible, fast on
modest hardware, usable without a build toolchain for front-end frameworks, and able to show
models and drawings with the same engine as Arc Review.

## Decision

- Pages are rendered as HTML on the server by `arc-hub-web` from typed templates, using the same
  domain operations as the REST API. Every page and form works without JavaScript for reading,
  navigation and form submission.
- Progressive enhancement adds partial page updates, inline editing, drag and drop upload, live
  updates over server-sent events and keyboard shortcuts through a small set of first-party
  scripts served as hashed static assets. No client-side application framework and no Node.js
  build step are used.
- Viewing uses the Arc Review viewer compiled to WebAssembly, loaded as a versioned static asset on
  viewer pages. The viewer is built in the `arc-review` repository and consumed as a release
  artifact; this repository has no compile-time dependency on it.
- Localization uses the same Fluent catalogues on the server; accessibility (landmarks, labels,
  focus order, contrast) is tested on rendered HTML.

## Consequences

- Most of the client is Rust, testable without a browser, and secured by a strict Content Security
  Policy without inline scripts.
- Interactions that need rich client state (the viewer, the media player, the Gantt chart, markup
  drawing) live in the WebAssembly viewer or in small dedicated scripts, each with its own tests.
- Highly interactive grid editing is less fluid than in a single-page application; inline editing
  and partial updates cover the common cases.
- The viewer version used by the Hub is pinned per release and recorded on the viewer page.
- Browser authoring loads the browser build of the owning Arc product as a further versioned
  static asset on an editing page (HB-FILE-028); customer extensions run only as sandboxed
  components or isolated frames ([ADR-0008](0008-sandboxed-extensions-and-curated-catalog.md)).

## Alternatives considered

- **Single-page application in a JavaScript framework**: rich interactions, but a large
  JavaScript codebase, a second toolchain and duplicated validation and authorization logic.
- **Whole client in Rust compiled to WebAssembly**: one language, but slow first load, weak
  accessibility of canvas-based UI and no server-rendered fallback.
- **Native desktop client only**: excludes the participants who most need a browser.

## Reopen when

A required workflow cannot meet its usability acceptance criteria with server rendering and
progressive enhancement, or accessibility tests show the enhancement scripts cannot meet the
baseline.
