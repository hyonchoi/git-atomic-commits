# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A Claude Code / Codex plugin consisting of two skills defined entirely as SKILL.md files — no build step, no runtime code, no tests. The skills instruct Claude how to behave when invoked by trigger phrases.

## Skills

| Skill | Triggers | Purpose |
|-------|----------|---------|
| `git-init` | "git init", "new project", "bootstrap project" | Initialize a repo with `.gitignore` and minimal scaffold files |
| `git-atomic-commits` | "commit", "commit changes", "git commit", "split commits" | Organize a dirty working tree into single-topic commits via line-level staging |

Each skill lives at `skills/<skill-name>/SKILL.md` and has YAML frontmatter (`name`, `description`, `triggers`) followed by markdown instructions.

## Plugin Manifests

- **`.claude-plugin/plugin.json`** — Claude Code manifest. Includes `"skills": "./skills/"` to register the skills directory.
- **`.codex-plugin/plugin.json`** — Codex CLI manifest. Includes `"skills": "./skills/"` to register the skills directory and `interface` metadata for Codex presentation.

When adding a new skill, create `skills/<name>/SKILL.md` and ensure both manifests are up to date.

### Version Sync

Keep `VERSION` in sync with the `"version"` field in both `.claude-plugin/plugin.json` and `.codex-plugin/plugin.json`. Bump all three together when releasing.

## Local Development

```bash
# Register locally and install
claude plugin marketplace add --scope local /path/to/git-atomic-commits
claude plugin install git-atomic-commits@hyonchoi-plugins
```

`.claude-plugin/marketplace.json` is gitignored — it's local state, not committed.

## Style Notes

- SKILL.md files use YAML frontmatter with `name`, `description` (block scalar `>`), and `triggers` (array)
- `git-init` asks questions one-by-one, scaffolds minimal content (README, VERSION, CHANGELOG, TODOS, ARCHITECTURE, PROJECT, LICENSE)
- `git-atomic-commits` uses `git add -e` for interleaved changes, checks `docs/plans/` for active plans, and commits in dependency order
