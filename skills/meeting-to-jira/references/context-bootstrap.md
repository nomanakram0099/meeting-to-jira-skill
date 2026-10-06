# Bootstrapping `.claude/project-context/`

Run once, when the folder doesn't exist. Goal: a short, accurate picture of the project that makes ticketing decisions better. Aim for each file under ~80 lines.

## Gather from the repo (read-only)

- `README*`, `CLAUDE.md`, `AGENTS.md`, any `docs/` overview files
- `package.json` / equivalent — stack and main dependencies
- Database schema or generated types (e.g. `src/types/database.types.ts`, `supabase/migrations/`) — entities and roles
- Top-level app structure (routes, main components, edge functions) — the product's areas
- `git log --oneline -50` — what's been worked on recently

## Write

`overview.md`
```
Product: <one paragraph — what it is, who pays, who uses it>
Users and roles: <role — what they do>
Main areas: <feature area — where it lives in code>
Stack: <frontend, backend, db, hosting, email>
```

`constraints.md` — hard rules that shape what gets built (e.g. "patients never log in; everything is done by front-desk staff"). Start with what's clear from code/docs; mark guesses with "(unconfirmed)".

`decisions.md` — dated log, newest last: `YYYY-MM-DD — <decision> — <who> — <source>`. Seed from docs or commit history only when clear.

`glossary.md` — client's words vs names in code (e.g. "practice" = `organization`, "front desk" = `staff` role).

`tickets.md` — table: `Key | Type | Title | Parent | Source meeting | Created`. Seed from Jira's open epics if a Jira tool is available.

`config.md`
```
Jira site: <https://xxx.atlassian.net>
Project key: <KEY>
Issue types: <Epic, Story, Task, Bug, Sub-task — as named in this project>
Epic link field: <parent | customfield_xxxxx>
Confluence space: <key or "none">
Client: <company, main contact and role>
Team: <names and roles>
```

`meetings/` — empty folder.

## Ask the user once

After writing drafts, ask only what the repo can't answer — typically: Jira project key, Confluence space (or none), client name and main contact, team members, and any rules that aren't in the code. Fill them in, then continue with the transcript.

If Jira is connected, discover the site, project, and issue types yourself and only ask the user to confirm the project.
