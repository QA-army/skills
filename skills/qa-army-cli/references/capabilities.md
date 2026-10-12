# QA.army CLI capability map

Use `qa-army capabilities --json` to inspect the installed runtime inventory.
The canonical public API remains the customer-support boundary. This reference
enumerates the CLI command families and calls out release-validation and pilot
limits where the runtime inventory is stale.

## Command inventory

| Area | Commands | Contract status |
| --- | --- | --- |
| Identity | `auth agent-register`, `auth status`, `auth logout`; root aliases `status`, `logout`, `signout` | Available |
| First setup | `setup`, `create test` | Available |
| Workspaces | `workspaces list`, `workspaces create`, `workspaces get`, `workspaces update` | Available |
| Projects | `projects list`, `projects create` | Available |
| Workspace access | `members list`, `invitations create` | Available |
| Native builds | `builds list`, `builds reserve`, `builds complete` | Registration only; not native execution |
| Test Groups | `groups list`, `groups get`, `groups create`, `groups update` | Available |
| Tests | `tests list`, `tests get`, `tests create`, `tests update`, `tests archive`, `tests delete`, `tests run` | Available |
| Runs | `runs list`, `runs create`, `runs get`, `runs start`, `runs watch`, `runs wait`, `runs cancel` | Available |
| Account | `api-keys list`, `api-keys create`, `api-keys revoke` | Available |
| Product memory | `memories questions`, `memories answer`, `memories list`, `memories create`, `memories update`, `memories approve`, `memories reject`, `memories archive`, `memories clear`, `memories settings`, `memories graph`, `memories summary`, `memories import`, `memories history` | Release validation pending |
| Dynamic PR Tests | `prs list`, `prs get`, `prs cancel`, `prs rerun`, `prs promote`, `prs settings`, `prs configure`, `prs usage` | Assisted pilot; DOGFOOD-PENDING |
| CLI metadata | `capabilities`, `docs`, `request capability` | Available |

The CLI currently accepts `projects star` and `projects unstar`, but their
endpoint is absent from the canonical public API. Do not use or advertise them
as customer-supported commands. The CLI must return a capability request until
the API publishes that contract.

`tests run` queues one durable saved Test in QA.army cloud. `--wait` returns the
truthful terminal outcome. Local-browser, group, environment, and uploaded-app
variants remain unavailable.

`builds` registers immutable native artifacts. It does not select a device,
execute a Mobile Test, or prove a passing Run.

## [request capability]

| Area | Recognized missing commands |
| --- | --- |
| Agent bootstrap | `agent init` |
| Local browser | `run`, `tests run --local` |
| Project lifecycle | `projects get`, `projects update`, `projects delete` |
| Environments | `projects environments`, `projects environments-create`, `projects environments-delete` |
| Project credentials/files | `projects credentials`, `projects credentials-create`, `projects files` |
| Test Groups | `groups delete`, `groups add-test`, `groups remove-test` |
| Test toggles | `tests enable`, `tests disable` |
| Group/mobile variants | `tests run --group`, platform/app/environment flags |
| Mobile artifacts | `upload-app` |
| Automation | `ci`, `pr run-dynamic` |

Do not emulate these operations with unrelated endpoints. Return the stable
capability receipt so product work can add the correct server-owned contract.

## Product memory (release validation pending)

Use `qa-army memories list --project prj_...` to read scoped knowledge.
`create` requires `--project prj_... --input JSON`. `update` requires
`--project prj_... --memory mem_... --version N --input JSON`. Review actions
`approve`, `reject`, and `archive` require `--project`, `--memory`, and
`--version` without `--input`. `settings` accepts `{ "auto_learn": true }`;
`graph`, `summary`, `import`, and `history` use the same Project. `clear`
archives Project memory and disables automatic updates. There is no canonical
individual-memory delete operation; do not use the stale CLI alias
`memories delete`.

Use an authenticated user session or profile API key. Setup-only WorkOS agent credentials do not authorize memory management. Treat observations as historical evidence, requirements as intended behavior, and document imports as proposals requiring review. Never store credentials. Memory usage does not establish correctness or prove improvement.

## Dynamic PR Tests assisted pilot — DOGFOOD-PENDING

When the Workspace is enrolled and the installed CLI supports them, use `qa-army prs list --project prj_...`, `prs get --verification prv_...`, and `prs usage --workspace wsp_...` for reports and shared usage. MCP equivalents are `prs.list`, `prs.get`, and `prs.usage`.

`prs settings` and `prs configure` use an existing integration ID. Configuration is owner-only and requires the selected GitHub repository, selected Vercel Project, sandbox confirmation, and any required sandbox accounts. Restricted agent setup credentials cannot manage the pilot.

Cancellation, explicit rerun and regression promotion use `prs cancel`, `prs rerun` and `prs promote` with a stable `--request-key`. A rerun may consume up to three new Runs; explain that before requesting one. Never retry an ambiguous target mutation automatically. Generated Tests are immutable; promotion creates an editable copy in the chosen group. Do not call missing coverage, infrastructure errors or absent evidence a pass. Pilot thresholds are targets, not proven results.

## Daily clarification (release validation pending)

`qa-army memories questions --project prj_...` / `memories.questions` reads the stable UTC-day Project questions. Ask the customer before selecting intended behavior; never submit a default choice. `qa-army memories answer --project prj_... --input JSON` / `memories.answer` accepts `day`, `question_id`, prior answer `revision` (0 initially), `choice` (0–2), `skipped`, and optional `elaboration`. A skip requires `choice: null` and `skipped: true`; selecting a choice requires `skipped: false`. Respect conflicts and refresh before correction.

All non-skipped answers become owner-review proposals, including owner answers. Treat them as declared intent, not observed Run facts. Publication and canonical retrieval remain server-owned. Inspect earlier conflicting answers before approval. Historical Test expectations and Run snapshots are unchanged. An exhausted set is not permission to invent questions, answers, evidence or confidence.

To revisit a saved set, select its date in Memory, use CLI `memories questions --project prj_... --day YYYY-MM-DD`, or pass `day` to MCP `memories.questions`. The historical API is `GET /v1/projects/{projectId}/memory/clarifications/{day}`. Only existing sets are returned; prior answers remain correctable with revision checks.
