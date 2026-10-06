# Jira and Confluence access

Use the first option that works in this session. Check before planning so you can tell the user early if nothing is connected (they can still get the plan and doc; creation waits).

## Option A — Atlassian MCP connector (preferred)

If tools with names containing `atlassian`, `jira`, or `confluence` are available (possibly deferred — search for them), use them. Typical capabilities: search issues with JQL, get issue types for a project, create issue, edit issue, add comment, link issues, create Confluence page, add remote link.

Steps:
1. Find the cloud/site ID and confirm the project key from `config.md`.
2. Fetch the project's issue types and required fields once; save names (Epic, Story, Task, Bug, Sub-task or company-specific equivalents) to `config.md` so later runs skip this.
3. In team-managed projects, children attach to an epic via the `parent` field. In company-managed projects, it may be `parent` or an "Epic Link" custom field — check the create metadata rather than guessing.

## Option B — Jira REST API

If env vars `JIRA_BASE_URL`, `JIRA_EMAIL`, `JIRA_API_TOKEN` are set, use the REST API v3 with basic auth via `curl`. Never print the token or write it to any file.

- Search: `POST /rest/api/3/search/jql` with `{"jql": "project = KEY AND statusCategory != Done", "fields": ["summary","issuetype","parent","status"]}`
- Create: `POST /rest/api/3/issue` — description must be Atlassian Document Format (ADF), not markdown. Build a minimal ADF doc of paragraphs, headings, and bullet lists.
- Child of epic or story: `"parent": {"key": "KEY-1"}`
- Comment: `POST /rest/api/3/issue/{key}/comment`
- Link to doc: `POST /rest/api/3/issue/{key}/remotelink` with `{"object": {"url": "<doc url>", "title": "<doc title>"}}`
- Confluence page: `POST <base>/wiki/api/v2/pages` with `spaceId`, `title`, and `body.representation = "storage"`.

## Option C — nothing connected

Do everything except creation: produce the reviewed plan and the meeting doc, save the doc to `.claude/project-context/meetings/`, and also write a `pending-tickets.md` next to it listing every ticket in creation order. Tell the user what to connect (Atlassian connector in claude.ai Settings → Connectors, or the three env vars) and that rerunning the skill on that meeting will create the tickets from the saved plan.

## Where the meeting doc goes

- Confluence space set in `config.md` → create the page there; link it from every epic and ticket.
- No Confluence → put the full meeting doc in the description of the main epic (or a "Meeting notes" Task if no epic was created), and link other tickets to that issue. Keep a copy in `meetings/`.

## Safety

- Create nothing before the user approves the plan.
- Don't transition, reassign, or close existing issues unless the user asked; comment instead.
- If a call fails mid-way, report exactly which keys exist and which items are still pending.
