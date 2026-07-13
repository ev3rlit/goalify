# Goalify

Goalify turns an ongoing conversation into a clear `/goal` prompt you can use to start fresh without losing the thread.

## How to use it

Talk through the work first. Explore the problem, make decisions, narrow the scope, and get to the point where the next step feels clear.

Then ask the assistant to use goalify:

```text
Use goalify to turn this conversation
```

Goalify will package the objective, context, constraints, and completion criteria into something you can paste directly into the next session.

Goalify's own instructions and reusable scaffold are written in English, but the generated `/goal` follows the language used by the user. It also tells the next session to use that language for progress updates and the final explanation.

It also asks the next session to teach you what happened after the work is complete. The final explanation covers the important stages in order, why each material choice was made, how the result was checked, and any limits that remain. Technical terms stay intact, but are explained with direct language and concrete examples.

## Example output

Goalify might produce a prompt like this:

```markdown
/goal

Build the saved reports view in `/workspace/acme-dashboard`.

Context:

- The user wants a compact reports screen, not a marketing-style landing page.
- The app already uses React, Tailwind, and the existing dashboard shell.
- Keep the current navigation and table styling consistent with the rest of the product.

Task:

1. Add a saved reports list with title, owner, updated date, and status.
2. Add filtering for report status.
3. Add an empty state for teams with no saved reports yet.

Verification:

- Run the existing type check and test commands.
- Open the app locally and confirm the reports page works on desktop and mobile.

Done when:

- The reports view is implemented.
- Existing behavior is preserved.
- The final verification commands pass.

After completing the work, explain it to the user:

- Walk through the starting point, important work stages, and final state.
- For each important stage, explain what changed, why that approach was chosen, and how it was verified.
- Explain the core mechanism in direct language a 12-year-old could follow, while defining any necessary technical terms.
- Separate verified results from anything that remains uncertain or untested.
```
