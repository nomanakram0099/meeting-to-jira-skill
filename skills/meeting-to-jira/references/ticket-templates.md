# Templates

Write for a developer who wasn't on the call. Plain language, no transcript dumps — one short quote is enough to show where it came from.

## Epic

**Title:** outcome in a few words ("Manager resources library")

**Description:**
```
Goal
<What changes for which users, and why the client wants it.>

Source
<Meeting date, who asked> — "<short quote>"
Meeting doc: <link>

Scope
- In: <bullets>
- Out: <bullets — things discussed but explicitly not included>

Success looks like
<1–3 observable outcomes>
```

## Story / Task

**Title:** what the user can do, or what is done ("Clinic admin sees bookings from every location")

**Description:**
```
Why
<Who needs it and why. Priority reason.>
Source: <meeting date, speaker> — "<short quote>"  ·  Meeting doc: <link>

What
<The change, in a few sentences. Mention affected roles/screens if known.>

Acceptance criteria
- [ ] <testable statement>
- [ ] <testable statement>
- [ ] <edge case worth stating>

Notes
<Relevant code areas, related tickets, constraints from project context. Optional.>
```

## Sub-task

**Title:** concrete step ("Add median to analytics query")
**Description:** one or two lines; link to parent is automatic.

## Bug

**Title:** what's wrong, from the user's view ("Clinic admin only sees one location's bookings")

**Description:**
```
What happens
<Observed behavior>

Expected
<Correct behavior>

Steps to reproduce
1. <if known; otherwise "Not given in call — needs repro">

Impact
<Who is affected, how badly. Priority reason.>

Source: <meeting date, speaker> — "<short quote>"  ·  Meeting doc: <link>
```

## Meeting doc

Title: `YYYY-MM-DD — <Client/Team> — <topic>`

```
Attendees: <names and roles>
Date: <date>

Summary
<3–5 sentences: what the meeting was about and the main outcomes.>

Decisions
- <decision> — <who>   (flag if it reverses an earlier decision, with date of the old one)

Work created
<Epic KEY — title>
  - <KEY — story title> (priority)
    - <KEY — sub-task>
<Bugs: KEY — title (priority)>
Updated existing: <KEY — what changed>

Open questions
- <question> — owner: <name>

Action items
- <who> — <what> — <when>

Not ticketed
- <item> — <why: undecided / conflicts with decision X / already built>
```
