# Sizing and shaping work

Think about who picks up the ticket and what they need to finish it. The structure exists to make work trackable, not to look thorough.

## Decision steps

1. **Is it decided?** No: open question, no ticket. Yes: continue.
2. **Does it already exist?** If a matching epic or ticket is in Jira or `tickets.md`, update or comment on it instead of duplicating. If a matching epic exists, new stories go under it.
3. **How big is it?**
   - Touches several roles, screens, or systems, or would take one person more than about two weeks: **Epic**. Break it into stories that each ship something visible.
   - One outcome a user would notice, roughly 1 to 5 days: **Story** if user-facing, **Task** if internal (infra, migration, cleanup, reporting).
   - Under a day and obvious: still a Story or Task, no sub-tasks.
4. **Does the story need sub-tasks?** Only if it has three or more distinct technical steps, different people will do different parts, or one step is risky enough to track on its own (a database migration, a data backfill, anything that emails real users). Otherwise the acceptance criteria are enough.
5. **Is it broken behavior?** Make it a **Bug**, link it to its epic, and include repro steps if the transcript has them.

## Splitting an epic into stories

Split by user outcome, not by layer. "Clinic admin can see bookings from every location" is a story. "Backend for bookings" and "frontend for bookings" are sub-tasks of it.

A good first story is the thinnest slice that delivers value and proves the approach. Later stories add roles, edge cases, and polish.

## Examples

**"Admins at the larger clinics only see one location's bookings. We need this fixed this week."**
Bug, Highest. Not an epic: it's one defect with one fix, even if the fix touches a few queries.

**"We'd like the reports page to show average and median wait time, broken down by clinic, and exportable."**
Story under the existing Reporting epic if there is one. If there isn't, and more reporting asks are clearly coming, create an Epic "Reporting v2" with stories for the wait-time card, the per-clinic breakdown, and CSV export.

**"Could patients maybe book through a mobile app?"** when `constraints.md` says the client decided against a mobile app last quarter.
No ticket. Raise it in review as a conflict with an earlier decision.

**"Staff need a shared document library: upload PDFs, filter by department, and get an email when something new is added."**
Epic "Staff document library". Stories: upload and list documents, department filter, new-document email. The email story gets sub-tasks (template, send logic, test against a staging inbox) because it emails real people.

## Priority

| Signal | Priority |
|---|---|
| Client blocked, data wrong, security issue, client said "ASAP" | Highest |
| Client asked with a date, or it blocks other planned work | High |
| Client asked, no urgency | Medium |
| Internal idea, nice-to-have, tech debt | Low |

Always say in the ticket why it has that priority.
