# API, clients and webhooks

The public programming surface of Arc Hub: the REST API, client authorization, service identities,
events and webhooks, and the openCDE APIs. The web client and Arc applications use the same domain
services as every external client. The design is described in
[`api.md`](../../architecture/api.md).

Domain objects:

- **Resource**: an addressable object of the API with identifier, ETag and links.
- **OAuth client**: a registered application with redirect URIs and allowed scopes.
- **Service account**: a non-human principal with grants and key-based credentials.
- **Personal access token**: a scoped, expiring token of a user.
- **Event**: a CloudEvents record of a state change with a per-organization sequence number.
- **Webhook subscription** and **delivery attempt**.
- **Custom action**: an administrator-defined action shown on records.

## REST API

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-API-001 | Resource API | Every operation available in the web client is available as a JSON REST operation with the same permission checks. The server publishes its OpenAPI 3.1 description at a fixed path. | T1 | 2 |
| HB-API-002 | Versioning | The major version is part of the path. Changes within a major version are additive; deprecated operations announce removal through Deprecation and Sunset headers and remain for at least two minor server releases. | T1 | 2 |
| HB-API-003 | Pagination, filtering and sorting | Collections use opaque cursor pagination (at most 500 items per page), the filter grammar of HB-SEARCH-005, sorting by declared fields and sparse field selection. | T1 | 2 |
| HB-API-004 | Concurrency and idempotency | Updates require If-Match with the resource ETag and fail with 412 on mismatch; POST operations accept an idempotency key honored for 24 hours; bulk operations declare whether they are atomic or return per-item results. | T1 | 2 |
| HB-API-005 | Error model | Errors are RFC 9457 problem details with stable error codes and field-level validation messages and never expose internal details. | T1 | 2 |
| HB-API-006 | Protective rate limits | Administrators set per-token request limits that protect the server; exceeding them returns 429 with Retry-After. Nothing is metered or billed. | T1 | 2 |
| HB-API-007 | Long-running operations | Operations that outlast a request return 202 with an operation resource that reports progress and result and emits an event on completion. | T1 | 2 |
| HB-API-008 | File transfer | Uploads use tus (HB-FILE-014), downloads support HTTP Range requests, and with S3-compatible storage the server can redirect to presigned URLs. | T1 | 2 |
| HB-API-009 | Client libraries | The Rust client is the `arc-hub-api` crate of arc-core; an official Python client offers typed resources plus helpers for transfers and event feeds. | T1 | 2 |
| HB-API-010 | API explorer | Signed-in users browse the API reference in the web client and try read operations with their own permissions. | T2 | 2 |

## Authorization of clients

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-API-011 | Authorization server | The server issues access tokens to registered clients through authorization code with PKCE, device authorization, and client credentials for service accounts only. Scopes exist per module with read and write variants, users see a consent screen, access tokens live 15 minutes and refresh tokens rotate. | T1 | 2 |
| HB-API-012 | Client registration | Organization administrators register clients with name, redirect URIs and allowed scopes; Arc applications use pre-registered public clients. | T1 | 2 |
| HB-API-013 | Service accounts | Service accounts are organization principals with grants like users and authenticate with a client secret or a private-key JWT; keys rotate with an overlap period, and the audit trail names the service account on every action. | T1 | 2 |
| HB-API-014 | Personal access tokens | Users create tokens with chosen scopes, expiry of at most one year and optional network restriction; the last use is shown and tokens are revocable. | T1 | 2 |

## Events and webhooks

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-API-015 | Event model | Every state change emits a CloudEvents 1.0 event (identifier, source, type such as a version-created type, subject, time, data) written in the same database transaction as the change, with a monotonic sequence number per organization. | T1 | 2 |
| HB-API-016 | Webhook subscriptions | Administrators and authorized clients subscribe an HTTPS endpoint to event types filtered by project, folder or record type, with a signing secret. | T1 | 2 |
| HB-API-017 | Delivery guarantees | Delivery is at least once with an HMAC-SHA256 signature over timestamp and body, retried with exponential backoff for 24 hours. A subscription is disabled after a configured number of consecutive failures and its owner is notified; the delivery log supports replay. | T1 | 2 |
| HB-API-018 | Event feed | Clients that cannot receive calls read events after a sequence number from a feed endpoint; events are kept for 30 days by default. | T1 | 2 |
| HB-API-019 | Permission-filtered payloads | Event payloads contain only data the subscription owner can read; subscriptions can choose minimal payloads with links instead of embedded data. | T1 | 2 |
| HB-API-020 | Push channels | Browsers and Arc applications receive live updates (notifications, markups, sessions, job progress) over authenticated server-sent events or WebSocket channels. | T1 | 2 |

## Standards and extensions

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-API-021 | openCDE Foundation API | The server implements the buildingSMART Foundation API for version discovery and authentication discovery, alongside the BCF API (HB-ISSUE-021). | T1 | 2 |
| HB-API-022 | openCDE Documents API | The server implements the buildingSMART Documents API so tools that support it select documents for download and upload new versions through the Hub's own selection pages. | T2 | 3 |
| HB-API-023 | Custom actions | Administrators add actions to record and document menus that open a URL with context parameters or start a job (HB-JOB-006); actions are visible only to the principals they name. | T2 | 3 |
