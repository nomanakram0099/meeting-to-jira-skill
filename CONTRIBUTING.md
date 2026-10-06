# Contributing

Thanks for taking a look.

## Reporting a bad call

The most useful thing you can send is a case where the skill got it wrong: made an epic that should have been a story, ticketed a "maybe", missed a duplicate, picked the wrong priority. Open an issue with:

- the transcript, or the relevant part of it (please anonymize names and companies)
- what it created
- what you expected instead

## Changing the skill

The skill is plain Markdown in `skills/meeting-to-jira/`:

- `SKILL.md` is the main flow. Keep it short; it's loaded on every run.
- `references/` holds the details (sizing rules, templates, Jira access, first-run setup). These are only read when needed.

When you change behavior, run the skill on `examples/clinic-portal/transcript.md` before and after, and include both plans in your PR so the difference is easy to review. If your change needs a new kind of meeting to test against, add another folder under `examples/`.

## Style

Write instructions the way you'd explain them to a new teammate: say what to do and why. Avoid piles of MUST and NEVER; the reason usually works better than the shouting.
