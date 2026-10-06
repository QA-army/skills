---
name: qa-army-test-writing
description: "Choose and write one meaningful plain-language QA.army Test for the most important user journey. Use during project setup or when turning a code change or reported failure into coverage."
---

# QA.army test writing

Use [auth.md](https://qa.army/auth.md) for authentication and the
[OpenAPI contract](https://api.qa.army/v1/openapi.json) for API mechanics.
This skill governs test selection, instructions, and evidence.

## Choose one useful Test

Inspect the requirements, relevant code or diff, existing coverage, and target
environment. Treat implementation as evidence, not authority for intended behavior.

Choose the smallest journey that proves an important user outcome. Prefer
high-impact behavior affected by the change or missing from existing coverage.
For initial setup, create one Test covering the application's primary value,
not merely a successful login.

Include cross-application boundaries when the outcome depends on them.
If essential intent, access, or test data is missing, ask before continuing.

## Write observable steps

- State the environment, starting state, actor, and required test data.
  Keep credentials out of instructions and evidence.
- Use ordered plain-language steps with one action or assertion per step.
  Describe visible controls rather than selectors.
- Specify expected values or states. "Verify it works" is not an assertion.
- Follow actions to their real outcome. A click, toast, or redirect proves
  only itself. Check persistence after refresh or a new session when relevant.
- Bound asynchronous waits. Never automatically repeat an action that may
  already have succeeded.
- Use isolated test data; clean up only what this Test created.
- Report inaccessible boundaries as coverage gaps. Do not substitute mocks
  and claim end-to-end success.

## Save directly through the Test API

The coding agent authors the Test. Send the complete `SaveTestRequest` to
`POST https://api.qa.army/v1/projects/{projectId}/tests` with the bearer token
and a stable `Idempotency-Key`. Reuse an existing matching Test when available.
AI Test generation is internal to the UI; do not call `test-generations` or
send a prompt for the server to expand.

The request uses the same types and ordered steps as the Test editor:

| UI option | API `type` | Authoring rule |
| --- | --- | --- |
| Act | `act` | One bounded user action, followed by internal outcome verification |
| Assert | `assert` | One observable expected outcome |
| Login | `login` | Reference an existing same-Project `account_id`; never embed credentials |
| Files | `files` | State the file interaction and available test data |
| Screenshot | `screenshot` | Capture a useful checkpoint; this is not an assertion |
| JavaScript | `javascript` | Describe the required browser operation explicitly |
| Microphone | `microphone` | State the audio interaction and prerequisites |

Steps run in array order. Set `enabled: false` to retain a step but skip it.
Keep explicit Verify steps for independent and final journey expectations. Screenshot-only Tests are valid: passing means the requested captures completed, not that product behavior was verified. Availability of a step type does not
establish that its required account, file, device, or browser capability exists.
If a prerequisite is missing, resolve it before running; never promise a pass.

ACT accepts an optional `verification` object with `expectation`, `timeout_ms` (default 30000, 1000–120000), and up to eight `checks` containing a visible `query` and scalar `equals` value. Exact values preserve their type. When omitted, the server infers and freezes an outcome before execution; ambiguous intent produces an actionable error before mutation. A completed click alone cannot pass ACT. Reobservation is read-only and bounded to three observations and the Run deadline. Explicit Assert steps remain separate and Screenshot remains a capture primitive.

For an application whose signup entry uses these controls, an authored payload is:

```json
{
  "name": "Signup entry point",
  "description": "Verify a visitor can start signup",
  "group_id": null,
  "enabled": true,
  "allow_web_search": false,
  "deep_thinking": false,
  "location_override": null,
  "viewport": {
    "width": 1440,
    "height": 900
  },
  "device_name": null,
  "steps": [
    {
      "type": "act",
      "instruction": "Open the Project homepage",
      "enabled": true
    },
    {
      "type": "assert",
      "instruction": "Verify the Get started link is visible",
      "enabled": true
    },
    {
      "type": "act",
      "instruction": "Click Get started",
      "enabled": true,
      "verification": { "expectation": "The signup form is visible" }
    },
    {
      "type": "assert",
      "instruction": "Verify the signup form has visible Email and Password fields",
      "enabled": true
    },
    {
      "type": "screenshot",
      "instruction": "Capture the signup form",
      "enabled": true
    }
  ]
}
```

Adapt the controls and expectations to evidence from the actual application.
The response contains `test.id`; use it with the normal Run API. For CLI setup,
save the authored payload as `test.json` and use:

```sh
qa-army setup --app-url https://your-app.example --project-name "Your project" --input "$(cat test.json)"
```

For an existing Project, use `qa-army tests create --project prj_... --input
"$(cat test.json)"`, then run the returned Test only when execution was requested.
Both commands save the supplied steps directly without server-side generation.

## Preserve execution truth

When execution is requested, follow the API contract to run the Test and
wait for its terminal result. Preserve the server's outcome:

- A new Workspace includes 10 free Runs with no credit card required. This is
  an initial allowance, not a monthly reset.
- Creating or editing Tests does not authorize execution. Start one Run only
  when the user explicitly requests it; never spend the remaining allowance
  merely because it is available.

- `PASSED`: every required expectation was observed and requested captures completed.
- `FAILED`: the product violated an expectation.
- `ERROR`: execution could not establish a trustworthy product result.

Report the live Test Run URL, known environment and revision, expected versus
observed behavior, and available evidence. For failures, identify the first
broken step and reproduction steps.

Never turn missing evidence or unexecuted steps into a pass. If waiting ends
before completion, report the last observed status; do not invent a terminal result.

Stop after the requested result. Do not expand coverage, schedule recurring
Runs, or promise unavailable capabilities unless asked.

## Product memory

Published requirements can inform expected behavior. Historical observations do not establish correctness: preserve the user’s intended assertion and verify current evidence. Imported document proposals need owner review. Never include passwords, tokens, or signed URLs in memory or Test text. Frozen Run memory revisions remain audit context even when current records are archived.
