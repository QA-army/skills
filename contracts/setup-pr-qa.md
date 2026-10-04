# QA.army agentic setup prompt

Paste this prompt into your coding agent for this project. Authentication details,
including the short-lived user-code handoff, live in [auth.md](https://qa.army/auth.md).
Each new Workspace includes 10 initial free Runs with no credit card required.
This starting allowance does not renew monthly. This prompt requests one initial
Run; additional capabilities can be added explicitly.

```text
Set up QA.army (https://qa.army), a QA agent that runs plain-language tests against web and mobile apps, for this project. Do everything yourself; I only approve one link.

1. Credentials: follow https://qa.army/auth.md. Register an agent identity, send me the claim link, then exchange the assertion for an access token. Use it as an Authorization: Bearer header everywhere; re-exchange on 401 and follow the message on 403.
2. API: https://api.qa.army/v1 (OpenAPI spec: https://api.qa.army/v1/openapi.json).
3. Create or reuse a Project with this repository's application URL, then follow https://qa.army/skill.md to write one plain-language Test covering the most important user flow.
4. Run that Test once. Share its live Run URL immediately, wait for the terminal result, and report the outcome with evidence. Do not automatically retry an ambiguous or mutating action.

If anything is missing, ask me before continuing.
```
