# NYC 26.2 — an adaptive marathon training plan

A single-file training plan for the 2026 NYC Marathon (Nov 1) that **re-plans itself from what you actually run**, not what you were supposed to run.

**Live:** https://YOUR-USERNAME.github.io/nyc-26.2/

No build step, no dependencies, no backend. One HTML file. Data lives in your browser (localStorage), with JSON export/import.

## Why

Static training plans fail at the first missed week, and most runners respond by either cramming (injury) or abandoning the plan (drift). The interesting part of coaching isn't the initial schedule — it's the *re-planning* after reality intervenes. This encodes that re-planning as a small set of deterministic rules.

The starting point is me: ~20-25 mpw base, a 1:45 half a year ago but detrained since, 16-17 weeks to race day. The generated plan builds from 20 to a ~43-mile peak week with an 18–20 mile long run three weeks out, then a three-week taper.

## The adaptation rules

Every run gets logged with miles, RPE (1–10), and an ache flag. The engine walks the completed weeks and rebuilds every future week from the latest evidence:

| Signal (per completed week) | Response |
|---|---|
| ≥85% of volume completed, normal RPE | Progress as planned (+8%/week, cap 44 mpw) |
| 60–85% completed | **Hold** — repeat the volume, don't progress |
| <60% completed | **Step back** ~10% and rebuild |
| Avg RPE ≥7.5 on easy runs, or 2+ ache flags | **Cutback week inserted** (−25%), ramp restarts gently |
| Long run missed | Next long run **repeats the missed distance** — it doesn't disappear, and you don't skip ahead |

Two rules apply regardless:

- **Legs over lungs.** The long run can never exceed the longest run of the last three weeks by more than ~2.5 miles, no matter what the schedule says. Aerobic fitness returns faster than tendon durability; this is the guard against your cardio writing checks your calves can't cash.
- **The taper is sacred.** The final three weeks only ever adjust downward. No fitness is gained in the last 14 days — it can only be lost or defended.

Mid-week, the engine reacts immediately to *strain* signals (repeated aches, RPE 8+ runs, a missed long run) but never to incomplete volume — you shouldn't be penalized on Wednesday for runs scheduled Saturday.

If the adapted trajectory can no longer reach a 16-mile peak long run, the app says so plainly and reframes the goal (finish strong, planned run-walk from mile 16) rather than pretending.

## Design notes

- The **season arc** chart shows three layers per week: the original plan (ghost), the current adapted plan (cobalt outline), and actual miles (ink). Watching the cobalt bars re-draw after a bad week is the whole product.
- Pace references derive from your half-marathon time via Riegel (exponent 1.06), but every run is prescribed by RPE first — pace is a sanity check, not a target.
- Everything is deterministic and re-derivable from the log: delete nothing, trust the replay.

## Run it

Open `index.html`. That's it. To publish: push to a repo, enable GitHub Pages (Settings → Pages → deploy from branch), done.

## Disclaimers

Not medical advice, not a coach. Rules are conservative heuristics from standard training-load principles (ACWR-adjacent thinking, 10%-rule lineage). Sharp pain means stop running and see a human.

---

Built by [Andy Robertson](https://arobertson.dev) while training for this exact race.
