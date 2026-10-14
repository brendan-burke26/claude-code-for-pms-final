# 03 · Rewind — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly.

Last session you read four conversations and every support ticket
since 4.2 — and found the two piles did not agree.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

Use the rook-database connector. Call describe_table on pings, then run_query. Every row is one time we pinged a responder, and whether they took it, turned it down, or missed it. Release 4.2 shipped on 12 August. Tell me what changed after that date. Show me the weekly numbers before and after, and show me the rows you used to get them.

### 2.

Did this happen to everyone, or did it happen to some people much more than others?

### 3.

I want to get to the bottom of this. What's the next question I should ask, or step I should take to investigate? Tell me and follow through on it

### 4.

Is there anything else to test at this time? If not, give me the one number that matters for me to put in front of product leadership

### 5.

Help me erxplain why acceptance rate is important

### 6.

1-2 sentences

### 7.

why more important that missed pings rising from 2 to 18%?

### 8.

Compare the support_tickets table on rook-database against the pings table. Do they agree with each other? When did people start writing in, and when do the numbers actually move?

### 9.

What do you think the next step is? Carry out and give me a short summary wit hdata

### 10.

Yes

### 11.

What's next, if anything? carry through and then tell me in brief

### 12.

Give me a brief summary of your findings. Point to the most important finding. Include data

### 13.

Pick one responder from the file who went quiet and seems to be the most interesting/have the most red flags. Show me every week for them, how many times we pinged them, how many they took. Then tell me what happened to that person, week by week, in plain English. Be brief.

### 14.

What's the next step in our investigation? carry it out

### 15.

Do it

### 16.

Very briefly, what is the biggest conclusion we can now reach? Give data

### 17.

Give me a 1-2 sentence hypothesis

### 18.

What's the most important data point

### 19.

Before we wrap up, three things. First: look back through this session and find the prompts I wrote myself, not the starter I pasted. Save them into 03-rewind/prompts.md, one per numbered slot, exactly as I typed them. Don't tidy them up. Second: add a few lines to the Working context in CLAUDE.md, anything we figured out today that isn't in there yet and that I'd want you to already know next session. Third: commit everything that's changed with a short message describing what this session did, then push. Tell me when it's done and give me the link to my repository on GitHub.
