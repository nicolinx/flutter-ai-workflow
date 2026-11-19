---
name: plan
description: Write a staged plan for a Flutter task, one stage per layer, and stop for approval before any code changes.
disable-model-invocation: true
---

Write a staged plan for the task the user gave. Do not edit any file.

## 1. Read the project CLAUDE.md

Read `CLAUDE.md` in the project root.

If it is missing, or the stack section (state management, dependency injection,
routing, networking, local storage) still has `TODO` lines:
1. Tell the user which one it is.
2. Ask: "CLAUDE.md is missing or incomplete. Run explore now?" Do not run it before they answer.
3. If they say yes, invoke the `explore` skill (`/flutter-ai-workflow:explore`).
4. If that call is blocked, stop and tell the user to type
   `/flutter-ai-workflow:explore` themselves, then run plan again.

Do not plan on top of a stack you have to guess.

## 2. Read the code the task touches

Find an existing feature that already follows the project structure. Say which
files you read. Match that structure. Do not introduce a new pattern without asking.

If the task is unclear, ask one specific question before planning.

## 3. Write the plan

One stage per layer, in this order:
1. Repository
2. Data layer
3. UI layer
4. Routing and wiring

For each stage list:
- the files to create or change
- what done looks like

Skip a stage the task does not need, and say so in one line.
Use the real names, folders, and packages from the project, never a default stack.
Do not add or upgrade packages in the plan without saying which one and why.

If the task is small (a bug fix, one field, a color), say "small change, no plan"
and name the one file.

## 4. List what is unsure

End with:
- anything you could not confirm from the code
- any file outside the stages that you think the task may need

## 5. Stop

Ask the user to approve the plan. Do not create or edit any file until they do.
If they ask for changes, update the plan and ask again.
