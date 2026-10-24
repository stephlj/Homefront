# Local changes

Changes made to this copy of the skill's own files - the procedure, not `tracker/rules.md`.
Claude adds an entry whenever **Make a change** makes a kind 3 change (a new behavior).

This log is what lets an update to Homefront carry your changes over: instead of trying to work
out from two versions of the text what you changed and why, an update reapplies each entry below
onto the new version, and asks you about any that collide with what changed upstream.

`VERSION` in this folder says which published version this copy started from. Don't edit it by
hand; an update sets it.

Format, newest first (example inside a fence, so it's never read as a real entry):

```
## 2026-11-03 - Weekly budget total in meal plans
Files:   meals-mode.md (Plan step 4, Propose)
Change:  Each proposed plan shows the week's estimated total from `cost:`, and the budget
         rule's check, under the per-night lines.
Why:     "I want to see what the week costs before I pick a plan." [stated]
Related: rules.md › Meals › Soft rules (weekly budget); meals.md Fields (budget group: `cost:`)
```

Each entry says where the change landed, what it does, and why in the user's words. The why is
the part that matters most: if an update has rewritten that step, Claude needs it to rebuild the
change in the new version.

## Changes

None yet.
