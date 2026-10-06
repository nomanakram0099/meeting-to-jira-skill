# Changelog

## 0.1.0 (2026-10-06)

First public version.

- Reads transcripts from Fathom, Zoom, Meet, Otter or pasted notes
- Sizes work into epics, stories, tasks, sub-tasks and bugs, and attaches to existing epics where it can
- Shows the full plan for review before creating anything
- Creates a meeting doc (Confluence, or in the epic if there's no Confluence) and links every ticket to it
- Keeps project context in `.claude/project-context/` and updates it after each meeting
- Works with the Atlassian connector or a Jira API token; plans and saves pending tickets when neither is set up
