# Saved reports implementation plan

Workspace: `/workspace/reports`
Canonical location in that workspace: `docs/reports-plan.md`
Revision: approved-v2

## Approved scope

Phase 1 and Phase 2 are approved. Phase 3 is a future proposal, not part of the approved implementation.

## Shared decisions

- Use the existing React dashboard shell and table styles.
- Preserve the public saved-report URL format and existing access controls.
- Do not add a new database or reporting backend.
- Existing user changes must be preserved.

## Phase 1 — Read-only report list

### Implementation

- Extend `src/reports/ReportsPage.tsx` with title, owner, updated date, and status columns.
- Read reports through the existing `ReportRepository.list` method.
- Add status filtering using the existing query-string helpers.
- Keep selection stable when the result list refreshes.
- Reuse `ReportsEmptyState` when no reports match the filter.

### Verification and completion

- Run `npm run typecheck` and `npm test -- reports-list`.
- Verify filtered, unfiltered, empty, and access-denied cases in the browser.
- The list, filter, stable selection, and empty state work with the existing URL and access contracts.

## Phase 2 — CSV export

Prerequisite: the Phase 1 report list and existing access checks.

### Implementation

- Add export through the existing `ReportActions` menu.
- Export only rows visible under the current status filter.
- Write CSV with UTF-8 BOM and CRLF records.
- Escape quotes and commas according to the existing CSV helper.
- Include title, owner, updated date, and status in that order.
- Disable export when there are no rows; use the existing toast for failures.
- Preserve the selected report and current filter after export.

### Verification and completion

- Run `npm run typecheck` and `npm test -- reports-export`.
- Verify Korean titles, embedded commas and quotes, empty results, export failures, and restricted reports.
- Check desktop and mobile behavior in the browser.
- Export respects the filter and access rules, retains selection, produces the specified format, and passes the required checks.

## Phase 3 — Future proposal

Scheduled delivery by email, delivery history, and a background job system may be explored later. Delivery frequency, provider, retry policy, and retention are unresolved. None is required for Phase 1 or Phase 2.
