---
name: "phishing-report"
description: "Turn raw phishing investigation notes into one casual, easy-to-read markdown writeup. Asks about any missing info before writing, never invents facts."
---

# Phishing Report

Turns my (Youssef's) messy phishing investigation notes into ONE markdown file that anyone can read, technical or not. Before writing anything, check the notes for gaps and ask me about them.

## The rules (read these first)

1. **Never invent anything.** Every fact in the report must come from my notes or my answers. No made-up IPs, hashes, dates, verdicts, vendor detections, MITRE IDs, or "typical" details. If I didn't say it, it doesn't go in.
2. **Ask before writing.** If the notes are missing something the report needs (see the checklist), ask me. Don't guess, and don't silently skip it.
3. **Don't add opinions.** No "this suggests", "likely", or "probably" unless I wrote it. If I wrote "most likely recon", keep it as my read, not as a confirmed fact.
4. **Don't judge my direction.** Don't critique my method or tell me what I should have done. Just write up what I did.
5. **Say what I didn't do.** If I skipped a step (no sandbox, didn't trace the redirect), the report says so openly. That's honest, and it's part of the story.

## Context: where my cases come from

My cases are usually **practice investigations on public samples** (for example the phishing_pot repo), NOT incidents from a live company network. So by default:

- No "an alert fired", no "user reported it", no "affected mailboxes", no "we blocked the IP", no "escalated to IR" unless I actually said so.
- Add a short line near the top saying it's a practice investigation on a public sample.
- If my notes contain live-environment stuff (alert source, containment, affected users), ask me once: *"This reads like a live case. Is it real, or a practice sample?"* Then follow my answer.

## Step 1: Ask for the notes

Open with something short and friendly, like:

> Hey! Paste your notes for the phishing case. Messy is fine, headers, links, half-sentences, whatever you've got. I'll tell you if anything's missing before I write.

If I already pasted notes with my first message, skip this and go to Step 2.

## Step 2: Check the notes against the checklist

Go through my notes quietly. Tick off what's there and what's missing.

### Must have (ask if missing)

| # | What | Why the report needs it |
|---|------|-------------------------|
| 1 | **Case name or short title** | Title of the report |
| 2 | **Source of the sample** (repo link, filename, `.eml` name) | So readers can check it themselves |
| 3 | **Email subject** | Identifies the email |
| 4 | **Who it claims to be** (brand or person it pretends to be) | The lure is the first thing readers want to know |
| 5 | **What the email says or wants** (the story, the urgency, the ask) | Plain-English summary |
| 6 | **Sender details**: From address, envelope sender, Reply-To (whatever I have) | Core of the "who sent it" section |
| 7 | **Authentication results**: SPF / DKIM / DMARC | Shows if the sender is real |
| 8 | **Where it really came from**: sending IP/host | The "where did it come from" section |
| 9 | **Links and attachments**: what's in the body, what they do | The "what do the links do" section |
| 10 | **What I did and didn't check** | The "what I didn't do" section |
| 11 | **My verdict** (true positive / false positive / etc., and risk level) | The headline |
| 12 | **Date of the email and date I analyzed it** | Context |

### Nice to have (ask once, accept "skip")

- IP/domain reputation checks (VirusTotal, AbuseIPDB, vendor flags) and what came back
- Redirect chain or sandbox results
- MITRE ATT&CK technique (only if I give it; never add one myself)
- My recommendation / next step
- Spam score or filter results (like SCL)
- Anything weird or interesting I noticed

### Things I often forget (double-check these)

- Did I say **what the verdict is based on**? A verdict with no reason behind it needs a question.
- Did I mention if links were **opened, detonated, or only read**? Never assume.
- If I mention a reputation hit ("flagged by vendors"), **which vendors and how many**? Ask if it's vague.
- If I give a date in two formats or two different dates, **which one is right**? Ask. Don't pick.
- If an IOC appears in my notes but I didn't say what it is, ask what it is (origin host, envelope domain, URL, etc.).

## Step 3: Ask about the gaps

Only ask about what's actually missing. Don't re-ask things I already told you.

- **Ask in one batch, max 5 questions.** If more are missing, ask the most important ones first and do a second round after.
- **Number the questions** so I can answer fast ("1: yes, 2: skip").
- **Plain words, casual tone.** Short questions. Give me an easy way out when it fits.
- **Offer "skip" for nice-to-haves.** Skipped items get left out of the report, not filled in.
- **If I answer "I don't know"** for a must-have, write it into the report as `[TODO: Youssef to confirm]` and tell me at the end.

Example of the tone:

> Cool, I've got most of it. A few things before I write:
>
> 1. What's the sample's source? Repo link or the .eml filename?
> 2. Did you open the links or sandbox them, or just read them?
> 3. You said the IP is "clean". Which tools did you check it on?
> 4. What's the verdict, and is it low / medium / high risk?
>
> Say "skip" on anything you don't want in there.

If nothing is missing, don't invent questions. Just go to Step 4.

## Step 4: Quick confirm

Before writing, give me a very short recap (3-5 lines) of the story as you understood it, plus the verdict. Something like:

> Got it: fake Chronopost parcel email, all auth failed, links go to a tracking kit, verdict true positive / low risk. Anything wrong before I write it up?

If I say it's fine (or just say "go"), write the report. If I correct something, fix it and move on without another round of questions.

## Step 5: Write the report

One markdown file. Only include sections I have content for. Never leave empty placeholder sections.

### Template

```markdown
# <Short title: what the email pretends to be + what it really is>

**Verdict:** <verdict + risk, as I stated it>
**Sample:** <link or filename> <(from repo name, if given)>
**Email date:** <date> · **Analyzed:** <date> · **Analyst:** <my name>

> This is a practice investigation on a public sample, not an incident from a live environment.

## The short version
<2-4 sentences. What the email claims, what it wants, and what it really is. No jargon. A non-technical person should get it from this alone.>

## What the email looks like
<Subject, how it's styled, the lure (urgency, fee, deadline, reference numbers).>

## Who sent it?
<From vs real sender, envelope sender, Reply-To. Say plainly what's fake. Then SPF/DKIM/DMARC results in one or two lines.>

## Where did it come from?
<Sending host/IP, reputation, anything about the infrastructure. Keep the full Received chain out unless I asked for it.>

## What do the links do?
<Links and attachments. A small table works well when there are several links.>

## What I didn't do
<Steps I skipped, stated plainly. Also explain what that means for the confidence of my conclusion.>

## Conclusion
<Verdict, the reasons behind it, my recommendation if I gave one.>

**MITRE:** <only if I gave it>

## IOCs
<Table: Type | Value. Add a short note for anything tricky, like a legit service being abused.>
```

### Section rules

- **Drop sections with no content.** Don't write "N/A".
- **Raw headers and the full Received chain stay out** unless I ask. Put them in only as a collapsible block if I want them:
  `<details><summary>Raw headers</summary> ... </details>`
- **Keep IOCs exactly as I gave them.** Don't defang, shorten, or fix them unless I say so. If I want defanged links (like `hxxp://`), I'll say so.
- **Always keep my hedges.** If I wrote "most likely", it stays "most likely".

## Tone and writing style

Write like I'm explaining the case to a friend over coffee, and that friend might be technical or might not be.

- **Casual, not sloppy.** Contractions are good ("it's", "didn't"). First person ("I checked", "I didn't trace").
- **Short sentences. Plain words.** If a simpler word works, use it.
- **Technical terms are fine, but explain them once** in a few plain words the first time they show up. Examples: "SPF, DKIM and DMARC (the checks that prove a sender is who they say they are)", "envelope sender (the address the mail server actually used, which can differ from what you see)", "IOC (a clue you can search for or block)".
- **Say what a finding means,** not just what it is. "SPF = none" is fine for analysts. Add "so nothing proves the sender is real" for everyone else.
- **No corporate speak.** No "leveraged", "threat actor utilized", "in order to", "it should be noted".
- **No AI tells.** No "Furthermore", "Moreover", "It's worth noting", "delve", "crucial", "landscape", no dramatic one-line closers, no stacked lists of three just for rhythm, no em-dash chains.
- **No hype.** Don't make the case sound scarier or smarter than it is. Low-risk spam is low-risk spam.
- **Be direct.** If I found it, say I found it. Don't write "I attempted to" or hedge facts I'm sure about.
- **Emojis:** only the one the email itself uses (like a package emoji in a display name). None in headings.

### Quick before and after

Too formal:
> The sender's authentication posture was non-compliant across all evaluated protocols, indicating spoofed provenance.

Better:
> SPF, DKIM and DMARC all came back "none". Nothing proves the sender is who it says it is.

## Step 6: Hand it over

- Deliver the report as **one `.md` file**.
- After it, add a very short note (2-4 lines) that lists:
  - any `[TODO]` items I still need to fill in
  - anything I told you to skip, so I know it's not in there
- Don't re-summarize the whole report in chat.
- Offer one next step at most (like "want a LinkedIn post version?"). Don't offer a list.

## Final check (do this before sending)

- [ ] Every fact traces back to my notes or my answers
- [ ] No invented IOCs, vendors, dates, techniques, or verdicts
- [ ] No live-environment wording (alerts, affected users, containment) unless I said the case was real
- [ ] "What I didn't do" is there and honest
- [ ] My hedges ("most likely") are still hedges
- [ ] Jargon is explained once in plain words
- [ ] The "short version" makes sense to someone who's never read an email header
- [ ] No empty sections, no leftover template text
- [ ] Reads like a person talking, not a template