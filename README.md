# NorthBay Logistics: Weekend Alert Correlation

> Bootcamp lab: simulated scenario (TripleTen Cybersecurity, 2026)

## Context
Over a weekend at NorthBay Logistics, five critical alerts were
closed as false positives by colleagues on different shifts, each
investigated in isolation. On Monday morning my supervisor didn't
care whether each one was a false positive on its own; he wanted
to know whether together they told a different story.

## What I did
- Re-triaged the five closed alerts with my own judgment, not the
  reasoning of the analysts who closed them, using a triage matrix
  (signal or false positive, operational severity, common anchor).
- Found the common anchor: the same service account on the same
  backup server appeared in all five alerts.
- Tested the closing premise of the first ticket ("password rotation
  not propagated") against the logs and showed it didn't hold up:
  the account's scheduled job had logged in fine after the rotation.
- Built one correlated timeline across AD, firewall, DNS/proxy and
  EDR, labeling every event as confirmed, inferred, or unknown.
- Found a clock conflict between EDR and AD (1m43s to 2m42s),
  applied a bounded correction to that single event, and flagged
  it as an unverified assumption.
- Added up about 72.6 GB sent to one external destination in three
  transfers (50 + 18.4 + 4.2 GB).
- Declared a time gap I couldn't close (about 1h28m after the
  account gained Domain Admin rights) instead of inventing events.
- Wrote a scope note with an explicit cut (what I would not do
  today and what evidence would change my decision) and a
  12-line executive report: Confirmed, Probable, Unknown, Actions.

## Decision
Escalate. I didn't choose Monitor because the exposure was still
open, and I didn't Contain blindly because the maintenance calendar
was out of date and it could have been authorized work.

## Tools & methods
Active Directory, firewall, DNS/proxy and EDR logs, plus ITSM,
CMDB and the corporate calendar for context. Triage matrix,
timeline correlation, clock-skew normalization.

## Results
Five tickets closed as "nothing to see here" turned out to read as
one sequence compatible with a compromised service account,
escalated with clear limits on what is confirmed, what is probable,
and what is still unknown.

## Deliverables
- Report: /Proyecto 5.pdf

## Instructor review
Approved. Praised for challenging a ticket's closure premise,
documenting the clock skew explicitly, and a justified Escalate
decision.

## What I learned
A closed ticket is not a resolved problem. When I put the five
alerts side by side, the premise behind one closure didn't hold up
against the logs. I learned to be explicit about clock skew and to
label an assumption as unverified instead of hiding it, to escalate
when the evidence justifies it without declaring a confirmed
intrusion, and to draw a clear line around what I won't do today.
And to write all of it in 12 lines that a non-technical manager can
act on.
