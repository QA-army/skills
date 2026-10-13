# QA.army CLI capability map

Use `qa-army capabilities --json` as the runtime authority. This reference
explains the intended mapping.

## Available

| Area | Commands |
| --- | --- |
| Identity | `auth agent-register`, `status`, `logout`, `signout` |
| First setup | `setup`, `create test` |
| Workspaces | `list`, `create`, `get`, `update` |
| Projects | `list`, `create`, `star`, `unstar` |
| Test Groups | `list`, `get`, `create`, `update`, `run` (explicit selection, 0.2.10+) |
| Tests | `list`, `get`, `create`, `update`, `archive`, `delete`, `run` |
| Runs | `list`, `create`, `get`, `start`, `watch`, `wait`, `cancel`, `evidence` (0.2.10+) |
| Deployment watch | `prs watch`, `prs watch-update` (0.2.11+, existing connected GitHub watch, Workspace Owner) |
| Account | `api-keys list`, `api-keys create`, `api-keys revoke` |

`tests run` queues one durable saved Test in QA.army cloud. `--wait` returns the
truthful terminal outcome. Local-browser, environment, and uploaded-app
variants remain unavailable.

## [request capability]

| Area | Recognized missing commands |
| --- | --- |
| Agent bootstrap | `agent init` |
| Local browser | `run`, `tests run --local` |
| Project lifecycle | `projects get`, `projects update`, `projects delete` |
| Environments | `projects environments`, `environments-create`, `environments-delete` |
| Project credentials/files | `projects credentials`, `credentials-create`, `files` |
| Test Groups | `groups delete`, `groups add-test`, `groups remove-test` |
| Test toggles | `tests enable`, `tests disable` |
| Group/mobile variants | `tests run --group`, platform/app/environment flags |
| Mobile artifacts | `upload-app` |
| Automation | `ci`, `pr run-dynamic` |

Do not emulate these operations with unrelated endpoints. Return the stable
capability receipt so product work can add the correct server-owned contract.

## Product memory (release validation pending)

Use `qa-army memories list --project prj_...` to read scoped knowledge. `create` accepts `--input` JSON; `update`, `approve`, `reject`, and `archive` require `--memory mem_... --version N`. `settings` accepts `{ "auto_learn": true }`; `graph`, `summary`, `import`, and `history` use the same Project. `clear` archives Project memory and disables automatic updates.

Use an authenticated user session or profile API key. Setup-only WorkOS agent credentials do not authorize memory management. Treat observations as historical evidence, requirements as intended behavior, and document imports as proposals requiring review. Never store credentials. Memory usage does not establish correctness or prove improvement.

## Existing deployment watch

A claimed agent acting for a Workspace Owner can read an existing connected
GitHub watch with `prs watch --integration <integrationId>` and update it with
`prs watch-update --integration <integrationId> --input <JSON>`. Send the full
`{repository_id, test_group_ids, enabled}` body, preserving the existing numeric
repository ID string and all Groups not intentionally removed. Groups must belong
to that Project. Read back the result; do not automatically retry an uncertain
write or treat saved configuration as a Run result. MCP 0.1.4 exposes `prs.watch`
and `prs.watch_update`. Integration creation, consent, and dynamic PR settings
remain separate operations.

## Dynamic PR Tests assisted pilot — DOGFOOD-PENDING

When the Workspace is enrolled and the installed CLI supports them, use `qa-army prs list --project prj_...`, `prs get --verification prv_...`, and `prs usage --workspace wsp_...` for reports and shared usage. MCP equivalents are `prs.list`, `prs.get`, and `prs.usage`.

`prs settings` and `prs configure` use an existing integration ID. Configuration is owner-only and requires the selected GitHub repository, selected Vercel Project, sandbox confirmation, and any required sandbox accounts. Restricted agent setup credentials cannot manage the pilot.

Cancellation, explicit rerun and regression promotion use `prs cancel`, `prs rerun` and `prs promote` with a stable `--request-key`. A rerun may consume up to three new Runs; explain that before requesting one. Never retry an ambiguous target mutation automatically. Generated Tests are immutable; promotion creates an editable copy in the chosen group. Do not call missing coverage, infrastructure errors or absent evidence a pass. Pilot thresholds are targets, not proven results.

## Daily clarification (release validation pending)

`qa-army memories questions --project prj_...` / `memories.questions` reads the stable UTC-day Project questions. Ask the customer before selecting intended behavior; never submit a default choice. `qa-army memories answer --project prj_... --input JSON` / `memories.answer` accepts `day`, `question_id`, prior answer `revision` (0 initially), `choice` (0–2), `skipped`, and optional `elaboration`. Skip uses null choice. Respect conflicts and refresh before correction.

All non-skipped answers become owner-review proposals, including owner answers. Treat them as declared intent, not observed Run facts. Publication and canonical retrieval remain server-owned. Inspect earlier conflicting answers before approval. Historical Test expectations and Run snapshots are unchanged. An exhausted set is not permission to invent questions, answers, evidence or confidence.

To revisit a saved set, select its date in Memory, use CLI `memories questions --project prj_... --day YYYY-MM-DD`, or pass `day` to MCP `memories.questions`. The historical API is `GET /v1/projects/{projectId}/memory/clarifications/{day}`. Only existing sets are returned; prior answers remain correctable with revision checks.

## Workspace invitations and private report links

`qa-army invitations create --workspace wsp_... --input JSON` forwards the
canonical `{emails, return_to?}` body. MCP `invitations.create` takes the same
fields plus `workspace_id`; optional destination support requires its matching
client/server release. `return_to` is a raw relative same-Workspace Test or Run
path; it cannot grant access or point to another origin. The server validates
resource ancestry and owner permissions.

Invitation management requires a Workspace owner with an authorized user session
or profile API key. Claimed WorkOS agents and setup credentials cannot invite or
accept for someone else. Direct recipients to the invitation email and browser
sign-in using that email, followed by explicit acceptance. CLI/MCP provide no
recipient acceptance, list, resend or revoke command; use the app for these tasks.

Report links remain Workspace-private. Signing in, copying a report link or
copying an access-request message does not grant access or send an invitation.
Ask the person who shared the link to arrange an owner invitation. Preserve
`SENT` as provider acceptance, not inbox receipt or membership. After an uncertain
create response, check pending invitations in the app before sending again;
separate invocations use new request keys. Expired links need owner recovery;
resend invalidates the earlier link. Do not bypass a denial with another credential.
