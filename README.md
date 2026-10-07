# QA.army skills

Public agent instructions for using QA.army and authoring Tests.

- [CLI skill](skills/qa-army-cli/SKILL.md): authentication, supported commands, Runs, and results.
- [Test-writing skill](skills/qa-army-test-writing/SKILL.md): ordered steps, assertions, and evidence.
- [Shared setup prompt](contracts/setup-pr-qa.md): canonical product/marketing handoff.

Read [AGENTS.md](AGENTS.md). Validate commands against the current CLI/MCP/API and run consuming parity checks when shared contracts change. This repo has no npm build; private runtime skills and credentials belong elsewhere.

Codex: open this repo folder as the project and select Worktree from `main`. `.codex/environments/environment.toml` provides setup, cleanup, and actions; dependency installation is explicit.

## GitHub owner review requests

When the owner-request API and clients are deployed, a coding agent may use CLI `connections request --project prj_... --request-key KEY` or MCP `connections.request` to save an expiring Project-scoped GitHub review request. Return its first-party `approval_url` to the owner; the URL is a selector, not a credential or grant. Use `connections status` / `connections.status` to inspect your original credential-scoped request, or `connections cancel` / `connections.cancel` to cancel it. Do not automatically notify another person without authorization.

An authenticated current Workspace owner reviews the request in the matching first-party environment, explicitly approves or declines, then uses the existing GitHub connection flow and verifies repository selection. Agent and profile-key credentials cannot approve. Approved request status does not prove Connected; organization App installation does not prove a Project mapping. Never export provider tokens, enable permissions yourself, substitute a production identity in staging, retry capability denials, or infer disconnected state from denied reads. Preserve existing working integrations.
