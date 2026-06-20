# git-atomic-commits

A Claude Code / Codex plugin for git workflows — project initialization and atomic, single-topic commits using line-level staging.

## Skills

### `git-init` — Project Initialization

Set up a new project from scratch with a comprehensive `.gitignore` and essential scaffold files.

When you ask Claude to initialize a project, this skill:

1. **Initializes** the git repository with `main` as the default branch
2. **Surveys** your project type (language, framework, package manager, infrastructure)
3. **Generates** a `.gitignore` with universal entries (macOS, Linux, Windows, VS Code, JetBrains, Cursor, Claude Code) plus framework-specific sections
4. **Creates** essential project files: `README.md`, `VERSION`, `CHANGELOG.md`, `LICENSE`, `TODOS.md`, `ARCHITECTURE.md`, and `PROJECT.md`
5. **Commits** everything in a single clean initial commit

### `git-atomic-commits` — Atomic Commits

Organize a dirty working tree into clean, single-topic commits using line-level staging.

When you ask Claude to commit, this skill:

1. **Inspects** the full diff of your working tree
2. **Proposes** logical groupings — one commit per topic, even across interleaved lines in the same file
3. **Stages** each group atomically using `git add -p` or `git add -e` for line-level precision
4. **Commits** in dependency order (independent changes first)

## Installation

### From the hyonchoi-plugins marketplace

```bash
claude plugin marketplace add hyonchoi/claude-plugins
claude plugin install git-atomic-commits@hyonchoi-plugins
```

### From source (local development)

```bash
claude plugin marketplace add --scope local /path/to/git-atomic-commits
claude plugin install git-atomic-commits@hyonchoi-plugins
```

## Usage

### Initializing a project

Ask Claude to set up a new project — any way you normally would:

- "init git"
- "git init"
- "initialize project"
- "new project"
- "set up repo"
- "bootstrap project"

The skill asks about your project type and generates a comprehensive `.gitignore` along with `README.md`, `VERSION`, `CHANGELOG.md`, `LICENSE`, `TODOS.md`, `ARCHITECTURE.md`, and `PROJECT.md`.

### Making commits

Ask Claude to commit — any way you normally would:

- "commit changes"
- "commit"
- "git commit"
- "stage changes"
- "atomic commits"
- "split commits"

The skill intercepts the request, analyzes your working tree, and proposes a commit plan before touching anything.

## How It Works

### .gitignore generation (git-init)

The skill surveys your project type and generates a `.gitignore` with universal entries for macOS, Linux, Windows, VS Code, JetBrains, Cursor, and Claude Code — then appends framework-specific sections based on your language, runtime, package manager, and infrastructure choices.

### Line-level staging (git-atomic-commits)

If `auth.js` has changes for both JWT authentication and rate limiting, those topics get committed separately — the skill uses `git add -e` to surgically stage only the lines that belong to each topic.

### Dependency ordering (git-atomic-commits)

The skill maps out commit dependencies before staging. If commit B relies on A, A goes first. Independent changes are committed alongside each other.

### Rename detection (git-atomic-commits)

When a file is moved or rewritten at a new path, the skill pairs the deletion and new file in the same commit so git shows it as a rename in `git log --stat`.

## Key Features

### git-init

- **Cross-platform .gitignore**: macOS, Linux, Windows entries always included
- **Editor coverage**: VS Code, JetBrains, Cursor, Claude Code configurations excluded
- **Framework-aware**: Adds language-specific and package-manager entries based on your project type
- **Project scaffold**: Creates `README.md`, `VERSION`, `CHANGELOG.md`, `LICENSE`, `TODOS.md`, `ARCHITECTURE.md`, and `PROJECT.md` with sensible defaults

### git-atomic-commits

- **Premise-aware**: Checks for active plans in `docs/plans/` and aligns commits with todo items
- **Interactive approval**: Proposes groupings with actual code lines (not just descriptions) — waits for your go-ahead
- **Edge-case handling**: Abstract base classes stay with their implementations, submodule pointer bumps are skipped, merge conflicts pause the workflow

## License

MIT
