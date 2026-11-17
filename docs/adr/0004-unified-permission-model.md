# ADR-0004: One permission model for every module

Status: accepted

## Context

Common data environments, product data management and production tracking each bring their own
permission ideas: folder levels with inheritance, state-based security in lifecycles, per-entity
and per-field rules, company-scoped visibility. Implementing these separately per module would
produce inconsistent behavior, duplicated code and permission leaks in search and listings, which
span modules.

## Decision

Arc Hub has one permission model, implemented once in `arc-hub-model`:

- Principals are users, groups, companies, project roles and service accounts.
- Grants assign an ordered level (Read, Download, Comment, Upload, Edit, Manage, plus Submit) on a
  scope (organization, project, folder, module) or rights (create, read variants, edit,
  transition, delete, administer) on a record type. The effective level is the maximum of
  applicable grants; folders inherit unless inheritance is broken.
- Container states and lifecycle states impose caps; the effective level on a version is the
  minimum of the folder level and the cap.
- Token scopes intersect with permissions for API calls; they never extend them.
- The evaluator answers checks, produces explanations and compiles list and search predicates to
  SQL. No module evaluates permissions itself.

## Consequences

- Users learn one model; the explanation view works everywhere.
- Search, listings and exports are filtered in the database with the same rules as single reads.
- The evaluator is the most security-critical code in the repository and carries a generated
  matrix test suite with a reference implementation.
- Some product-specific models (for example deny rules) are not expressible; restriction is
  achieved by breaking inheritance or by state caps.

## Alternatives considered

- **Per-module permission systems**: closer to each domain's conventions, inconsistent across the
  server and hard to secure in cross-module search.
- **Allow and deny rules with precedence**: expressive, but hard to explain and to compile to
  efficient SQL; deny rules are a frequent source of surprise.
- **External policy engine**: flexible policies, but another component and policy language for
  operators, and list filtering still needs database predicates.

## Reopen when

A customer requirement cannot be met by grants, inheritance and caps, and the requirement is
shared by more than one installation.
