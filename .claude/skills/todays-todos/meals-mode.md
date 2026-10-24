# Meals mode

Runs only when **Meals** is on in `tracker/rules.md`. Flows:

- **Plan** a week of dinners. Used by meals mode ("let's meal plan for next week"), by the
  check-in's planning day, and in a smaller form by **Single night**.
- **What can I make with…** — "I have half a cabbage, what can I do with it?"
- **Show an inventory** — "what's in the freezer?" Read-only.
- **Update an inventory** — "I froze…", "there are 2 left", "we finished the…".
- **Add or fill a meal** — a new record, or the `?`s on an existing one.

Plus **Thaw**, which the check-in uses to remind the user the day before a dinner that draws on the
freezer.

Where things live - this file is the procedure only:

- The rules: **Meals** in `tracker/rules.md`, split into hard and soft. Read them every time.
- What a record holds, and which field groups are on: **Fields** in `tracker/meals.md`. Use only
  fields from active groups; a step below that names a field from an inactive group is skipped.
- Inventory counts and the `(freezer: …)` tags plans write: the inventory sections of `meals.md`.
- Which optional features are on (freezer, weather, diet tags, key ingredients): the **Active
  groups** line in `meals.md`, and only there. "Freezer on" below means the freezer group is
  listed; the rules say how each feature is used.

Say "serving" for what the rules define as one, and "single portion" for one person's dinner when
the rules freeze those - nothing else. A night needs the number of servings **Meals › Household**
says a normal night takes, adjusted for anyone out or coming over; every count below - placing a
cook, drawing from the freezer - is in those terms.

Save and commit as the skill's **Saving changes** says.

# Plan

## 1. Pin the week

- **Resolve the dates and say them.** "Next week" means the coming week as **When to plan** in the
  rules defines the nights. Say which dates you're taking, and let the user correct you.
- **Plan only the open nights,** among the nights the rules say to plan. A night that already has
  a dinner keeps it and counts toward every rule. If every night is planned, show them and ask
  whether to change any; stop there if not.
- **Read the nights around it** that any rule reaches back into: the rest of the same week, the
  previous week, earlier dinners with an open `pair:`. If a rule can't be fully checked because a
  night is unknown, say so.
- **Check `schedule.md` for anything that changes the week:** a night out, a late event, a night
  already marked as needing no dinner. A night they're away gets no dinner. If the **Privacy**
  rules keep travel dates out of the tracker, the schedule won't say they're away, so ask about
  it when settling the week (below), and never write why a night is empty: the adjustment line
  says only `Thu 3/12 no dinner needed`, with no reason, and nothing in a row, record or commit
  message ties the night to a trip.
- **Settle the week in one AskUserQuestion call.** Show each night's prep limit from the rules,
  plus any `Week adjustment:` line already in the week's block. Then ask:
  - whether this week is different - busier, lighter, particular nights.
  - whether anyone's out or coming over (if the household is more than one person, or guests
    matter), and whether any night needs no dinner at all.
  - if **Meals › Using things up** asks for it: whether there's anything in the fridge or pantry
    that needs using - unless an inventory in `meals.md` already tracks it.

  Write adjustments and who's home to the week's `Week adjustment:` line right away (step 5 has
  the format). They apply only to the dates they name; the rules never change from here.
- **Notice repeats.** If the previous week had the same adjustment, ask once whether it should be
  a standing rule. If yes, that's **Make a change** in `setup-mode.md`.

## 2. Take stock

- **Inventories:** for each tracked inventory, build its table from **Show an inventory**. Plan
  from the **Free** column.
- **Weather (if the weather group is on):** steering by weather is a soft rule unless the rules
  say otherwise. Fetch the forecast from the one source **Meals ›
  Weather** names, for the location it names, with WebFetch. Note each night's high and the
  issue time.
  - Use that one source only. Never fill gaps from search snippets or a second site: snippets are
    often seasonal averages, not the forecast.
  - If it doesn't give a clear high for every night, or the fetch fails, show what it did give
    and ask the user for the missing highs. Don't guess.
  - Show the highs in the plan and say where they came from.
- **Settle `?`s before placing anything.** A `?` in a field the plan relies on gets asked before
  that dish goes on a night, using **Filling in blanks**. Batch them into as few calls as you can.
- **Fridge items:** anything the user named in step 1 gets matched to meals that use it - by
  `key ingredients:` when that group is on, otherwise by name and judgment - and those meals are
  favored this week.

## 3. Fill the nights

Each night's limit is **Prep by day** in the rules, with this week's adjustments applied.

1. **Nights out first,** if **Meals › Eating out** plans for them: a set night ("Friday is takeout") gets an
   `eat out` dinner, unless that night is already planned. A limit ("at most once a week") counts
   nights out already planned or confirmed in that week, and the plan never adds one past it -
   though it can offer one on a night nothing at home fits. A night out has no prep, isn't a
   repeat of anything, and carries no diet tags unless **Meals › Eating out** says otherwise.
2. **Nights only one person eats** (if single portions are frozen): give each a free single
   portion that passes the hard rules. A night with some people out but more than one eating still
   takes a full serving, unless **Meals › Household** says otherwise. Without one, plan it like any other night.
3. **Find the cooks.** Which days have time to cook, and at what prep level? A cook makes its
   `makes:` in servings, so it covers `makes:` divided by the servings a night takes - a `makes: 8`
   chili is two nights for a household of four counting one-person servings.
   A **cook row** is the row for the session where the cooking happens: a prep-session row on its
   own day, or the dinner row itself when the cook is that night's dinner.
4. **Place each cook's servings** on nights whose limit fits. A serving after the cook night is
   `reheat`. Account for every serving:
   - With the freezer on, a dish that freezes can put its spares in the freezer, and can be cooked
     bigger to bank more. Its cook row gets a `+N` tag (step 5).
   - Otherwise every serving is placed within its `eat:` window and the repeat limits. If they
     don't fit, suggest a half batch; if that doesn't fit either, say what will go to waste.
   - A meal with `leftover:` banks that component too.
5. **Fill the gaps** with no-cook meals and free inventory servings, within each night's limit.
6. **Source the components.** Each serving of a main with `requires:` needs a serving of each
   component, taken from, in order: a free inventory serving (a draw used in a prep session goes
   on that prep row, not the dinner row); an extra from an earlier prep session; making it that
   night, which raises the night's prep to the component's.
7. **Pick the sides.** For each side-kind field that isn't `none`: a side with servings already
   made, inside its `eat:` window; otherwise the best fit from the main's list (plus every side of
   that kind when the list ends in `any`), ranked by any sides rule in the rules, then by oldest
   `last eaten:`. Say so when a pick comes from outside the main's list or skips a recent one.
8. **Judge each night as a whole plate** - main, sides and components: its prep is its heaviest
   part, and it carries a diet tag only if every part does.
9. **Check every rule** in the rules, with this week's adjustments applied: hard rules, then soft
   rules, then variety last as the tiebreak - among plans that pass everything, prefer mains with
   the oldest `last eaten:`. Any limit on repeats is whatever the rules set, as a hard or soft
   rule; the tiebreak isn't a limit. Hard rules must pass; when one can't, say which and
   why, and offer the least-bad version rather than hiding it. A missed soft rule gets flagged
   with the swap that would fix it.

If **Meals › Suggestions** allows the web, and the repertoire can't fill a night well (everything
recent, nothing fits the prep or the weather), offer one or two ideas from a quick web search,
marked as new, each with an offer to add it through **Add or fill a meal**. With "only my list",
never do this.

## 4. Propose

Offer **two plans** when there's a real choice; one when the week is mostly settled. For each
plan, show one line per night with the columns that matter for this user's rules - prep, diet
tags, the high - then a checklist of the rules:

```
Mon 3/9   chili (freezer) + green salad                quick          58F
Tue 3/10  sheet-pan gnocchi + roasted squash           medium  vegan  61F
Wed 3/11  pulled pork sandwiches (freezer)             reheat         66F  late meeting
Hard: ✓ prep by day  ✓ max 2x/week  ✓ nothing wasted
Soft: ✓ vegan 1+  ✓ weather (chili on the coolest night)
```

When the week has adjustments, the checklist shows the adjusted rule, with a short marker.

Then list:
- **Prep sessions:** what to cook and when.
- **Shopping:** anything from a meal's `shopping:` field, plus what the prep sessions need.
- **Inventories:** each tracked inventory's table with this plan counted in.
- **Variety:** anything left out for being recent, a line each.

Ask which plan they want, or what to change. Dinner plans are tentative, so changes are cheap.

## 4b. Change a night

0. **A change to the week isn't a swap.** If what they say changes the week's conditions ("I'm
   late Monday too", "a friend's coming Thursday"), write it to the `Week adjustment:` line first.
   While the plan is still a proposal, re-run it from step 3 and show what moved. For a week
   already written, check each written night against the change and run steps 1-6 for the ones
   that no longer fit.
1. **Ask what to swap:** the main, a side, or the whole plate.
2. **Rule it out for this session,** so it isn't offered again for that night. Don't write it to
   the record: a mood isn't a fact about the meal.
3. **Rank the candidates for that night,** every other night held fixed: hard rules, then soft,
   then variety.
4. **Offer the top 2-3,** each with one line on why it ranks there.
5. **If nothing fits,** name the rule that's blocking and offer the smallest change to another
   night that frees it up.
6. **Re-check the whole week before writing:** sides, `eat:` windows, repeat limits, pairs,
   inventory servings freed or used, and where each component now comes from.

## 5. Write it down

- **One block per week.** Dinners go under `Dinners (tentative), week of Mon M/D:` in the
  `schedule.md` section their dates belong to. Create the block if it's missing.
- **The `Week adjustment:` line opens the block,** with real dates and the reason when given:
  `- [stated] Week adjustment (Mon 3/9–Fri 3/13): Wed 3/11 quick only - late meeting; Thu 3/12 friend over`.
- **A dinner row** is open, dated, and names the main, its components and its sides - nothing a
  record already says. Its freezer tag goes at the end:
  `- [ ] Mon 3/9: chili + green salad (freezer: @chili)`. Add `[stated]` when the user named it.
  A planned night out is `- [ ] Fri 3/13: eat out`.
- **Prep sessions** are rows on their own days, with a size tag and capacity checked like any
  other row: `[ ] Make chili (large) (freezer: +2 @chili)` in a dated section, or `[ ] Make chili -
  Sat 3/14 (large) (freezer: +2 @chili)` anywhere else.
- Counts and `last eaten:` don't change from a plan.

## Single night

When the check-in finds an empty night it should suggest for, or the user asks what's for dinner:

1. **Freezer options are always on the table** (freezer on). Some things cook from frozen and
   some need thawing, and the user knows which. So offer them at any hour. From mid-afternoon on,
   add a short note to each: "if this one needs thawing, it's getting late - pick another or do a
   quick thaw." Earlier in the day, a note that it can come out now is enough.
2. **If only one person is eating tonight** and single portions are frozen, look for a free one
   first.
3. **Check tonight against the plan's rules** - prep limit, hard rules, the week so far, open pairs
   and `eat:` windows.
4. **Suggest one or two options,** each with its sides. For a cook that makes more than tonight
   needs, say where the other servings go.
5. **When they pick one,** write it as step 5 does.

Skip the forecast unless the weather rule would change the choice.

# What can I make with…

"I have half a cabbage and some feta, what can I do with it?"

1. **Search the repertoire.** Match each named item against `key ingredients:` when that group is
   on, otherwise against names and your judgment of what a dish uses. Rank matches by how many of
   the items they use, then by the rules (tonight's prep limit if it's for tonight, then oldest
   `last eaten:`).
2. **Show the matches,** a line each, saying which of the items each one uses.
3. **Then follow Meals › Suggestions in the rules:**
   - *Only my list* - stop there. If nothing matches, say so.
   - *My list first, then the web* - if the list has fewer than two good matches, do a quick web
     search and offer two or three ideas, cited, marked as new.
   - *Web welcome* - always add two or three web ideas after the list's matches.
4. **Offers:** to put a pick on a night (that's **Single night** or a change to the week's plan),
   and to add a web idea to the repertoire through **Add or fill a meal**.

Read-only unless they take an offer.

# Thaw

Freezer on only. Some freezer items need time to thaw and some don't; the user knows which, and
the reminder is there in case. Out the night before is best for anything that does need it, so
the reminder comes the day before - as a heads-up, not a task that must happen. When tomorrow's
dinner row, or a prep-session row dated tomorrow, isn't `[x]` and has a `(freezer: …)` draw,
today's list gets:

`Defrost if needed: chili, for Tue 3/10 dinner (small)` - tagged with the smallest size in the
rules.

- Name every container the draw lists. A `+N` on a cook row is banking, not a draw.
- Look one day ahead only. Don't flag tonight's dinner as late after a skipped check-in.
- Derived from the tag every time, never written to `schedule.md`.

# Show an inventory

"What's in the freezer?" Read-only: no questions, no edits, no commit.

For a counted inventory like the freezer, one row per line in `meals.md`, plus anything only
planned to bank:

```
Item             Banked   Scheduled             Free   Planned to bank   Free incl. planned
chili            2        Mon 3/9               1      +2 Sat 3/14       3
pulled pork      1        Wed 3/11              0      —                 0
```

- **Banked** is the count in `meals.md`.
- **Scheduled** is every draw on a row not yet `[x]`, by date. A past night not yet confirmed is
  marked `(unconfirmed)`.
- **Free** is Banked minus Scheduled. Below zero, flag it: those nights have lost their food.
- **Planned to bank** is every `+N` on a cook row not yet `[x]`.
- A `?` count shows as `?`, and so does everything computed from it.

An inventory `meals.md` defines differently (a fridge list with no counts, say) is shown the way its
section in `meals.md` describes.

End with one line offering to update it.

# Update an inventory

1. **Show the table.**
2. **A dinner that got eaten isn't an inventory update.** If a change matches a scheduled draw or
   last night's dinner, confirm it the way step 2 of the check-in does - that lowers the count
   once. Lowering it here too would count it twice.
3. **Ask what changed.** Read each change as an addition or a new total, and repeat back which you
   took: "I froze 4 more" adds 4; "there are 2 left" sets it to 2. When it's ambiguous, ask.
4. **A batch that matches a planned cook row:** mark that row `[x]` too. Use what they say went in,
   not the row's `+N`.
5. **Check it against the plan.** A count lower than what's scheduled means those nights lost
   their food. Offer to re-plan them.
6. **Every freezer item needs a record** - a main, side or component. For a new item, run **Add or
   fill a meal** first.
7. **An item at zero comes off the list.** Its record stays.

# Add or fill a meal

Which fields a record has, which are required, and their allowed values all come from **Fields**
in `meals.md`, limited to the active groups. Read them there every time, so a field added later
gets asked about automatically.

**Adding a meal:**

1. **Check for a match.** Read every record and look for one that sounds like the same dish - a
   synonym, a reworded name. If anything is close, ask whether it's the same meal (then show it and
   offer to fill its `?`s) or a new one.
2. **Name and key.** Suggest an `@key` (short, lowercase, hyphenated) and check it isn't taken.
3. **Main, side or component?** Ask this first; it decides the fields.
4. **Required fields,** in an order where earlier answers decide later questions: a main's
   `requires:` before its prep; `makes:` (and `freezes:`) before `eat:`; for each side-kind field,
   first whether it takes that kind at all. A side or component it names with no record gets one,
   through this same flow.

   Ask each field as its own question with its own options, and never ask the user to type values
   "in the notes". Every question offers "Not sure", stored as `?`. AskUserQuestion takes up to 4
   questions per call and 4 options per question; when a field's values plus "Not sure" come to
   more than 4, say "not sure" can be typed in the free-text answer.
5. **Optional fields:** a multi-select of the optional fields for that kind from the active groups.
   A new main or side gets `last eaten: ?` unless they say when.

   If they say something about the meal that no field holds, don't write it loosely. Say which
   field is missing, and offer to add it to **Fields** for every record - that's **Make a
   change**, kind 2 in `setup-mode.md`.
6. **Check a `pair:`:** the partner has to exist, the pair goes only on the meal that opens the
   shared ingredient, and it needs a window in days and a reason.
7. **Write the record** under `## Meals`, `## Sides` or `## Components`, tagged `[stated]`. Show it
   back as one block.

**Filling in blanks:** a blank is a `?` or a missing required field (`last eaten: ?` isn't one).
One question per field, up to 4 per call, naming the meal and its known values so they can correct
those too. Write answers as you go.
