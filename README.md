# Goalify

Goalify prepares a copy-pasteable `/goal` prompt for a new session from a conversation, an existing plan, or both.

## How to use it

After discussing a task, ask:

```text
Use goalify to turn this conversation into a goal for the next session.
```

Goalify collects the accepted objective, necessary context, constraints, and completion criteria. It preserves the latest corrections and keeps future ideas separate from the current task.

When you already have a plan, ask:

```text
Use goalify to prepare a goal for Phase 2 of docs/saved-reports-plan.md.
```

Goalify reads the relevant source and checks whether it defines the selected work sufficiently. If it does, the prompt connects the next session to that document and scope, without rewriting its implementation steps or acceptance checklist. Later accepted decisions appear only as explicit additions or overrides. The source must be accessible to the next session; an unreadable or incomplete document is identified as such, and missing requirements are not invented.

The generated prompt follows your language and asks the next session to use it for progress updates and the final explanation. The final explanation covers material changes or findings, important reasons, verification evidence, and remaining limits. Its depth follows the task and your requests.

## Conversation example

The following assumes these requirements and checks were agreed during the conversation:

```text
/goal

Build the saved reports view in /workspace/acme-dashboard using the existing dashboard shell, React, Tailwind, navigation, and table styling. Keep it a compact reports screen.

Include title, owner, updated date, and status in the list; add status filtering and an empty state. Preserve existing user changes and unrelated behavior.

Run the project's existing type check and test commands, then verify the view on desktop and mobile. Complete the task when the list, filtering, and empty state work and the required checks pass.

Use English for progress updates and the final response. Explain the material changes, rationale, verification results, and remaining limits in plain language.
```

## Existing-plan example

The following assumes Phase 2 is defined in the document and the export-format correction was accepted afterward:

```text
/goal

In /workspace/reports, complete Phase 2 of docs/saved-reports-plan.md. Read that section and its referenced prerequisites, check the current implementation, and finish the remaining work using the plan's design, verification, and completion criteria for Phase 2. Phase 3 is outside this task.

The later accepted correction overrides Phase 2's export format: use JSON instead of CSV. All other Phase 2 requirements remain as documented.

Use English for progress updates and the final response. Explain the material changes, rationale, verification results, and remaining limits in plain language.
```

A short document-based goal is a complete result when the referenced specification supplies the details. For Codex CLI, the generated goal stays under 4,000 characters; other destinations do not inherit that limit automatically.

## Evaluation cases

`Skills/goalify/evals/evals.json` contains conversation regressions and document handoff cases. Input files are relative to the skill directory. The fixtures are supplied documents, not instructions to execute their implementation tasks. Evaluate prompt outputs for requirement preservation, usable references, scope, explicit overrides, and absence of a duplicate plan. A structural validator alone does not establish those behaviors.
