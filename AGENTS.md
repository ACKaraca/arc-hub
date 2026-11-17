# AGENTS.md

Arc Hub is the self-hostable collaboration and data-management server of the Arc suite: common data
environment, product data management, production tracking and an automation API in one Rust server
(axum, PostgreSQL, filesystem or S3-compatible blob storage) with a server-rendered web client.

## Status and where to start

Design phase: no code and no builds exist. Read in this order:
[`docs/product/vision.md`](docs/product/vision.md),
[`docs/product/capabilities/README.md`](docs/product/capabilities/README.md),
[`docs/architecture/overview.md`](docs/architecture/overview.md),
[`docs/architecture/domain-model.md`](docs/architecture/domain-model.md),
[`docs/architecture/security.md`](docs/architecture/security.md),
[`docs/roadmap.md`](docs/roadmap.md) and the [ADRs](docs/adr/README.md).

## Repository layout (target)

```
crates/arc-hub-model        domain kernel: tenancy, permissions, properties, records, workflows, events
crates/arc-hub-documents    Documents module (files, issues, reviews, transmittals, ISO 19650, records)
crates/arc-hub-collab       design packages, multi-user sessions, branches, model sets, clash results
crates/arc-hub-pdm          product data: lifecycles, revisions, items, BOMs, change orders
crates/arc-hub-production   production tracking: entities, tasks, submissions, playlists, planning
crates/arc-hub-store        PostgreSQL repositories and migrations, outbox, queue, blob store
crates/arc-hub-http         REST API, OAuth server, openCDE and BCF APIs, WebDAV, SCIM, push, MCP
crates/arc-hub-web          server-rendered web client and viewer page
crates/arc-hub-worker       job worker, sandboxed Arc runners, webhook and email delivery
apps/arc-hub                server binary (roles: all, web, worker, cache)
apps/arc-hub-admin          administration command
apps/arc-hub-sync           desktop sync agent (milestone M3)
docs/                       product, architecture, roadmap, ADRs
xtask/                      workspace tasks
```

## Commands

No build exists yet. Once code lands, these apply:

- `cargo fmt --check`
- `cargo clippy --workspace --all-targets -- -D warnings`
- `cargo nextest run --workspace`
- `cargo xtask layers`

## Suite rules

These rules apply in every Arc repository. The canonical text is
[`docs/engineering/agent-rules.md`](https://github.com/ACKaraca/arc-suite/blob/main/docs/engineering/agent-rules.md)
in `arc-suite`; change it there first.

1. **A person is accountable.** Whoever runs an agent answers for what it submits.
2. **Start from an issue.** Search issues and pull requests, check that nobody else is assigned,
   assign yourself and set the status to *In Progress* before changing files. When you finish,
   comment with what changed, the commit or pull request link and open items, then update the status.
   *Done* requires met and verified acceptance criteria.
3. **Verify, do not guess.** Open a definition before using it. Confirm a cause in the code.
4. **The document is the authority and everything is a command.** No change reaches a document
   except through a validated command inside a transaction. Programmatic paths never open dialogs.
5. **Respect the layers.** A crate depends only on lower layers. Nothing below the UI layer depends
   on windowing, GPU or UI crates. Products never depend on other products.
6. **Never crash, never lose work.** No `unwrap`, `expect`, `panic!`, `todo!` or `unreachable!` in
   production code. No `unsafe` outside the FFI crates named in an ADR. Treat every file and message
   as hostile: bound recursion, allocation and counts. A crash fix ships with a regression test.
7. **Clean room.** Implement from published specifications, observed behavior and original
   reasoning. Never copy or consult another product's code, decompiled output, file-format reverse
   engineering, documentation text, icons, fonts, templates or sample files. Do not name our
   features after another company's features.
8. **Licenses.** Add a dependency only if its license is on the allowlist in
   `docs/engineering/dependency-policy.md` in `arc-suite`. Record every asset's origin and license.
9. **Rust only in desktop applications.** No web view, no JavaScript or TypeScript.
10. **Small, tested changes.** One concern per pull request, with the test that fails without it.
    Update the documents that describe the behavior you change in the same pull request.
11. **Report only what ran.** Say what you tested and what you could not run.
12. **Ask a human before** adding a license outside the allowlist, publishing or releasing,
    changing a file format or public protocol, adding a new repository or remote, spending money,
    handling secrets, or any destructive git operation. Otherwise keep working.
13. **Keep secrets and personal data out** of repositories, logs and requests.
14. **Commit directly to `main` for now.** Until the project has a second maintainer and this
    repository has reached its MVP release (for `arc-suite` and `arc-core`: the first product MVP),
    work on `main`: no feature branches and no pull requests. Wherever these rules or the documents
    say "pull request", read "commit". Run the checks before every push and name the issue in the
    commit message. This rule is temporary and is deleted from a repository when that point is
    reached.

## Repository invariants

- Every domain state change is a registered operation executed in one PostgreSQL transaction
  together with its audit record and its event; no other path writes domain state.
- Permissions are evaluated only by the evaluator in `arc-hub-model`; lists and search are
  filtered in SQL, never after fetching ([ADR-0004](docs/adr/0004-unified-permission-model.md)).
- Every organization-scoped query is constrained by `org_id`; nothing reads across organizations
  except named, audited server-administration functions.
- Versions, decisions, sent transmittals and audit records are immutable; blobs are
  content-addressed and written before the version that references them commits.
- The server never parses heavy formats or links geometry kernels; that work runs in sandboxed
  headless Arc applications on workers ([ADR-0005](docs/adr/0005-headless-arc-runners.md)).
- Domain crates do not depend on axum, SQL drivers or storage SDKs; the server uses no `arc-gpu`,
  `arc-ui` or `arc-app`.
- The web client works without JavaScript for reading and forms; enhancement scripts are
  first-party, hashed and few ([ADR-0003](docs/adr/0003-server-rendered-web-client.md)). Customer
  extensions run only as sandboxed components or isolated frames with an approved permission set
  ([ADR-0008](docs/adr/0008-sandboxed-extensions-and-curated-catalog.md)).
- Conflicts keep both sides, also between replicated sites and between a device and the server; no
  code path discards a user's upload or change set.
- Migrations are forward-only and follow expand then contract.
- No outbound network call happens unless an administrator configured it.

## Documentation rules

- Never mention planning sources, and never describe Arc as derived from, copying, "like" or
  equivalents; never name features after another company's features.
- Other companies' names appear only as formats, standards and protocols, in factual
  interoperability statements, once in the positioning paragraph of `vision.md`, and for libraries.
- Use Arc's own descriptive vocabulary.
- Each fact once: suite-wide rules live in `arc-suite` and are linked. This file is the exception.
- Write behavior in original words; do not copy another product's documentation, dialog text or
  default value lists.
- Capability IDs (`HB-<AREA>-<NNN>`) are permanent and used once; status lives in issues, not in
  capability tables.

## Issue workflow

Search issues and pull requests first; confirm nobody is assigned; assign yourself and set
*In Progress*. Statuses: Backlog, Ready, In Progress, In Review, Blocked, Done (only when acceptance
criteria are met and verified). When finishing, comment with what changed, the commit or pull
request link and open items, update the status, and open follow-up issues after checking for
duplicates. Labels: `type: …`, `tier: 1|2|3`, `phase: 0…5`, `area: <token>`. Commits follow
Conventional Commits with a DCO sign-off.

## Verification

Run the narrowest relevant check first (one crate's tests, one migration test, one permission
matrix case), broaden only when needed, and report only what ran. Changes to permissions,
tenancy, migrations or the audit trail always run the permission matrix and store tests.

## Ask a human about

Changes to the permission model, the public API or event schemas, migrations that rewrite data,
new outbound network flows, sandbox limits, anything touching secrets or keys, new dependencies,
and any change to the suite contracts (`arc-hub-api`, document package, Arc Scene).
