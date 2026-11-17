# Application integrations

A toolkit of integrations that run inside content-creation and CAD applications and connect them
to Arc Hub: publish, load, scene inventory, file-system templates and task context. Each
integration is a connector that runs inside the other application through that application's
public API, as described in suite
[ADR-0017](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0017-exchange-connectors-and-licensed-bridges.md),
and calls the Hub API. The toolkit adds no server behavior of its own: production entities, tasks,
published outputs and path templates are those of
[19-production-entities.md](19-production-entities.md), and the connectors are developed in the
connector repository of the suite.

Domain objects:

- **Integration**: a connector for one host application and version range, with its configuration.
- **Session context**: the project, entity, task and step an integration works in.
- **Scene inventory**: the list of Hub-sourced items loaded into the open scene or drawing.
- **Work file**: a file saved by an integration as versions of one document.

## Toolkit

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-PIPE-001 | Integration toolkit | Each integration provides the same operations (sign in, set context, publish, load, inventory, save work file, submit for review) in the idiom of its host, over one shared Hub client core and the public API. No integration holds privileges that another API client lacks. | T3 | 5 |
| HB-PIPE-002 | Sign-in | An integration signs in with OAuth device authorization or PKCE (HB-API-011), stores its token in the operating system's credential store and shows the signed-in user and server in the host. | T3 | 5 |
| HB-PIPE-003 | Task context | The user picks a task from My tasks (HB-PROD-012) or the host opens with one. The integration reads entity, step, status, due date, notes, upstream outputs and working files and keeps them as the session context shown in a panel; changing the context asks to save open work first. | T3 | 5 |
| HB-PIPE-004 | Publish | Publishing registers the selected content, or the whole scene, as published outputs of the task (HB-PROD-017). Content is written in an open format or as an Arc exchange package, with a thumbnail, comment, output type, frame range and the upstream outputs it uses, so the dependency graph stays complete. | T3 | 5 |
| HB-PIPE-005 | Publish validation | Administrators define checks per step and host: naming, units, missing references, scene statistics and custom checks through the Python client. A failing check blocks or warns according to the user's role, and the result is stored with the output. | T3 | 5 |
| HB-PIPE-006 | Load | Users browse the published outputs of upstream tasks, versions and the task's own outputs and load them into the scene as a reference or as an import. The integration records which version was loaded. | T3 | 5 |
| HB-PIPE-007 | Scene inventory | The panel lists every Hub-sourced item in the open scene with loaded version, latest version and status (HB-PROD-020), and lets the user update, switch to a chosen version, replace or remove items. The inventory is also stored with published outputs so that downstream users see what a result was built from. | T3 | 5 |
| HB-PIPE-008 | File-system templates | Integrations resolve working-file paths, cache folders and output locations from the project's path templates (HB-PROD-018) for the current entity, step and version. Save-as proposes the next version name; the user cannot save outside the templates unless an administrator allows it. | T3 | 5 |
| HB-PIPE-009 | Work files | Saving a work file creates a new version of its document in the task's working folder, with a lock held while the file is open (HB-FILE-019) and a warning if a newer version exists. Opening a work file restores the session context from the task. | T3 | 5 |
| HB-PIPE-010 | Review submission | An integration captures a viewport image, a playback or a render from the host and submits it with the published outputs (HB-MEDIA-001); notes and statuses of the submission show in the panel. | T3 | 5 |
| HB-PIPE-011 | Host menu actions | Custom actions (HB-API-023) addressed to the task appear in the host's menu; an action opens a page or starts a job (HB-JOB-006) with the session context as parameters. | T3 | 5 |
| HB-PIPE-012 | Hooks | Studios attach scripts to named points (before and after publish, load, save, context change) through the Python client. Hook configuration is a versioned file in the project, so every user of the project runs the same hooks. | T3 | 5 |
| HB-PIPE-013 | Offline tolerance | Without a connection an integration works from the last task context it fetched, queues publishes as pending with their content stored locally, and sends them in order on reconnection, reporting any that the server refuses. | T3 | 5 |
| HB-PIPE-014 | Packaging and compatibility | Integrations are packaged per host version and listed in a compatibility table kept with the server's documentation. Administrators distribute packages from a project or organization folder, and the integration reports its own version and the host's with every version it creates (HB-FILE-008). | T3 | 5 |
