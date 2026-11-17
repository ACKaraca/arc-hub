# Automation and jobs

Server-side processing and automation: a durable job queue, workers that run headless Arc
applications in sandboxes, triggers, automation rules and guard plug-ins. The same commands that a
user runs in a desktop application run here without dialogs.

Domain objects:

- **Job definition**: runner application and version, command or script, parameter schema, inputs, outputs, limits.
- **Job run**: one execution with inputs, outputs, logs and resource use.
- **Worker**: a server process in the worker role with declared capabilities and labels.
- **Trigger**: an event filter or schedule that starts a job definition.
- **Automation rule**: an event, a condition and actions, optionally authored as a node graph.
- **Guard plug-in**: a WebAssembly module that allows or refuses an operation.

## Job execution

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-JOB-001 | Job queue | Jobs are stored durably in the database with priority and per-organization fair scheduling; their states are queued, running, succeeded, failed and cancelled, visible to users with access to the job's project. | T1 | 2 |
| HB-JOB-002 | Workers | Workers register capabilities (installed Arc command-line applications and versions, operating system, GPU) and labels, send heartbeats, and scale horizontally. Jobs of a worker that stops sending heartbeats are returned to the queue. | T1 | 2 |
| HB-JOB-003 | System jobs | Derivatives, thumbnails, text extraction, sheet extraction, clash runs, structure extraction, property write-back, archives and reindexing run as system jobs in the same queue with system priority. | T1 | 2 |
| HB-JOB-004 | Headless Arc runners | A job runs an Arc application's command-line tool with a command identifier or a Python script through the automation protocol, on input document versions. No dialog can open; outputs are stored as new versions, derivatives or reports. | T1 | 3 |
| HB-JOB-005 | Sandboxed execution | Each run executes in an isolated process with a private temporary directory, CPU, memory, time and disk limits, and no network except the Hub callback. It acts as the job's service account with only the granted scopes; secrets are injected by reference and never written to logs. | T1 | 3 |
| HB-JOB-006 | Job definitions and manual runs | Administrators define jobs with runner and version constraint, command or script, a JSON Schema for parameters, input selectors, output folder and naming, timeout and retries. Users run a job on selected versions through a form generated from the schema. | T1 | 3 |
| HB-JOB-007 | Triggers | Jobs start on events (version added to a matching folder, state transition, review completed, record status change), on cron schedules or on signed inbound calls, with debounce for bursts. | T1 | 3 |
| HB-JOB-008 | Run records | Each run keeps streamed logs, inputs, outputs, duration, exit status and resource use for a configurable period and can be re-run with the same inputs. | T1 | 3 |
| HB-JOB-009 | Batch operations | Batch jobs process whole folders, for example plotting all drawings to PDF or validating all IFC versions, with per-file results. | T1 | 3 |
| HB-JOB-010 | Worker pools | Job definitions target worker labels such as gpu, windows or large-memory; jobs wait until a matching worker is available. | T2 | 3 |
| HB-JOB-011 | Job chaining | Job definitions form simple pipelines of ordered steps with conditions on previous results; each step's outputs are the next step's inputs. | T2 | 3 |
| HB-JOB-012 | Render jobs | RenderBuilder farm jobs split frames or tiles across workers with the render capability, report progress per frame and store results as image sequence versions. | T2 | 4 |

## Rules and guards

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-JOB-013 | Automation rules | Rules state an event, a condition in the filter grammar and actions: set property, transition, notify, create record, start review, run job or call a URL. Rules are edited in a form and can be tested in dry-run mode against recent events. | T2 | 3 |
| HB-JOB-014 | Rule safety | Events caused by rules carry their causation chain; a chain deeper than five rule executions stops with an alert. Every rule execution is audited. | T2 | 3 |
| HB-JOB-015 | Guard plug-ins | WebAssembly plug-ins without ambient authority validate uploads, check-ins and lifecycle transitions: they receive the operation and its records and return allow or refuse with a message within a 2-second limit. A plug-in fault refuses the operation. | T3 | 4 |
| HB-JOB-016 | Rule graphs | Rules with multi-step logic can be authored as node graphs on the suite's node engine and are evaluated on the server with bounded steps and time. | T3 | 4 |
| HB-JOB-017 | External compute | A job definition can target an external batch system through an adapter that submits the same sandboxed runner command, for example to a container cluster. | T3 | 5 |
