# ADR-0005: Heavy processing and automation by headless Arc applications

Status: accepted

## Context

The Hub must view, index, compare, check and convert models and drawings, run clash tests, write
properties into files, repair references, copy designs, merge model branches and run customer
automation. All of this requires the format mappings and domain logic that live in the Arc
products. Products never depend on each other at compile time, and parsing hostile files inside
the server process would put every tenant at risk.

## Decision

- The server process never parses heavy formats or links geometry kernels. Format work runs in the
  job worker, which executes the command-line tool of the owning Arc application (for example
  `arc-review-cli` for derivatives and clash runs, `solidbuilder-cli` for structure extraction and
  copy design, `draftbuilder-cli` for plotting) in a sandboxed process.
- The worker drives the application through the Arc Automation Protocol, using the same commands
  as the desktop. Customer automation uses the same runner with job definitions and Python scripts.
- Each run has resource limits, private storage, no network except the Hub callback, and a token
  scoped to its project and inputs.
- Results are uploaded through the public API and keyed by input digest and application version.

## Consequences

- One implementation of each format mapping for desktop and server; server results match what
  users see in applications.
- Server crashes from malformed files are excluded by construction; a failed job is retried or
  reported.
- Workers need the Arc applications installed, which makes worker images larger and ties
  derivative behavior to application versions; derivatives record the converter version.
- Latency of derived data is bounded by queue and process start-up time; frequent small jobs are
  batched per worker process.

## Alternatives considered

- **Linking format crates into the server**: lower latency, but heavy dependencies in every web
  node, hostile parsing in process, and duplicated product logic.
- **Proprietary cloud conversion services**: contradicts self-hosting and open formats.
- **Plug-ins inside the server process**: fast, but weak isolation for customer code.

## Reopen when

Sandbox start-up cost prevents meeting derivative or clash latency targets after batching, or a
lightweight operation (for example thumbnail extraction from Arc packages) proves safe and common
enough to run in process.
