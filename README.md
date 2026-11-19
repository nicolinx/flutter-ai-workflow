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
- **How I work:** the stage order (repository, data layer, UI layer, routing and wiring), building logic before UI, and no separate scaffold step. The performance rules in the template come from my own posts. Change the template to fit your project.

## What has been tested

`explore` has been run on one small throwaway project in three cases: no `CLAUDE.md`, a `CLAUDE.md` with `TODO` lines, and a complete one. That was in non-interactive mode only.
`plan` has been run once, with an incomplete `CLAUDE.md`. It asked before running `explore`. When I said yes, Claude Code blocked the call, because every skill has `disable-model-invocation: true`, so `plan` told me to run `/flutter-ai-workflow:explore` myself. A full plan has not been tested.
`verify` and `review` have not been run yet. I have not measured any effect on speed or code quality.

## License

MIT. See [LICENSE](LICENSE).
