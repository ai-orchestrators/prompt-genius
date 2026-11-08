# Prompt craft

How to write the final prompt when the deliverable is a prompt for Claude or another model. Based on Anthropic's prompting guidance for current Claude models.

## Contents

- Clarity
- Structure
- Examples
- Action and tools
- Thinking and checking
- Agents and long runs
- Before you hand it over

## Clarity

- **Colleague test.** Would a smart new hire with no context follow it? If they'd be confused, so will the model.
- **Give the why.** "Read aloud by a speech engine, so no ellipses" beats "NEVER use ellipses". The model generalises from the reason.
- **Say what to do, not what not to do.** "Write in flowing paragraphs" beats "don't use markdown".
- **Ask for more when you want more.** If the work should go beyond the basics, say so. Current models do what's asked, literally.
- **Calm wording.** Shouting (CRITICAL, YOU MUST) makes current models overreact. "Use this tool when..." is enough.
- **Numbered steps only when order or completeness matters.**

## Structure

- **XML tags** separate instructions, context, examples and inputs: `instructions`, `context`, `example`, `input`. Keep tag names consistent. Nest when content has a natural hierarchy.
- **Long material first, the question last.** Put documents near the top, wrapped with their source, and the ask at the end.
- **Ground in quotes.** For long documents, have the model pull the relevant quotes first, then answer from them.
- **Match the prompt's style to the output wanted.** A markdown-heavy prompt gets markdown back.
- **One-line role plus its method.** "You're a financial analyst: compare each line to last year and flag changes over 15%" beats a long persona.

## Examples

- When output style or format matters, add 3-5 examples, wrapped in example tags. Skip them when it doesn't.
- Relevant (close to the real use), varied (cover edge cases so the model doesn't copy one pattern), and clearly separated from instructions.
- For reasoning tasks, show the problem, the method and the answer.

## Action and tools

- **Explicit verbs.** "Change this function" gets changes. "Can you suggest changes" gets suggestions.
- **Parallel where independent.** Ask for independent tool calls in parallel, dependent ones in order.
- **Confirm before hard-to-reverse actions**: deletes, force pushes, anything sent, posted or shared.
- **Investigate before answering.** Read the file before making claims about it.

## Thinking and checking

- **General guidance over scripted steps.** "Think it through carefully" usually beats a hand-written reasoning recipe. Control depth with the effort setting, not "think step by step".
- **Commit to an approach.** Pick one, see it through, change course only on new information.
- **Self-check line**: "Before you finish, check your answer against [criteria]." Leave it out for models whose notes say they over-verify.
- **Pass criteria at the end of every prompt**, tied to evidence the model can actually produce.

## Agents and long runs

- **Subagents** only for parallel, isolated or independent work. Work directly for simple or sequential tasks.
- **State on disk.** Tests in a structured file, progress notes in plain text, git as the record. A fresh session can pick up from those.
- **Minimal solutions.** Only what's asked or clearly needed. No extra files, abstractions or features.
- **Real fixes, not test-passing.** Solve the general problem. Don't hardcode to the tests. If a test is wrong, say so.
- **Research**: clear success criteria, several sources, competing hypotheses, confidence noted as it goes.

## Before you hand it over

**Cut pass.** Read the prompt as the model will. Remove any rule that conflicts with another, repeats another, or covers a case that can't happen. Keep the fewest words that still hold the outcome, the context and the check. Short prompts often beat long ones, because every extra requirement is a chance to contradict an earlier one.

- Check the target model's own prompting notes for known differences.
- Run the colleague test once more.
- The prompt ends with what done looks like and how it will be checked.
