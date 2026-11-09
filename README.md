# prompt-genius

A Claude Code skill for the moment you know you need something, but the outcome isn't nailed down yet. It pins the real goal, raises the ask, writes a spec Claude can act on, sets how "done" will be checked, and picks the right way to run the work.

## Why this exists

Most AI work goes wrong before it starts. The ask is the size of the task ("make a monthly report"), not the size of the outcome ("the board cuts the two weakest channels with confidence"). Nobody says what done looks like, so the model grades its own work and calls it finished.

prompt-genius fixes that in order:

1. **The outcome.** It asks how deep to go, then interviews you for the goal behind the goal.
2. **The bar.** It looks for one example of what good looks like before writing anything, and says so when there isn't one.
3. **The ask.** It aims higher, deeper or wider, but only where that makes the result better, not just bigger.
4. **The spec.** Small checkpoints, precise words, the raw source rather than a summary, sized to how much context your project already gives Claude.
5. **The check.** Pass criteria written before the work, each tied to evidence the worker can actually get. Never "looks good to me".
6. **The run.** Single pass, goal loop, timed loop, parallel subagents, filter-then-delegate, background session, or an existing skill.

## What makes it different

| Feature | What it does |
|---|---|
| **"How deep should this go?"** | Asked first whenever a person is there to answer. Quick, Standard or Deep. Your answer sets how many questions, whether a second model red-teams the plan, and how the run is shaped |
| **Plan-mode aware** | In Claude Code's plan mode it recommends Deep and always plans how the work will run. Pick Deep and it keeps interviewing, round by round, until the outcome is clear (5 rounds max) |
| **Doesn't wait on questions unattended** | In a headless run, scheduled task or subagent it asks nothing and skips its own play-backs. It keeps the outcome exactly as asked and writes its assumptions down. Your project's approval rules still apply, and missing access is reported, not guessed around |
| **Evidence-based checks** | Each pass criterion names a test run, tool output, live system, source quote or past example. A second model's opinion is not proof |
| **Delivers what you asked for** | Asked for a plan, you get a plan and it stops. Asked for a prompt, you get the prompt. Asked for a task, it does the work |
| **Visual explanations** | When the goal is understanding, it asks whether you want text, a diagram, an HTML page or an explainer video |
| **Prompt craft built in** | When the output is a prompt, it follows a checklist drawn from Anthropic's published prompting guidance and finishes with a cut pass that removes conflicting rules |

## What's in the box

```
prompt-genius/
├── SKILL.md                         the flow, as decision tables
└── references/
    ├── deep-outcome.md              the interview ladder and stop rules
    ├── thinking-commands.md         ELI5, Manual English, pre-mortem, steelman, red team, first principles
    ├── run-shapes.md                /goal, /loop, subagents, filter-then-delegate, background, train loop
    └── prompt-craft.md              how to write the final prompt, plus the cut pass
```

References load only when a step needs them, so the skill stays light in everyday use.

## Installation

### Claude Code (CLI or desktop app)

User-level (every project):

```bash
git clone https://github.com/ai-orchestrators/prompt-genius.git ~/.claude/skills/prompt-genius
```

Project-level:

```bash
git clone https://github.com/ai-orchestrators/prompt-genius.git .claude/skills/prompt-genius
```

### Verify

In Claude Code, type `/prompt-genius` followed by a fuzzy ask, for example:

```text
/prompt-genius I need a monthly report for the board
```

Claude should ask "How deep should this go?" before anything else.

## Trigger phrases

- `/prompt-genius`
- "sharpen this"
- "what's the real goal"
- "make this prompt better"
- "write a spec for"
- "what does done look like"
- "tighten this brief"
- "this is vague"
- "aim higher" / "go deeper on this"
- "turn this into a proper prompt"
- Any plan-mode build whose outcome isn't measurable yet

It stays out of the way for open brainstorming ("I don't know what I want"), for fixing a prompt that used to work, and for ranking lists of ideas.

## Example

You type:

```text
/prompt-genius write a spec for the new invoice page in our client portal
```

You get, in order:

1. **How deep should this go?** You pick Standard.
2. **One round of up to 4 questions:** what happens after an invoice is viewed, who uses the page and for what decision, what would make it a clear win, what's off limits.
3. **The outcome in one line**, played back for your yes: "Clients pay from the invoice page without emailing support."
4. **The bar:** the closest existing page in your codebase to match.
5. **The spec** in small checkpoints, each reviewable.
6. **The check**, for example: "pass = the end-to-end payment test against staging prints 0 failures, and the page passes the accessibility check".
7. **The run shape:** a goal loop with a turn limit, because Claude prints the test results each turn, so done is something the conversation itself shows.

## Make it yours

Everything in the skill follows your project's own `CLAUDE.md` on models, agents, routing and approvals. Unattended runs are stricter: anything hard to reverse, or that reaches a person, stops and is reported for a person to approve.

To give it your own setup (where your prompting guidance lives, which model reviews, which tools make diagrams or videos, which skills to prefer), add a file at `references/local-routing.md`. The skill reads it before step 1. Leave it out and the skill falls back to your project's rules.

## Requirements

- Claude Code, current release. Uses the AskUserQuestion tool, and optionally `/goal`, `/loop` and subagents.
- `/goal` needs hooks enabled.
- The skill itself ships no scripts, makes no network calls and has no dependencies.
- Optional, from your own setup: a second model or reviewer for Deep-depth red teams, and diagram, HTML or video tools for visual explanations. Without them the skill says what's missing and falls back: a text diagram instead of a visual, and a red team run in a fresh subagent (where your project allows one) or by Claude itself instead of by a second model, labelled as such.

## License

MIT
