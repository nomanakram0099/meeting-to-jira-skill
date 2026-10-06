---
name: meeting-to-jira
description: Turn a meeting or client-call transcript (Fathom, Zoom, Meet, Otter, pasted notes) into a project-manager-quality plan — a meeting doc plus Jira epics, stories, sub-tasks and bugs, linked to each other and to the doc — using the project's own saved context so nothing is duplicated or contradicts past decisions. Use this whenever the user shares a transcript, call recording summary, meeting notes, or client feedback and wants tickets, a backlog, epics, a plan, "put this in Jira", "make tickets from this call", "what did the client ask for", or wants to keep project documentation up to date after a meeting — even if they don't say "Jira" or "skill".
---

# Meeting to Jira

You are acting as the project manager for this repo. A call happened; your job is to turn what was said into work the team can pick up tomorrow, without losing context, duplicating tickets, or turning every "maybe" into a commitment.

The flow is: **load context → read transcript → plan → user review → create in Jira + doc → update context**. Never skip the review step. Creating tickets is outward-facing and noisy to undo; a wrong epic costs the team more than a 30-second confirmation.

## 1. Load project context

This skill keeps its own memory of the project in `.claude/project-context/` (committed to the repo, so every teammate's Claude sees the same thing). Don't rely on any other plugin, personal memory, or tool being installed.

Read, if present:
- `.claude/project-context/config.md` — Jira site, project key, issue types, Confluence space, people
- `overview.md`, `constraints.md`, `decisions.md`, `glossary.md`
- `tickets.md` — epics/tickets this skill created before, with keys and source meeting
- The last 2–3 files in `meetings/`

If the folder doesn't exist, bootstrap it first — see `references/context-bootstrap.md`. That's a one-time step; tell the user you're doing it.

Also look at Jira itself (open epics and unresolved issues in the project) when a Jira tool is available. `tickets.md` can be stale; Jira is the source of truth for what exists.

## 2. Get the transcript

Accept whatever the user gives: pasted text, a file path, a Fathom share link, or a Fathom summary. If a link needs auth you don't have, ask them to paste the transcript or export it. Note the meeting date, attendees, and who is the client vs the team — this matters for priority (client asks outrank internal musings).

## 3. Extract

Go through the transcript and pull out, with the speaker and a short quote for each:
- **Requests** — features or changes someone wants
- **Bugs** — something broken or behaving wrong
- **Decisions** — things agreed ("we're not doing a mobile app")
- **Open questions** — undecided, "let me check", "maybe", "let's think about it"
- **Action items** — someone committed to do something, with a date if said
- **Context changes** — new facts about the client, users, constraints

Be skeptical of enthusiasm. "It'd be cool if…" is an open question, not a story. A request is real when someone with authority asks for it or the team agrees to do it.

Cross-check every item against context:
- Already exists in Jira or `tickets.md`? → plan an update/comment on that ticket, not a new one.
- Contradicts `decisions.md` or `constraints.md`? → don't ticket it; raise it as a conflict in the review.
- Already built in the code? → check quickly (grep the feature); say so instead of ticketing.

## 4. Plan like a project manager

Size each real item and pick its shape. Full rules and examples are in `references/sizing.md`; the short version:

| Scope | Shape |
|---|---|
| Multi-week, spans several screens/roles/systems, or needs multiple people | **Epic** with Stories under it |
| One user-visible outcome, roughly 1–5 days | **Story** (or Task if not user-facing), under an existing epic when one fits |
| Concrete technical steps inside a story | **Sub-tasks** — only when they help the assignee; skip for small stories |
| Something broken | **Bug**, linked to the related epic |
| Undecided | **No ticket** — goes in the doc's open questions with an owner |

Prefer attaching to existing epics over creating new ones. A board with 40 epics is as useless as one with none.

Every Story/Task/Bug gets: a clear outcome-focused title, why it matters (who asked, quote), acceptance criteria, priority, and labels. Use the templates in `references/ticket-templates.md`.

Priority: client-blocking or client-said "ASAP" → Highest/High; client request with no urgency → Medium; internal ideas → Low. Say why in the ticket.

## 5. Review with the user

Show the plan in chat before touching Jira, in this order:
1. One-paragraph meeting summary
2. Decisions and conflicts with earlier decisions (call these out — they're the most valuable thing you can catch)
3. Proposed tree: Epic → Stories → Sub-tasks, plus Bugs, each with priority and one line of why; mark which are **new** vs **update existing KEY-123**
4. Open questions (no tickets) with suggested owner
5. Items you deliberately dropped and why

Then ask: create all, or what to change. Apply edits and re-show only the changed parts. Don't create anything until the user approves.

## 6. Create in Jira and write the doc

Use whatever Jira/Confluence access the session has — see `references/jira-access.md` for the options (Atlassian MCP connector, or REST API with env vars) and the order of operations.

Order matters so links resolve:
1. Create/update the **meeting doc** (Confluence page if a space is configured; otherwise see fallback in `references/jira-access.md`)
2. Create **Epics**, then **Stories/Tasks/Bugs** under them, then **Sub-tasks**
3. Link every epic and ticket to the meeting doc (remote link / "Confluence pages" link), and add the created ticket keys back into the doc
4. Comment on existing tickets that the meeting touched, quoting the relevant part

If a create fails, stop and report what was created and what wasn't — don't leave the user guessing about a half-made backlog.

The meeting doc format is in `references/ticket-templates.md` (Meeting doc section).

## 7. Update project context

Write back so the next run is smarter:
- `meetings/YYYY-MM-DD-<short-topic>.md` — the meeting doc content, with ticket keys
- Append new decisions to `decisions.md` (dated, with who decided)
- Update `constraints.md`, `glossary.md`, `overview.md` only if the call changed them
- Append to `tickets.md`: key, type, title, parent epic, source meeting, date

Keep these files factual and short. They're read at the start of every run, so bloat costs every future meeting.

## 8. Report

End with: link to the meeting doc, the list of created/updated keys grouped by epic, open questions with owners, and any conflicts the user still needs to resolve. Keep it short.
