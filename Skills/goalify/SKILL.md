---
name: goalify
description: Turn conversations or existing plans into a fresh-session /goal or handoff prompt. Use when asked to prepare the next session's task, not to execute it.
---

# Goalify

Produce a copy-pasteable prompt that lets a fresh agent start the intended work. Synthesize scattered decisions when needed; when a usable plan already exists, connect the next session to that plan and the requested scope. Rewriting an existing plan creates a second specification that can lose details or drift from the source.

## Understand the handoff

Identify the intended outcome and task type, workspace, branch or worktree boundaries, selected scope, accepted decisions, latest corrections, rejected approaches, and relevant evidence from the available conversation. Preserve implementation, investigation, review, and planning boundaries. Keep tentative ideas separate from agreed work.

Read documents supplied or explicitly referenced for the task when accessible, including relevant linked sections needed to understand the selected scope. Inspect only what the handoff needs; goal generation does not require a repository-wide investigation or executing the work.

Decide how much the prompt needs to contain based on the source content, not its title or length:

| Available context | How to write the handoff |
| --- | --- |
| Decisions are scattered through conversation | Synthesize an actionable objective, necessary context, constraints, and observable completion criteria. |
| An accessible plan defines the selected outcome, scope, important decisions, and verification or completion criteria | Reference the plan and selected section as the task specification. Add only information needed to start or absent from the plan. |
| A usable plan has later accepted changes | Reference the plan and state only the accepted changes, their precedence, and any resulting change to scope or completion. |
| The document is incomplete or unavailable | Preserve what is known, identify the specific gap, and carry forward the necessary check or decision without inventing its answer. |

A plan can be sufficient for one phase without resolving later phases. Do not turn a bounded implementation handoff into a new planning task merely because unrelated questions remain open.

## Build the prompt

### From conversation

Write the objective and the context a fresh session would otherwise lose: relevant paths, accepted constraints, known state, important observations, and how success will be checked. Include exact commands when known. Label reported results and hypotheses appropriately; do not present earlier claims as freshly verified facts.

Use headings or task lists only when they help. Do not fill a fixed template with invented preflight steps, architecture choices, tests, or non-goals. For planning-only or read-only work, preserve those limits rather than adding implementation steps.

### From an existing plan

Name the source with a repository-relative path and workspace, or another usable document locator. Identify the requested phase, section, or requirement IDs when applicable. If multiple documents matter, distinguish their roles instead of silently merging them into a new specification.

Treat the user-designated plan as the task specification, subject to the user's explicit instructions and later accepted corrections. Preserve a known revision when the user selected one. Carry changes not yet in the document as explicit overrides; suggestions and unresolved alternatives are not overrides. If sources conflict and no accepted decision resolves the conflict, expose that particular decision rather than silently choosing a new design.

Keep the goal to a short outcome, source, scope, and any necessary additions. Refer to the selected scope's design, validation, and completion criteria instead of copying or paraphrasing its implementation steps, interfaces, test list, and acceptance checklist into parallel sections. The prompt is complete when the next session can locate and use its requirements; it need not contain every requirement inline.

Preserve known completed work and the requested starting point. When resuming a plan, direct the next session to check the current implementation against the selected scope and finish what remains, rather than blindly repeating completed phases. Do not include future phases or optional work unless requested.

### Gaps and access

A document reference is useful only if the next session can resolve it. Do not invent a path, section, revision, or document contents. If the source is pasted text without a durable locator and will not be available in the next session, include the necessary content inline rather than pretending it is stored in a file.

If a referenced document cannot be read, say so and do not claim to have validated its scope or completeness. When the intended task is still clear, produce a limited handoff requiring access and inspection before dependent implementation. Ask a focused question only when the missing information prevents a usable handoff; ordinary implementation choices can remain with the executing agent.

For incomplete plans, add only requirements already established in conversation. Mark remaining material gaps as decisions or checks. Do not silently author missing design or acceptance criteria as if the user approved them.

## Shared output rules

- Use one fenced `text` block unless the user requests another format. Put `/goal` on the first line inside it for a goal or new-session handoff, unless a different format is explicitly requested.
- Follow the user's explicit language choice, otherwise the latest task-setting message. Require that language for the next session's progress updates and final response too.
- Preserve relevant execution constraints, such as existing user changes, requested TDD, and authorization limits. Do not add commit, push, or publication instructions without the user's request, or duplicate project-wide rules without a task-specific reason.
- Add a concise completion explanation contract: explain the material changes or findings, why important choices were made, verification evidence, and remaining limits in plain language. Scale detail to the work and honor explicit requests for deeper teaching or process explanations; avoid raw command logs or private chain of thought.
- For Codex CLI handoffs, keep the generated goal under 4,000 characters. This is a ceiling, not a target or a condition for using references. Preserve scope and important constraints before background detail; do not silently drop essential requirements to fit.
- Return the handoff. Do not start its implementation, create an active goal, or write a replacement plan unless the user separately requests that action.

## Examples

These illustrate different amounts of context, not mandatory templates. Paths and decisions below belong only to the examples.

### Scattered conversation

```text
/goal

In /workspace/shop-api, fix the cart price-cache bug. The prior investigation identified that internal/cart/cache.go omits the pre-discount price from the cache key. Preserve existing user changes.

Verify the fix with go test ./..., including a regression case for a price change that previously reused the wrong cached result. The affected case must return the correct price and existing tests must pass.

Use English for progress updates and the final response. Explain the change, its rationale, verification results, and remaining limits in plain language.
```

### Existing plan with a later correction

```text
/goal

In /workspace/reports, complete Phase 2 of docs/saved-reports-plan.md. Read that section and its referenced prerequisites, check the current implementation, and finish the remaining work using the plan's design, verification, and completion criteria for Phase 2. Phase 3 is outside this task.

The later accepted correction overrides Phase 2's export format: use JSON instead of CSV. All other Phase 2 requirements remain as documented.

Use English for progress updates and the final response. Explain the material changes, rationale, verification results, and remaining limits in plain language.
```

## Final check

Ensure the prompt preserves the user's latest intent, is usable in the destination session, and makes completion observable directly or through a precise source reference. For document-based handoffs, check that it adds no second implementation plan. For conversation-based handoffs, check that the context needed to act has not been reduced to a vague instruction. Remove anything that neither preserves a decision nor helps the next session start, execute, or verify the agreed work.
