# Lookup mode

Answers "what do you have for X", "where are we on X", "when is X", "did I ever X". Step 0 of the
skill has already read the tracker. This flow reads; it writes only if the user takes up an offer
at the end.

## 1. Find it

- **Search on the subject, not the verb.** In "what do you have for cleaning the gutters?" the
  subject is "gutters". "Cleaning" would also hit the garage cleanout, which isn't what they asked.
- **Search everything that holds text:** every slug and record title, every field of every record
  in `threads.md` (`history:` included), every row in every section of `schedule.md` (`[x]` rows
  and dinner blocks included), and the labels in `priority.md`. Match plurals and stems. Search
  `meals.md` only when the subject is food. Skip fenced format examples.
- **A slug on a matching row is the same effort.** Resolve it and summarize the thread.
- **Several real matches:** summarize each, open ones first. If the subject could mean two
  different efforts and the question doesn't say which, ask.
- **Near-misses** (matched only on the verb or a loose word) get one line at the end.
- **Nothing in the files:** look where done work goes when it's cleared. Search `tracker/done.md`
  whenever it has anything in it (version control may have been turned on after it filled). With
  version control on, also search git for items that were done and removed:
  `git log -i -G "<word>" --format="%ad %s" --date=short -- tracker/`. Report what it shows, with
  dates. If there's nothing, say so and offer to capture it.

## 2. Follow the links one hop

From each match, take the other nouns that carry weight - people, places, things - and search for
those the same way. Report any open row or thread they hit, with its date, and how it bears on the
match. One hop only.

## 3. Build the summary

For each match:

- **Kind:** a thread (`#slug`) or a one-off row.
- **Status:** open, done (with the date) or overdue. For a thread, its open rows in date order,
  each with its date and size tag.
- **For a thread:** `deadline:`, `next:` (or each part's status, next step and deadline), any
  `open q:` with its recorded findings, the last one or two `history:` entries, and any
  `surface:` or `cadence:` that will bring it up soon.
- **For a one-off:** the row text is its only record, so give all of it.
- **Day check:** run the capacity rule and day constraints against the next open row's day. Name
  the day's other items only if there's a conflict.
- **Related:** what step 2 found.
- **Not recorded:** what a natural next question would need that the tracker doesn't say. Say it's
  missing; don't fill it in.
- **History:** for a one-off, use git (if on) to date when it was added and how it changed.

## 4. Output

Short, one block per match, near-misses last. The shape:

> **Clean the gutters** (`#gutters`): open, next step "get two quotes", scheduled **Tue 3/10
> (small)**. Deadline end of October, before the rainy season.
> - **Open question:** is the downspout by the garage clogged too? Not looked up yet.
> - **Related:** `#garage`'s shelf sort is the same Saturday, and the ladder's in the garage.
> - **Not recorded:** a budget for the job.

## 5. Offers, not actions

End with any that apply, as offers:

- **A one-off that has built up background or bundles two tasks:** offer to promote it to a
  thread, using the row-or-thread test in **Capturing something new**.
- **A day conflict:** offer a nearby day.
- **An `open q:`:** offer to research it, per the rules' **Research** section.

If they take one up, that's a write. Follow **Capturing something new** and **Saving changes**, and
commit as `todos: lookup YYYY-MM-DD - <what changed>`. A lookup never updates `Last check-in:`.
