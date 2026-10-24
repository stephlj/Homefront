# Threads

One record per ongoing effort, keyed by a `#slug`. Holds the background, the real deadline, the
next step and the history.

A `#slug` in [[schedule]] or [[priority]] is a pointer to a record here, never a copy of it.
Deadlines live here; the dates in [[schedule]] are when the work is planned, which is a different
fact. Priority order lives in [[priority]]. How things get scheduled lives in [[rules]].

A record can add other fields where it needs them, but these are the ones the check-in reads, and
they need these exact names:

- `deadline:` - printed beside every row that carries the slug, so the rows don't repeat it.
- `next:` - the next step, never the date it's planned for. That date lives on the row.
- `surface:` / `cadence:` - when the thread is due to come up, even with no row scheduled.
- `days:` / `timing:` / `contact:` - which days and hours the work can actually happen.
- `open q:` - something unresolved that makes the planned task possibly wrong. The check-in
  raises it before the task. Anything uncertain belongs here; left in conversation it's lost.
- `research:` - findings from a lookup still useful once its `open q:` is settled.
- `last done:` - for a recurring chore, when the last cycle finished.
- `done when:` - what finishes the thread; `never` for a recurring one.
- `no row:` - why the thread deliberately has no row in [[schedule]].
- `history:` - dated entries: what happened, and what changed on the record.
- `related:` / `note:` - pointers to other threads, and context that fits no other field.
- `part <name>:` - for a thread made of several efforts, one field per part: its background,
  then its own `Next:`, `Done when:` and, if it has one, `Deadline:`, inline. A part with no
  deadline takes the thread's. The thread then has no `next:` of its own.

Examples (inside a fence, so they're never read as real data):

```
### #gutters - clean the gutters before the rains [stated]
deadline:  end of October - rainy season starts in November
next:      get two quotes
contact:   cleaners answer weekdays 8-5 only
open q:    is the downspout by the garage clogged too, or just the gutter?
done when: gutters cleaned and downspouts flowing
history:   2026-03-02 added after the overflow in the last storm.

### #smoke-alarms - test smoke alarms, replace batteries [stated]
cadence:   twice a year, spring and fall
last done: 2026-03-10
next:      test every alarm and replace batteries
done when: never - recurring. After each check, schedule the next ~6 months out.

### #garage - garage cleanout [stated]
part sort:    shelves into keep / donate / toss. Next: sort the shelves. Done when: every shelf sorted.
part donate:  drop-off at the donation center. Next: book a drop-off. Done when: dropped off.
              Deadline: before the spring yard sale.
done when:    both parts done
```

## Threads
