# meeting-to-jira

A Claude Code skill that reads a meeting transcript and turns it into a Jira backlog: epics, stories, sub-tasks and bugs, each linked to a meeting doc that explains where it came from.

I built this because I was spending an hour after every client call rewriting my notes into tickets, and still missing things. Half the ideas in a call are "maybe" and shouldn't become tickets at all. The other half need to land in the right epic, not as yet another loose story. That's a judgment call, so this skill tries to make it the way a project manager would, and then asks you before it creates anything.

## What it does

Give it a transcript (Fathom, Zoom, Google Meet, Otter, or just pasted notes) and it will:

1. Read your project's context: what the product is, past decisions, constraints, and what's already in Jira.
2. Pull out requests, bugs, decisions, open questions and action items, with who said what.
3. Drop the "maybes" into an open questions list instead of the backlog.
4. Catch duplicates of existing tickets and things that contradict earlier decisions.
5. Size the real work: big things become epics with stories, small things become stories under an existing epic, sub-tasks only when a story actually needs them, broken things become bugs.
6. Show you the whole plan and wait for your OK.
7. Create the meeting doc and the tickets in Jira, linked to each other.
8. Save what it learned to a small context folder in your repo, so the next meeting starts from where this one left off.

See [`examples/clinic-portal`](examples/clinic-portal) for a sample transcript and the plan it produces.

## Install

In Claude Code:

```
/plugin marketplace add nomanakram0099/meeting-to-jira-skill
/plugin install meeting-to-jira@meeting-to-jira
```

Or copy the skill into a single project, so everyone working on that repo gets it:

```
git clone https://github.com/nomanakram0099/meeting-to-jira-skill
cp -r meeting-to-jira-skill/skills/meeting-to-jira your-project/.claude/skills/
```

Restart Claude Code after installing.

## Connect Jira

The skill uses whichever of these is available:

**Atlassian connector (recommended).** In claude.ai go to Settings → Connectors and add Atlassian. This covers both Jira and Confluence, and Claude Code picks it up on the next start.

**Jira API token.** Set these in your shell:

```
export JIRA_BASE_URL=https://your-team.atlassian.net
export JIRA_EMAIL=you@company.com
export JIRA_API_TOKEN=...   # https://id.atlassian.com/manage-profile/security/api-tokens
```

**Nothing connected.** It still does the planning and writes the meeting doc, and saves a `pending-tickets.md` list. Run it again on the same meeting once Jira is connected and it creates the tickets from that list.

## Use it

Paste a transcript and say what you want:

```
here's today's call with the client, make tickets from it

<transcript>
```

Other ways people ask that work fine: "what did the client ask for in this call", "put this in Jira", "update the backlog from these notes".

The first time you run it in a repo, it builds a context folder by reading your README, schema, routes and recent commits, then asks you a few things the code can't tell it: your Jira project key, Confluence space (if any), and who the client is. After that it doesn't ask again.

## Project memory

Everything the skill knows about your project lives in `.claude/project-context/` in your repo:

```
.claude/project-context/
  config.md         Jira site, project key, issue types, Confluence space
  overview.md       what the product is, roles, main areas, stack
  constraints.md    hard rules ("patients never log in")
  decisions.md      dated log of what was agreed and by whom
  glossary.md       the client's words vs the names in your code
  tickets.md        tickets this skill created, and which meeting they came from
  meetings/         one summary per meeting
```

It's plain Markdown. Commit it, so your whole team's Claude works from the same picture, and edit it by hand whenever it's wrong. It doesn't depend on any other plugin or on anyone's personal Claude memory.

## How it decides what's an epic

| What was said | What it creates |
|---|---|
| Multi-week work across several screens, roles or systems | Epic with stories |
| One thing a user would notice, a few days of work | Story (or Task if internal), under an existing epic when one fits |
| A story with several distinct technical steps, or a risky step | Sub-tasks under that story |
| Something broken | Bug, linked to its epic |
| "Maybe", "let me check", "we should think about" | Nothing. It goes in the doc's open questions with an owner |

Priority comes from what the client said: blocked or "ASAP" is Highest, a request with a date is High, a request with no urgency is Medium, internal ideas are Low. Each ticket says why it got its priority.

The full rules are in [`skills/meeting-to-jira/references/sizing.md`](skills/meeting-to-jira/references/sizing.md). If your team sizes things differently, edit that file.

## What it won't do

- Create anything before you approve the plan.
- Close, reassign or transition existing tickets. It comments on them instead.
- Read a Fathom or Zoom link that needs a login. Paste the transcript or export it.
- Store your API token anywhere.

## Limitations

- It works from the transcript, so if something was agreed in Slack afterwards, it doesn't know. Add it to `decisions.md`.
- Sizing is a judgment call. It's usually right about epic vs story; it's less reliable at guessing days of effort, so it doesn't set story points.
- Speaker labels matter. If the transcript doesn't say who's the client, tell it.
- Tested with Jira Cloud. Jira Server/Data Center may need field tweaks.

## Contributing

Issues and PRs welcome, especially example transcripts (anonymized) where it made the wrong call. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT
