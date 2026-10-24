# Reviewing the skill

Homefront's skill changes as you use it. Rules pile up, and a new step can quietly contradict an
old one. After any change to the procedure (the files in `.claude/skills/todays-todos/`), open a
**new** Claude Code session in this folder and paste the prompt below. Fix what it finds in a
separate step, once you've read the report.

---

Review the Homefront skill for internal consistency. Read every file in
`.claude/skills/todays-todos/`, `CLAUDE.md`, and every file in `tracker/`. Don't edit anything;
report.

Look for:

1. **Contradictions** - two instructions that can't both be followed, within the procedure, within
   `rules.md`, or between them. Quote both.
2. **Dangling references** - a step that names a section, field, mode, file or rule that doesn't
   exist, or exists under a different name.
3. **One fact, two homes** - the same fact or rule written in more than one place, especially a
   personal preference written into the procedure instead of `rules.md`. Check the data too.
   Which file and section owns each kind of fact is defined in the "Each fact has one home" line
   of `CLAUDE.md` and in each tracker file's own header (including any sections or fields added
   since setup, like a new inventory) - take ownership from there, not from this prompt. Typical
   drift:
   - a row that restates what its thread record already says - deadline, background, contacts;
   - a field that's meant to hold one kind of fact carrying another, like a date inside `next:`
     or a next step inside a `priority.md` label;
   - a count, date or status written outside the section that owns it, like an inventory count
     on a meal record or a dinner's night in `meals.md`;
   - a rule written on a record or a row instead of in `rules.md`;
   - a reference with nothing behind it, like a `#slug` or `@key` with no record, and a thread
     with no row in `schedule.md` and no `no row:` field saying why;
   - a file or section holding data that no ownership line covers - flag it, since it means the
     ownership list needs updating.
4. **Dead rules** - a rule in `rules.md` the procedure never reads, or a procedure branch that no
   rule can turn on.
5. **Gaps** - a situation the procedure obviously meets (a missed day, an empty week, a new month,
   a field left `?`) where it doesn't say what to do.
6. **Ambiguity** - an instruction two careful readers would follow differently.
7. **Against the goals** - a step or rule that works against "What this is for" at the top of
   `SKILL.md`: one that lets something slip, shows more than is needed, overloads a day, decides
   for the user, or makes the check-in heavier than it needs to be.
8. **Unlogged changes** - if this folder is a git repository, compare the skill files' history
   (`git log -p -- .claude/skills/todays-todos/`) against `LOCAL-CHANGES.md`, and list any change
   to the procedure that has no entry there.

For each finding: the file and line, a quote, what goes wrong in a concrete scenario, and a
suggested fix. Rank by how likely it is to cause a wrong check-in. Say so plainly if you find
nothing in a category.
