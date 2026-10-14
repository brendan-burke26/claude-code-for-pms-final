# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

Is there anything that would be helpful to dig into depper (ping data, ticket data, interview data)? Consider all of that, and the ntell me if you can with confidence answer the following statement in one sentence: Based on what I found, the reason some responders are getting no pings at all is ___, because ___.

### 2.

I want you to check for recent changes in the code and see if these affect your conclusions. I need you to be more brief. I'm exhausted of reading

### 3.

Keep this short but make more explicit. Who and what happened: After 12 Aug the four stopped being offered callouts in their own areas, which were previously almost always theirs, and this happened before their recent-acceptance scores had fallen far, so the score is probably not the main reason they were skipped.

### 4.

Make it one sentence

### 5.

The pings table on rook-database has last session's numbers. Read that alongside this code. Does it back up what I found in Lab A? And does that answer the engineering manager's question above?

### 6.

Ok, so what does explain it?

### 7.

My manager asked this. Can we explore this? Quick one, did the change to who gets asked first apply to responders who'd already been turning jobs down, or just new ones? Can't tell from the code and don't want to guess on this. Anyone know?

### 8.

Help me answer my managers question in one sentence

### 9.

Find me the part of this code that takes points off somebody when they miss a ping or turn one down. Show it to me and explain it in plain English. Then find me every single thing in this code that puts points back on.

### 10.

Answer this: "According to my findings in the code, for someone who's gone quiet, they would need to ___."

### 11.

So then, what is the code not doing in terms of helping low-ranked people?

### 12.

Does that change your previous according to my findings statement?
