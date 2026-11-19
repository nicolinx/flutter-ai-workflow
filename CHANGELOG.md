# Changelog

## Unreleased

- `verify`: run each command as its own call, with no chaining or temp files.
- `review`: run each git command as its own call, and say which part of the diff was not reviewed if one fails.
- Add MIT license.
- Add `review` skill: reviews the uncommitted diff against the plan and `CLAUDE.md`.
- Add `verify` skill: runs the format, analyze, and test commands from the project `CLAUDE.md`.
- Add `plan` skill: staged plan per layer, stops for approval. Asks before running `explore` when `CLAUDE.md` is missing or incomplete.
- Add `explore` skill: detects the stack and drafts a `CLAUDE.md` from the template.
- Add marketplace manifest, so the repo can be added as a marketplace.
- Add plugin manifest. No `version` is set while the plugin is changing.
- Add `templates/CLAUDE.md` for Flutter projects.
