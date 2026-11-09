# Run shapes

How to run the work once the spec and check are set. Pick the simplest shape that fits. Every run that nobody watches states its limit and its finish signal.

## Single pass

Default. Do the work, check it against the criteria, hand it over. Use for anything a person will review straight after.

## Goal loop (`/goal`)

Claude keeps working until a condition is met. After each turn a separate small model judges the condition from the conversation alone. It can't run commands or read files.

- Write the condition as something Claude's output will show: "the test command prints 0 failures", "the summary lists all four items, each with a quote".
- Always add a turn limit: "or stop after 20 turns".
- Good for: fix-until-green, iterate-until-the-rubric-passes, research until every question has a sourced answer.
- Not for: done conditions that live outside the conversation (a deploy, an inbox), unless Claude prints the evidence each turn.
- Needs hooks enabled. `/goal` alone shows status, `/goal clear` removes it.

## Timed loop (`/loop`)

Runs a prompt again on an interval while the session stays open.

- `/loop 10m [prompt]` for a fixed interval, or `/loop [prompt]` to let Claude pace itself.
- Good for: watching a build, a queue, a deploy, a metric.
- Session-bound: it stops when the session ends and expires after 7 days. Not a scheduler for long-term jobs. Those belong in the project's scheduler.

## Parallel subagents

Several agents work side by side and report back.

- Good for: independent reads, research across separate sources, reviewing separate files.
- Not for: single-file edits, steps that depend on each other, anything a direct search answers faster.
- Brief each one fully: the goal, what's already ruled out, where to look, what to return. They know only what the brief says.
- Their reports are claims. Check them before acting.

## Filter, then delegate

For a big pile of data or items: emails, logs, rows, pages, search results.

1. Filter with code first: spam, duplicates and anything a pattern can catch. Code is cheap and repeatable. Keep source links with each item, and check a sample of what the filter dropped.
2. Hand what's left to subagents in batches. It doesn't need to be neatly formatted.
3. One orchestrator splits the work, checks a sample of the results and writes the summary.

Which model plays each part, and at what effort, is set by the project's model routing rules. This shape never picks models or effort itself.

## Background session

A full session that keeps working without anyone watching.

- Good for: long, self-contained work that ends in something you can review (a branch, a report, a set of files).
- State a time or turn limit, the owner who reviews it, and what done looks like.
- Not for: anything that sends, posts, pays or contacts a person without approval.

## Train loop (recurring jobs only)

Makes a job reliable before it runs unwatched.

1. Run it 3 to 5 times on real inputs.
2. Grade each run against the pass criteria and the gold example.
3. Compile one fix list across all runs.
4. Fold the fixes into the skill or SOP.
5. Re-run until the criteria pass twice in a row.

Output: the improved skill or SOP, plus the inputs used as test cases. Not for one-off work.

## Existing skills and agents

Before building a method from scratch, check what the project already has. If a skill or agent covers the job, name it in the plan and call it, rather than re-describing its method.

## Model per worker

Every worker's brief names a model line. Name a model only when the work clearly calls for one. Otherwise the project's routing rules decide, and when they are unsure the worker keeps the session's own model. Never downgrade by default.

| Work | Tier |
|---|---|
| Lookups, file reads, simple extraction | Fast, cheap model |
| Routine builds, research synthesis, queue work | Everyday model |
| Judgement, plans, architecture | Top model |

## Unattended hand-off

The brief lists the pass criteria as `done_when:` lines. The run reports DONE only when each one passes, and says which did not when it reports anything else.

## Depth and cost

| Depth | Shapes allowed |
|---|---|
| Quick | Single pass |
| Standard | Single pass, goal loop with a tight limit, filter then delegate, existing skills |
| Deep | Any, including the train loop. Token spend is accepted. Limits and finish signals are still stated |
