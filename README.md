# flutter-ai-workflow

A Claude Code plugin with skills for working on Flutter apps. It gives you a
repeatable flow: explore the project, plan in stages, implement, verify, review, commit.

**Status: v0.x. It is early and still changing.** Skill behavior and file names may change between commits.

## The flow

```
Explore → Plan (staged, needs approval) → Implement (logic before UI) → Verify → Review → Commit
```

The plugin has four skills:

| Skill | What it does |
|---|---|
| `/flutter-ai-workflow:explore` | Reads the project, detects the stack, and helps you write a clear `CLAUDE.md`. |
| `/flutter-ai-workflow:plan` | Writes a staged plan (repository, data, UI, routing) and stops for your approval. It does not edit code. |
| `/flutter-ai-workflow:verify` | Runs the format, analyze, and test commands listed in your `CLAUDE.md` and reports the results. |
| `/flutter-ai-workflow:review` | Reviews the uncommitted diff against the plan and your `CLAUDE.md`. It reports findings and does not rewrite code. |

Implementing and committing are not skills. You or Claude do them between the steps above.
All skills read your project's own `CLAUDE.md` and code. None of them assume a stack.

## Install

```bash
claude plugin marketplace add nicolinx/flutter-ai-workflow
claude plugin install flutter-ai-workflow@flutter-ai-workflow
```

Then, inside your Flutter project, start Claude Code and run `/flutter-ai-workflow:explore`.

## Your project's CLAUDE.md

A plugin cannot ship a `CLAUDE.md` that loads by itself. This repo includes a template at
`templates/CLAUDE.md`. `explore` fills it in with you and writes it to your project only after you confirm.
You can also copy the template by hand.

## What is general practice, and what is how I work

- **General practice:** explore before you plan, plan before you code, run format, analyze, and tests, and read the diff before you commit.
- **How I work:** the stage order (repository, data layer, UI layer, routing and wiring), building logic before UI, and no separate scaffold step. The code conventions in the template are my own defaults. Change the template to fit your project.

## What has been tested

Everything below was run on small throwaway Flutter projects.

- **`explore`:** with no `CLAUDE.md`, with `TODO` lines, and with a complete one. It asked questions only when something was missing.
- **`plan`:** with an incomplete `CLAUDE.md`, it asked before running `explore`. Skills cannot call each other, so it told me to run `explore` myself. With a complete `CLAUDE.md`, it wrote a four-stage plan, listed what it was unsure about, and waited for approval.
- **`verify`:** with everything passing, and with a type error. On the error it stopped at `flutter analyze`, quoted the error, marked `flutter test` as not run, and changed no code.
- **`review`:** on a small diff with planted problems (an unplanned file, a screen with no loading, error, or empty state, a debug print, no test). It reported each one with file and line.
- **Install:** adding the marketplace and installing from GitHub with the commands above worked.

Not tested: large projects, `verify` and `review` in a long interactive session, and Windows or Linux.
I have not measured any effect on speed or code quality.

## License

MIT. See [LICENSE](LICENSE).
