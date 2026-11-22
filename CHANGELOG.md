# Changelog

## 0.1.0

- Set `version` to `0.1.0` in the plugin manifest. Every release from now on must bump it, or users will not get the changes.
- `verify`: run each command as its own call, with no chaining or temp files.
- `review`: run each git command as its own call, and say which part of the diff was not reviewed if one fails.
- Add MIT license.
- Add `review` skill: reviews the uncommitted diff against the plan and `CLAUDE.md`.
- Add `verify` skill: runs the format, analyze, and test commands from the project `CLAUDE.md`.
- Add `plan` skill: staged plan per layer, stops for approval. Asks before running `explore` when `CLAUDE.md` is missing or incomplete.
- Add `explore` skill: detects the stack and drafts a `CLAUDE.md` from the template.
- Add marketplace manifest, so the repo can be added as a marketplace.
- Add plugin manifest.
- Add `templates/CLAUDE.md` for Flutter projects.
