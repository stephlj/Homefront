# Meals

What's available to cook, not what's planned. A meal gets a night in a week's
`Dinners (tentative)` block in [[schedule]]; its record stays here either way, since the
repertoire doesn't shrink by being used.

The rules that decide which meal fits which night - prep by day, repeats, weather, diet aims -
live in [[rules]] under Meals. Don't restate them here.

**A serving** is defined in [[rules]] under Meals › Household, along with how many servings a
normal night takes. Every part of a plate - main, sides and components - is counted in servings.

## Fields

Every main, side and component is a `### @key - Name` record. An `@key` is a meal pointer,
separate from the `#slug`s in [[threads]]. `?` means not known yet - ask before relying on it.

There's no free-text field. Something a record needs to say that no field covers means a field is
missing - add one here, for every record, rather than writing it loosely on one.

**Field groups.** Core fields are always on. This line is the one switch for every optional meal
feature: setup sets it, **Make a change** changes it, and the skill checks it - not [[rules]] -
to know whether the freezer, weather, diet tags or key ingredients are in use. Only fields in a
group listed here are asked or used.

Active groups: core

### Core (always on)

- `prep:` - how much cooking it takes: `reheat` / `quick` / `medium` / `long`. `reheat` also
  covers no-cook. A serving that's already made - leftovers, or from the freezer - is always
  `reheat`. For a main with `requires:`, prep is with every component already made.
- `makes:` - servings one cook makes.
- `eat:` - `within N days`: every serving from one cook is eaten within N days of the cook night.
  Only for a dish that makes more than one night's servings (and, with the freezer group on,
  doesn't freeze).
- `last eaten:` - the date it was last actually eaten, set when the check-in confirms a dinner or
  the user says so - never from a plan. `?` counts as not eaten recently.
- Mains only: one field per kind of side, named for the kind (`carb:`, `veg:` by default; setup
  can change the kinds). Values: `none` (doesn't take one), `any`, `@x, @y` (these, in order of
  preference), or `@x, @y, any`.
- Mains only: `requires:` - components the main can't go without, often made ahead, like
  `@cooked-rice`. Left off when there's none.
- Sides only: `kind:` - which kind of side it is. A new kind means a new field on every main.

Components are parts a main names, not picked on the night. They have only `prep:`, `makes:` and
any optional group fields that apply to components.

Required when a record is added: mains - `prep:`, `makes:`, each side-kind field, and `eat:` when
it applies; `requires:` is asked too. Sides - `kind:`, `prep:`, `makes:`, `eat:` when it applies.
Components - `prep:`, `makes:`. Every main and side also gets `last eaten:`.

### Optional groups

- **diet** - `diet:` with the tags defined in [[rules]] under Meals › Diet tags (e.g. `vegan`,
  `low-carb`), or `none`.
  Required on mains, sides and components when on. A plate has a tag only if every part does.
- **freezer** - `freezes:` (yes / no) on everything; required when on. Turns on the Freezer
  section below.
- **weather** - `weather:` `warm` or `cool` on mains and sides that suit one; left off if it
  doesn't care.
- **ingredients** - `key ingredients:` on mains, sides and components: the few ingredients that
  define the dish, used to answer "what can I make with ...". Required when on.
- **extras** - optional on mains and sides: `max/week:` (lower than the rules' repeat limit),
  `pair:` (another meal that should land within a window of this one, and why - written once, on
  the meal that opens the shared ingredient), `leftover:` (a component a cook of this also makes,
  by `@key` and servings), `shopping:` (fresh things to buy for each night it's eaten).

Example records (inside a fence, so they're never read as real data):

```
### @chili - Chili [stated]
prep:            medium
makes:           3
freezes:         yes
diet:            none
key ingredients: ground beef, kidney beans, canned tomatoes
carb:            @cornmeal-muffins, any
veg:             @green-salad
last eaten:      2026-02-20

### @green-salad - Green salad [stated]
kind:            veg
prep:            quick
makes:           1
freezes:         no
diet:            vegan
key ingredients: lettuce, cucumber
last eaten:      ?

### @cooked-rice - Cooked rice (component) [stated]
prep:            quick
makes:           2
freezes:         yes
diet:            vegan
key ingredients: rice
```

## Meals

## Sides

## Components

## Freezer

Only when the freezer group is on. Counts only - what's physically in the freezer, each line under
the record it belongs to. Counts change only on what the user says: lowered when a dinner that drew
on them is confirmed eaten, raised when they say a batch went in. A plan never changes them - it
tags rows in [[schedule]] instead, at the end of the row:

- A dinner that draws on the freezer: `(freezer: @chili)` is one serving; `(freezer: @chili,
  @cornmeal-muffins)` is one of each; `(freezer: 2 @chili)` is two servings. A night takes as many
  servings as Meals › Household says a normal night does, so with one-person servings for four
  people, a frozen dinner is `(freezer: 4 @chili)`.
- A cook that banks spares: `(freezer: +2 @chili)`.

If servings are everyone's dinner and the rules freeze **single portions** (one person's dinner,
frozen separately for a night someone's out), lines count them separately: `@chili: 2 servings,
1 single`, and tags write `single @chili`.

What's free is the count minus the draws on rows not yet `[x]`.

<!-- e.g. - @chili: 2 servings -->
