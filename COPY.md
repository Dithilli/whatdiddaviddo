# Copy deck — whatdiddaviddo.com

Every piece of text on the page, in scroll order. Rewrite the right-hand side in your voice;
hand it back and I'll drop it in.

**Constraints worth knowing:**
- **Headlines break manually.** Each line is its own element, so you control where it wraps. Keep lines roughly the same length as what's there or the ragged edge gets ugly. Fewer/more lines is fine — tell me how many.
- **Eyebrows** render uppercase in mono with wide letter-spacing. Keep them under ~34 characters or they wrap awkwardly.
- **Ledes** are capped at 31em wide, so ~2–3 sentences is the natural size.
- **Notes** are the indented ochre-bar callouts. Bold inside them is a deliberate emphasis — mark what you want bold.
- **Numbers animate** (count up / roll). If you change a figure, say so explicitly so I update the animation target too.
- Anything in `CAPS` below is rendered in caps by CSS — write it however you like, I'll case it.

---

## Hero

| Slot | Current text |
|---|---|
| Eyebrow | `8 — 14 AUGUST 2026` |
| Headline L1 | `What did` |
| Headline L2 | `David do?` |
| Stat 1 value / label | `26` / `MERGED` |
| Stat 2 value / label | `16` / `OPEN` |
| Stat 3 value / label | `3` / `REPOS` |
| Scroll cue | `SCROLL ↓` |

*Below this sits the ticker: 14 linked PR titles. Those are pulled from the actual PR names — listed at the bottom of this file if you want them reworded.*

---

## Card 01

| Slot | Current text |
|---|---|
| Eyebrow | `01 — SHIPPING RATE` |
| Headline | `Monday did the work.` |
| Lede | `Pull requests merged, by day. Thursday looks quiet. Thursday was not quiet.` |
| Chart labels | `SAT SUN MON TUE WED THU FRI` (values 0 / 2 / 8 / 7 / 2 / 4 / 3) |

---

## Card 02

| Slot | Current text |
|---|---|
| Eyebrow | `02 — THE OTHER HALF OF THE WEEK` |
| Headline L1 | `Nineteen hours` |
| Headline L2 | `of meetings.` |
| Lede | `Twenty-one of them. Four were separately, independently called a *dry run*.` |
| Chart labels | `MON 4h55` / `TUE 3h20` / `WED 3h30` / `THU 4h25` / `FRI 2h55` |
| Vignette caption | `10:00` |
| Note | `Thursday, 10am: **three meetings booked simultaneously.** Thursday was also the biggest coding day of the week — 25 commits. Wednesday, by contrast, was three and a half hours of calls and two merges. That one isn't a mystery.` |

---

## Card 03

| Slot | Current text |
|---|---|
| Eyebrow | `03 — THE INVISIBLE CORPUS` |
| Headline L1 | `134 folders.` |
| Headline L2 | `We could see four.` |
| Lede | `Unmounted shared folders don't appear in the listing API at any depth. Not hidden — *invisible*. And that one call backs both the folder picker and the sync.` |
| Legend A | `VISIBLE — 4` |
| Legend B | `EXISTED ANYWAY — 130` |

---

## Card 04

| Slot | Current text |
|---|---|
| Eyebrow | `04 — THE TELL` |
| Headline L1 | `It reported` |
| Headline L2 | `one hundred percent.` |
| Lede | `The sync ran. It completed. It raised nothing. Nobody was paged, because nothing failed.` |
| Meter readout (animates 0→100) | `0% — BACKFILL` → `100% — BACKFILL COMPLETE` |
| Meter red figure | `0 DOCUMENTS INDEXED` |
| Vignette word | `BUT` |

---

## Card 05

| Slot | Current text |
|---|---|
| Eyebrow | `05 — BUILT TWICE` |
| Headline L1 | `The connector returned` |
| Headline L2 | `nothing with a name on it.` |
| Lede | `Tuesday shipped a full accounting integration. Then the vendor started returning records with no identifiers — which is what it does *instead of* raising an auth error. It cannot distinguish a bad credential from an empty company.` |
| Ghost rows (×4) | `id: —` + `INVOICE` / `BILL` / `CREDIT MEMO` / `CONTACT` |
| Vignette label | `ID` (×3 on the blank badges) |
| Note | `So Thursday went to a direct transport built around them. **25 commits, one evening.** That's why Thursday only shows four merges — Thursday was one enormous thing.` |

---

## Card 06

| Slot | Current text |
|---|---|
| Eyebrow | `06 — ATTRIBUTION` |
| Headline | `It was us.` |
| Lede | `A malformed identifier returned a gateway error — telling customers the upstream provider was down when we had a validation bug. Three fixes this week reduce to one sentence: stop reporting our own faults as somebody else's outage.` |
| Big numbers | `502` → `400` (400 rolls in on digit reels) |
| Vignette caption | `IT COMES BACK` |

---

## Card 07

| Slot | Current text |
|---|---|
| Eyebrow | `07 — FRIDAY MORNING` |
| Headline L1 | `An entire region` |
| Headline L2 | `had never synced.` |
| Lede | `The EU flow was minting its connections in the US region. Region is part of a connection's identity — a US-minted connection is unreadable by EU infrastructure, permanently. There is no migration. They can only be recreated.` |
| Diagram labels | `EU CONNECT` / `US REGION` / `EU REGION` / `0 SYNCS` |
| Panel 1 heading / body | `US` / `Where every connection was created. Including all the European ones.` |
| Panel 2 heading / figure | `EU — SUCCESSFUL SYNCS, LIFETIME` / `0` |
| Note | `Found, filed, fixed and merged before lunch. It now resolves the region from the host instead of a hardcode, and **fails closed** if those ever disagree again.` |

---

## Card 08

| Slot | Current text |
|---|---|
| Eyebrow | `08 — NEGATIVE RESULTS` |
| Headline L1 | `Two of them` |
| Headline L2 | `were failures.` |
| Lede | `Both retrieval experiments. Both written up, both left open, both labelled so nobody merges them by accident.` |
| Card A link / body | `PR #3171` / `Resolve "my", "me" and "I" to the person asking. It worked. I oversold the measurement.` |
| Card B link / body | `PR #3170` / `Stop the answer model second-guessing itself. It traded polite abstentions for confident wrong answers.` |
| Stamp (×2) | `DO NOT MERGE` |

---

## Card 09

| Slot | Current text |
|---|---|
| Eyebrow | `09 — RECONCILED` |
| Headline | `Then the paperwork.` |
| Lede | `Eight of this week's pull requests had no ticket at all. Four tickets disagreed with what had shipped — including one marked *done* while its zero-document fix sat unmerged.` |
| Tally 1 | `26` / `MERGED` |
| Tally 2 | `16` / `OPEN` |
| Tally 3 | `8` / `TICKETS CREATED` |
| Tally 4 | `4` / `STATUSES FIXED` |

---

## Footer

| Slot | Current text |
|---|---|
| Disclaimer | `Week of 8–14 August 2026, across three repositories. Merged and open counts are pull requests; meeting hours are from calendar. Customer names removed.` |
| Signature | `WHATDIDDAVIDDO.COM` |

---

## Ticker items

Linked to the real PRs. Reword freely — the numbers stay attached to the links.

| # | Label |
|---|---|
| 3181 | Odoo accounting integration |
| 3225 | Direct XML-RPC transport |
| 3230 | Dropbox shared namespaces |
| 3193 | Per-tenant query effort floor |
| 3175 | Recency must reorder, not delete |
| 3172 | Jira per-app project denylist |
| 3176 | PPTX speaker-notes serializer |
| 3220 | Fellow AI-notes mapping |
| 3250 | EU Unified region resolution |
| 3217 | Kill switches survive a flag outage |
| 3174 | Chunkless documents terminal status |
| 3215 | Malformed id returns 400 |
| 3188 | Hibernated apps labelled in the switcher |
| 3185 | Live-search evidence kept distinct |

---

## Also editable, separately

- **`SCRIPT.md`** — the spoken narration, one block per card. Written to be talked, not read, so it's already looser than the page copy.
- **`BALLAD.md`** — the power ballad.
