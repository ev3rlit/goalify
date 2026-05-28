---
name: goalify
description: Use when the user asks to turn the current or prior conversation into a fresh-session /goal prompt, context prompt, handoff prompt, implementation goal, completion criteria, or next-session task brief.
---

# Goalify

Use this skill to produce a copy-pasteable prompt for a new session. The output should let a fresh agent understand the work, constraints, context, and completion criteria without rereading the whole conversation.

## Core Behavior

- Start the prompt with `/goal` on the first line when the user asks for a new-session handoff.
- Preserve the user's language. If the conversation is in Korean, write natural Korean.
- Convert scattered discussion into an actionable objective, not a chat summary.
- Include concrete paths, files, commands, branches, tests, and validation handles when they are known.
- Separate required scope from optional future work.
- Do not invent requirements. If something is uncertain, label it as a decision point or optional check.
- Keep the prompt self-contained enough for a new session to begin work immediately.
- Keep the generated goal under 4,000 characters.

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

4. Add execution discipline only when relevant:
   - preserve dirty worktree changes
   - run formatters
   - run test commands
   - use TDD when the user asked for it
   - commit/push only if the user explicitly requests it

5. Finish with a fenced `text` block unless the user asks for prose only.

## Prompt Shape

Use this shape for implementation or investigation handoffs:

```text
/goal

`<workspace-path>`에서 <task title>를 진행한다.

반드시 <language>로 진행한다.

먼저 확인할 것:

- `<command or file>`
- `<file>`

목표:

- <primary objective>
- <accepted design decision>

중요한 방향:

- <boundary or architecture rule>
- <behavior rule>
- <non-goal if important>

구현/조사할 내용:

1. <task item>
2. <task item>
3. <task item>

테스트/검증:

- <test command>
- <smoke command>
- <expected result>

완료 조건:

- <observable done condition>
- <observable done condition>
- <final validation command passes>
```

For planning-only requests, omit implementation/test sections that do not apply and focus on goal, constraints, decisions, open questions, and completion criteria.

## Quality Bar

A good goalified prompt:

- can be pasted into a new session as-is
- has `/goal` as the first line when requested
- names exact files and commands where possible
- captures the user's latest correction, not just the first request
- avoids generic filler like "analyze and implement"
- makes success observable through tests, output, docs, or commands
- keeps future ideas separate from the current scope

## Common Additions

Use these only when they are true for the task:

- "코드를 변경하기 전에 `git status --short --branch`를 확인한다."
- "기존 사용자 변경은 되돌리지 않는다."
- "Go 코드 수정 후 `gofmt`를 실행한다."
- "마지막에 `go test ./...`를 실행한다."
- "새 error는 기존 프로젝트 error 패턴을 따른다."
- "CLI 출력은 JSON/table 출력 계약을 유지한다."
- "스펙/도메인 레이어는 storage, CLI, presentation을 알면 안 된다."

## What Not To Do

- Do not include long transcript summaries.
- Do not add implementation details the user has not agreed to unless clearly marked as a suggested approach.
- Do not ask the next session to commit or push unless the user explicitly asked for that.
- Do not hide unresolved ambiguity; call it out as a decision point.
- Do not make the prompt project-specific unless the conversation is project-specific.
