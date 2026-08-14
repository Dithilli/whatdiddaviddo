# Narration script

Nine cards. Roughly 25–40 seconds each — about four and a half minutes total if you don't rush.
The page carries almost no detail on purpose. The detail is here, in your mouth.

---

## Card 00 — "What did David do?"

> So — one week. Eight days if you count the weekend, and I did work the weekend.
> Twenty-six pull requests merged, sixteen still open. Three different repos.
> Every colored square up there is something that shipped. The empty ones are still in flight.
> Let me walk you through the interesting ones, because honestly most of the week was one long lesson in software confidently lying to you.

---

## Card 01 — "Monday won."

> Quick shape of the week first.
> Monday was the biggest merge day — eight PRs, all small, all things a customer would actually notice.
> Tuesday was seven, but that's misleading, because one of those seven was two and a half thousand lines.
> Thursday only shows four. Thursday was actually the hardest day of the week. I'll get to why.
> And Saturday's zero isn't a day off — that's the day I opened a bunch of stuff that landed later.

---

## Card 02 — "Also: 19 hours of meetings."

> Before I get into the actual work — context.
> Twenty-one meetings this week. Nineteen hours. That's basically half the week on video calls.
> Four separate events on my calendar were literally called a "dry run," which tells you what kind of week it was.
> And look at Thursday. Thursday I had four and a half hours of calls, *and* at ten in the morning I had three meetings booked on top of each other.
> Thursday was also my single biggest coding day of the week — twenty-five commits and about two thousand lines. I don't fully know how. Adrenaline, probably.
> Wednesday's the opposite: three and a half hours of calls, and only two things shipped. That's not a mystery, that's just arithmetic.

---

## Card 03 — "134 folders. We could see 4."

> Okay. This one's my favorite.
> A customer told us the Dropbox folder picker was, quote, "giving us only the wrong folders." They were seeing about ten. We were actually showing seven.
> And here's the thing — all seven were *correct*. They were also completely useless, because they were that one person's private folders.
> The entire business lived in shared folders. A hundred and thirty-four of them. Finance, Insurance, Operations, Product Recall, Shipping and Customs.
> A hundred and thirty of those are what Dropbox calls "unmounted," and an unmounted shared folder doesn't show up in the folder-listing API. Not deeper in the tree. Not with a flag. It is not hidden — it is *invisible*.
> And that one API call is what backs both our folder picker and our actual sync. So we couldn't see a single byte of their real business.
> The fix walks a different API entirely. Funny bonus: doing it correctly turned out to be about nine times *fewer* API calls than what we were doing before.

---

## Card 04 — "It said 100%. It found nothing."

> This is what that bug looked like from the outside, and this is the part that bothers me.
> The sync ran. It went to a hundred percent. It finished cleanly. It reported success.
> Zero documents.
> Nobody got paged, because nothing failed. That's the tell I'm now looking for everywhere — a green bar and an empty index.
> Fun fact, I diagnosed this on a connection that had already been *deleted*, because the vendor's copy of it outlives ours. So I could go replay the exact call our picker makes and watch it come back empty.

---

## Card 05 — "The connector sent back ghosts."

> Different integration, same species of problem.
> Tuesday I shipped a whole accounting integration — invoices, bills, credit memos, contacts, the works. Twenty-five hundred lines. Felt great.
> Then it started returning records with no IDs on them. Just… empty shapes.
> That's what that vendor's connector does instead of telling you your credential is bad. It cannot tell the difference between "your password is wrong" and "this company has no invoices." Both come back as polite little ghosts.
> So Thursday I built a direct connection to the provider that goes around them entirely. Another two thousand lines. Twenty-five commits. One extremely long evening.
> That's why Thursday only shows four merges. Thursday was one enormous thing.

---

## Card 06 — "Turns out it was us."

> Recurring theme of the week, and it's an embarrassing one.
> Send our API a malformed ID and it returned a 502 — a gateway error. Which means we were telling customers *the upstream provider is down* when actually we had a validation bug.
> Calendar auth failures were doing the same thing.
> Three separate fixes this week all come down to one sentence: stop blaming other people for our own bugs.
> A 400 says "you sent me something wrong." A 502 says "someone else is broken." Those are very different conversations to have with a customer.

---

## Card 07 — "An entire region. Zero syncs. Ever."

> This is Friday, and it's the worst one.
> We run separate infrastructure in the US and the EU. Our connection flow was creating *every* connection in the US region — including all the European ones.
> And the region a connection is created in is part of its permanent identity. A US-created connection is unreadable by our European infrastructure. Forever. There's no migration path. You can only throw it away and make a new one.
> So every EU connection we had? Looked connected. Looked fine in the dashboard. Had never synced. Not once. Ever.
> Found it Friday morning, filed it, fixed it, got it reviewed and merged before lunch. It now works the region out from the host it's running on instead of a hardcoded value — and it refuses to start at all if those ever disagree again.

---

## Card 08 — "Two of these were bad ideas."

> Quick honest note.
> Two of the sixteen open PRs are failures, and I labelled them so nobody merges them by accident.
> The first one taught the system to understand when you say "me" or "my." It works! I just oversold how much it helped when I measured it.
> The second one loosened up the model so it would stop second-guessing itself. It did — and it traded a bunch of polite "I don't know"s for actual, confident wrong answers. Worse. Much worse.
> I'd rather have those written up and sitting there than quietly deleted. Knowing which two ideas don't work is a result.

---

## Card 09 — "And then the paperwork."

> Last one.
> When I went back and checked, eight of this week's pull requests had no ticket at all. Just work that happened.
> And four tickets were actively lying — two marked "planned" for things that shipped days ago, and one marked *done* while the fix that makes it actually produce documents was still sitting unmerged.
> So that's all reconciled now. Eight new tickets, four statuses corrected.
> Twenty-six merged, sixteen open. That was the week.
