# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.0.4] - 2026-06-29

### Fixed

- Rewrote the plugin description and both SKILL.md descriptions to map natural language → skill instead of framing invocation as an explicit `Invoke 'git-atomic-commits'` command. Smaller local models read the old wording literally and only fired on the skill name, not on plain phrasings like "commit my changes". The description now leads with the bare phrases a user actually types and tells the model to act without being asked for the skill by name. Expanded the `triggers` lists with common natural phrasings.

## [1.0.3] - 2026-06-29

### Fixed

- Shortened plugin description to front-load trigger phrases so models parse them before truncation. The skill's SKILL.md triggers were not being injected into context — only the plugin description is — so commit/init phrases must live in the plugin manifest.

## [1.0.2] - 2026-06-23

### Fixed

- Fixed natural language triggers in both Claude Code and Codex plugin manifests.

## [1.0.1] - 2026-06-22

### Fixed

- Bumped plugin manifests after adding the `git-init` skill so Codex installs refresh beyond the original `1.0.0` cache.
- Added explicit skill directory metadata to both plugin manifests.
