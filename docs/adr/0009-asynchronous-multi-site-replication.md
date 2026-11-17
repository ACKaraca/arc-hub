# ADR-0009: Asynchronous multi-site replication that keeps both sides of a conflict

Status: accepted

## Context

Organizations with several offices on slow or unreliable links need to work at local speed on the
same projects. Cache nodes (HB-OPS-022) serve downloads but not writes, and folder federation
(HB-PKG-013) copies content between organizations without sharing history. A consensus layer would
make every write depend on the slowest link, and Arc Hub adds none
([deployment](../architecture/deployment.md)). Versions, decisions, transmittals and audit records
are immutable, and no code path may discard a user's upload or change
([AGENTS.md](../../AGENTS.md)).

## Decision

- Servers form a replication group. Each peer has its own PostgreSQL database, blob storage and
  identity provider configuration, and a stable site identifier.
- Replication is asynchronous and per project. Domain operations, their events and audit records are
  exchanged as ordered streams per origin peer, stamped with site identifier and logical time;
  delivery resumes from the last acknowledged position and applies each operation once.
- Conflict rules follow the invariants of the server. Concurrent versions are all kept and ordered
  by logical time and site identifier. Scalar field conflicts keep the later value and record the
  other. Immutable objects are created at one peer and never merge. A transition that is invalid at
  the receiving peer becomes a conflict record. Locks are held at the document's home peer.
- Conflict records are listed and resolved by people; resolutions replicate like any change.
- Blob placement is a policy per project and peer; every fetched blob is verified by digest.
- Revocations replicate with priority.

## Consequences

- Each office works without waiting for other sites, and an unreachable peer never blocks the rest.
- Users can see versions and conflicts that arose elsewhere; the interface must show origin and
  make resolution easy.
- Recovery after a site loss is bounded by replication lag, which the documentation states.
- Operations must be deterministic and carry enough context to replay at another peer, which
  constrains the operation registry.
- Replication adds a permission and audit surface that is covered by the same permission matrix
  and store tests.

## Alternatives considered

- **A single server with cache nodes only**: no local writes at remote sites.
- **Database-level multi-master replication**: replicates data without understanding domain
  invariants such as immutable versions and hash-chained audit records.
- **A consensus layer across sites**: strong consistency at the cost of availability over slow links.
- **Last-writer-wins for everything**: silently discards work.

## Reopen when

Conflict records accumulate faster than people resolve them in pilot use, or a deterministic
ordering of concurrent versions proves confusing enough to need per-site version series.
