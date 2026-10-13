# QA.army agentic setup prompt

Paste this prompt into your coding agent for this project. Authentication details,
including the short-lived user-code handoff, live in [auth.md](https://qa.army/auth.md).
Each account includes 10 free Runs per UTC calendar month, shared across all
Workspaces, with no credit card required. The allowance resets monthly; creating
or joining a Workspace does not add free Runs. This prompt requests one initial
Run; additional capabilities can be added explicitly. This setup targets Web
applications. Native Android, iOS, and Desktop execution are not generally
available; this prompt does not establish their launch readiness.

```text
Set up QA.army (https://qa.army), a QA agent that runs plain-language tests against web apps, for this project. Do everything yourself; I only approve one link.

1. Discover this repository's application URL; ask me only if you cannot determine it. Reuse valid existing QA.army credentials. Otherwise follow https://qa.army/auth.md: register an agent identity, send me the claim link, then exchange the assertion for an access token. Use it as an Authorization: Bearer header everywhere; re-exchange on 401 and follow the message on 403.
2. API: https://api.qa.army/v1 (OpenAPI spec: https://api.qa.army/v1/openapi.json).
3. Create or reuse a Project with this repository's application URL, then follow https://qa.army/skill.md to write one plain-language Test covering the most important user flow.
4. Run that Test once. Share its live Run URL immediately, wait for the terminal result, and report the outcome with evidence. Do not automatically retry an ambiguous or mutating action.

If anything is missing, ask me before continuing.
```
