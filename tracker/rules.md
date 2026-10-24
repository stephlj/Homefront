# Rules

How this household wants Homefront to work. The skill reads this file at the start of every run
and follows it; the skill's own files never change to personalize it.

Setup fills this in. After that, tell Claude whenever something should work differently
("stop asking me about flex time", "no errands on Mondays") and it updates the matching section.
Rules are plain sentences: write what you want, the way you'd say it.

`[stated]` marks a rule the user said directly.

Setup: not done

## About

<!-- What Homefront should help with (tasks, meals, both), and how many people's tasks and meals
it covers. -->

## Check-in

### Look-ahead

Today only. Don't preview later days unless I ask; deadlines with nothing scheduled, and days at
or over capacity, are the exceptions.

### Task sizes

Two sizes: `(small)` - quick, one sitting; `(large)` - real time or several steps.

### Capacity

<!-- How much fits in a day, e.g. "two small tasks and one large", in the terms of Task sizes.
"No limit" means no day is ever flagged as heavy. -->

### Over capacity

Show me every row anyway and say the day is over. Don't suggest what to drop or move - that's my
call.

### Day constraints

<!-- Days or hours that only fit some kinds of work, e.g. "errands on weekends only". -->

### Daily questions

<!-- Questions to ask at each check-in, and on which days. Mark any that are only to be
considered, not recorded. -->

### Flex time

<!-- Whether to ask each day about free time and suggest unscheduled tasks that fit, the hour
after which not to ask, and any rule that reorders the picks. -->

### Research

<!-- When to look things up before suggesting a task (e.g. multi-step processes like a permit),
and any subjects not to research. -->

### Other

<!-- Anything else about scheduling. -->

## Privacy

<!-- Kinds of detail never to write into the tracker or a commit message. Specific words to block
go in hooks/safety-words, which is never committed. -->

## Version control

<!-- on or off. On: the skill commits tracker/ after each run. -->

## Meals

<!-- on or off. Everything below applies only when on. Which optional meal features are on - the
freezer, weather, diet tags, key ingredients - is set by the Active groups line in meals.md, and
only there; the sections below say how each is used. -->

### Household

<!-- What one serving is - everyone's dinner, or one person's - and how many servings a normal
night takes (1 if a serving is everyone's dinner; the headcount if it's one person's). With
everyone's-dinner servings and more than one person: whether to freeze single portions for nights
someone's out. -->

### When to plan

<!-- Which day(s) of the week to plan the coming week's dinners, and which nights to plan. -->

### Suggest on empty nights

<!-- Which nights get a dinner suggestion at the check-in when nothing's planned. -->

### Prep by day

<!-- How much cooking each night allows: reheat / quick / medium / long. -->

### Eating out

<!-- A set night ("Friday is takeout"), a limit ("at most once a week"), or none. Whether a night
out counts toward any diet tag. -->

### Diet tags

<!-- The `diet:` tags meals can carry (e.g. vegan, low-carb), named for the food. Aims that use
them go under Soft rules. -->

### Using things up

<!-- Whether to ask about the fridge or pantry when planning a week. -->

### Hard rules

<!-- Rules a plan must not break, e.g. "no main more than twice a week". -->

### Soft rules

<!-- Aims a plan tries to meet and flags when it misses, e.g. "vegan at least twice a week". -->

### Weather

<!-- The location, the one forecast source, and how temperature steers the plan. Used only when
the weather group is on in meals.md. -->

### Suggestions

<!-- Where meal ideas come from: only my list / my list first, then the web / web welcome. -->

### Other

<!-- Anything else about meals. -->
