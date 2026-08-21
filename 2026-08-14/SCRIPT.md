# Narration script — with source detail

Each card has two parts:

- **SAY** — a draft narration, roughly 25–45 seconds. Rewrite in your voice.
- **THE ACTUAL THING** — what it's really about, so you can decide what to cut and what to keep. Not for reading aloud.

Customer names are **in** here (unlike the page). This file is internal.

---

## Card 00 — "What did David do?"

**SAY**
> One week. Twenty-six pull requests merged, sixteen still open, three repos.
> Every filled square is something that shipped; the outlines are still in flight.
> Most of the week was one long lesson in software confidently lying to you.

**THE ACTUAL THING**
- 26 merged 8–14 Aug: 20 in `hyperspell/hyperspell`, 2 in `hyperspell-n8n-node`, 4 in `hyperspell-openclaw`.
- 16 open, of which 2 are deliberately marked do-not-merge.
- ~11,700 lines added / ~590 deleted in the main repo alone.
- The "lying" framing is real and holds across five of the nine cards: a backfill that reports complete with 0 documents, a connector that returns ghost records instead of auth errors, a 502 that was our own SQL error, decks that reach COMPLETED with their content gone, and connections that look connected but have never synced.

---

## Card 01 — "Monday did the work."

**SAY**
> Monday was the biggest merge day — eight PRs, all small, all customer-visible.
> Tuesday was seven, but one of those was two and a half thousand lines.
> Thursday only shows four. Thursday was the hardest day of the week.
> Saturday's zero isn't a day off — that's when I opened things that landed later.

**THE ACTUAL THING**
- Merges by day: Sat 0, Sun 2, Mon 8, Tue 7, Wed 2, Thu 4, Fri 3.
- Monday's eight: two PPTX parser fixes, Slack bot identity bridge onto the identities table, live-search evidence kept distinct, org-wide connection rows (ENG-3727), hibernated apps labelled in the switcher, File System tab kill switch, plus an openclaw fix.
- Wednesday's two were both in the n8n node repo, not core.
- PR count is a bad proxy for effort here and the card says so — that's the setup for card 02.

---

## Card 02 — "Nineteen hours of meetings."

**SAY**
> Twenty-one meetings, nineteen hours — about half the week on video.
> Four separate calendar events were called a "dry run."
> Thursday: four and a half hours of calls, three of them booked on top of each other at 10am. Also my biggest coding day — twenty-five commits.
> Wednesday is the opposite. Three and a half hours of calls, two merges. Not a mystery.

**THE ACTUAL THING**
- Mon 4h55 (6 meetings, incl. All-Hands, ShopBack dry run, week planning, ShopBack testing, ShopBack weekly health check)
- Tue 3h20 (Dev office hours, Nature's Fusion dry run, IntentHQ dry run — the last was 90 min)
- Wed 3h30 (IntentHQ 90 min, dev office hours, Odoo + Outlook testing)
- Thu 4h25 (6 meetings: IntentHQ, Nature's Fusion quick sync, Nature's Fusion call, dev office hours, Unified weekly, Manu 1:1)
- Fri 2h55 (IntentHQ, Roundup & Planning 90 min, Corgi dry run)
- The 10am Thursday collision: Nature's Fusion / Hyperspell, Dev Open Office Hours, and the Lemma launch all overlapping.
- The four "dry runs": ShopBack, Nature's Fusion, IntentHQ, Corgi.

---

## Card 03 — "134 folders. We could see four."

**SAY**
> Nature's Fusions said the Dropbox picker was "giving us only the wrong folders." They saw about ten; we were actually showing seven.
> All seven were correct. All seven were useless — they were one person's personal folders.
> The business lived in shared folders. A hundred and thirty-four of them. A hundred and thirty were unmounted, and an unmounted shared folder doesn't appear in the listing API at any depth, under any flag.
> That one call backs both the picker and the sync. So we couldn't see a byte of their real business.

**THE ACTUAL THING**
- Customer: Nature's Fusions, app 4119. PR #3230, ticket ENG-3921.
- An unmounted shared folder reports `path_lower: null` and never appears under `files/list_folder`. That call backs both Unified's storage listing and our own walk.
- The 130 include Finance, Insurance, Operations, Order Processing, Product Recall, Shipping and Customs, lab, Incorporation Docs.
- **App 4104 has the same signature** (`backfill_completed_at` set, 0 documents) and is probably the same bug — worth saying out loud if the audience can act on it.
- Diagnosed on a connection that had already been deleted: the Unified connection object outlives our row, so it was recovered by matching `external_xref` to our `IntegrationConfig.id` and replaying the picker's exact call.
- The fix walks shared namespaces recursively rather than treating each as its own root — which also avoids double-indexing and costs ~9x fewer API calls.
- Two follow-ups split out: ENG-3916 (resource identity flips when a folder gets mounted → duplicate documents) and ENG-3917 (persist the namespace delta cursor, else every sync re-walks the whole corpus).
- Also in that PR: `.xlsx`/`.xlsm` workbooks now index from Dropbox.

---

## Card 04 — "It reported one hundred percent."

**SAY**
> This is what that bug looked like from the outside.
> The sync ran. Went to a hundred percent. Finished clean. Reported success. Zero documents.
> Nobody got paged, because nothing failed. That's the tell I look for now — a green bar and an empty index.

**THE ACTUAL THING**
- Not a separate PR — this is the *signature* shared by card 03 and card 05, which is why it sits between them.
- Concretely: a Dropbox connection with `backfill_completed_at` set and 0 documents; three Odoo connections healthy and polling since 11 Aug with 0 documents.
- The related class of bug is card 09's ENG-3927: documents that chunk to nothing never leave `PROCESSING`. Measured 9 Aug: 1,482 of the 20,000 most recent `PROCESSING` docs have zero chunks (7.4%). App 3004 had 14 Google Meet docs wedged while every other source was terminal.
- If you want one more example: PPTX decks reached `COMPLETED` with chunks and embeddings and *no body copy* — 81 of 81 body shapes dropped on a 100-slide deck. Visible in search, content gone.

---

## Card 05 — "The connector returned nothing with a name on it."

**SAY**
> Tuesday I shipped a whole accounting integration — invoices, bills, credit memos, contacts, organisations. Twenty-five hundred lines.
> Then it started returning records with no identifiers. Empty shapes.
> That's what Unified's Odoo connector does instead of telling you the credential is bad. It cannot tell "your password is wrong" from "this company has no invoices."
> So Thursday went to building a direct XML-RPC transport around it. Another two thousand lines, twenty-five commits, one long evening.

**THE ACTUAL THING**
- PR #3181 (2,473 lines) then PR #3225 (2,193 lines). Customer: Nature's Fusions. Ticket ENG-3849.
- The proof, 13 Aug: five different auth shapes — including ones carrying credentials that *cannot* be valid — all returned byte-identical `[{"status":"DRAFT","type":"INVOICE"}]`. A connector actually reaching Odoo would tell those apart.
- Structural cause: Odoo needs **database + login + API key**. Unified's hosted form collects only `["API key", "Domain"]` — nowhere to put a login. Odoo 19 has a bearer-token API that would fix it; the tenant is on Odoo 18, which doesn't.
- Three production connections had produced zero documents since 11 Aug while reporting `is_healthy: true`.
- The direct transport proved out read-only against the live tenant: discovered the database name, authenticated as uid=2, listed invoices, bulk-read 5 line items in one call.
- Design detail worth keeping if the audience is technical: **line items are the point.** Item names are the only natural language a financial record has. Header-only documents answer "what's the balance?" and never "what was on that invoice?"
- Odoo's connector is the tightest-limited one we run — 1 call/sec, strictly serial, 60s retry-after rather than the 30s baseline (a 30s backoff re-queues into the same exhausted window).
- Shipped gate-closed deliberately, since nothing had ever been verified against a live tenant.
- Still open: PR #3201, the initial-pull-sync dispatch — the actual zero-document fix.

---

## Card 06 — "It was us."

**SAY**
> Send our API a malformed connection ID and it returned a 502 — a gateway error. We were telling customers the provider was down when we had a validation bug.
> That one kept the investigation pointed at Linear for two days.
> Three fixes this week come down to one sentence: stop reporting our own faults as somebody else's outage.

**THE ACTUAL THING**
- PR #3215, ticket ENG-3697. A customer's n8n Live → Search was failing with `502 "Upstream source error."` **No provider was ever contacted.**
- Real cause in the logs: `connection_id` arrived as a *source name* — `linear`, `github`, `google_drive` — because an AI agent filled the field from its description. That string hit the UUID-typed column, asyncpg raised `DataError`, and a blanket `except Exception` reported it as upstream failure.
- Two defects, and the second is the expensive one: `_guard` laundered our own bugs into apparent provider outages. `SQLAlchemyError` now re-raises as a 500, logged with traceback, landing in error tracking as ours. The 502 arm still works for genuine provider failures.
- `InvalidConnectionId` (400) is deliberately split from `ConnectionNotFound` — "nothing is connected" and "your request is malformed" need opposite follow-up. An agent told the former will go connect an already-connected source.
- The other two in the family: Google Calendar live-search auth failures were 502-ing instead of being classified, and the n8n node's List operation was deleting its own response content.

---

## Card 07 — "An entire region had never synced."

**SAY**
> Friday, and it's the worst one.
> Our EU connect flow was creating every connection in the US region. Region is part of a connection's permanent identity — a US-minted connection is unreadable by EU infrastructure. Forever. No migration. You can only recreate them.
> So every EU connection looked connected, looked fine in the dashboard, and had never synced once.
> Found it, filed it, fixed it, merged before lunch.

**THE ACTUAL THING**
- PR #3250, ticket ENG-3920, marked Urgent. Filed 11:01, completed 11:49.
- One line was the cause: `unified-oauth.ts` built every OAuth URL against `https://api.unified.to`, for every cell.
- Core *pins* the EU cell to `api-eu.unified.to` and refuses to boot otherwise (EU residency enforcement). So EU connections were minted in a region the core that owned them could never read.
- Live prod evidence, same API key, probed from inside both pods: IntentHQ's 3 connection IDs returned **200 on `api.unified.to`, 404 on `api-eu`**. Connections visible in the EU workspace: **0**. Against US: 100+.
- All 6 EU connections sat at `last_synced_at = NULL` with `webhook_ids = []`. Tenant logs showed `404 unknown connection_id` on webhook registration.
- Nothing failed at connect time — OAuth completed and the row was written. Every subsequent read 404'd.
- The fix resolves region from the Connect host through the same private `cellFromHost()` the API base uses, so the two can never disagree, and fails closed if the maps drift.

---

## Card 08 — "Two of them were failures."

**SAY**
> Two of the sixteen open PRs are failures, labelled so nobody merges them by accident.
> The first taught retrieval to understand "me" and "my." It works — I just oversold the measurement, and I corrected it in public rather than quietly editing.
> The second loosened the answer model so it would stop second-guessing itself. It traded polite "I don't know"s for confident wrong answers. Strictly worse.
> I'd rather have those written up than quietly deleted.

**THE ACTUAL THING**
- **#3171** — original body claimed 0/8 → 8/8. Eye-checking the answers found roughly *one* genuinely good answer. One was a graceful non-answer scored as success; one confidently said "yesterday would be July 30" when the run was Aug 9. The classifier was binary answered/abstained, so a confident wrong answer and a polite non-answer both scored as wins.
  - The deeper lesson: appending a name to the query is a retrieval hack that only works when the name literally appears in the documents. It retrieves documents that *mention* someone rather than documents that *are theirs*. The right fix is identity as a filter on indexed metadata — and the data was already local.
  - What survives: the diagnosis is reproducible, and the pronoun gate is real — appending unconditionally took "Who owns ShopFit today?" from 2/2 to 0/2.
- **#3170** — original reported 1/20 → 4/20 on a 20-question arm. The full 48-question suite showed capability flat, and the conversational gain was attributable to #3171, not this. Hard failures (`AnswerGenerationFailed`) rose from 1/48 to 5/48. Isolated A/B: net +1 answer, +3 hard failures. A NULL is strictly worse than an abstention — the caller gets no answer *and* an error.
  - Likely mechanism: more refinement eats the budget, leaving no tail for the forced final answer.

---

## Card 09 — "Then the paperwork."

**SAY**
> When I went back and checked, eight of this week's pull requests had no ticket at all.
> And four tickets were lying — two marked planned for things that shipped days ago, one marked done while the fix that makes it actually produce documents was still unmerged.
> All reconciled. Eight new tickets, four statuses corrected.

**THE ACTUAL THING**
- Created: ENG-3921 (Dropbox namespaces), 3922 (query effort floor), 3923 (PPTX, both defects), 3924 (recency), 3925 (Jira denylist), 3926 (app switcher), 3927 (chunkless docs), 3928 (Slack bot connection kind).
- Corrected: ENG-3690 → Done (Fellow connector shipped 11–13 Aug, still marked Planned), ENG-3884 → Done (File System tab, shipped 10 Aug, still in Triage), ENG-3819 → In Progress, ENG-3849 → reopened to In Review because PR #3201 is unmerged.
- Two of the eight genuinely have no customer attached — the effort floor and the app switcher — and the tickets say so rather than implying one.

---

## Things not on the page you may want to mention

- **Query effort floor (#3193, ENG-3922).** Every surface defaulted differently — `minimal` on REST, `medium` on MCP ask, whatever the Slack bot's model picked. A tenant paying for the best answers couldn't chase that dial across four clients; it read as "the brain is worse in Slack than in the CLI." Now a per-app floor every surface obeys. Needed three supporting fixes to work end to end (transport cancel, MCP daemon timeout, Slack turn budget). Caveat: a customer on a generated SDK at its default 60s timeout will cut a floored `very_high` query client-side.
- **Recency (#3175, ENG-3924).** Asking for a recency *preference* silently became a recency *filter*. On ShopBack app 3872, "Who owns ShopFit today?" returned 8 documents with no preference, 2 at 90 days, 1 at 30 days, **0 at 1 day** — same corpus. The floor dropped 26 chunks on one query. It annihilated precise semantic questions specifically, because broad queries were exempted by a full-text-hit carve-out.
- **PPTX (#3176).** Root cause is genuinely good material: `Comment.author` referenced a class declared 20 lines below it, so pydantic left the class incomplete. It heals lazily on first validation — which masks the bug anywhere one process both builds and saves. The PPTX parser is the exception because slides parse in a spawned child, which heals its *own* copy, pickles home, and the parent dies on serialize. A deck with no speaker notes sails through. That's why test decks passed and customer decks didn't.
- **Fellow (#3220).** Shipped ahead of vendor support with every `raw.*` path inherited from Fathom and never observed. First live connection showed both predicted-to-miss paths missed: 91 speaker turns intact, but no summary, action items, decisions or topics. Fixed by keying on the *shape* of a section rather than its type or title, since note templates are user-authorable.
- **Kill switches (#3217, ENG-3887).** Both fleet-wide switches were set as 100% PostHog conditions. Both gates fail open by design — so the moment PostHog is unreachable, every app in the cell resumes brain synthesis at once, at the most expensive end of the pipeline, with nothing logged as an error. 163 US apps had a completed tree at the time. Moved the fleet-wide default into configuration; flags went back to being per-app exceptions.
- **Jira denylist (#3172, ENG-3925).** ShopBack's Jira corpus is 89% one bot-driven project. The webhook payload carries `project_id` but not `project_key`, so a denylist of `["DM"]` filters the sync walk and *not* the webhook — you must list both key and numeric id. For ShopBack: `["DM", "12726"]`.
