# Architecture decision records

Decisions specific to Arc Hub. Suite-wide decisions (repository topology, document model,
collaboration stages, extensibility, licensing and others) are recorded in
[arc-suite](https://github.com/ACKaraca/arc-suite/tree/main/docs/adr) and are not repeated here.

## Index

| ADR | Title | Status |
|---|---|---|
| [0001](0001-single-binary-server.md) | Single server binary with roles | accepted |
| [0002](0002-postgresql-and-blob-storage.md) | PostgreSQL and content-addressed blob storage | accepted |
| [0003](0003-server-rendered-web-client.md) | Server-rendered web client with the WebAssembly viewer | accepted |
| [0004](0004-unified-permission-model.md) | One permission model for every module | accepted |
| [0005](0005-headless-arc-runners.md) | Heavy processing and automation by headless Arc applications | accepted |
| [0006](0006-record-workflow-engine.md) | One record and workflow engine | accepted |

## Template

Copy this into `NNNN-<slug>.md` with the next free number. Numbers are never reused; a replaced
decision is marked `superseded by ADR-NNNN`.

```markdown
# ADR-NNNN: <decision in a few words>

Status: proposed | accepted | superseded by ADR-NNNN

## Context

The forces at play: requirements, constraints, suite rules that apply.

## Decision

What we do, stated so that an implementer can follow it.

## Consequences

What becomes easier, what becomes harder, what we accept.

## Alternatives considered

Each option with the reason it was not chosen.

## Reopen when

The observable trigger that would make us revisit this decision.
```
