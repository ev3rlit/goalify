---
name: goalify
description: Use when the user asks to turn the current or prior conversation into a fresh-session /goal prompt, context prompt, handoff prompt, implementation goal, completion criteria, or next-session task brief.
---

# Goalify

Use this skill to produce a copy-pasteable prompt for a new session. The output should let a fresh agent understand the work, constraints, context, and completion criteria without rereading the whole conversation.

## Core Behavior

- Start the prompt with `/goal` on the first line when the user asks for a new-session handoff.
- Write the generated `/goal` in the language the user is using, never in English by default merely because this skill is written in English.
- If the user explicitly requests a language, use it. If the conversation mixes languages, follow the language used in the latest task-setting message.
- State inside the generated goal that all progress updates and the final explanation must use that same user language.
- Convert scattered discussion into an actionable objective, not a chat summary.
- Include concrete paths, files, commands, branches, tests, and validation handles when they are known.
- Separate required scope from optional future work.
- Do not invent requirements. If something is uncertain, label it as a decision point or optional check.
- Keep the prompt self-contained enough for a new session to begin work immediately.
- Require the next session to explain completed work so the user can understand what changed, why each important choice was made, and how the result was verified.
- When preparing a goal for Codex CLI, keep the generated goal under 4,000 characters.
- If the source context is too large for that limit, summarize aggressively and point to files instead.

## Workflow

1. Identify the intended next-session task:
   - implementation
   - bug fix
   - investigation
   - refactor
   - documentation
   - review
   - planning

2. Extract durable context from the conversation:
   - workspace or repository path
   - target branch or worktree constraints
   - relevant docs and source files to read first
   - existing decisions the user accepted
   - requirements the user explicitly rejected
   - data samples, commands, errors, benchmarks, or observations

3. Turn the task into a structured prompt:
   - objective
   - required preflight checks
   - design direction
   - behavior rules
   - tests or verification
   - completion criteria
   - known non-goals
   - explanation after completion

4. Add execution discipline only when relevant:
   - preserve dirty worktree changes
   - run formatters
   - run test commands
   - use TDD when the user asked for it
   - commit/push only if the user explicitly requests it

5. Add an explanation contract for the final response:
   - explain the starting point, important work stages, and final state in order
   - for each important stage, state what was done, why it was done, and how it was checked
   - explain the core mechanism in direct, concrete language that a 12-year-old could follow
   - keep necessary technical terms, but define them immediately with a simple example or analogy
   - report material decisions, changes, failed approaches that affected the result, and verification without dumping every command or private chain of thought
   - distinguish verified facts from remaining assumptions or limitations

6. Finish with a fenced `text` block unless the user asks for prose only.

## Prompt Shape

Use this shape for implementation or investigation handoffs:

```text
/goal

In `<workspace-path>`, complete <task title>.

Use <the user's language> for all progress updates and the final response.

Preflight:

- `<command or file>`
- `<file>`

Objective:

- <primary objective>
- <accepted design decision>

Important direction:

- <boundary or architecture rule>
- <behavior rule>
- <non-goal if important>

Implementation or investigation:

1. <task item>
2. <task item>
3. <task item>

Testing and verification:

- <test command>
- <smoke command>
- <expected result>

Completion criteria:

- <observable done condition>
- <observable done condition>
- <final validation command passes>

After completion, explain the work to the user:

- Do not list only the result in the final response.
- Explain the starting state -> important work stages -> final state in order.
- For each important stage, state directly and concretely what was done, why it was done, and how it was verified.
- Explain the core mechanism with plain language and a concrete example a 12-year-old could follow. Keep necessary technical terms, but define them immediately.
- Do not dump every command or private chain of thought. Include the decisions, changes, failed attempts, and verification that materially affected the result.
- Separate verified results from unverified details and remaining limitations.
```

For planning-only requests, omit implementation/test sections that do not apply and focus on goal, constraints, decisions, open questions, and completion criteria.

## Quality Bar

A good goalified prompt:

- can be pasted into a new session as-is
- has `/goal` as the first line when requested
- uses the user's language and explicitly requires the next session to use that language for progress updates and the final explanation
- names exact files and commands where possible
- captures the user's latest correction, not just the first request
- avoids generic filler like "analyze and implement"
- makes success observable through tests, output, docs, or commands
- keeps future ideas separate from the current scope
- requires a completion explanation that teaches the user the important process and rationale, not only the result

## Common Additions

Use these only when they are true for the task:

- "Before changing code, check `git status --short --branch`."
- "Do not revert existing user changes."
- "Run `gofmt` after modifying Go code."
- "Run `go test ./...` at the end."
- "Follow the project's existing error patterns for new errors."
- "Preserve the JSON and table output contracts for CLI output."
- "The spec and domain layers must not depend on storage, CLI, or presentation concerns."

## What Not To Do

- Do not include long transcript summaries.
- Do not add implementation details the user has not agreed to unless clearly marked as a suggested approach.
- Do not ask the next session to commit or push unless the user explicitly asked for that.
- Do not hide unresolved ambiguity; call it out as a decision point.
- Do not make the prompt project-specific unless the conversation is project-specific.
- Do not interpret "show the process" as a request to expose private chain of thought or dump raw command logs. Ask for a concise, evidence-based account of material actions and reasons instead.
