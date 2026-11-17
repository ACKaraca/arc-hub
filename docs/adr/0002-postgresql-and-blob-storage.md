# ADR-0002: PostgreSQL and content-addressed blob storage

Status: accepted

## Context

The Hub stores two kinds of data with opposite properties: metadata (records, permissions,
versions, events, queues, search text) that needs transactions and rich queries, and file content
(often gigabytes per file) that is written once and read many times. Operators want few moving
parts, standard backup tools, and the choice between local disks and object storage.

## Decision

- **PostgreSQL is the only database.** It holds all metadata, the transactional outbox, the job
  queue (claimed with `FOR UPDATE SKIP LOCKED`), sessions, full-text indexes (per-language text
  search configurations for English and Turkish), and the element property index (typed columns
  plus JSONB for property sets). Row-level security policies back up tenant predicates.
- **Blobs are content-addressed and immutable.** Each file is stored under its SHA-256 digest in a
  storage location that is either a filesystem directory or an S3-compatible bucket. A blob is
  written and verified before the version that refers to it commits. Unreferenced blobs are
  deleted by garbage collection after a seven-day grace period.
- Derivatives are blobs too, keyed by source digest, kind and converter version.

## Consequences

- One database to back up, monitor and secure; one transaction covers a change, its audit record
  and its event.
- Deduplication is automatic; identical uploads cost storage once.
- Backups are consistent without coordination, because blobs never change and outlive their last
  reference by the grace period.
- Search relevance and features are bounded by PostgreSQL full-text search; semantic search uses
  an optional embedding provider rather than a second database.
- Very large element indexes increase database size; partitioning by organization and project is
  planned in the schema from the start.

## Alternatives considered

- **Separate search engine**: better relevance tuning and faceting at very large scale, but a
  second stateful service with its own consistency and permission-filtering problems.
- **Message broker for the queue and events**: unnecessary at expected volumes and breaks the
  single-transaction guarantee between a change and its event.
- **Embedded database (single-file)**: simplest for one node but blocks horizontal scaling and
  concurrent writers.
- **Storing file content in the database**: simple consistency, but poor for multi-gigabyte files,
  backups and direct-to-storage transfer.
- **Mutable files with path-based storage**: familiar layout, but loses deduplication and makes
  consistent backups and integrity checks hard.

## Reopen when

Search latency targets cannot be met on PostgreSQL for the largest supported installation, or
queue throughput becomes a measured bottleneck that partitioning does not solve.
