# Goalify

Goalify turns an ongoing conversation into a clear `/goal` prompt you can use to start fresh without losing the thread.

## How to use it

Talk through the work first. Explore the problem, make decisions, narrow the scope, and get to the point where the next step feels clear.

Then ask the assistant to use goalify:

```text
Use goalify to turn this conversation
```

Goalify will package the objective, context, constraints, and completion criteria into something you can paste directly into the next session.

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
```
