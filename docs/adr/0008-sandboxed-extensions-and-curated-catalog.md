# ADR-0008: Sandboxed extensions and an administrator-curated catalog

Status: accepted

## Context

Organizations want pages and panels in the web client that show their own data and start their own
actions: a dashboard of project metrics, a form for an internal process, a panel that reads an
enterprise system. Running third-party scripts in the Hub's pages would break the content security
policy and the single permission model ([ADR-0003](0003-server-rendered-web-client.md),
[ADR-0004](0004-unified-permission-model.md)). The suite's extension model allows sandboxed
WebAssembly components with no ambient authority
([suite decisions](https://github.com/ACKaraca/arc-suite/tree/main/docs/adr)). The Hub also needs a
way for administrators to decide what is allowed to run, without becoming a store.

## Decision

- An extension is a package with a manifest that declares its kind, entry points and a permission
  set (API scopes, network origins, storage, placements). Content is addressed by digest and signed.
- Two kinds exist. A **WebAssembly component** runs on the server in the component model without
  ambient authority and returns declarative views that the host renders; no third-party script
  reaches the page. An **isolated frame** runs in a sandboxed frame from a separate origin with a
  content security policy limited to the declared origins and no access to Hub cookies; it talks to
  the host through a message protocol and a short-lived token of the viewing user.
- An extension's effective rights are the intersection of its approved permission set and the
  permissions of the user it acts for. It cannot exceed the user, and its calls are attributed to it.
- Administrators curate a catalog. Only approved versions can be enabled; a version that asks for
  more permissions needs a new approval; a kill switch disables an extension at once.
- The catalog handles no payments, prices or ratings. Registries are contacted only when an
  administrator has configured one.
- Developer mode loads unsigned extensions for a test project and is off by default.
- The Hub's own enhancement scripts stay first-party, hashed and few; extensions never add scripts
  to the Hub's origin.

## Consequences

- Customers can extend the client without forking the server or weakening its security model.
- The component model gives strong isolation and resource limits; declarative views limit what a
  component can show, which is acceptable for tables, forms, charts and text.
- Frame extensions need a stable message protocol that is versioned and documented, which becomes
  a public contract.
- Administrators carry the review work; the permission diff on update keeps it small.

## Alternatives considered

- **Scripts injected into Hub pages**: simplest for authors, but defeats the content security
  policy and lets an extension act with the full rights of the page.
- **Only custom actions that open external pages**: safe, but no data or controls inside the Hub.
- **A public marketplace with payments**: contrary to self-hosting and to the product principles.
- **Native server plug-ins**: no isolation for customer code.

## Reopen when

Declarative views prove too limited for the leading extension use cases, or the isolated-frame
model cannot meet the accessibility baseline for extension pages.
