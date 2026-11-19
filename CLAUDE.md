# flutter-ai-workflow: instructions for building this plugin

This repo is a Claude Code plugin. It holds the skills I use to work on
Flutter apps with Claude Code, so other people can follow the same workflow.
This file tells Claude how to build and change **the skills in this repo**.
It is not the CLAUDE.md of a Flutter app. That one is a template in
`templates/CLAUDE.md`.

Read this file at the start of every session in this repo.

---

## 1. What this plugin is for

Give a Flutter developer a repeatable way to work with Claude Code:

**Explore → Plan (staged, needs approval) → Implement (logic before UI) → Verify → Review → Commit**

The skills turn that flow into commands. They are meant to be shared, so they
must work in any Flutter project, not just mine.

Prime rule: **the skills must be honest and generic.** They read each
project's own `CLAUDE.md` and code. They never assume a stack.

## 2. Repo layout

```
flutter-ai-workflow/
├── CLAUDE.md                      this file
├── README.md                      what it is, the flow, how to install, "still v0.x"
├── CHANGELOG.md                   short, one line per change
├── .claude-plugin/
│   ├── plugin.json                manifest (and nothing else in this folder)
│   └── marketplace.json           so others can add this repo as a marketplace
├── skills/
│   ├── explore/SKILL.md
│   ├── plan/SKILL.md
│   ├── verify/SKILL.md
│   └── review/SKILL.md
└── templates/
    └── CLAUDE.md                  template users copy into their Flutter project
```

Rules for this layout:
- Only `plugin.json` and `marketplace.json` go inside `.claude-plugin/`.
  `skills/` lives at the repo root. A `skills/` folder inside `.claude-plugin/` does not load.
- One folder per skill, each with a `SKILL.md`.
- A plugin cannot ship a `CLAUDE.md` that loads automatically. The file in
  `templates/` is a template users copy. Skills must say so when they point to it.
- Do not add folders or tooling that the plugin does not need.

## 3. The skills

Skills are invoked as `/flutter-ai-workflow:<name>`.

### explore
Runs first, in any project. Purpose: understand the project and make sure
there is a clear `CLAUDE.md` before any planning happens.

Steps:
1. Read what exists: `pubspec.yaml`, the `lib/` tree (two levels), and the
   project `CLAUDE.md` if there is one.
2. **Detect from code before asking.** Infer from `pubspec.yaml` and file
   names: state management, routing, dependency injection, networking,
   local storage, testing setup, code generation. Never ask a question the
   code already answers.
3. Report what was found, in a short list, and say what is uncertain.
4. If `CLAUDE.md` is missing, or it is unclear or has unfilled `TODO` lines,
   ask clarifying questions about the gaps only. Typical gaps:
   - state management and how state is structured
   - layers and folder structure (repository, data, UI, routing)
   - UI conventions (theme, design system, shared widgets)
   - navigation flow and main screens
   - networking, auth, and local storage
   - testing expectations
   - flavors, environments, code generation
5. Ask in small batches (at most four questions at a time). Offer the
   detected value as the default option when there is one. Allow free text.
6. Draft a filled `CLAUDE.md` from `templates/CLAUDE.md` using the answers
   and the detected facts. Show it. **Write the file only after the user confirms.**
7. Never invent a stack detail. Anything unknown stays as `TODO` and is
   listed back to the user.

### plan
Reads the project `CLAUDE.md`. If it is missing or still has `TODO` in the
stack section, run `explore` first.

Writes a staged plan, one stage per layer in this order:
repository → data layer → UI layer → routing and wiring.
Each stage lists the files to create or change and what done looks like.
Skips stages the task does not need. **Stops and waits for approval.** It
never edits code itself.

### verify
Runs the commands the project `CLAUDE.md` lists (format, analyze, test).
Reports results plainly. On failure, says what failed and stops. It never
weakens or deletes a test to make it pass.

### review
Reviews the uncommitted diff against the approved plan and the project
`CLAUDE.md`. Reports findings only: scope creep, missing states, convention
breaks, missing tests. Does not rewrite code unless asked.

There is no scaffold skill on purpose. The workflow goes straight to logic.
There is no commit skill. Committing stays manual, or uses a separate commit skill.

Add a new skill only after a real use shows it is missing. Record why in `CHANGELOG.md`.

## 4. How to write a skill

Each `skills/<name>/SKILL.md` starts with frontmatter:

```markdown
---
name: <same as the folder name>
description: <one sentence: what it does and when to use it>
disable-model-invocation: true
---
```

- Keep `disable-model-invocation: true` on every skill. They are run by me or
  by the user on purpose, and this keeps their descriptions out of every
  turn's context.
- `description` is one plain sentence. No marketing words.
- Body is written as direct instructions to Claude: short steps, imperative.
  Prefer a numbered list for ordered steps.
- Every skill starts by reading the project `CLAUDE.md`. If it is missing,
  say so and point to `explore`.
- Never hard-code a stack (BLoC, Riverpod, a router package). Read it from the project.
- Never hard-code my own project names, paths, or preferences.
- When information is missing, ask. Do not guess and do not fill in plausible values.
- When a skill needs the user to choose, ask a specific question with
  concrete options, and put the detected or recommended option first.
- Keep each `SKILL.md` short. If it grows past about 100 lines, something
  belongs in the project `CLAUDE.md` instead.

## 5. Manifest and versions

`.claude-plugin/plugin.json`:
- `name`: `flutter-ai-workflow` (no spaces). Keep it the same as the `name` in the marketplace entry.
- `description`, `author`.
- **Leave `version` out while the plugin is still changing.** With no
  version, users get every new commit. If a version is added later, it must
  be bumped on every release or users will not receive changes.

`.claude-plugin/marketplace.json` needs `name`, `owner`, and a `plugins`
array whose entry `name` matches the manifest `name`.

## 6. Test before every push

```bash
claude plugin validate .
claude --plugin-dir .
```

In the session started with `--plugin-dir`:
1. Run `/reload-plugins` after each edit.
2. Run each changed skill on a real or throwaway Flutter project.
3. Check `/plugin` → Errors if a skill does not appear.

For `explore`, test at least these cases: no `CLAUDE.md`, a `CLAUDE.md` with
`TODO` lines, and a complete one. It must ask questions only in the first two.

Do not say a skill works unless it was run. If it was not run, say it was not run.

## 7. Git and commits

- Conventional Commits: `feat(skills): ...`, `fix(explore): ...`, `docs: ...`.
- Small commits. One skill change per commit when possible.
- **Do not add `Co-Authored-By` or any Claude/AI attribution** to commits or PR text.
- Do not commit or push unless I ask.
- Update `CHANGELOG.md` for any change a user would notice.
- No secrets, personal paths, or private project details in any file here. This repo is public.

## 8. README rules

The README is the first thing people see. It must:
- Say what the plugin is and show the flow in one short diagram or list.
- Show the exact install commands.
- Say plainly that it is early (`v0.x`) and still changing.
- Describe only what exists. No claims about results, speed, or quality that I have not measured.
- Say which parts are general practice and which are how I actually work.

## 9. Do not

- Do not invent features, commands, or behavior the plugin does not have.
- Do not add dependencies, scripts, or build tooling without asking.
- Do not make skills longer to look thorough.
- Do not overwrite `templates/CLAUDE.md` with project-specific details.

## 10. Communication

- Be direct and short. Say what changed and what was tested.
- If a rule here conflicts with something I say in chat, follow the chat and mention the conflict.
- If something is unclear, ask one specific question.
