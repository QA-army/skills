# Public skills

When this checkout is inside a local QA.army workspace, find the nearest ancestor containing `.qa-army-workspace` and read that ancestor's AGENTS.md before changes. If absent, use this repository's instructions independently; private workspace access is not required.

- Own public agent workflows, skill documentation, and shared setup prompt contracts.
- Describe actual supported platform/CLI/MCP behavior. Skills must not grant themselves credentials, choose tenant scope, or bypass product policy.
- Preserve canonical shared prompt text in `contracts/setup-pr-qa.md`; coordinate product and marketing copies and revision pins when changing it.
- Check referenced commands, options, tool names, links, and installation paths against the affected surface. Run consuming platform/marketing parity checks when shared contracts change.
- This repository currently has no CI workflow; do not invent an npm validation command. Add focused validation only when it checks a useful behavior.
- Use a clean task branch from current origin/main, review public content, and preserve unrelated work.
- Keep private operational instructions and credentials out of this public repository.
