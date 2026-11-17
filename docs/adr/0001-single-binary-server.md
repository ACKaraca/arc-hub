# ADR-0001: Single server binary with roles

Status: accepted

## Context

Arc Hub is self-hosted by default. Many installations belong to small firms with one machine and
no operations staff; others need separate workers with GPUs or Windows, and scaled web tiers.
The suite principle "simple until proven otherwise" argues against splitting the server into
services before a second concrete need exists. Background work (derivatives, clash runs,
webhooks, email) must not slow interactive requests.

## Decision

Arc Hub ships as one executable, `arc-hub`, with roles selected at startup: `all` (web, API and
worker in one process), `web` (HTTP only), `worker` (job processing only) and `cache` (remote-site
blob cache). A second executable, `arc-hub-admin`, performs offline and maintenance tasks. All
roles share one configuration format and one database schema. Workers have no inbound ports; they
claim work from the database queue.

## Consequences

- A single-node installation is one binary, PostgreSQL and a directory.
- Scaling out means running more processes of the same binary with different roles; there is no
  service mesh, message broker or service discovery to operate.
- Every role must be correct when co-located with every other role; in the `all` role the worker
  runs with bounded concurrency and lower scheduling priority.
- The binary contains all features, including some an installation may not use; features are
  disabled by configuration, not by different builds.

## Alternatives considered

- **Separate services per module** (documents, product data, production, jobs): independent
  scaling, but many deployables, network contracts between them and distributed transactions for
  operations that span modules.
- **Server plus mandatory message broker**: decouples work, but adds a required component to every
  installation; PostgreSQL queues meet the expected load.
- **Hosted-only service**: contradicts self-hosting as the default deployment.

## Reopen when

A single role cannot meet the performance targets on the largest supported installation even
after horizontal scaling, or a module needs a release cadence that the shared binary blocks.
