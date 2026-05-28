# Goalify

Goalify is a Codex skill that turns an existing conversation into a fresh-session `/goal` prompt.

It is useful when you want a new agent session to continue work with the right objective, constraints, context, and completion criteria without rereading the whole thread.

## Contents

- `Skills/goalify/SKILL.md` - the skill instructions
- `Skills/goalify/agents/openai.yaml` - OpenAI agent metadata for the skill

## Install

Copy the skill directory into your Codex skills folder:

```sh
cp -R Skills/goalify ~/.codex/skills/goalify
```

Then restart Codex or reload skills if your environment supports it.

## Usage

Ask Codex to use Goalify when you need a handoff prompt:

```text
Use goalify to turn this conversation into a new-session /goal prompt.
```
