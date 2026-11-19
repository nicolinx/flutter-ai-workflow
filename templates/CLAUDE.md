# Flutter project: working agreement for Claude Code

This file is the contract between me and Claude Code in this project.
Read it at the start of every session. If something here conflicts with a
direct instruction from me in chat, follow the chat and tell me about the
conflict.

Lines marked `TODO` are project-specific. Fill them in once per project.

---

## 1. Project

- App: TODO (name, one-line purpose)
- Platforms: TODO (Android / iOS / web)
- Flutter / Dart version: TODO (check `pubspec.yaml` and `.fvmrc` if present)

## 2. Stack and architecture

- State management: TODO (BLoC / Cubit / Riverpod / Provider)
- Dependency injection: TODO
- Routing: TODO
- Networking: TODO
- Local storage: TODO

Layers, in the order I build them:

1. **Repository**: the interface the rest of the app talks to.
2. **Data layer**: models and data sources behind the repository.
3. **UI layer**: screens, widgets, and their state.
4. **Routing and wiring**: navigation, dependency injection, hooking screens together.

Folder layout: TODO (paste the real `lib/` tree here, two levels deep).

Before adding code, look at an existing feature that already follows this
structure and match it. Do not introduce a new pattern without asking.

## 3. Commands

```bash
flutter pub get
dart format .
flutter analyze
flutter test
flutter run            # only when I ask, I run the app myself unless told otherwise
```

TODO: add project-specific commands (code generation, flavors, etc.).

## 4. How we work (the important part)

### 4.1 Explore first
Before proposing anything, read the code that the task touches. Say which
files you read. Do not guess at structure you have not looked at.

### 4.2 Plan before code
For anything bigger than a small fix, write a plan and **stop for my
approval** before changing any file.

The plan is staged, one layer per stage:

1. Repository
2. Data layer
3. UI layer
4. Routing and wiring

For each stage list: the files you will create or change, and what done
looks like. Skip a stage if the task does not need it, and say so.

Small changes (a bug fix, one field, a color) do not need a staged plan.
Say "small change, no plan" and do it.

### 4.3 Logic before UI
Build in plan order. Logic (repository, data, state) comes before UI.
There is no separate scaffold step: write the real logic directly, do not
create empty placeholder files first.

### 4.4 Verify after every stage
After each stage, run `dart format`, `flutter analyze`, and `flutter test`.
Report the result in plain words.

- If something fails, fix it before starting the next stage.
- If you cannot fix it, stop and tell me what is failing. Do not move on.
- Never delete or weaken a test to make it pass.

### 4.5 Stay inside the plan
Only touch files named in the approved plan. If you find you need another
file, stop and tell me why, then wait.

### 4.6 Review before commit
When all stages are done, summarize the diff: what changed per layer, and
anything you are unsure about. I read the diff myself before committing.

### 4.7 Commits
- Small commits, one per stage when it makes sense.
- Conventional Commit format: `feat(scope): short summary`.
- **Do not add `Co-Authored-By` or any Claude/AI attribution line** to
  commit messages or PR descriptions.
- Do not commit or push unless I ask.

## 5. Code conventions

Dart and Flutter:
- Follow the Dart style guide for naming (`lowerCamelCase`, `UpperCamelCase`,
  `snake_case` file names).
- Prefer `const` constructors and `const` widgets wherever possible.
- Prefer small widget classes over helper methods that return widgets.
- No disk or network I/O inside `build()`. Do it outside and keep the result in memory.
- Long lists use `ListView.builder` (or a sliver equivalent), not a built `Column` of children.
- Be careful with `ClipRRect`, `Opacity`, and other widgets that trigger
  `saveLayer()` on large or frequently rebuilt areas. Mention it if a plan uses them.
- Handle loading, error, and empty states for every screen that loads data.

These are defaults, not laws. If one of them gets in the way of the task,
tell me instead of silently ignoring it.

General:
- No dead code, no commented-out blocks, no leftover debug prints.
- Keep functions short and names plain.
- Add a comment only when the reason is not obvious from the code.

## 6. Tests

- New logic (repository, data, state) comes with unit tests.
- Widget tests for screens only when I ask, or when the screen has real branching.
- Test names say what is being verified, not how.

## 7. Do not

- Do not add or upgrade packages without asking. Say which package and why.
- Do not invent API endpoints, response shapes, or field names. Ask me or
  read the real source.
- Do not edit generated files (`*.g.dart`, `*.freezed.dart`). Change the
  source and re-run code generation.
- Do not put secrets, keys, or tokens in code or in this file.
- Do not refactor unrelated code while doing a task. Note it and ask.

## 8. Communication

- Be direct and short. Say what you did and what you found.
- If a step is ambiguous, ask one specific question instead of guessing.
- If you are not sure something works, say so. Do not report success you did not check.
- When you finish a stage, end with: what changed, what you verified, and what is next.

## 9. Definition of done

A task is done when:
- [ ] The approved plan is fully implemented, with no extra files touched.
- [ ] `dart format`, `flutter analyze`, and `flutter test` all pass.
- [ ] Loading, error, and empty states are handled.
- [ ] The diff summary is written and I have read it.
- [ ] Nothing is committed unless I asked.
