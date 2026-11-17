# Security architecture

The threat model of Arc Hub and the controls that answer it: tenancy and isolation, the
permission model, the audit trail, secrets, the boundaries of automation and agents, and data
residency under self-hosting. Vulnerability reporting is described in
[`SECURITY.md`](../../SECURITY.md).

## Assets and actors

| Asset | Why it matters |
|---|---|
| Design content (models, drawings, product data, media) | Commercially sensitive, sometimes export-controlled or classified by clients. |
| Records and decisions (reviews, approvals, signatures, transmittals) | Contractual evidence; integrity matters more than secrecy. |
| Identity data and credentials | Gives access to everything above. |
| Audit trail | Evidence of who did what; must be tamper-evident. |
| Server configuration and keys | Compromise gives full control. |

| Actor | Trust |
|---|---|
| Server administrator | Fully trusted for the installation; actions audited. |
| Organization administrator | Trusted within one organization only. |
| Member, guest | Trusted within their grants. |
| External recipient (transmittal link, client review link) | Untrusted except for the shared items. |
| Service account, job, integration | Trusted within scopes and grants of its identity. |
| Agent acting for a user | Bounded by the user's permissions and the agent boundaries below. |
| Anonymous network client | Untrusted. |

## Threat model

| Threat | Examples | Controls |
|---|---|---|
| Cross-tenant access | Guessing identifiers of another organization; a query missing a tenant filter | Organization-scoped repositories, row-level security as a second layer, permission tests per endpoint, unguessable identifiers. |
| Privilege escalation inside a tenant | Editing a folder grant without Manage; changing a record type's workflow as a member | Single permission evaluator; operations declare required rights; tests over generated permission matrices. |
| Information leak through search and listings | Counts or snippets revealing unreadable documents | Permission predicates compiled into SQL; no post-filtering; leak tests (HB-SEARCH-002). |
| Account takeover | Phishing, password reuse, stolen refresh token | OpenID Connect delegation, MFA, WebAuthn, refresh token rotation with reuse detection, session revocation. |
| Malicious files | Crafted IFC, DWG, PDF or archive exploiting a parser; ZIP bombs | Parsing only in sandboxed worker processes with limits; bounded decompression; the server never parses heavy formats in process. |
| Malicious job or plug-in | A script exfiltrating data or attacking the network | Sandbox without network except the Hub callback; scoped job token; resource limits; plug-ins without ambient authority. |
| Web attacks | Cross-site scripting (XSS), CSRF, clickjacking, injection | Templates escape by default; strict Content Security Policy; SameSite cookies plus per-form tokens; frame-ancestors restrictions; parameterized SQL only. |
| Outbound request abuse (SSRF) | Webhook targets or import URLs pointing at internal addresses | Outbound requests through a client that blocks private, loopback and link-local ranges unless an administrator allowlists them. |
| Tampering with evidence | Editing decisions or audit records in the database | Immutable decision rows, hash-chained audit records with external digests, verification command. |
| Denial of service | Upload floods, expensive queries | Quotas, rate limits, query cost limits, bounded page sizes, job fair scheduling. |
| Data loss | Disk failure, faulty upgrade, operator error | Immutable blobs, trash and retention, backups with verification, forward-only tested migrations. |

## Tenancy and isolation

- Every organization-scoped table carries `org_id`. Repository functions in `arc-hub-store` take
  an organization context as a required argument and add the predicate; there is no repository
  function that reads across organizations, except server administration functions that are
  separately named and audited.
- PostgreSQL row-level security policies on organization-scoped tables enforce the same predicate
  using a per-transaction setting, as defense in depth against a missing predicate.
- Blobs are stored per storage location; organizations can be given separate locations
  (HB-ORG-004). Blob keys are digests, but a blob is served only through a version the reader may
  read, so knowing a digest grants nothing.
- Caches and push channels are keyed by organization and principal.
- Cross-organization sharing (HB-PKG-011) is the only path for content between tenants; it is
  explicit, capped by the owner and audited on both sides.

## Permission model

The model is defined once in `arc-hub-model` ([ADR-0004](../adr/0004-unified-permission-model.md)):

1. **Principals**: users, groups (nested), companies, project roles, service accounts.
2. **Scopes**: organization, project, folder (inheriting), record type, module.
3. **Grants**: principal, scope, level. Effective level is the maximum of applicable grants.
4. **State caps**: container and lifecycle states impose a maximum level per principal on the
   versions in that state. Effective level on a version is the minimum of the folder level and
   the cap.
5. **Record permissions**: per record type, rights to create, read (all, own company, own and
   assigned), edit, transition, delete and administer, plus field visibility.
6. **Token scopes**: intersect with the above for API calls.

The evaluator answers single checks, explains its answer (HB-ACCESS-023), and compiles list and
search predicates to SQL so that every list is filtered in the database. Permission changes take
effect on the next request; decision caches are invalidated by grant-change events.

## Audit trail

- Audit records are written in the same transaction as the operation they describe; an operation
  cannot succeed without its audit record.
- Records are hash-chained per organization (`hash = SHA-256(prev_hash || canonical record)`); a
  daily digest of the chain head is written to the server log and can be exported to an external
  store chosen by the operator.
- Records hold actor, acting-for principal, client, network address, action, object and a summary
  of changes; file content and secret values are never recorded.
- `arc-hub-admin audit verify` recomputes chains and reports the first inconsistency.
- Erasure requests pseudonymize personal fields through a mapping table outside the chain, so the
  chain stays verifiable (HB-ACCESS-010).

## Secrets

- Configuration refers to secrets by reference: environment variables, files with restricted
  permissions, or an external secret store reached through a documented interface. Secrets are
  never stored in the database in plain text.
- Values the server must store (webhook secrets, identity provider client secrets entered in the
  web client, SMTP passwords) are encrypted with a data-encryption key, itself encrypted with a
  key-encryption key from configuration. `arc-hub-admin keys rotate` re-encrypts.
- Passwords use Argon2id; tokens and recovery codes are stored as hashes; token signing keys
  rotate with an overlap period and are published through the server's key set endpoint.
- Logs and traces redact authorization headers, cookies, tokens, secret references' values and
  file content.

## Automation and agent boundaries

| Boundary | Rule |
|---|---|
| Job identity | Each job definition runs as its own service account; runs get a token limited to the run's project, inputs and time limit. |
| Job sandbox | Separate process with private temporary storage, CPU, memory, disk and time limits, no network except the Hub endpoint, no access to server configuration or other runs. |
| Guard plug-ins | WebAssembly without ambient authority, receives only the operation's records, 2-second time limit, fault means refuse. |
| Automation rules | Run as the rule's service identity; causation depth limit; every execution audited. |
| Webhook targets | HTTPS only by default; private network ranges blocked unless allowlisted. |
| Extensions | Frames run in a sandboxed frame from a separate origin with a content security policy limited to declared origins; components run without ambient authority and with resource limits. Rights are the intersection of the approved permission set and the acting user's permissions; every call is audited and attributed (HB-INTEG-013 to HB-INTEG-015, HB-INTEG-020). |
| Bridge workers | Run a licensed conversion library as a separate process in the job sandbox, with no network except the Hub endpoint; their output is treated as untrusted input like any uploaded file. |
| Mobile devices | The offline store is encrypted with a key in the platform's secure storage; revocation invalidates tokens at once and wipes the store at the next contact; push payloads carry no content (HB-MOBILE-012, HB-MOBILE-016). |
| Replication peers | Mutual authentication; every received operation is validated by the receiving server's own operation registry and permission evaluator; revocations take priority (HB-OPS-023, HB-OPS-027). |
| Stream sources | Authenticated per source, validated against the stream definition and rate-limited (HB-DATA-015). |
| Agents | Authenticate as a user or service account; MCP tools map one-to-one to API operations with normal permission checks; write tools produce drafts that a person applies; agents cannot change permissions, delete permanently, decide reviews, sign or release (HB-ASSIST-004). |
| Assistant providers | Content leaves the server only to configured endpoints, for allowed content types, with each request audited (HB-ASSIST-002). |

## Data residency under self-hosting

- The operator chooses where PostgreSQL and blob storage run; Arc Hub makes no outbound connection
  unless configured (HB-OPS-008). There is no telemetry.
- Each organization can be assigned its own storage location, for example a bucket in a specific
  region or a filesystem on specific hardware; derivatives are stored with originals.
- Outbound flows that move data are explicit and listed per organization: webhooks, extracts,
  cross-server federation, assistant providers, email (which carries links, not files).
- Organization export (HB-ADMIN-012) gives the customer all of its data in documented formats,
  so leaving a server never depends on its operator's cooperation beyond running the export.

## Web client hardening

- Content Security Policy forbids inline script and restricts script sources to the server's own
  hashed assets; the WebAssembly viewer is loaded with `wasm-unsafe-eval` scoped to the viewer page.
- Cookies are `HttpOnly`, `Secure`, `SameSite=Lax`; state-changing forms carry a per-session token.
- User-supplied Markdown is rendered to a restricted HTML subset on the server; uploaded HTML and
  SVG are served as downloads with `Content-Disposition: attachment` and a sandboxing policy.
- Share links and external review links are random 256-bit tokens, stored hashed, expiring and
  revocable.
