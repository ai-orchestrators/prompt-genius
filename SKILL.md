---
name: prompt-genius
description: Sharpen this, what's the real goal, make this prompt better, write a spec for, what does done look like. Turns a known need with a fuzzy outcome into a sharp spec or prompt. Asks how deep to go, interviews for the real goal (deeper in plan mode), raises the ask, sets pass criteria tied to real evidence, and picks how to run it (single pass, goal loop, loop, subagents, background session, existing skills). Also use for /prompt-genius, "tighten this brief", "what are we actually trying to get", "aim higher", "go deeper on this", "turn this into a proper prompt", "this is vague", "help me get the outcome right", or a plan-mode build whose outcome is not yet measurable. Not for open exploration when you don't yet know what you need ("brainstorm", "I don't know what I want"), fixing a prompt that used to work and stopped ("this prompt stopped working"), ranking a list of ideas, or a high-stakes decision that needs several advisors.
---

# Prompt Genius

Most work goes wrong before it starts: the ask is the size of the task, not the size of the outcome, and nobody said what done looks like. This skill fixes the ask first, then the spec, then the check, then how it runs.

The project's own CLAUDE.md wins on models, agents, routing and approvals. If you add your own `references/local-routing.md` (your models, reviewers and tools), read it before step 7.

## 1. Read the mode

| Signal | Mode |
|---|---|
| A plan-mode notice is in context | **Plan**: recommend Deep and always include a run-shape section. The user's depth answer still governs |
| Anything else | **Normal**: stay light, skip what the task doesn't need |
| No person can answer (headless run, scheduled task, subagent) | **Unattended**: never ask, and skip this skill's own questions and play-backs. Keep the outcome exactly as asked and write each assumption down. The project's approval rules still apply: anything hard to reverse, or that reaches a person, stops and is reported. Missing access stops that step and is reported |

## 2. Pin the outcome

Use the AskUserQuestion tool whenever a person is there. Put your recommended answer first in every question.

**First question, every time: "How deep should this go?"**

| Option | What it buys |
|---|---|
| Quick | One question round, no second opinion, one-line run shape |
| Standard | One question round, a check against the criteria before hand-over |
| Deep (tokens are fine) | Rounds until the outcome is clear, a pre-mortem and second-model red team, a full run plan |

Recommend Deep in plan mode and Standard otherwise. The user's answer governs everything after it.

Then ask about the outcome, not the task. Skip anything the context already answers.

- **Quick or Standard:** one round, up to 4 questions. Good ones: what happens after this is done, who uses it and for what decision, what would make it a clear win, what's off limits.
- **Deep:** keep going a level deeper each round ("and what does that get you?"). Follow `references/deep-outcome.md`. Stop at 5 rounds, or sooner when the next decision is clear, the outcome has a number that matters, or the user says go.
- **Play back the key decisions** in one short list and get an explicit yes before building on them.
- **When the goal is understanding something**, ask how they want it shown: text, a diagram, an HTML page or a short explainer video. A picture is often faster to take in than a thousand words. Use the tools the project has for each. Without one, use a text diagram and say what's missing.

## 3. Find the bar

Before writing the spec, find one gold example of good: the user's own past version, an approved output (a sent email, a published post, a merged change), or a page in the project's knowledge base. Past decisions, meeting notes and the code are the places to look.

- Found one: the pass criteria reference it ("matches the example on X and Y").
- None found and the job will recur: recommend the user does it once by hand first. That run becomes the example.
- None found and the job is one-off: say so and set criteria without it.

## 4. Raise the ask

Use only the types that fit. Most tasks need one or two.

| Type | Question | Skip when |
|---|---|---|
| **Higher** | What's the outcome beyond the task? Fixing one bug vs keeping the feature bug-free | Creative work with no outcome beyond itself |
| **Deeper** | What would the best person in the field do, from what raw material, to what hard target? | Throwaway work |
| **Wider** | Which other fields would help, and what do their experts actually do (their method and tools, not their voice)? | A narrow fix with one right answer |

Better, not bigger: if raising the ask only adds deliverables, drop it. If the bigger outcome changes what the user asked for, ask first.

## 5. Write the spec

- Small chunks, each with a checkpoint the user can review. No big-bang delivery.
- Precise words. Every vague word is a guess Claude will make for the user.
- The why behind each rule, so the reader can handle cases the rule missed.
- The raw source (transcript, data, code, the customer's words), not a summary of it. Pull it from the project's knowledge base, past decisions, meeting notes or the code.
- Examples of good when style or format matters.

**Size it to the setup.** Where the project already gives Claude the context (CLAUDE.md, skills, access, past work), a short verb prompt works best: "update, deploy, secure, harden". Where it doesn't, write the fuller spec. More requirements are not better: they often contradict each other. A short prompt still ends with its pass criteria, even if that's one line or a pointer to the project's own checks.

When the output is a prompt for Claude or another model, write it with `references/prompt-craft.md`, and finish with its cut pass.

## 6. Set the check

Write the pass criteria before any work starts.

- If step 3 found a gold example, at least one criterion points at it.
- Each criterion names evidence the worker can actually get: a test run, tool output, the live system, a quote from the source, a past example to match.
- Never pass work on its own say-so. A second model is an opinion, not proof.
- Where it's unclear a model or tool can do the job, build a small test of that exact skill first. Your own probe beats a public benchmark.
- Record the result against each criterion, misses included. A missed hard target with honest numbers is a good result. A faked pass is not.
- If the evidence needs access nobody has, name what's missing and what it would make possible.

## 7. Second opinion and routing

| Depth | Second opinion |
|---|---|
| Quick | None |
| Standard | Check the work against the criteria before handing it over. This is a criteria check, not an independent review |
| Deep | A different model red-teams the plan or spec before work starts. Follow the project's routing rules for which model. If none is available, run the red team in a fresh subagent and say no second model was used |

## 8. Pick the run shape

Quick or Standard: one line, only when it isn't a single pass. Deep or plan mode: always a short section. Pick only shapes the chosen depth allows (table at the end of `references/run-shapes.md`). Every unattended run states its limit and its finish signal.

| Situation | Shape |
|---|---|
| Done is something Claude's output can show | Goal loop: `/goal` with the condition, plus a turn limit |
| Something needs checking over time | `/loop` with an interval |
| Independent reads or research that can run side by side | Parallel subagents |
| A big pile of data or items to process | Filter with code first, then hand what's left to subagents |
| Long work nobody needs to watch | Background session, with a time limit and an owner |
| The job recurs and the output is not yet reliable | Train loop: run it on real inputs, grade, fix, repeat |
| An existing skill or agent already does the job | Name it and call it |
| Anything else | Single pass |

Every worker (subagent, background session, loop run) gets a model line in its brief. Name a model only when the work clearly calls for one. Otherwise the project's routing rules decide, and when unsure the worker keeps the session's own model. Never downgrade by default. Generic tiers: fast cheap model for lookups and file reads, everyday model for routine builds and research synthesis, top model for judgement, plans and architecture.

Deep depth means token spend is accepted. Limits are still stated.

## 9. Stress-test the thinking

Pick from `references/thinking-commands.md`.

| When | Command |
|---|---|
| Deep depth, always | Pre-mortem, then red team |
| A decision between options | Steelman the side you're leaning against |
| Strategy, pricing, a new approach | First principles |
| The user is learning or asks | ELI5 |
| Instructions or explanations that must be read fast and exactly | Manual English |

## 10. Make it repeatable (one line, only if it applies)

- The job will come back: suggest saving it as a skill or memory, and save the spec, criteria and gold example as a bench case where the project keeps its test cases.
- A rule can never be broken: suggest a hook, not a CLAUDE.md line.

## 11. Deliver what was asked

| Asked for | Deliver |
|---|---|
| A task to do | The outcome line, the check, then do the work at the agreed depth |
| An unattended hand-off (a background session, a queued run) | The brief lists the pass criteria as `done_when:` lines. The run reports DONE only when each one passes |
| A plan | The plan, then stop for approval |
| A prompt to improve or write | The prompt in one fenced block, ending with its pass criteria, then stop |
| An explanation | In the format they chose: text, diagram, HTML page or explainer video |

Output order, scaled to the task: the real outcome in one line, the types used, the spec, the check, the run shape, what's missing, then the deliverable. A one-line ask gets a few lines, not a report.
