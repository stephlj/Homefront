# Setup mode

Three flows:

- **Setup** - the first run, when `tracker/rules.md` says `Setup: not done`.
- **Make a change** - any time after that, for any change at all: a new preference, something new
  to track, a whole new feature. Not limited to what setup asked about.
- **Walk through my settings** - only when the user asks to go over everything, or asks to set up
  again after setup is done.

Setup and **Make a change** write `tracker/rules.md`, and sometimes the **Fields** or inventory
sections of `tracker/meals.md`. Setup also dates the section headers in `schedule.md`, writes
`hooks/safety-words`, and sets up git. **Walk through my settings** writes only through **Make a
change**. None of them edits the skill's own files, except kind 3 of **Make a change**.

Setup runs on its own, outside the check-in, but step 7 uses the check-in's **Capturing something
new** and **Saving changes** and meals mode's **Add or fill a meal**. Read every tracker file
first, as the check-in's step 0 does.

# Setup

Warm, unhurried, and short per step. Tell the user up front: a few questions, about ten minutes,
everything can change later by just telling you. Ask open questions and let them answer in their
own words; the example lists are there to jog ideas, not to be read out as a form. Nothing they
don't mention gets turned on - except the three defaults already written in `rules.md` (step 3),
which you name.

Write each section of `rules.md` as soon as its step is settled, in plain sentences, tagged
`[stated]` - a default they confirm gets the tag too. Replace the `<!-- -->` placeholder; leave
sections that don't apply with a single line saying so ("Off.", or "None." where a section has
nothing to say). When meals are off, every Meals subsection gets "Off." too.

**Resuming.** A section counts as written once it has a `[stated]` line. The three defaults the
template ships with (Look-ahead, Task sizes, Over capacity) don't count until the user confirms
them. If some sections are written and others aren't, an earlier setup was interrupted: say which
steps are done, and pick up at the first step with unwritten sections. Don't re-ask a written
section; change it only if the user asks.

## 1. Version control

Before checking for git, say: "I'm going to check whether a tool called git is installed. On a
Mac, if it isn't, a system window may pop up offering to install developer tools - that's just
this check, and it's fine to cancel it." Then run `git --version`.

- **Git is installed:** explain in two sentences: "I can keep an undo history of your tracker, so
  nothing is ever lost if I make a mistake. You won't need to do anything - I'll save a snapshot
  after each check-in." Recommend yes.
- **Git isn't installed:** say the undo history needs a free tool called git, and that installing
  it is their choice. On a Mac, the pop-up from the check installs it (Apple's command-line
  tools); on Windows, it's the installer from git-scm.com. Offer to wait while they install it, or
  carry on without it. Never install it yourself.

If yes (and git is available), run **Turn on version control** below.

If no: write `Off. No commits; there's no undo history.` and say once, plainly, that edits can't be
undone. They can turn it on any time later. If this folder is already a git repository anyway -
they cloned it - still run step 2 of **Turn on version control** (the remote check), since a
clone points at the public template whether or not they use the history.

### Turn on version control

Used by setup, and by **Make a change** or **Walk through my settings** whenever version control
goes from off to on. Each step checks before acting, so it's safe to run on a folder that's
already partly set up.

1. **A repository.** If `git rev-parse --show-toplevel` fails, or names a folder other than this
   one (this folder sits inside some other repository), run `git init` here.
2. **A remote, if any.** Run `git remote -v`. If there's a remote - usually because they cloned
   this from GitHub rather than downloading it - say plainly that it points at the public
   Homefront template (or their copy of it), and that pushing would publish their tracker there.
   Recommend removing it (`git remote remove <name>`) and do it if they agree. The privacy talk in
   step 4 covers sharing.
3. **Who commits.** If `git config user.name` or `git config user.email` is empty, ask for a name
   and an email to label the snapshots - any values work, nothing is sent anywhere - and set them
   for this folder only (`git config user.name "…"`, `git config user.email "…"`).
4. **The privacy hook.** `git config core.hooksPath hooks` and `chmod +x hooks/pre-commit
   hooks/commit-msg hooks/safety-check`. Check `git check-ignore hooks/safety-words`; if it isn't
   ignored, fix `.gitignore` before anything else.
5. **Privacy before the first commit.** If the tracker already has data in it (version control
   is being turned on later, not during setup), do step 8 first, then reread every tracker file
   against the Privacy rules and reword anything that slipped in. Only then commit.
6. **A starting point.** If the repository has no commits yet (`git log` fails), commit
   everything as it is now: `git add -A && git commit -m "Homefront <contents of VERSION>"`. Without
   this, the skill's own files stay uncommitted forever and every check-in would see them as
   unrelated changes.
7. **The rule.** Write **Version control** in the rules: `On. Commit tracker/ after each run.`
8. **The privacy talk and word list** from step 4, if they haven't had them - when this runs from
   setup, step 4 does it in order.

## 2. About

Start with the work, not the people. Ask what they'd like Homefront to carry for them - tasks and
projects, meals, or both - and how wide it reaches: just their own tasks, or ones they share with
a partner or housemates? A number and a rough sense is plenty ("tasks for two of us, meals for
four"); don't ask for names or who's who. If they offer names, use them, but only where a task
needs one ("Sam picks up the dry cleaning").

Write **About** in those terms. Then write `On.` or `Off.` in place of the placeholder directly
under `## Meals` - that line is what the skill checks to know whether meals are in use. If meals
are off, write `Off.` under each Meals subsection too, and skip step 5.

## 3. Scheduling

Ask one open question: **"What do you take into account when you plan your tasks?"** Then offer
the examples below as a menu to react to, a few at a time, in the user's terms. They pick, adjust
or add their own.

Name the three defaults already in the rules and ask whether to keep them:

- **Today only.** The check-in shows only today; the rest of the week stays out of sight unless you
  ask. (Deadlines with nothing scheduled, and overloaded days, still get flagged.) This is most of
  what makes the tracker feel lighter - but some people like a look-ahead.
- **I don't prioritize for you.** When a day's over capacity, I say so and show everything; what
  moves is your call. Some people would rather get a suggestion.
- **Two task sizes,** small and large (see Task sizes below).

Examples to offer:

- **Task sizes:** every task gets a size tag, and capacity and free-time picks are counted in
  them. The default is small (one sitting) and large (real time or several steps). Some people
  want three sizes, or time estimates like 15m / 1h / half day.
- **Capacity:** "I can only really do one big thing a day." "Two calls and one errand, max."
  Does a day hold a set number of each size? With no limit, no day ever gets flagged as heavy.
- **Free time:** "Ask me each morning if I have free time, and suggest something that fits - a
  small task for a small window, a big one for a big window." The hour after which not to ask.
- **Priority:** "If I haven't touched my top project in a while, push it." "One area of my life
  should always get first pick of free time."
- **Days and hours:** "Errands only on weekends." "Calls only during business hours." "Nothing on
  Sundays."
- **Research:** "If a task is a process I haven't done before - renewing a document, a permit -
  look up the steps first and tell me which happen online and which in person." Subjects not to
  research.
- **Many things due at once:** "If several recurring things come due on the same day, spread them
  out instead of dumping them on today."
- **Daily questions:** anything to be asked at every check-in or on certain days - "On Fridays, ask
  whether I'd rather take a walk than do more tasks." Whether it's to be recorded or just
  considered.

Write the answers into the **Check-in** subsections, by this map:

| Example | Section |
|---|---|
| Today only | Look-ahead |
| Task sizes | Task sizes |
| Capacity | Capacity |
| I don't prioritize for you | Over capacity |
| Days and hours | Day constraints |
| Daily questions | Daily questions |
| Free time, Priority | Flex time |
| Research | Research |
| Many things due at once, anything else | Other |

Settle Task sizes before Capacity and Flex time, since both are written in its terms. Anything
that matches no section goes under **Other** in their words.

## 4. Privacy

Explain: the tracker is plain text, and if they ever share or sync it, it's readable. Ask what
should never be written down. Offer these categories, which the user can pick, adjust or add to:

- **Health** - that something needs doing is fine ("book the appointment"); conditions, tests,
  medications or a specialist's field aren't.
- **Finances** - "call the bank" is fine; institution names, amounts, balances and account numbers
  aren't.
- **Travel dates** - "a trip in early May" is fine; exact days away aren't (an empty house).
- **Access and identifiers** - codes, passwords, where a key is hidden, ID numbers, plates.
- **Other people's details** - personal phone numbers, addresses, and where someone regularly is
  (a child's school and pickup times, a parent's care schedule).

Mention that a private fact can leak without being named - a run of empty nights shows a trip, a
diet tag named for a condition shows the condition - and that the skill words things to avoid
that, too.

**If version control is on, say this plainly, and don't soften it:**

- **Keep it local.** The recommendation is to never push the tracker anywhere - the undo history
  works entirely on this computer, with no account and no upload.
- **If they want to share it** - with a partner, or across their own devices - the remote must be
  a **private** repository, shared only with the people who should see it. This folder started
  from a public template, and a copy put on GitHub isn't private unless they choose that.
- **Why it matters even with the privacy rules on:** the rules keep out the most sensitive
  specifics, but a todo list still says a great deal - what someone's dealing with, when they're
  busy, how they live, what they're worried about. Anyone who can read it learns a lot about them.

Ask them to confirm they've understood, and don't help set up a remote until they bring it up.
If they ever ask to push, repeat the point about a private repository first.

Write **Privacy** in the rules, each category with what's fine and what isn't.

**Then, if version control is on, the word list.** Ask for specific words that should never land in
a commit: their bank, their doctor's practice, a street name. These go in `hooks/safety-words`, one
per line, which the hook checks every commit against. Before writing the file, run `git
check-ignore hooks/safety-words`; if it isn't ignored, stop and fix `.gitignore` first - that file
is a list of exactly the words they don't want stored. Tell them they can add words any time.

## 5. Meals (if on)

Ask one open question: **"What do you take into account when you plan meals?"** Then offer the
examples as a menu:

- **How many you're cooking for**, usually, and what one serving is: everyone's dinner (simplest
  when it's usually the same people) or one person's (when the headcount changes a lot). If
  servings are everyone's dinner and there's more than one of them: whether to freeze single
  portions for nights someone's out.
- **When to plan** - which day of the week to plan, and which nights (weeknights only? all seven?).
- **Suggestions on unplanned nights** - which nights the check-in should suggest a dinner for, and
  which to leave alone.
- **Time to cook** - how much cooking each night allows: reheat, quick, medium, long.
- **Eating out** - do they eat out or get takeout often? A set night ("Friday is always takeout"),
  a limit ("no more than once a week"), or a nudge the other way ("planning is about cooking at
  home, not defaulting to takeout"). And whether a night out counts toward a diet goal.
- **Variety** - how often a meal can repeat in a week; not repeating last week's.
- **Weather** - soups on cold nights, salads on hot ones. Needs a location and one forecast source
  (in the US, the National Weather Service point forecast for their location works well; elsewhere,
  ask which site they use) and the temperatures that count as hot and cold.
- **The freezer** - track what's in it, batch-cook to bank servings, get a reminder the day before
  to defrost.
- **Diet goals** - vegan a few nights a week, low-carb, less meat. Becomes `diet:` tags, named for
  the food rather than any health reason behind it, if health is private.
- **Using things up** - being asked about the fridge while planning; "what can I make with…"
  (needs key ingredients on each meal).
- **Ideas from the web** - only my list / my list first, then the web / web welcome.
- **Sides** - mains that take particular sides; kinds of side other than carb and veg.
- **Shopping** - fresh things to buy for particular meals.

Then structured follow-ups only for what they chose (prep per weekday, the location and source,
the diet tags, the temperature thresholds). Write the answers into the **Meals** subsections, by
this map:

| Example | Section | Also |
|---|---|---|
| How many you're cooking for | Household | |
| When to plan | When to plan | |
| Suggestions on unplanned nights | Suggest on empty nights | |
| Time to cook | Prep by day | |
| Eating out | Eating out | |
| Variety | Hard rules or Soft rules, as they want it enforced | |
| Weather | Weather | weather group |
| The freezer | Other, for anything about how they use it | freezer group |
| Diet goals | Diet tags (the tags), Soft rules (the aims) | diet group |
| Using things up | Using things up (the fridge question) | ingredients group, for "what can I make with…" |
| Ideas from the web | Suggestions | |
| Sides | Other | rename the side kinds in Fields if they changed |
| Shopping | Other | extras group |
| anything else | Other | |

Every Meals subsection they didn't mention gets "None." (or "Off." where it's a feature).

**If they chose weather, test the source now:** fetch it once for their location and show the
next few days' highs. A wrong link should surface here, not at the first meal plan.

Then in `meals.md`: set the **Active groups** line from the "Also" column - that line is the only
switch for these features, so the rules sections never say whether a feature is on - and delete
the Freezer section if the freezer group is off. A new inventory they want (a fridge or pantry
list) gets its own section, defined the way **Make a change** kind 2 describes.

## 6. Read back

Show the finished `rules.md`, grouped, as the skill will apply it - "Each morning I'll show only
today, and ask if you have free time unless it's after 6pm." Let them correct anything. Then change
the `Setup: not done` line in `rules.md` to `Setup: done YYYY-MM-DD`.

## 7. Get started

First, put real dates on the relative sections of `schedule.md`, so rows land in the right place:
`## Next week (week of Mon YYYY-MM-DD)`, `## Later this month (through Ddd YYYY-MM-DD)`, `## Next
month (Month YYYY)`. Then offer, in order:

1. **A brain dump.** "Tell me everything that's on your mind - big projects, small chores,
   anything with a deadline." Capture each one per **Capturing something new**, asking only the
   questions it needs. Recurring chores get a `cadence:`.
2. **A few meals** (if on): the dinners they make most. Add them through **Add or fill a meal**;
   "Not sure" is fine everywhere.
3. **What's in the freezer** (if the freezer group is on): run **Update an inventory** in
   `meals-mode.md` once, so the first plan starts from real counts. Each frozen item needs a
   record, so this often follows step 2.
4. **The ranking.** Show the threads and ask which matter most, for `priority.md`.

Then commit (if on) as `todos: setup YYYY-MM-DD - rules and first entries`, and tell them: "Just ask for today's todos
whenever you want to check in. And whenever something should work differently, tell me - I'll
update your rules."

# Make a change

The user wants something to work differently, and it can be anything: "stop asking me about flex
time", "no errands on Mondays", "I want to track the fridge too", "I want meal planning to stay
within a weekly budget", or a correction of something the skill just did. Setup's example lists
are a starting point, not the limit of what Homefront can do.

**Understand it first.** Ask only what you need to pin down what the user wants. For the budget, that
might be: a limit per week or per meal? Should a plan that goes over be blocked, or just flagged?
Where do the costs come from?

**Then sort it by what it would change.** A big request is often several kinds at once. Split it,
and do the kinds in order, so the procedure changes only for what's left over:

1. **A preference** - anything expressible as a sentence the procedure can follow. Write or edit
   the line in the matching section of `rules.md` (or under **Other**), quoting them where it
   helps, tagged `[stated]`. Read back the new rule as it will be applied. This is almost every
   request.

   **Turning version control on** is a preference that needs setting up: run **Turn on version
   control** in Setup step 1, which also gives the privacy talk and word list from step 4.
   Turning it off just changes the rule; nothing is deleted.

   Some preferences come with existing data written the old way. **Changing the task sizes** is
   the main one: offer to retag every open row (say how old sizes map to new, and let them
   adjust), and rewrite any rule in `rules.md` that counts in the old sizes, like Capacity.
2. **New data to track** - a fridge list, a field on meals, a new thread field. Define it where the
   data lives: a new field in **Fields** in `meals.md` (for every record, in an existing group or a
   new one, added to **Active groups**), a new inventory section in `meals.md` that says what its
   lines hold and when they change, or a new field in the header of `threads.md`. Then add the rule
   for how it's used to `rules.md`. Existing records get `?` for a new required field; offer to
   fill them in. If the data needs a file of its own (`tracker/budget.md`, say), give it a header
   saying what it owns, like the other tracker files, and add it to the "Each fact has one home"
   line in `CLAUDE.md` - that line is what keeps facts from drifting, and what the review checks
   against. Commit that `CLAUDE.md` edit together with the rules change - it isn't a change to the
   skill, and left uncommitted it would block every later check-in's commit. The procedure
   doesn't change.
3. **A new behavior** - a step, mode or flow the procedure has no place for, even with the new
   rule and data in place. This means editing the skill's own files, and that's allowed - it's
   their copy. Before editing, say plainly: "This changes how the skill itself works, not just your
   rules. Your copy will then differ from the published Homefront, so if you ever take an update,
   this change will need to be carried over - I'll log it so that's easy. Changes like this are also
   where rules start to conflict, so afterward I'd suggest a review with the prompt in REVIEW.md, in
   a fresh Claude session." Then make the change, keep it as small as it can be, and point it at
   `rules.md` for anything personal so the procedure stays general. **If the change renames or
   removes a core term** (the list in `CLAUDE.md`), search for every use of it - the skill files,
   the tracker files' headers and Fields, `REVIEW.md`, `README.md` and `CLAUDE.md` - and change
   them all in the same edit, then retag the data that uses it. One missed mention silently
   breaks the step that reads it. Finally, add an entry to
   `LOCAL-CHANGES.md` next to the skill files, in the format that file shows - unless a
   `MAINTAINER` file exists at the repo root, in which case follow **Maintainer's copy** in
   `CLAUDE.md` instead (the changelog and `VERSION`).

The budget, worked through: a `cost:` field on meals is kind 2 (a new field, in a new **budget**
group). "Keep each week's dinners under $120; flag a plan that goes over, don't block it" is kind 1:
a soft rule, and the Plan flow already checks every soft rule and flags misses. Showing the week's
total under each proposed plan needs a line in the plan's output: that's kind 3, and the only part
that is.

Say in one line which kinds the request turned out to be. If it's unclear whether something is kind
1 or 3, try kind 1 first: a rule the procedure can follow as written beats a new step.

Commit (if on) as `todos: rules YYYY-MM-DD - <what changed>`. A kind 3 change touches files
outside `tracker/`: commit those separately as `skill: <what changed>`, and only when the user
says to.

# Walk through my settings

Runs only when the user asks to go over everything ("let's revisit my settings", "a lot has
changed - new job"), or asks for setup again after `Setup: done`. Never wipe anything, and never
re-run setup from scratch over existing rules.

1. **Ask which areas** they want to go over - scheduling, privacy, meals, version control, or
   all of them. Skip the rest.
2. **For each section in those areas,** show the current rules in plain language, as the skill
   applies them, and offer keep / change / remove / add. Bring back that area's example list from
   **Setup**, so they can pick up ideas they passed on the first time.
3. **Anything they want changed** goes through **Make a change**, so new data or new behavior is
   handled the same way.
4. **Read back** what changed, all together at the end, and commit (if on) as
   `todos: rules YYYY-MM-DD - revisited <areas>`.
