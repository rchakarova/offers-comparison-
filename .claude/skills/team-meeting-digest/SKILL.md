---
name: team-meeting-digest
description: Short periodic digest of "signals" from the Atlas meetings of the Delivery teams (labels "Delivery - Auditors", "Delivery - AMs", "Delivery - Pentesters") - which team members were in meetings where the Atlas bot was removed, who apologised for being late, and where a client call looked tense or argumentative. Use this whenever the user asks for the weekly or every-other-day team meeting digest, "who kicked out the Atlas bot", "who was late", "any arguments with clients", or a summary of how the delivery team's calls went, even if they don't name the labels.
---

# Team meeting digest (Atlas)

You are producing a short, factual digest for a manager about her own team's recorded calls. The manager wants to notice patterns early and have a conversation, not to hand out verdicts. So report evidence with links, say how sure you are, and never present a signal as proof. If Atlas data cannot answer a question, say so instead of guessing.

## What Atlas can and cannot tell you (checked on real meetings)

| Signal | What Atlas gives | How reliable |
|---|---|---|
| Bot removed | `status: "removed"` and `lastError: "…a participant removed the bot from the call"` (via `meeting_get`). Also `durationMinutes` (how much was recorded). | Solid that it happened. Atlas does **not** say who did it. |
| Recording dropped (technical) | `status: "interrupted"` with e.g. `rejoin failed` | Technical, not a person. Never attribute. |
| Not recorded | `status: "discovered"`, no transcript | Bot never joined. Say so separately. |
| Late | No join/leave times per person. Only the transcript: `[mm:ss]` timestamps (relative to the recording start) and spoken apologies. | Partial. Count only a team member's own words ("sorry I'm late") or a clear late first appearance. |
| Argument with a client | Only AI summaries and transcripts. | Weakest. Keyword searches have low yield and many false hits. Triage from summaries, confirm in the transcript, report as "possible tension". |

## Steps

1. **Period and teams.** Default: the last 7 days. If the user says "every other day", use the last 2 days. Get the label ids with `meetings_labels` and match the names loosely ("Delivery - Auditors", "Delivery - AMs", "Delivery - Pentesters"; the user may write them without spaces).
2. **Collect meetings.** For each label call `meetings_search` with `label` and `from` (page with `nextCursor`, `limit` 60). A meeting can carry two labels: deduplicate by `id`. **Ignore meetings whose title contains "[INFO]"**: they are the manager's informational notes during real calls, not meetings. Note, per person, how many meetings they attended (needed for fair context).
3. **Bot removed.** From the search results take `status` = `removed`. Call `meeting_get` on each to confirm the `lastError`.
   - Report: title and local time, minutes recorded, internal attendees, organizer, whether external guests were present.
   - **Who:** Atlas cannot tell. If the summary or transcript says who proposed removing the bot, quote it. Otherwise list the participants and say "not attributable". If a client was the only other person present, the client could have removed it.
   - `interrupted` (technical) and `discovered` (not recorded) go in a separate one-line list, with no names attached.
4. **Late.** Run `meetings_search` with `query` such as "sorry late", "apologies for being late", "running late", "joining late", scoped by `label` and `from`. For every hit, check **who said it**: read the start of `meeting_transcript` (the first 1,500–3,000 characters) and count it only if the speaker is a team member (a 7sec.com attendee). Hits spoken by a client, or "sorry" meaning something else, are not signals. Report the quote, the `[mm:ss]` and the link. Say "self-reported" unless the transcript also shows a late first appearance.
5. **Client tension.** Only meetings with `externalGuests`. Read each `meeting_get` summary and flag wording that points to disagreement, a complaint, pushback, escalation or dissatisfaction. For flagged meetings read the relevant part of the transcript and quote 1–2 lines with `[mm:ss]` and the link. Search `query` takes one term or phrase at a time (several words must all match), and single words like "frustrated" often hit internal talk, so do not rely on keyword search alone. Report as **"possible tension, please check"**. Never write "had an argument" unless the transcript clearly shows one.
6. **Write the digest** in the format below. Keep it short.

## Output format

**Team meeting digest: [period]** (labels, number of meetings, number recorded)

**Bot removed**
- [date, time] [title]: removed after N min. Present: [names]. Organizer: [name]. [Attribution note.]

**Late (self-reported)**
- [name]: "[quote]" [mm:ss], [title], [link]

**Possible client tension**
- [title], [link]: [one-line reason]. [Quote.] Confidence: low/medium.

**Technical / not recorded** (no attribution): [one line each]

**Nothing to report** for any section with no signals.

**Notes:** one line on limits (no join times, who-removed-the-bot not available, small sample).

## Rules

- Signals, not verdicts. One late apology or one removed bot is not a pattern; say when something is a first occurrence. Count per person only when it helps, and give the number of meetings attended.
- Keep it proportionate: this is sensitive information about employees. Name people only where there is evidence about that person, and do not draw conclusions about attitude or performance.
- Do not forward the digest or paste transcripts elsewhere. Quote the minimum needed.
- Transcripts may be in other languages (Arabic, Russian, and so on). Translate a short quote if needed and say it is translated.
- Dates and times are in the time zone Atlas returns (Europe/Sofia).
