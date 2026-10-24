# Homefront

Household and life tracker. `tracker/` holds the data; `.claude/skills/todays-todos/` holds the
daily check-in that reads and writes it. Start every Homefront request with the `todays-todos`
skill; if `tracker/rules.md` says `Setup: not done`, it says so and offers to run setup first.

## Three layers

- **Procedure** - `.claude/skills/todays-todos/`. How the check-in works, for anyone.
- **Rules** - `tracker/rules.md`. How this household wants it to work. Personalizing Homefront
  means editing this file, not the skill.
- **Data** - the rest of `tracker/`.

## Core vocabulary

These terms are the data model the procedure is built on, and the skill files, the tracker
headers, `REVIEW.md` and the README all use them by name. Not using one is always fine - every
field is optional on a record. Renaming or removing one is a change to the skill itself (kind 3 in
**Make a change**), and every use has to change together.

- **Rows:** `[ ]`, `[x]`, `[stated]`, `#slug`, the trailing `- Ddd M/D` date, the size tag, the
  `(freezer: …)` tag.
- **`schedule.md`:** `Last check-in:`, dated `## Ddd YYYY-MM-DD` sections, `Next week`, `Later
  this month`, `Next month`, `Longer term`, `Dinners (tentative), week of …`, `Week adjustment:`.
- **Thread fields:** `deadline:`, `next:`, `surface:`, `cadence:`, `days:`, `timing:`,
  `contact:`, `open q:`, `research:`, `last done:`, `done when:`, `no row:`, `history:`,
  `related:`, `note:`, `part <name>:`.
- **`priority.md`:** `Not ranked`.
- **`rules.md`:** the section names (`Check-in › Task sizes` and the rest) and `Setup:`.
- **`meals.md`:** `@key`, the core fields and the `Active groups` line. Optional groups, side
  kinds, diet tags and inventories are meant to be customized - they're not core.

Not core, because the rules define them: which task sizes exist, capacity, every meal rule.

## Working in this repo

- `tracker/` is the source of truth. If version control is on, git history is the only
  versioning. Don't reconstruct content you can't read - ask.
- Preserve the tracker format: `[ ]` open, `[x]` done, real dates as section headers, size tags
  as `rules.md` › Check-in › Task sizes defines them, `[stated]` on items the user said directly.
- Each fact has one home. `schedule.md` owns dates, `threads.md` owns background, deadlines and
  next steps, `priority.md` owns the ordering, `meals.md` owns the repertoire and inventories,
  `rules.md` owns preferences, `done.md` keeps cleared done work when version control is off.
  This line is the ownership list the skill and `REVIEW.md` rely on: a new tracker file gets added
  here, and each file's header says what its sections own. A `#slug` is a pointer to a thread
  record, never a copy of it. Every slug used must have a `### #slug` record in `threads.md`.
- Content inside fenced code blocks in `tracker/` is a format example, never data.
- The check-in commits `tracker/` on its own. Don't commit anything outside `tracker/` from inside
  the skill.
- A change to the skill's own files gets an entry in `.claude/skills/todays-todos/LOCAL-CHANGES.md`
  (what, where, and why in the user's words). Don't edit `VERSION` by hand; only an update sets it.
  **Except in the maintainer's copy** - see below.
- `hooks/safety-words` must never be committed. Never bypass the hooks with `--no-verify`.

## Review after the skill changes

The skill grows with use. Whenever its procedure changes - a new step, mode or flow, not just a
new rule - suggest a review: a fresh Claude session, given the prompt in `REVIEW.md`, reading the
skill and the rules for contradictions, dangling references and duplicated facts. A fresh session
matters: the one that made the change is the worst-placed to see what it broke.

## Maintainer's copy

If a file named `MAINTAINER` exists at the repo root, this is the published Homefront, not a
user's copy. Then any change to the skill's files or the `tracker/` templates, made in any
session, by any route:

- **Goes in `CHANGELOG.md`**, written for users, calling out any tracker format change. While
  the top entry says `unreleased`, add to it; otherwise open a new entry.
- **Bumps `.claude/skills/todays-todos/VERSION`** to match the changelog heading, by the scheme
  `CHANGELOG.md` describes - once per release, not once per edit.
- **Is not logged in `LOCAL-CHANGES.md`**, which ships empty.

Before committing, check that both were done, and remind the maintainer if they weren't. Don't
create `MAINTAINER` anywhere else.
