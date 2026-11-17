# Identity and access

User accounts, authentication, provisioning, membership and the permission model used by every
module. The model is evaluated in one place on the server and explained to users on request.

Domain objects:

- **User account**: a server-level person record linked to one or more identities.
- **Identity link**: the binding between an account and an external identity provider subject.
- **Session**: a browser or client sign-in with lifetime and revocation.
- **Membership**: a user's participation in an organization or project, with roles and module access.
- **Group** and **role**: principals used for assignment and permission grants.
- **Grant**: a principal, a scope and a permission level.
- **State cap**: a ceiling on permission levels imposed by a container state or lifecycle state.

## Accounts and authentication

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ACCESS-001 | User accounts | An account holds a stable identifier, display name, verified primary email, locale, time zone and avatar. One account can be a member of several organizations on the same server. | T1 | 2 |
| HB-ACCESS-002 | OpenID Connect sign-in | Server or organization administrators configure OpenID Connect providers (issuer, client identifier, secret reference, scopes, claim mapping). Users sign in with the authorization code flow and PKCE; accounts link by provider subject, or by verified email on first sign-in; just-in-time account creation is a per-provider option. | T1 | 2 |
| HB-ACCESS-003 | Local accounts | Installations without an identity provider can enable built-in accounts with passwords of at least 12 characters, hashed with Argon2id and optionally checked against an offline list of breached passwords. Local accounts can be disabled server-wide. | T1 | 2 |
| HB-ACCESS-004 | Multi-factor authentication | Local accounts support TOTP and WebAuthn security keys and passkeys; an organization can require a second factor. For OpenID Connect sign-in the second factor is delegated, and the organization can require a specific authentication context claim. | T1 | 2 |
| HB-ACCESS-005 | SAML sign-in | Organizations can configure SAML 2.0 identity providers with metadata import, signed assertions required and attribute mapping equivalent to the OpenID Connect claim mapping. | T2 | 3 |
| HB-ACCESS-006 | SCIM provisioning | The server exposes SCIM 2.0 Users and Groups endpoints per organization. Provisioned users are created, updated and deactivated by the directory; deactivation revokes sessions and tokens within 60 seconds; directory groups become read-only organization groups. | T2 | 2 |
| HB-ACCESS-007 | Browser sessions | Browser sessions use an HttpOnly, Secure, SameSite cookie with idle timeout and absolute lifetime from the organization's session policy. Users list and revoke their own sessions; administrators revoke all sessions of a user. | T1 | 2 |
| HB-ACCESS-008 | Desktop and command-line sign-in | Arc applications and command-line tools sign in through OAuth device authorization or a loopback redirect with PKCE. Refresh tokens rotate on every use and are revocable per device from the user's security page. | T1 | 2 |
| HB-ACCESS-009 | Account deactivation | A deactivated user cannot sign in and keeps authorship on all records. Their locks stay visible and can be released by administrators; a reassignment view lists their open assignments (issues, review steps, tasks) for bulk transfer. | T1 | 2 |
| HB-ACCESS-010 | Personal data requests | A user exports their profile and a list of content they authored. Erasure replaces the display name and email in records with a pseudonym while keeping the audit chain verifiable. | T2 | 3 |

## Membership and groups

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ACCESS-011 | Organization membership | Users join an organization by email invitation (valid 14 days), by just-in-time provisioning or by SCIM, with an organization role of owner, administrator, member or guest. Pending invitations can be resent or revoked. | T1 | 2 |
| HB-ACCESS-012 | Project membership | Project administrators add users, groups or whole companies to a project and set each member's company, project roles and module access levels; members can be added in bulk from CSV with per-row results. | T1 | 2 |
| HB-ACCESS-013 | Groups | Organization groups contain users and nested groups (nesting depth at most 5, cycles rejected). Groups managed by an identity directory are read-only in the web client. | T1 | 2 |
| HB-ACCESS-014 | Project roles | Roles (for example document controller or structural engineer) are defined per organization with optional project-level additions. Members can hold several roles; roles act as assignees in records and workflows and as permission principals. | T1 | 2 |
| HB-ACCESS-015 | Guests | Guests see only the projects they are invited to, cannot browse the organization directory and see only people and companies of shared projects. Guests use every module their grants allow; the server has no license tiers. | T1 | 2 |
| HB-ACCESS-016 | Access requests | A user who follows a link to a resource they cannot read can request access. Project administrators, or holders of Manage on the folder, approve with a chosen level or deny; unanswered requests expire after 30 days. | T2 | 2 |

## Permission model

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ACCESS-017 | Folder permission levels | Folders use ordered levels: Read (metadata and viewer), Download (plus original files), Comment (plus markups, issues and comments), Upload (plus new documents and versions), Edit (plus metadata, rename, move, delete, locks) and Manage (plus permissions, folder settings, subfolders). A separate Submit level lets a principal upload into a folder and see only their own uploads. | T1 | 2 |
| HB-ACCESS-018 | Grant principals | Grants can name a user, group, company or project role. A user's effective level on a scope is the highest level among all grants that apply to them. | T1 | 2 |
| HB-ACCESS-019 | Inheritance | A folder inherits grants from its parent and can add grants. Restricting a subfolder below its inherited level requires breaking inheritance, which needs Manage and converts the current effective grants into explicit grants that can then be edited. | T1 | 2 |
| HB-ACCESS-020 | State caps | Container states (HB-INFO-013) and lifecycle states (HB-PDM-013) define a maximum level per principal, for example Download for non-administrators on published versions. The effective level on a version is the lower of the folder level and the cap of the version's state. | T1 | 2 |
| HB-ACCESS-021 | Record-type permissions | Each record type (issue type, review workflow, change order type, construction record type, production entity type) grants per principal: create, read all, read own company, read own and assigned, edit, transition, delete and administer. Custom fields can be hidden or read-only per principal. | T1 | 2 |
| HB-ACCESS-022 | Module administrators | A module administrator configures one module of a project (for example issue types or pipeline steps) without being project administrator. Project administrators hold every module administrator right. | T1 | 2 |
| HB-ACCESS-023 | Permission explanation | For any user and resource the server shows the effective level and the grants, inheritance and state caps that produce it. The same explanation is available from the API. | T2 | 2 |
| HB-ACCESS-024 | Permission change preview | Before a grant change is applied, the dialog shows how many users gain or lose access and lists them; applied changes are recorded in the audit trail with before and after grants. | T2 | 2 |
| HB-ACCESS-025 | Permission report | Administrators export a matrix of folders by principals with explicit and effective levels as CSV or XLSX. | T2 | 2 |
| HB-ACCESS-026 | Time-limited grants | A grant can carry an expiry date; the server removes it at expiry, notifies the grantee three days before, and records the removal in the audit trail. | T2 | 3 |
| HB-ACCESS-027 | Network restrictions | Organization administrators restrict sign-in and API access to listed network ranges; personal access tokens and service accounts can carry their own narrower ranges. | T2 | 3 |
