---
name: verify
description: Run the format, analyze, and test commands listed in the project CLAUDE.md and report the results plainly.
disable-model-invocation: true
---

Run the project's own checks and report what happened. Do not change code to fix a failure.

## 1. Read the project CLAUDE.md

Read `CLAUDE.md` in the project root and find its commands section.

If `CLAUDE.md` is missing, or it lists no format, analyze, or test command, say so and
stop. Tell the user to run `/flutter-ai-workflow:explore`. Do not guess the commands.

## 2. Run the commands

Run the format, analyze, and test commands exactly as `CLAUDE.md` lists them, in that order.
Do not run any other command, such as `flutter run`, unless the user asks.

If a command rewrites files (format usually does), list which files changed.

## 3. On a failure

Stop at the first command that fails. Do not run the ones after it.

Report:
- which command failed
- the exact error lines, not a summary
- which files or tests are named in the output

Never weaken, skip, or delete a test to make it pass. Never edit code unless the user asks.

## 4. Report

State plainly, per command: passed, failed, or not run.
If everything passed, say so in one line. Do not claim more than the commands showed.
If a command was not run, say that and why.
