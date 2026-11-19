---
name: explore
description: Understand a Flutter project and make sure it has a clear CLAUDE.md before any planning starts.
disable-model-invocation: true
---

Understand this Flutter project, then make sure it has a clear `CLAUDE.md`.
Do not edit any file until step 7.

## 1. Read what exists

Read these, and say which ones you read:
- `pubspec.yaml`
- the `lib/` tree, two levels deep
- the project `CLAUDE.md`, if there is one

If there is no `pubspec.yaml`, this is not a Flutter project root. Say so and stop.

## 2. Detect before asking

Infer from `pubspec.yaml`, file names, and folder names:
- state management
- routing
- dependency injection
- networking
- local storage
- testing setup
- code generation

Never ask a question the code already answers.

## 3. Report

Give a short list of what you found. Mark each item as found or uncertain.
For anything uncertain, say what you looked at and why it was not enough.

## 4. Check the CLAUDE.md

Ask questions only if `CLAUDE.md` is missing, unclear, or still has `TODO` lines.
If it is complete, say so, report, and stop here. Do not ask anything.

Ask about the gaps only. Typical gaps:
- state management and how state is structured
- layers and folder structure (repository, data, UI, routing)
- UI conventions (theme, design system, shared widgets)
- navigation flow and main screens
- networking, auth, and local storage
- testing expectations
- flavors, environments, code generation

## 5. Ask in small batches

- At most four questions at a time.
- Put the detected value first as the default option when there is one.
- Allow a free-text answer.
- Wait for the answers before the next batch.

## 6. Draft the CLAUDE.md

Use the template at `${CLAUDE_PLUGIN_ROOT}/templates/CLAUDE.md`. A plugin cannot ship
a `CLAUDE.md` that loads by itself, so the template is a starting point you fill in.

- Fill it with the user's answers and the facts you detected.
- Never invent a stack detail. Anything unknown stays as `TODO`.
- Show the full draft.
- List every `TODO` that is left.

## 7. Write only after confirmation

Ask the user to confirm the draft. Write `CLAUDE.md` in the project root only after
they say yes. If a `CLAUDE.md` already exists, say what will change before overwriting it.

When done, tell the user the next step is `/flutter-ai-workflow:plan`.
