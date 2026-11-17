# ADR-0006: One record and workflow engine

Status: accepted

## Context

Issues, reviews, transmittals, change requests, change orders, requests for information,
submittals, forms, meetings, production tasks and notes share most of their behavior: numbering,
custom fields, statuses with allowed transitions, assignees and watchers, due dates and reminders,
comments, links, history, permissions, notifications, list views and exports. Several also need
step routing with participants and completion rules. Building each separately would multiply code
and inconsistencies across five modules.

## Decision

`arc-hub-model` provides one record engine and one workflow engine:

- A **record type** belongs to a family (issue, review, change order, RFI, task and so on) and
  defines numbering, field set, status workflow and permissions.
- A **record** stores common columns (number, title, status, assignees, watchers, dates, custom
  fields) and a typed family payload in its family's own table (for example BCF data for issues,
  affected records for change orders, entity and step for tasks).
- The **workflow engine** enforces statuses and transitions with required fields and permitted
  principals, and runs **step routes** (sequential steps, parallel branches, completion rules,
  rejection paths, due offsets, delegation, escalation) for families that need them.
- Family-specific invariants (BCF compliance, change-order reservation, task dependency graphs)
  live in the module crates as validators called by the engine.

## Consequences

- New record families are configuration plus a typed payload and validators, not a new subsystem.
- Lists, filters, exports, reminders and reports behave identically across families.
- The engine must stay generic; family-specific shortcuts are added as validators or actions, not
  as branches inside the engine.
- Each family still has its own table, so queries and constraints remain typed.

## Alternatives considered

- **Separate implementation per record family**: tailored, but duplicated behavior and drift.
- **Fully generic records without typed payloads**: maximum flexibility, but weak constraints,
  poor query performance and loss of standard compliance such as BCF.
- **External workflow engine**: powerful orchestration, but another component for operators and a
  second source of truth for record state.

## Reopen when

A record family needs workflow semantics that step routes and status workflows cannot express,
for example long-running compensation across external systems.
