# QA.army skills

Public agent instructions for using QA.army and authoring Tests.

- [CLI skill](skills/qa-army-cli/SKILL.md): authentication, supported commands, Runs, and results.
- [Test-writing skill](skills/qa-army-test-writing/SKILL.md): ordered steps, assertions, and evidence.
- [Shared setup prompt](contracts/setup-pr-qa.md): canonical product/marketing handoff.

Read [AGENTS.md](AGENTS.md). Validate commands against the current CLI/MCP/API and run consuming parity checks when shared contracts change. This repo has no npm build; private runtime skills and credentials belong elsewhere.

Codex: open this repo folder as the project and select Worktree from `main`. `.codex/environments/environment.toml` provides setup, cleanup, and actions; dependency installation is explicit.
