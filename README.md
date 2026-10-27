# Homefront

A Claude Code skill that carries the mental load of running a life in the modern world. Tell it things as they come
up - one-off tasks, long-running projects, recurring chores, and, if you want, what's for dinner.
Whenever you check in, it shows you what needs you today and keeps track of everything else, so
you don't have to.

## Why I built this

For years I have gotten by with a to-do list consisting of emails to myself, calendar alerts,
various pieces of paper with hand-written lists, and physical piles of objects (run out of
contacts, put the empty box on my desk to remind me to order more).

In this modern age of AI, a better solution should be possible.

Homefront provides a "todays-todos" skill, which does a morning check-in with
me, keeps track of one-off short-term tasks and longer-term multi-step projects, and even has a
meal planning mode with extra niceties like checking the weather and suggesting
weather-appropriate options. The skill relies on a couple of text files that are
rationally designed and have fairly structured metadata, while still allowing extensive, fast customization. It is optionally version controlled, for peace of mind - I don't have to worry that Claude will accidentally delete something I need to remember to get to. (This is key to reducing the mental burden!)

This repo is the skill abstracted from the details of my life, with a structured way to customize it for your own.

## What it's for

The goal of Homefront is to take the mental load off you, so you can stop carrying it around and be present for everything else. Everything the skill does comes back to a few key "north stars":

- **Nothing slips from inattention.** Every commitment has a home and a date. Deadlines are
  watched, recurring chores come back on their own, and a project that's gone quiet gets noticed.
  You shouldn't have to lie awake wondering whether you dropped the ball on something.
- **The tracker holds the rest, so you don't have to.** You see what needs you today, and the rest stays out of sight until it's due. Seeing everything at once leads to overwhelm, but usually the only alternative is dropping the ball; Homefront ensures there's a third option.
- **Steady progress at a sane pace.** Big projects keep moving a little at a time, without piling
  onto days that are already full. If you set a limit on how much fits in a day, it holds to it.
- **The step before the step.** A task should be doable on the day it lands, so whatever it
  depends on - a decision from someone else, a measurement, a window that opens - gets caught early.
- **You decide.** It shows you everything that's due and offers options, but what matters most is
  your call.
- **You can trust it.** It never loses or quietly changes anything, and it asks rather than
  guesses. A tracker you have to double-check isn't lifting any load.
- **Light to use.** A check-in takes a few minutes and feels like a friendly nudge, not an audit.

## What it does

- **Check-in.** Shows today's tasks - every one, as you wrote it - with each project's deadline
  beside it. Asks what you finished since last time and where the rest should move. Daily works
  well, but miss a few days and it catches up on everything since.
- **Nothing gets dropped.** Long-running projects keep their background, deadline and next step in
  one place. If a deadline is coming with nothing scheduled to meet it, the check-in says so.
  Recurring chores reschedule themselves.
- **Only today.** The rest of the week stays out of sight unless you ask, so the list stays
  something you can face.
- **Capture anytime.** "Remind me to call the plumber next Thursday." It turns relative dates into
  real ones and files it.
- **Steps you'd have missed.** A big task that depends on something else - agreeing on dates with
  someone, a measurement, a registration day - gets split, with the first step scheduled early
  enough to matter. So the day you're due to book something isn't the day you realize you can't.
- **Lookups.** "Where are we on the gutters?" gets the whole story, history included.
- **Meal planning** (optional). Plans the week's dinners from your own repertoire, around your
  rules: how much time you have to cook each night, variety, the weather, the freezer, diet goals,
  what's in the fridge that needs using.
- **Privacy.** You decide what never gets written down - health details, bank names, travel dates -
  and a commit hook backs that up.

## What you need

- [Claude Code](https://claude.com/claude-code), in the terminal or the desktop app.
- Optionally, [git](https://git-scm.com), for an undo history. Setup explains it and does the git
  part for you.

## Getting started

1. Download this repo (green **Code** button → **Download ZIP**, then unzip), or clone it. Put the
   folder wherever you like and rename it if you want.
2. Open the folder in Claude Code.
3. Tell Claude: **"Verify you can see the /todays-todos and if so, set up homefront."**.

**Keep your tracker local.** It's your life in plain text, and even with the privacy rules on, a
todo list says a lot about you: what you're dealing with, when you're busy, how you live. The undo
history works entirely on your computer; there's no need to push it anywhere. If you do want to
share it - with a partner, or between your own devices - use a **private** repository, shared only
with the people who should see it.

Setup asks what you take into account when you plan your days, and your meals, with plenty of
examples to react to. It writes your answers as plain-language rules in `tracker/rules.md`, then
helps you dump everything currently on your mind into the tracker.

After that, ask for **"today's todos"** whenever you want to check in (e.g., first thing
in the morning).

## Making it yours

Everything personal lives in `tracker/rules.md`, in sentences like "Errands only on weekends" or
"Don't ask about free time for scheduling the next-priority todo after 6pm". After setup, there are two ways to change it:

### Make a change

For one specific thing. Just tell Claude, any time - it can be anything, not only the options setup
offered:

- "Stop asking me about flex time."
- "No errands on Mondays."
- "I want to track the fridge too."
- "I want meal planning to stay within a weekly budget."

It works out what kind of change that is (a big request is often a mix):

- **A preference** goes into your rules. Almost everything is this.
- **Something new to track** (a fridge list, a new detail on each meal) gets defined in the
  tracker files.
- **A new behavior** that the skill currently has no place for means editing the skill itself. That's allowed -
  it's your copy - but Claude will say so first: your copy will then differ from the published
  one. It logs each of these changes in
  `.claude/skills/todays-todos/LOCAL-CHANGES.md`, so they can be carried over if you ever pull an
  update from the published version. It is highly suggested that you ask for a review (see below) for new behaviors that change your skill file.

### Walk through my settings

For a bigger look, say "walk through my settings". Claude
asks which areas to go over (scheduling, privacy, meals, version control), shows what your rules
say now in plain language, brings back setup's examples in case something's newly useful, and
changes only what you ask. Nothing gets reset.

### Fixed for now

A few things are built into the skill rather than set in your rules: weeks start on Monday,
deadlines get flagged when they're about two weeks out, freezer suggestions get a "might need
thawing" note from mid-afternoon on, meal planning covers dinners, and cooking time is measured as
reheat / quick / medium / long. You can still change any of them - it's a change to the skill
itself, so Claude will say so first.

## Keeping it healthy

Rules pile up, and a new one can quietly contradict an old one. After you change how the skill
works, open a fresh Claude Code session and paste the prompt in [REVIEW.md](REVIEW.md). It reads
the skill and your rules looking for contradictions, broken references and duplicated rules, and
reports back without changing anything.

## How it's organized

```
.claude/skills/todays-todos/   the procedure - the same for everyone
  SKILL.md                     the check-in, capture, and saving
  lookup-mode.md               "where are we on X?"
  meals-mode.md                planning, the freezer, adding meals
  setup-mode.md                first-run setup, making changes, walking through settings
  VERSION                      which published version this copy started from
  LOCAL-CHANGES.md             your changes to the skill itself, for carrying over in an update
tracker/
  rules.md                     how you want it to work
  schedule.md                  what happens when - dated sections, then next week, next month, later
  threads.md                   one record per ongoing project: deadline, next step, history
  priority.md                  which projects matter most
  meals.md                     your meals, their details, what's in the freezer
  done.md                      finished tasks, if you're not using git (git keeps them otherwise)
hooks/                         the privacy check that runs before each save
REVIEW.md                      the review prompt
CHANGELOG.md                   what changed in each published version
```

Each fact lives in exactly one file (and the review ensures this remains the case). A task's date is in `schedule.md`; its project's deadline is in
`threads.md`; the check-in puts them together when it shows you the task. That's what keeps the
files from drifting out of sync as they grow.

## TODO: an update flow

Not built yet - add if updates are made to the published skill.

The pieces it needs are
already in place: `VERSION` records where each copy started, `LOCAL-CHANGES.md` records what each
user changed and why, and `CHANGELOG.md` records what changed upstream.

When a user says "update Homefront":

1. **Fetch** the latest skill files and `CHANGELOG.md` from GitHub. Compare its version with the
   local `VERSION`; if they match, say so and stop.
2. **Explain** what changed upstream since their version, in plain language, from the changelog
   entries in between.
3. **Check for local changes.** If `LOCAL-CHANGES.md` has no entries, the new skill files simply
   replace the old ones. Their rules and data are in `tracker/`, which an update never replaces.
4. **Otherwise, reapply each local change** onto the new files, using its "why" to rebuild it where
   upstream rewrote that step. Where an upstream change and a local one collide, show both and let
   the user choose: keep theirs, take upstream's, or combine.
5. **Migrate the tracker** if the changelog says the format changed (a renamed field, a new
   section) - show what will change in the user's files and confirm before touching them.
6. **Offer new setup questions** if upstream added any, using **Walk through my settings** for just
   those areas.
7. **Finish:** set `VERSION` to the new version, keep `LOCAL-CHANGES.md` entries that still apply,
   commit (if version control is on) as `skill: update to <version>`, and suggest a review with
   [REVIEW.md](REVIEW.md).

A user who cloned with git can also get the files with `git pull`, but the reapply and migrate steps
still apply; a user who downloaded a ZIP has no git history to merge from, which is why `VERSION`
exists.
