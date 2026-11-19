---
name: review
description: Review the uncommitted diff against the approved plan and the project CLAUDE.md, and report findings only.
disable-model-invocation: true
---

Review the uncommitted changes and report findings. Do not edit any file or rewrite
code unless the user asks.

## 1. Read the inputs

- Read `CLAUDE.md` in the project root. If it is missing, say so and tell the user to
  run `/flutter-ai-workflow:explore`.
- Find the approved plan in this conversation. If there is none, say so and review
  against `CLAUDE.md` only. Do not make up a plan.
- Get the uncommitted changes with these commands. Run each one as its own call. Do not
  chain them with `&&` or `;`, and do not wrap them in loops or variables.
  1. `git status --short` to list changed and untracked files.
  2. `git diff HEAD` for the staged and unstaged changes to tracked files.
  3. Read each untracked file with the Read tool.
- If a git command fails or is blocked, say which one, and say that part of the diff
  was not reviewed. Do not describe output you did not see. Do not assume that no tracked
  file changed.

If there is no diff, say so and stop.

## 2. Check these four things

1. **Scope creep.** Files or changes the plan does not name. Name each one.
2. **Missing states.** Screens that load data without a loading, error, or empty state.
3. **Convention breaks.** Anything that goes against the project `CLAUDE.md`,
   including its code conventions and its "do not" list.
4. **Missing tests.** New logic (repository, data, state) with no test, or a test
   that was deleted or weakened.

Check only what the diff shows. Do not review code that did not change.

## 3. Report

Group findings under the four headings above. For each finding give:
- the file and line
- what is wrong, in one sentence
- which plan stage or `CLAUDE.md` rule it breaks

Say "none found" under a heading with no findings. Do not pad the list.
Mark anything you could not confirm as uncertain, and say what you would need to check.

End with one line: what you reviewed, and what you did not (for example, "did not run tests").
