# Plan shown for review

This is what the skill showed before creating anything, for [`transcript.md`](transcript.md). The project context already had a `Reporting` epic (`CLIN-12`) and a decision from 2026-06-03 not to build a mobile app.

---

**Summary**

Weekly check-in with Dana Ruiz (Northfield Clinics). Urgent bug: admins with more than one location only see one location's bookings. Dana asked for average and median wait time per clinic with Excel export for the end-of-October board pack, and for a staff document library with department filters and email alerts. The engineering team raised a recurring backup job failure.

**Decisions**

- Mobile booking app stays out of scope. Dana confirmed she doesn't want to reopen it. *Matches the 2026-06-03 decision, no change.*
- Any admin can upload documents for their own clinics; head office can upload for all. (Dana)

**Proposed tickets**

```
NEW  Bug    Admins with multiple locations only see bookings for one        Highest
            "We need that fixed this week if at all possible" (Dana). Affects every
            multi-location admin, e.g. Priya (3 locations) sees only the first.

     CLIN-12 Reporting (existing epic)
NEW    Story  Wait time report shows average and median per clinic          High
              Due before end-of-October board pack.
NEW    Story  Export wait time report to Excel                              High

NEW  Epic   Staff document library
NEW    Story  Admins upload and list documents for their clinics            Medium
NEW    Story  Head office uploads documents for all clinics                 Medium
NEW    Story  Staff filter documents by department                          Medium
NEW    Story  Staff get an email when a new document is added               Medium
NEW      Sub-task  Email template
NEW      Sub-task  Send to staff of the affected clinics only
NEW      Sub-task  Test against staging inbox before enabling

NEW  Task   Fix weekly nightly-backup failures                              Low
            Internal. Retries succeed and no data lost, so not urgent.
```

**Open questions (no ticket)**

- Monthly reports emailed to the board automatically? Owner: Dana, checking if the board wants it.

**Not ticketed**

- Mobile booking app: conflicts with the 2026-06-03 decision and Dana confirmed it stays out.

---

Create all of these, or change something first?
