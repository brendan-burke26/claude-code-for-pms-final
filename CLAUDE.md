# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

Sources: rook-wiki (About Rook, Glossary, Team directory, Releases, Q3 roadmap,
Product briefs, 4.2 release comments) and 00-rook/company/notes/handoff-from-priya.docx.
Wiki snapshot as of 13 Oct 2026. Verified = in a wiki page; Hypothesis = someone's opinion.

### Me and the job
User is the new PM for Rook Dispatch (started Mon 24 Aug 2026), taking over from Priya.
Reports to Helen Achebe. Tools: rook-wiki (pages) and rook-database (SQL, read-only).

### Products
- **Rook Dispatch** (mine, flagship): responder coordination — availability, proximity,
  who gets pinged for a callout, whether they take it. Users: handlers (mostly at the
  console, desktop-only) and responders (phones).
- **Rook Supply**: gear provisioning — requisitions, maintenance, failure reports.
  Users: handlers, quartermasters. Shares the Responder Availability Record (Dispatch
  writes, Supply reads).
- Subscription priced per active responder. Monthly release train; point releases are 4.x.
- Hard constraint: Rook stores no mapping from cover identity to legal identity. Never
  design for it or try to work out who anyone is (Security Policy 4.1).

### People (wiki comments use roles, not names)
| Role in comments | Person | Notes |
|---|---|---|
| Engineering manager | Marcus Oyelaran, Site Aleph | Runs Dispatch eng. Priya's "start here" |
| Staff engineer | Wen Li, Berlin | Built the ping-ranking logic; no written doc exists |
| Product designer | Sofia Marino, Site Aleph | Console + phone app; ran the September interviews |
| Support lead | Nadia Hoffmann, Berlin | Owns tickets; has the 4.2 ticket breakdown |
| Director of Product | Helen Achebe, Site Aleph | My manager; owns roadmap and commitments |
| (author "Priya") | Priya Raghunathan | Previous PM, left 21 Aug; Marcus has her number |
| — | Ravi Menon, Singapore | Data Analyst; owns the weekly acceptance reporting |
Handoff says engineering manager can "pull numbers", but the real weekly numbers are
Ravi's. Handlers named in the wiki: Halloran (Supply, Sgt. Bulwark), Aunt Dot (Vesper),
Mr. Ambrose (Captain Vantage), Kip (Meteor Mite, The Gale).

### Vocabulary (full list: wiki Glossary)
- **Callout**: request for a responder to attend an incident. **Ping**: one callout
  offered to one responder. **Taken / turned down / missed** (no answer before ping wait;
  logged separately from turned down; both move the ping to the next responder).
- **Ping wait**: how long a ping stays live, same for everyone. Now 60s (was 90).
- **Acceptance rate**: pings taken / all pings. Headline metric. **Time-to-accept**: median
  seconds ping sent to taken. **Coverage gap**: no available responder had the needed
  tags — different from low acceptance (nobody could vs nobody would).
- **Routing priority**: score ranking responders: proximity (travel-time estimate),
  availability, capability match, recent acceptance history. Turning down or missing
  pings lowers recent acceptance, which lowers later rank.
- **Capability tags**: flight, structural-entry, hazmat-tolerant, cold-weather, aquatic,
  crowd-management, de-escalation. **Mutual aid**: cross-responder cover, unsupported, Q4 list.
- Supply: requisition, field failure report, service interval, quartermaster.

### Release history (wiki Releases)
- 4.0 (7 Apr): new console nav, responder profile redesign, routing override audit log.
- 4.1 (16 Jun): proximity uses travel-time estimate; bulk callout; push reliability.
- 4.2 (12 Aug): proximity weighted up vs recent acceptance; ping wait 90→60s; console
  filters persist; 3 defect fixes.

### Where things stand
**4.2 is the live problem.** Since 12 Aug: callout tickets ~3x normal, elevated and flat
as of 26 Aug. Split ~2/3 "phone never goes off", ~1/3 "gone before I could answer".
A handler emailed Nadia directly (rare).
- **Data (rook-database, read-only SQL; tables callouts, pings, responders, handlers,
  support_tickets; data ends 6-7 Sep).** Weekly taken rate was flat at 75-78% for six
  weeks, then 54% (week of 10 Aug), 66, 67, 73. Missed pings went from ~2% to 21%, then
  18, 15, 13. Tickets went from 5-8/week to 20-32/week; 83 of the 108 filed since 10 Aug are open.
  That is a step change at 12 Aug, so "seasonal" does not fit; callout volume also
  dipped ~10-20%, which is the only seasonal-looking part.
- **The loss is concentrated.** 12 responders held at ~64-71% taken (down ~10 pts from
  ~76%). Four collapsed: Vesper, The Undertow, Meteor Mite, Farlight went from 70-86
  pings each pre-4.2 to 11-17 after, 18-29% taken, 53-64% missed. Turn-downs did not
  rise; misses did, so they look cut off, not unwilling (Ambrose: slower answers used to
  still win the job). Their pings fell from 12-14/week to 1-5 within 2 weeks, no recovery by 7 Sep.
- **Code (00-rook/code/dispatch-routing; owner Wen Li; config.py says tell Marcus before
  changing anything).** 4.2 set timeout 90→60s and weights proximity .45→.60, recent
  acceptance .40→.25, capability .15. history.py: taken +0.08, turned down OR missed
  -0.12 (identical), floor 0, no decay (2019 TODO asks if scores should ease back to
  neutral). offer.py walks the ranked list one at a time, so a low rank means rarely
  reached, and only taking work raises a score. **Mechanism is in the code; the four's
  actual scores and response times are not in the DB (unverified).** Timeout and weights
  shipped together, so their effects can't be separated from the data. Marcus asked 14
  Aug whether the weighting should apply to responders who turn jobs down; Wen never answered.
- Gaps: aggregate weekly acceptance reporting hides the split. Supply books maintenance
  into low-callout windows from Dispatch's availability record, so quiet responders look
  free to Supply. Security Policy 4.1 is not in the wiki.
- **Interviews (Sofia, 2-5 Sep, console redesign research, 4 handlers)** match: Aunt
  Dot (Vesper): quiet stretches, and the phone "gone" before he can answer; Kip: Meteor
  Mite dead quiet while The Gale (same area) never stops; Ambrose and Halloran: callouts
  gone before the responder was ready. Also asked for: bigger text, dark mode, per-responder
  alert sounds, filter-reset warning; Halloran's Supply complaints (one queue, no
  feedback on failure reports, poor catalog search) are separate from Dispatch.
- Priya's read ("August is seasonal, wait for September, don't revert 4.2") is not
  supported by the data above; her point that proximity was a requested change stands.
  Marcus (28 Aug) wanted to regroup on 4.2 with me. Nadia has the ticket breakdown;
  Ravi owns the weekly numbers (ask him to confirm mine).

**Roadmap (Q3, last reviewed 30 Jun, stale; all owned by Priya, so now ownerless):**
- 4.2 Committed: who-gets-pinged change (shipped); ping timeout tuning (shipped as the
  90→60 cut, "tuning" may mean more); **Availability Confidence — not in 4.2 notes,
  looks slipped, still marked Committed.**
- 4.3 Committed: Requisition approval chains (Supply).
- Q4 Exploring: Handler phone app; Shared cover between responders.
Priya says the Director of Product conversation on which squeezed-out items are still
committed has not happened. It needs to.

**Briefs (wiki Product briefs):** Bulk Callout (Jan) and Routing Override Audit Log (Mar)
read as unbuilt, but the Releases page shows both shipped (4.1, 4.0) — briefs are stale.
Handler Phone App (8 Sep, designer exploring; driven by one handler quote). Requisition
Approval Chains (Jul) has scope creep: one real ask (second approval over $2,000) plus
four add-ons, phasing TBD.

### Open to-dos from the handoff
1. **Helen's request (05-super-speed/director-request.txt):** one-pager plus clickable
   prototype of what a handler like Kip and a quiet responder would experience. Not a
   setting change. Nothing is to be touched in the code before that.
2. Write down how routing decides who gets pinged (Wen Li is the source; code README
   covers files, not the product logic).
3. Settle Q3 commitments with Helen (Availability Confidence looks slipped).
4. Get response-time logs and current scores from Wen/Marcus; data stops 7 Sep.
5. Console filter ticket noise: low priority. Briefs for Bulk Callout and Audit Log are stale.

### Session 1 conclusions (13 Oct 2026)
- Key question: are the quiet responders refusing work or cut off before they can answer?
  Data says cut off (misses up, turn-downs flat). Lead theory: 60s timeout turns slow-but-
  willing responders into misses, then identical miss/decline penalty plus no score decay
  keeps them buried. Untested; needs response-time logs and current scores from Wen/Marcus.
- Unexplained: after 4.2 every responder is pinged for callouts outside their usual area
  more often (~80% in-area before, 55-70% after), though proximity was weighted up.
- DB tips: callouts and pings join on callout_number; ping order within a callout = rank
  by sent_at; 4.2 cutoff is sent_at >= '2026-08-12'; responders table links handler names.
- Handlers' console asks (dark mode, bigger text, per-responder sounds) are separate
  from the 4.2 problem; keep them out of the fix.

### Working notes
Handoff author was the only PM for 14 months and says she moved faster than she checked.
Verify claims against wiki or data. Cite the source when stating a fact.

### Session 2 conclusions (13 Oct 2026)
- Tickets (147, support_tickets) and interviews (4 handlers) are almost different crowds: only Ambrose filed a ticket. Tickets: 30 quiet-responder, 15 gone-before, 16 filters, 26 Supply, 6 phone glitches (all pre-4.2), 54 other console/account; 83 open, all 45 quiet/gone tickets open. Interviews stress "gone before"; tickets stress "quiet" 2:1. Dark mode is only 4 tickets.
- Replaying history.py on pings (assumes start 0.5): Vesper, Meteor Mite, The Undertow, Farlight hit score 0 after 11-17 pings following clusters of 4-6 early misses; the other 12 stay 0.7-1.0. Halfmoon and Corporal Ashgrove are a mild dip (~25-30% fewer pings), not a collapse.
- The loss is broad: callouts nobody took went 5.5% to 13.5% outside the four's areas vs 5.4% to 7.0% in theirs; everyone's miss rate went ~2% to 13% at every hour. The four were pinged on 21.5% of their own-area callouts after 4.2 (82% before); the other 12, 99% (94% before). Area volume and time of day do not separate the four.
- Weights alone should not bury the four (acceptance weight .40 to .25, max penalty .25 = ~19 min travel); that cannot be checked without travel times. availability.py says Supply only reads the availability record, so the Supply loop is weak.
- Current hypothesis: the 60s wait raised misses for nearly everyone and doubled unfilled callouts; with miss = refusal and no decay, four responders with early miss clusters sank to the floor. Unproven: that the four are slower. Response-time logs and live scores are in neither DB nor code.
- Everyone here is a simulation: do not suggest asking Wen, Marcus, Kip or anyone for data. Work from the wiki, DB and code only.

### Session 3 conclusions (14 Oct 2026)
- Pings, before vs after 12 Aug: taken 76.6% (831/1,085) vs 64.0% (391/611); turned down 21.1% vs 18.0%; missed 2.3% vs 18.0%. Turn-downs did not rise.
- Ping timing: the gap from a missed ping to the next ping in the same callout fell from a median 92s to 62s (matches the 90→60s change). Turn-down gaps stayed at ~22s. The four are not reached later in the callout queue than before.
- The four (Farlight, The Undertow, Meteor Mite, Vesper): 306 pings before, 56 after. Vesper and Meteor Mite have zero support tickets, and no ticket body mentions either. The Undertow (21 tickets) and Farlight (19) describe quiet stretches (20 of 33 post-12 Aug tickets), not callouts gone before (2 of 33). Ticket dates match the ping gaps.
- Replaying The Undertow's 89 pings through history.py rules (start 0.5): score reaches 0 in the last ~15 pings. Home-area share fell 84%→64%; taken 77%→21%.
- Not in DB or code: response times, live scores, eligible-responder lists, travel times, capability tags. Callouts have no required-capability column; responders has no capabilities column.

### Session 4 conclusions (14 Oct 2026)
- Code is unchanged since the 2 Oct initial commit. Every score change goes through history.py: taken +0.08 (offer.py:28), turned down or missed -0.12 (offer.py:30). Nothing else adds points: no decay, reset, extra wait time or "gone quiet" alert. Scores are in memory only (a restart would reset all to 0.5; unverified in production). The 4.2 weights apply to every responder, old and new, with no exception for past turn-downs.
- Correction to the score-replay theory: replayed scores for the four were still 0.84-1.0 on 11 Aug and only hit 0 on 27 Aug-5 Sep, after their pings had already fallen (10-14/week to 3-5 in the week of 17 Aug). Each had 4-5 misses in 12-16 Aug just before the drop. So the score is probably not the main reason they were skipped.
- Farlight (Uptown), The Undertow (Harborside) and Vesper (Old Town) are the only responders in their areas; they were pinged on every area callout before 12 Aug, and on only 6/31, 9/32 and 10/38 after, with an out-of-area responder pinged first instead. Meteor Mite shares Eastgate (62/118 before, 9/57 after).
- Still open: what actually explains it. Candidates not testable here: dropped from the available list (availability.py is placeholders), changed travel-time estimates, or real scores differing from the replay. Agreed position for now: the four stopped being offered their own areas' callouts after 12 Aug, and a falling score alone doesn't explain it.
- Answer for Marcus's 14 Aug question: the weighting applied to everyone, and 4.2 made past turn-downs count for less (acceptance weight .40 to .25). The data points to misses, not refusals.
