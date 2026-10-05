# Skills — agent guidance

**Be a doer.** Inspect, implement, test, review, merge, deploy or publish when needed, and verify the requested outcome. Fix failures and continue until delivered or concretely blocked; a plan, PR, or green build alone is not completion.

If an ancestor contains `.qa-army-workspace`, read its `AGENTS.md` first. Otherwise, this repo stands alone.

- Own public workflows and the shared setup prompt in `contracts/setup-pr-qa.md`.
- Describe supported platform/CLI/MCP behavior. Never invent credentials, tenant scope, permissions, or successful results.
- Keep commands, tool names, links, and examples current; coordinate shared prompt changes and revision pins with product and marketing.
- Review the final diff on a clean branch from current main. Use consuming parity checks for contract changes; there is no npm build or CI workflow here.
- Keep secrets and private runtime/operational instructions out of this public repo.

## Direction

- Build a better product than [TesterArmy](https://tester.army/); read its [blog](https://tester.army/blog) for product guidance and verify ideas against customer needs.
- Get funding through shipped value, measurable traction, and repeatable demos backed by real evidence. Never invent results.
- QA.army is the first customer: use the same product, permissions, integrations, and release journeys customers use; turn findings into general fixes.
- Build for SaaS, web, mobile, and desktop customers across domains. Customer URLs, IDs, selectors, and workflows belong in configuration or Tests, never product-code special cases. Do not overbuild unrequested abstractions.
