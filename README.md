# git-atomic-commits

A Claude Code / Codex plugin for organizing a dirty working tree into clean, single-topic commits using line-level staging.

## What It Does

When you ask Claude to commit, this skill:

1. **Inspects** the full diff of your working tree
2. **Proposes** logical groupings — one commit per topic, even across interleaved lines in the same file
3. **Stages** each group atomically using `git add -p` or `git add -e` for line-level precision
4. **Commits** in dependency order (independent changes first)

## Installation

### From source (local development)

```bash
# Install as a local project plugin
claude plugin install /path/to/git-atomic-commits
```

### From a marketplace or git URL

```bash
claude plugin install git+https://github.com/your-username/git-atomic-commits.git
```

## Usage

Just ask Claude to commit — any way you normally would:

- "commit changes"
- "commit"
- "git commit"
- "stage changes"
- "atomic commits"
- "split commits"

The skill intercepts the request, analyzes your working tree, and proposes a commit plan before touching anything.

## How It Works

### Line-level staging

If `auth.js` has changes for both JWT authentication and rate limiting, those topics get committed separately — the skill uses `git add -e` to surgically stage only the lines that belong to each topic.

### Dependency ordering

The skill maps out commit dependencies before staging. If commit B relies on A, A goes first. Independent changes are committed alongside each other.

### Rename detection

When a file is moved or rewritten at a new path, the skill pairs the deletion and new file in the same commit so git shows it as a rename in `git log --stat`.

## Key Features

- **Premise-aware**: Checks for active plans in `docs/plans/` and aligns commits with todo items
- **Interactive approval**: Proposes groupings with actual code lines (not just descriptions) — waits for your go-ahead
- **Edge-case handling**: Abstract base classes stay with their implementations, submodule pointer bumps are skipped, merge conflicts pause the workflow

## License

MIT
