# Authentication implementation plan — draft

Workspace: `/workspace/auth-service`
Canonical location: `docs/auth-plan.md`

## Agreed requirements

- Keep existing users signed in during migration.
- Preserve the public login endpoint and error-response contract.

## Unresolved decisions

- The team has not chosen cookie sessions or bearer tokens.
- Token/session expiration and server-side storage are undecided.
- Migration rollout and acceptance checks have not been agreed.

This is an incomplete working draft. Its title does not imply that implementation decisions or completion criteria have been approved.
