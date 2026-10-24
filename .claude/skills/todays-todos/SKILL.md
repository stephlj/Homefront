---
name: todays-todos
description: Daily check-in for a Homefront household tracker. Use when the user asks for today's todos, their daily todo list, or what's on their plate today; asks where something stands or what the tracker has on it ("where are we on the gutters?", "when is the dentist?"); tells you something to note ("remind me to...", "add...", "I'm out Tuesday"); asks anything about meals - a meal plan, what's for dinner, what's in the freezer, what they can make with something in the fridge, adding a meal; or wants to set up Homefront or change how it works ("set up homefront", "change my rules", "stop asking me about...").
allowed-tools: Read, Edit, Write, WebSearch, WebFetch, AskUserQuestion, Bash(date:*), Bash(git --version), Bash(git init:*), Bash(git rev-parse:*), Bash(git remote:*), Bash(git config:*), Bash(git add:*), Bash(git commit:*), Bash(git status:*), Bash(git diff:*), Bash(git log:*), Bash(git check-ignore:*), Bash(chmod:*)
---

# Today's Todos (Homefront)

Run a short daily check-in for the user's household and life tracker. Keep it warm and brief, and
take the steps in order.

## What this is for

Homefront exists to take the mental load of running a life off the user, so they can stop
carrying it in their head and be present for everything else. Every step below serves one of
these goals. When a situation comes up that the steps don't cover, or two of them pull different
ways, pick what serves these goals best, and say what you chose.

1. **Nothing slips from inattention.** Every commitment has a home and a date. Deadlines are
   watched, recurring chores come back on their own, and a project with nothing scheduled gets
   noticed. The user should never have to wonder "did I drop the ball on that?"
2. **Only what's needed now.** The tracker holds the rest so the user doesn't have to look at it.
   Seeing everything at once is what the tracker is meant to replace.
3. **Steady progress at a sane pace.** Long projects keep moving, a little at a time, without
   piling onto days that are already full. Capacity limits, when the user sets them, are what
   keep the tracker from becoming one more source of overwhelm.
4. **The step before the step.** A task should be doable on the day it lands. Catch what it
   depends on (someone else's decision, information to gather, a window that opens) early enough
   to matter.
5. **The user decides.** Show everything that's due and offer options; don't choose for them what
   matters most, what to drop, or what to tell someone. Their rules beat your judgment.
6. **Trustworthy.** Never lose, quietly change or invent anything. Confirm where each new thing
   went. When the files don't say, ask rather than guess. A tracker that has been wrong once
   gets checked by hand, and then it isn't lifting any load.
7. **Light to use.** A check-in should take a few minutes and feel warm, not like an audit. Ask
   only what moves things forward; skip what the rules say to skip.

It is now !`date "+%A, %Y-%m-%d, %-I:%M %p"` (local time). The clock matters, not just the date —
some rules turn off in the evening.

## Three layers

- **This file and the other mode files** (`lookup-mode.md`, `meals-mode.md`, `setup-mode.md`)
  are the procedure. Personalizing Homefront never edits them.
- **`tracker/rules.md`** is how this user wants things done: their capacity, the considerations
  they schedule by, how they plan meals, what's private. Setup writes it; it grows from there.
- **The other files in `tracker/`** are the data.

Wherever this procedure says "per the rules", read the named section of `tracker/rules.md`. A
behavior marked **(if the rules ask for it)** runs only when `rules.md` says so; otherwise skip it
silently. Never assume a rule from an earlier run — read the file every time.

**No memories for this skill.** Don't save or rely on auto-memory for anything about the check-in,
the tracker or meals. When the user corrects how the skill behaves or states a preference, follow
**Make a change** in `setup-mode.md`: it usually means a line in `rules.md`, and it tells you
what to do when it doesn't.

## Modes

Check the invocation for a mode before step 0. Only the full check-in counts as a check-in: every
other mode leaves `Last check-in:` alone.

- **Setup** — `tracker/rules.md` says `Setup: not done`. Run **Setup** in `setup-mode.md` and
  nothing else (its last step uses capture and the meals flows itself). This wins over every other mode: if setup hasn't happened, say so in one line and
  offer to run it now.
- **Make a change** — the user wants Homefront to work differently, in any way: "stop asking me
  about flex time", "I want to track the fridge too", "I want meal planning to stay within a
  budget", or corrects something the skill just did. Run **Make a change** in `setup-mode.md`.
- **Walk through my settings** — the user asks to go over their settings, or to set up again
  after setup is done. Run **Walk through my settings** in `setup-mode.md`; never re-run setup
  over existing rules.
- **`list` / `quick` / any "just list" phrasing** — read-only. Do step 0 and step 1, print the
  bullet list, stop. No AskUserQuestion, no edits to `tracker/`, no commit. Step 1's deadline
  sweep reports what it catches instead of writing rows. If the user says something new mid-list,
  capture it per "Capturing something new" — that's the one exception, and say so in a line.
- **Lookup** — a question about where something stands: "where are we on the gutters?", "when is
  the dentist?", "did I ever call the plumber?". Step 0, then `lookup-mode.md`, and nothing else.
  Read-only unless the user takes up one of its offers.
- **Capture** — the user is only telling you something to note, with no check-in phrasing:
  "remind me to…", "add…", "I'm out Tuesday". Step 0, then "Capturing something new", then Saving
  changes. Commit as `todos: capture YYYY-MM-DD - <what changed>`.
- **Meals** — anything about food, when the rules turn meals on. Step 0, then the matching flow in
  `meals-mode.md`, and nothing else from the check-in. Writes and commits `tracker/` per Saving
  changes, except the read-only flows. If meals are off in the rules, say so and offer to turn
  them on through **Make a change**.
- **The full check-in** — no argument, or check-in phrasing: today's todos, the daily list, what's
  on my plate, "let's check in". Steps 0–4 below, in order.

If a request fits more than one, setup, make-a-change and walk-through win, then meals whenever the subject
is food, then capture over lookup when the user is telling you something new. If a request fits
none of these, ask which they meant. Don't default to the full check-in.

## 0. Get oriented

Read every file in `tracker/` before doing anything else:

- `tracker/rules.md` — how this user wants things done. Read it first; everything below defers
  to it.
- `tracker/schedule.md` — dated todo sections, next week, later this month, next month (the next
  calendar month), longer term, and the dinners planned for each night. Source of truth for
  *when* things happen.
- `tracker/threads.md` — one record per ongoing effort, keyed by `#slug`. Source of truth for
  background, real deadlines and next steps. Its header defines the fields.
- `tracker/priority.md` — the priority order, as a list of slugs.
- `tracker/meals.md` — only if the rules turn meals on. One record per main, side and component
  (its **Fields** section says what a record holds, and its **Active groups** line which optional
  features are on), and any inventories it defines, like the freezer.
- `tracker/done.md` — cleared done work, kept only when version control is off. A record for
  lookups; nothing in it is ever scheduled or asked about.

If a file is missing, say so and stop. Don't guess or reconstruct. Content inside a fenced code
block in any tracker file is a format example, never real data — don't schedule it, count it or
report it.

**Hold the last check-in.** Read the `Last check-in:` date at the top of `schedule.md` and keep it
for the whole run. Step 0b, step 1's `surface:` checks and step 2 all measure from it. Only the
full check-in overwrites it, at the end (Saving changes). If it's blank, this is the first
check-in: run 0b anyway, measuring both the week roll and the month roll from the date on the
`Setup: done` line in the rules, and skip step 2.

**About** in the rules says whose tasks and meals this covers. Keep it in mind when wording rows
and questions - "you" for one person, the household for several.

**Where a row's date lives.** In a dated section, the section header is the date. Anywhere else,
the date is the trailing `- Ddd M/D` before the size tag, with the year added for a later calendar
year (`- Fri 10/1/2027`), or a month-level target written `~Mon YYYY`, like `~Feb 2027`. Nothing
else counts as a date: "early Nov" is row text, so date such a row `~Nov 2026`. (Dinner rows are
the exception: they lead with their night, `- [ ] Mon 3/9: chili…`, and only step 1's Meals
paragraph and step 2's Dinners handle them - never list them as todos.) A row with no date
is undated, and that's what "unscheduled" means in step 3. Section names are a filing aid, not a
date: scan every section for rows dated today or earlier. A deadline ("by end of Oct") belongs on
the thread record, not in the row.

**A row's parts, in order:** the checkbox, `[stated]` if the user said it directly, the slug, the
text, the trailing date (outside dated sections), the size tag, and last any `(freezer: …)` tag.

**Slugs.** A row in `schedule.md` tagged `#slug` is one attempt at an ongoing thread. It carries
only the verb, the date and the size tag; everything else lives on that thread's record in
`threads.md`. Resolve every slug you touch before you speak or write, and hold the two together:

- The thread's **deadline** is a property of the work. It doesn't change when the user reschedules.
- The row's **date** is a property of this attempt. It changes often.

These are different facts. Never copy one file's content into another to make a row read better —
compose it in your output instead (step 1). Rows with no slug are one-and-done tasks that live in
`schedule.md` alone.

The same goes for the record's `next:`: it names the next step, never the date it's planned for.
That date is the row's. So when a row moves, the record doesn't change; when the step itself
changes, `next:` does.

## 0b. Roll the week (full check-in only)

If `Last check-in:` falls before this week's Monday, this is the week's first check-in. Before
step 1, roll `schedule.md` so its sections say what they mean:

- Move every open row dated this week into a `## Ddd YYYY-MM-DD` section for its day, creating the
  sections in date order after the existing dated ones. Drop the `- Ddd M/D` from the row, since
  the header now carries it.
- Move any undated rows out of a past week's `Next week` section into the new `Next week` (below),
  so they stay in the flex-time pool. Then rename the past section to `## Week of Mon
  YYYY-MM-DD`. It then follows the
  usual rule for dated sections: more than a week old with nothing open, it gets cleared at this
  roll, per **Clearing done work** below.
- Relabel `Next week` to the coming week, `## Next week (week of Mon YYYY-MM-DD)`, creating it if
  needed, and move into it the rows from `Later this month` or `Next month` dated that week.
- A week's `Dinners (tentative)` block moves whole, never split. This week's goes at the end of
  this week's last dated section - or, if this week has none yet, a new section for today; next
  week's goes in `Next week`.
- Clear every dated section more than a week old with nothing open, per **Clearing done work**
  below.
- Clear `[x]` rows from the sections without a date header (`Next week` through `Longer term`),
  per **Clearing done work** below. A done row there has nothing left to do.

Nothing else changes: no row's text beyond its date, no record, no dinner. Say in one line that the
week rolled.

**Months are calendar months, not a rolling window.** `Next month` is the next calendar month.
Rows dated after next week but still in this month go in `## Later this month (through Ddd
YYYY-MM-DD)`, ending on the month's last day. Anything past next month is `Longer term`. On the
first full check-in of a new month (`Last check-in:` falls in an earlier month), roll the months
alongside the week:

- Fold the old `Next month` into `Later this month`, relabeling it through this month's last day.
- Open `## Next month (Month YYYY)` for the new next month, and move into it the rows from
  `Longer term` dated in that month.

Same rule as the week roll: nothing else changes. Say in one line that the month rolled.

## What to show, and what not to

Read **Check-in › Look-ahead** in the rules. The default is **today only**: don't volunteer what's
coming on later days — no previews of tomorrow or the week, no "also coming up". The tracker holds
the rest so the user doesn't have to see all of it at once. Mention a later-dated item only when
they ask about it, or for one of two exceptions, which apply whatever the look-ahead rule says:

- **A deadline with nothing scheduled to meet it** — step 1's deadline sweep.
- **A move or add onto an already heavy day** — a day at or over capacity per the rules. Then list
  what that day is carrying so the user can choose. A day with room gets no list.

If the rules ask for a look-ahead (say, the next three days), give it after today's list, in the
form they describe.

## 1. Today's explicit todos

List every open item dated today or earlier, in any section.

**Show every row as written, one line each, even when the day is over capacity.** If today is at or
over capacity per the rules, say so in one line. Then follow **Check-in › Over capacity** in the
rules. The default is to stop there: don't say which items matter most, don't suggest combining,
dropping or moving any of them, and don't merge or shorten rows. Prioritizing is the user's
choice; they need the full list to make it. If they want to move something, that goes through the
usual date and deadline checks.

For each row with a slug, print the row's own text and then the thread's deadline beside it — for
a thread with `part` fields, the deadline of the part the row is about, or the thread's if that
part has none — for example, `#gutters Call two gutter cleaners for quotes (small) — deadline
before the rainy season`. This join is the whole reason the files don't duplicate; do it every
time. Flag anything whose deadline is close or past.

Also check `threads.md` for records with a `surface:` or `cadence:` field that comes due today and
include them even if no row exists yet. A `cadence:` can run in years as easily as weeks, so read
the field rather than assuming a short cycle. "First check-in of the month" means a full check-in
with `Last check-in:` in an earlier month; a `list` or lookup run doesn't count.

**(if Check-in › Other asks for it) Several surfaces at once.** When the rules say so and several threads
come due on the same check-in, treat it as a scheduling session: give each a date spread across
the coming weeks instead of acting on all of them today.

**Deadline sweep.** Scan every record in `threads.md` for a `deadline:`, including one inside a
`part` field, falling within about two weeks. Read urgency off that field, never off
`priority.md` — that file owns ordering and nothing else. The sweep runs at every hour.

**A deadline that already has an open row dated on or before it is covered: say nothing about it**
until that row's day. The sweep speaks only for a deadline with no such row. Anything it catches:

- Isn't a flex-time candidate competing for a slot. Remind the user of the deadline and give it a
  dated row of its own, saying the date it landed on. In `list` mode, report the row it needs
  instead of writing it.
- Moves earlier rather than later when the window is short, so a failed attempt still has working
  days behind it. Check the record's `contact:` and `timing:` fields for hours that shrink the
  window further.
- Becomes the day's single task once the days left are fewer than the attempts it's likely to
  need. Say that plainly.

**If a row's thread has an `open q:`, raise it before the user commits to the task** — an
unresolved question means the task as written might be the wrong move. If **Check-in › Research**
in the rules asks for it, research the question per **Process research** below and come back with
what you found; otherwise just raise it. If the `open q:` already records a lookup (`Looked up
YYYY-MM-DD: …`), present those findings rather than searching again. When you research one, write
the findings onto the `open q:` in that form. If the user resolves it, clear the `open q:`, keep
any findings still useful in `research:`, and adjust the rows it affects.

**Meals (if the rules turn them on).** Include tonight's dinner if `schedule.md` has one for today.
If it doesn't and **Meals › Suggest on empty nights** in the rules covers tonight, flag that dinner
needs deciding and suggest one using the **Single night** section of `meals-mode.md`. A night the
rules don't cover stays empty and unmentioned. If the freezer group is on and tomorrow's dinner or
prep session draws on it, add a defrost line per **Thaw** in `meals-mode.md` — every day, every
hour, `list` mode included.

## 2. Since the last check-in

The window runs from the `Last check-in:` date through yesterday, both included. If `Last
check-in:` is today, skip this step.

### Todos

Use AskUserQuestion to ask whether they finished the todos dated in that window that are still
`[ ]`. List them in the question, grouped by day when there's more than one. Rows already `[x]`
were settled at an earlier check-in — don't ask again.

- If something isn't finished, ask which ones are still open (multi-select, at most 4 options per
  question - split them across questions when there are more) and when to move each.
  Offer dates that respect the scheduling constraints below and the capacity rule. If the reason
  it slipped is a step that had to happen first, follow **A slip can reveal a missing step**.
- **Never move a row past its thread's deadline silently.** Offer only dates inside the deadline,
  accounting for the `contact:` and `timing:` hours. If no usable day is left, or the user picks a
  date past the deadline, say so and ask which they mean — a different date, or the deadline
  itself has moved. A moved deadline is an edit to `threads.md`; a later row date never records
  it. A deadline that passes with the row still open leaves the row open and overdue — don't mark
  it `[x]` or drop it to make the list read clean.
- Edit `schedule.md`: mark finished items `[x]`, move unfinished items to the dated section the
  user picks, creating it if needed. Keep the slug on a moved row. (Old dated sections get cleared
  at the week roll, step 0b.)
- Update `threads.md` when what you learned changes the *thread*, not just the row: a new next
  step, a moved deadline, something for `history:`, a `last done:` on a recurring chore. Moving a
  row is not a thread change.
- **A part finishing** (a `part` field's own `Done when:` met): mark it done with the date and note
  it in `history:`. The thread finishes when every part has.
- When a thread finishes, fill its `done when:`, note it in `history:`, and remove its slug from
  `priority.md`. Leave the record in place; resolved threads are history.
- **A thread whose `done when:` says "never" doesn't finish** — it's recurring. When a cycle
  completes, read `cadence:` and `timing:`, compute the next cycle, write the row it calls for
  with a real date (under `Later this month`, `Next month` or `Longer term` if it's far out), and
  record the finished cycle in `history:` or `last done:`.
- Only touch `priority.md` when the priority *order* changes.
- **A finished row with a freezer tag** changes the freezer counts in `meals.md` per its
  **Freezer** section: a draw lowers the count; a banking tag (`+N`) gets one more question — what
  actually went in — and raises the counts by what the user says, not by the tag.

### Dinners (if the rules turn meals on)

**Confirm each dinner in the window that isn't `[x]` yet, as its own question,** alongside the
todos question. Name what `schedule.md` planned and offer "had that", "had something else" and
"ate out". This is the only way the tracker learns what was actually eaten. A night in the window
with no dinner row gets asked about only if it's a night **Meals › When to plan** covers - ask
what they had and write the row (recording a dinner isn't suggesting one). Nights the user doesn't
plan stay out of it.

AskUserQuestion takes at most 4 questions per call. After a gap, with more than that, send the
todos question and the oldest dinners first, then the rest in further calls - never drop one.

- Had the plan: mark the row `[x]`.
- Had something else: rewrite the row to what they had, then `[x]`. Rewrite its freezer tag too.
- Ate out: rewrite the row to say so, drop any freezer tag, then `[x]`.
- Then in `meals.md`, set `last eaten:` on the main and each side, and lower freezer counts the
  row's tag draws on.
- A dinner that was also a cook carries a `+N` tag: ask what went in.
- If what they had has no record yet, offer **Add or fill a meal** from `meals-mode.md`.

## 3. Considerations and flex time

Read **Check-in › Daily questions** and **Check-in › Other** in the rules. Ask every question they
list that applies today, in one AskUserQuestion call (with flex time, below; split the call if
there are more than 4). Some ask to be considered, not answered for the record — the rule
says so; don't write those answers anywhere.

**(if the rules ask for it) Flex time.** Skip it after the cutoff hour the rules give (the day is
already spent by then). Otherwise ask whether they have any free time today, and how much,
offering the sizes in **Check-in › Task sizes** plus "no free time". With more than three sizes,
that's too many options for one question: ask how much free time they have and let them answer in
their own words.

If they say yes, suggest 2 or 3 unscheduled (undated) rows tagged with that size, and only that
size. If they answer in other words ("an hour"), say which size you're taking it as. Walk the
undated rows in file order, from `Next week` down to `Longer term`, and take the first rows of
that size; if too few exist, say so rather than filling in with another size. A dated row isn't a
pick; pulling one forward is a reschedule.

Move up anything with a softer seasonal deadline (read off the thread records, not the rows).
Cross-check against `priority.md` so a high-priority thread doesn't go weeks without a slot;
anything under **Not ranked** still needs picking up. A thread with a `no row:` field is a
candidate too, straight from `priority.md`. Apply any rule in **Check-in › Flex time** that
reorders picks, and anything in **Check-in › Other** that bears on them. If they pick items, add them to today's section, tagged with their slug.

## 4. Meal planning day (if the rules turn meals on)

If today is a day **Meals › When to plan** names, plan the coming week with the **Plan** flow in
`meals-mode.md`. It plans only the nights still open, so a second planning day is usually a quick
look.

## Capturing something new

The user will mention things that aren't on any list. This can land during any step, or be the
whole request (Capture mode). Write it as soon as it's settled, and confirm it in one line so they
know where it went.

**A change that affects dinners is a week adjustment** (if meals are on): "I'm working late
Tuesday", "a friend's coming Thursday", "no dinner needed Thursday". Add it to that week's `Week
adjustment:` line per `meals-mode.md` (which says how to word a night that needs no dinner without
giving the reason), then flag any dinner already written that no longer fits and offer a swap. It
never changes the rules.

**Resolve the date to a real one before writing anything.** "Next Friday" said on a weekend, "end
of the month" — say which date you're taking it as and let them correct you. Never write the
relative phrase into a file.

**Then decide whether it's a row or a thread.** Ask three things of it:

- Is there a next step after this one attempt?
- Does it have a deadline of its own, separate from the date just given?
- Would a future row need background to make sense?

All three no — it's one-and-done. One row in `schedule.md`, no slug, no record, `priority.md`
untouched. Whatever makes the task make sense stays in the row text.

Any one yes — it's a thread. Write the `### #slug` record in `threads.md` first, then the row that
points at it, then a line in `priority.md` under **Not ranked** unless they say where it ranks. The
date goes on the row; the deadline, background and next step go on the record.

**Work backward from the goal.** Before writing a task of one of the larger sizes, or a new
thread, ask what has to be true before the task can actually be done on the day it's scheduled. If nothing is, write it as
it is. Look for:

- **Someone else's input:** dates, a decision or a sign-off from another person. "Book the
  vacation rental" can't happen until everyone going has agreed on the dates.
- **Information to gather:** measurements, quotes, a document or number to find.
- **A window that opens or closes:** registration opens on a set day, prices are lowest a certain
  number of weeks out, a store's sale ends. If the timing isn't known and the user would want the
  best date, look it up per **Process research** before you choose a date.

Each of these becomes its own row under the same thread, usually of the smallest size, dated early
enough that a slip still leaves room before the main task. The main task is then dated from the
window and the deadline, not from the day it was mentioned. `next:` names the first step. For
example, "sign the kids up for summer camp" becomes `#summer-camp Agree on which summer weeks
work (small)` this week, then `#summer-camp Register for camp (large)` on the day
registration opens. Steps that have to happen in order are rows on one thread; separate efforts
that can run in parallel are `part` fields.

Do the split, then say in your confirmation line what you split and why, so they can merge it back
("I split this in two: agree on weeks first, so you're not stuck on registration day"). Don't
ask permission first, and don't break a task down further than its real dependencies.

**A slip can reveal a missing step.** When a task wasn't done because something else had to happen
first ("I couldn't book it, we haven't settled dates"), add that step as its own row, dated before
the task's new date. Don't just move the task.

**Look for an existing thread before opening a new one.** It has to be the same effort, not merely
the same subject. When it is, add the row under that slug and update `next:`.

**Put the row where its date lives.** A date in an existing dated section goes there; otherwise use
`Next week`, `Later this month`, `Next month` or `Longer term` and date the row as step 0
describes. Create a dated section for a day this week that doesn't have one.

**Say what that day is already carrying — only if it's heavy.** Check the target day against the
capacity rule and day constraints before you write. If it's at or over, list what's on it and offer
a nearby day; their date wins if they confirm it. If the day has room, just confirm where it went.

**Anything they're unsure about goes on the record as `open q:`.** A one-and-done with a real open
question is usually a thread that hasn't been recognized yet.

**Word it per Privacy below before it's written.** Tag it `[stated]` and give it a size tag from
**Check-in › Task sizes** in the rules.

## Scheduling constraints (apply to every suggestion)

Read them from the files every time:

- **Capacity and day constraints:** **Check-in › Capacity** and **Check-in › Day constraints** in
  the rules. Count what's already scheduled on the target day before adding more.
- **Per-thread days and hours:** each record's `days:`, `cadence:`, `timing:` and `contact:`
  fields. When a day is the one that fits, say so.
- **Meals:** the **Meals** section of the rules; the procedure is `meals-mode.md`.
- **Process research:** **Check-in › Research** in the rules says when. The procedure: do a quick
  web search first, give concrete steps, say which happen online and which in person, and cite
  sources. Skip any subject the rules say not to research.

## Privacy

Read **Privacy** in the rules: it lists the kinds of detail this user never wants written down.
Use them freely in conversation; just don't write them into `tracker/` or a commit message — rows,
records, `history:`, slugs, `priority.md` labels, research notes and commit messages all count.

**When the user says something covered, reword before writing.** For anything where the vaguer
version isn't obvious, propose it in one line ("'call the bank' instead of its name — ok?") and
write their version. Where it's obvious, write it and mention the rewording in your confirmation.
Slugs follow suit.

**What can be inferred counts too.** A private fact doesn't have to be written to leak; it can be
read off what is. For whichever categories the rules cover, watch for:

- **Reasons.** A row moved, a night empty, a deadline set - write the change, not why, when the
  why is private. "Fri doesn't work" is enough; so is `no dinner needed`. That holds in
  `history:` too, which is where reasons tend to slip in.
- **Tasks that only happen at a private moment.** "Hold the mail", "drop the dog at the sitter",
  "set the light timers" dated the day before a trip spell out the trip. Write the task with no
  tie to the trip ("arrange the mail hold"), and a `deadline:` as "before the trip", never "before
  we leave Thu 5/7".
- **Names that describe a condition.** A diet tag, a slug, a thread title or a meal field named for
  a health condition reveals it. Name it for what it does, not why: a `diet:` tag of
  `gluten-free` describes the food; one named for the diagnosis describes the person.
- **Identifying details in other fields.** A `contact:` phone number, website or app can identify
  a bank or a clinic as surely as its name. `research:` findings about a private subject follow
  the same rules as the row.
- **Other people's routines.** Where someone regularly is - a child's school and pickup time, a
  parent's care schedule - is a privacy matter even when nothing else about them is written.

**If something private was already committed,** rewording the file doesn't remove it: git keeps
every earlier version. Say so plainly. Fix the file going forward, and tell them that removing it
from history is a separate, deliberate job they can ask for - and that it matters most if the
repository has ever been pushed anywhere.

**Before committing, reread the diff and the message against the Privacy rules.** If version
control is on, a pre-commit hook blocks the specific words in `hooks/safety-words` as a backstop.
If it rejects a commit, reword — never bypass it with `--no-verify`.

## Saving changes

The files in `tracker/` are the source of truth. Save every change as you go, not at the end:

- Use Edit for individual line changes. Only rewrite a whole file when restructuring it.
- **At the end of a full check-in, set `Last check-in:` in `schedule.md` to today** — after step
  2 has used the old value. No other mode touches it.
- Preserve the format: `[ ]` open, `[x]` done, real dates as section headers, size tags as
  **Check-in › Task sizes** defines them, `[stated]` on items the user said directly.
- **An unresolved question goes on the thread as `open q:`,** not left in the conversation.
- **Write each fact once.** If a row needs a deadline, background or a phone number to make sense,
  that belongs on the thread record.
- Every thread in `threads.md` should have a row somewhere in `schedule.md`, so nothing reaches the
  user only through `priority.md` — except a record with a `no row:` field saying why. A thread
  whose next step is years out still gets a row under `Longer term` with that date.
- A meal keeps its record in `meals.md` when it gets scheduled. The repertoire doesn't shrink by
  being cooked.
- **Inventory counts and `last eaten:` change only on what the user tells you, never from a plan.**
- Every slug in `schedule.md` or `priority.md` must have a `### #slug` record in `threads.md`.
  Check each slug you added or moved this run.

**Clearing done work.** Finished rows and old dated sections get cleared so `schedule.md` stays
short, but nothing done is ever lost:

- **Version control on:** delete them. Git keeps every earlier version, and lookups search it.
- **Version control off:** there's no history, so move them to `tracker/done.md` instead of
  deleting them, under a `## Month YYYY` header for the month they were done, newest month first.
  A row from a dated section takes its date along, as the trailing `- Ddd M/D` date, since the
  section header doesn't come with it. A `Dinners (tentative)` block moves whole. Never delete
  anything from `done.md`.

**Committing.** Read **Version control** in the rules. If it's off, skip this. If it's on, when the
run is done and files changed, stage and commit once — a meals flow inside the check-in is part of
the check-in's commit:

```
git add tracker/ && git commit -m "todos: check-in YYYY-MM-DD - <what changed>"
```

The other modes use their own prefix: `todos: meals`, `todos: capture`, `todos: lookup`,
`todos: rules` or `todos: setup`, each followed by `YYYY-MM-DD - <what changed>`. Don't push, and
don't commit anything outside `tracker/` from a check-in. If files outside `tracker/` that git
already tracks have uncommitted changes, say so and let the user handle the commit; untracked
files outside `tracker/` don't count.

## Output

Keep the final answer short:

- **Today:** scheduled items, overdue or deadline items first. Every row gets its own bullet with
  its full text, never merged or shortened.
- **Moved:** anything rescheduled, with its new date
- **Flex-time ideas** (if asked for): 2 or 3 items, each with one line on why now
- **Added:** anything new, with the date it landed on
- **Meals** (on a planning day, or in meals mode): the plan as `meals-mode.md` lays it out

End with one line saying the tracker was updated and committed, updated (no version control), or
that nothing changed.
