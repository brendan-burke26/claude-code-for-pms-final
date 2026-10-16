# Brief: Missed pings and quiet responders after release 4.2

**For:** Helen Achebe
**Status:** Draft. No prototype and no code changes.

## Executive summary

- Release 4.2 (12 August 2026) made three changes: proximity now counts for more than recent acceptance, the ping wait was cut from 90 to 60 seconds, and console filters now persist.
- Missed pings rose from 2.3% to 18.0% of all pings. Turn-downs barely moved (21.1% to 18.0%). The rise is in misses, not refusals.
- The median gap between a missed ping and the next ping in the same callout fell from 92 to 62 seconds, which matches the timeout change. This comes from our earlier analysis and has not been re-run.
- The 60-second wait is the most direct explanation for the rise in misses. We have not tested it, and we have no response-time data.
- Four responders also stopped being offered their own areas' callouts. That is a ranking effect and is separate from the wait.
- This brief describes a visible flag for handlers and a tap-to-return step for responders. It does not change the wait, the weights, or the scoring.

## Problem and affected users

A ping that is not answered within 60 seconds counts as missed and moves to the next responder. Under the current scoring, a miss lowers a responder's score by the same amount as a turn-down. Scores recover only when a responder takes work. A responder who misses several pings falls in the order, is offered fewer callouts, and so has fewer chances to take work. Nothing in the system raises them back.

Two groups are affected.

- **Handlers** use the desktop console. They can see that a responder has gone quiet but not why. In interviews on 2–5 September, Kip described Meteor Mite going quiet while The Gale, in the same area, kept receiving callouts.
- **Responders** use the phone. Some report that the phone never goes off. Others report that the callout was taken before they could answer. Tickets lean toward the first. Interviews lean toward the second.

## Solution

**Goals**

1. A handler can see that a responder is being missed and that their offers have dropped.
2. A responder is told on the phone that they have been missed, and can confirm they are available with one tap.
3. Neither change alters ping timing, ranking, or scoring, so the cause can be studied separately.

**For handlers:** a flag on the responder list showing the number of missed pings and the number of own-area callouts the responder was not offered. The flag is view-only in this version.

**For responders:** after a run of missed pings, the phone shows: "You have missed several pings, so your place in the order has dropped. Tap to confirm you are available." A tap restores own-area offers for a set period. The length of that period is not yet decided.

## Out of scope

- Changing the 60-second ping wait, or restoring 90 seconds.
- Changing the proximity and acceptance weights.
- Changing how misses and turn-downs are scored.
- Fixing push delivery. Reports that the phone never goes off are recorded as a separate problem.
- Showing scores to responders.
- Identifying any responder, or linking a cover identity to a legal identity (Security Policy 4.1).
- Console requests such as dark mode, text size, and alert sounds.
- Supply's maintenance queue.
- Mutual aid (Q4 list).
- Guaranteeing that a responder will be pinged.

## Success metrics

Baselines come from our earlier analysis and have not been confirmed by Ravi Menon.

| Measure | Before 4.2 | After 4.2 | Proposed target |
|---|---|---|---|
| Missed share of pings | 2.3% | 18.0% | Under 5% within 6 weeks |
| Taken share of pings | 76.6% | 64.0% | About 75% |
| Own-area callouts offered to the four | 82% | 21.5% | Above 60% |
| Pings a week, each of the four | 12–14 | 1–5 | At least 8 |
| Callout tickets a week | 5–8 | 20–32 | 5–8 |

**Guardrails:** the turn-down share stays at or below 18%, coverage gaps do not increase, and time-to-accept does not get worse.
