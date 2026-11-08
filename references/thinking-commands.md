# Thinking commands

Six ways to stress-test or explain thinking. Each says when to use it, the prompt to run, and what to hand back. Use the method, not just the label: a red team that only lists generic risks has not been run.

| Command | Use when | Skip when |
|---|---|---|
| ELI5 | The user is learning a hard topic, or asks | They already know the field |
| Manual English | Steps, runbooks or explanations that must be read fast and followed exactly | Persuasive or creative writing |
| Pre-mortem | Any plan before it starts. Always at Deep depth | One-step tasks at Quick depth |
| Steelman | Choosing between options or views | There's only one sensible option |
| Red team | A plan, pitch, spec or decision before it ships. Always at Deep depth | Throwaway work at Quick depth |
| First principles | Strategy, pricing, a new approach, "everyone does it this way" | Following a known, proven recipe |

## ELI5

Explain it like the reader is five: plain words, one idea at a time, an everyday comparison for each hard part.

Prompt: "Explain [topic] like I'm five. Use short words, one idea per line, and one everyday example for each hard part. Then give the three things I need to know to act on it."

Hand back: the plain explanation, then what to do with it. Check: could someone new to the topic repeat it back?

## Manual English

Write in the style of an aircraft repair manual. This borrows the main writing rules of Simplified Technical English (ASD-STE100). It does not check text against that standard's approved word list, so don't call the result compliant.

Prompt: "Explain [topic] in manual-style plain English. One instruction or fact per sentence. Instructions: 20 words or fewer. Descriptions: 25 words or fewer. Use the active voice and the same word for the same thing every time. Use common words with one clear meaning. No idioms, no filler."

Hand back: numbered steps or short facts. Check: a reader can act on each line without rereading it.

## Pre-mortem

Pretend it's six months from now and the plan failed. Work backwards to why.

Prompt: "It's six months from now and [plan] failed. List the most likely reasons, most likely first. For each: the early warning sign we'd see, and what we change in the plan today to prevent it."

Hand back: the top 3-5 causes, each with a warning sign and a plan change. Fold the changes into the plan. Don't just list them.

## Steelman

Argue the strongest version of the other side, the one a smart person who disagrees would make.

Prompt: "Steelman the case against [position]. Make the best argument a smart opponent would make, using real evidence where it exists. Then say what would have to be true for them to be right."

Hand back: the strongest opposing case, and the test that would settle it. Check: would someone on that side say it's fair?

## Red team

Attack the work from every angle a sceptic would use.

Prompt: "Red team [plan/pitch/spec]. Attack it as a sceptical customer, a competitor, a short seller, an auditor and the person who has to build it. Rank the weaknesses by how much damage they'd do. For each: why it matters and the fix."

Hand back: ranked weaknesses with fixes, then the 3 changes that matter most. At Deep depth, run it with a different model where the project's routing allows, otherwise in a fresh subagent, and say which.

## First principles

Strip the problem back to what's actually true, then rebuild from there instead of copying what's normal.

Prompt: "Break [problem] down to first principles. List what we know is true (facts and hard limits), what we're only assuming, and what everyone in the field does by habit. Then build the answer from the facts alone and show where it differs from the usual approach."

Hand back: facts vs assumptions vs habits, the rebuilt answer, and where it breaks from the norm. Check: each fact can be sourced.
