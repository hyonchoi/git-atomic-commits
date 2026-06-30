---
name: git-init
description: >
  Use this skill whenever the user wants to start, initialize, or bootstrap a new git repository — including plain, natural-language requests that never name the skill. Trigger on phrasings like "git init", "init git", "initialize project", "set up a repo", "new project", "start a new project", "bootstrap project", "create a new repo", "init a new project", or any phrasing that asks to begin a new git-tracked project. Do not wait to be asked for the skill by name.

  Also use it for: "add .gitignore", "generate gitignore", "set up .gitignore", "create default project files", or "scaffold project files".

  What it does: it runs git init, writes a comprehensive .gitignore, and creates minimal scaffold files (README, etc.).
triggers:
  - git init
  - init git
  - initialize project
  - new project
  - set up repo
  - bootstrap project
---

# Git Init

Initialize a new git repository with a comprehensive .gitignore and minimal project scaffold files.

## Core Principle

Every project should start with version control, a robust .gitignore, and a README — nothing more. The scaffold is minimal because the project details will emerge as development proceeds. Heavy documentation (ARCHITECTURE.md, PROJECT.md, TODOS.md, etc.) is noise before there's something to document.

---

## Workflow

### Step 1: Confirm the project root

Determine the correct directory. If the user specified a path, use it. Otherwise, use the current working directory.

```bash
pwd
test -d .git && echo "git repo exists" || echo "not a git repo"
```

If it's already a git repo, inform the user and ask whether to proceed with adding missing files and updating .gitignore.

If the directory is not empty, list the existing files and ask whether to proceed anyway or choose a different directory.

### Step 2: Initialize the repository

```bash
git init -b main
```

Use `main` as the default branch. If the user has a different preference, use that instead.

### Step 3: Ask questions — one at a time

Ask each question individually and wait for the answer before moving on. This keeps the conversation focused and avoids overwhelming the user with a wall of options.

**3a. Project name:**

```
Project name?
```

**3b. Brief description:**

```
Brief description (one line)?
```

**3c. Language / runtime:**

```
What language or runtime? (e.g. Python, TypeScript, Go, Rust, Java, etc.) — "none" if language-agnostic.
```

Use the answer to determine which framework-specific .gitignore entries to include. If the user answers a language, follow up with the natural next question for that language (e.g., framework for Node.js/Python, etc.).

**3d. Framework** (if applicable based on language):

```
Any specific framework? (e.g. Next.js, Django, FastAPI, Spring, etc.) — "none" if not applicable.
```

**3e. Container / infrastructure** (only if relevant):

```
Using Docker, Kubernetes, or Terraform? — "none" if not applicable.
```

**3f. License:**

```
License? (MIT / Apache-2.0 / GPL-3.0 / BSD-3-Clause / Unlicense / none)
```

Default to MIT if the user is unsure.

### Step 4: Generate .gitignore

Create a comprehensive .gitignore organized by category. **Always** include the cross-platform and editor sections below regardless of project type. Then append framework-specific sections based on the answers above.

#### Universal section (always included)

```gitignore
# ===========================
# macOS
# ===========================
.DS_Store
.AppleDouble
.LSOverride
._*
.DocumentRevisions-V100
.fseventsd
.Spotlight-V100
.TemporaryItems
.Trashes
.VolumeIcon.icns
com.apple.timemachine.donotcurrent

# Linux
*~
.bash_history
.zsh_history
.lesshst
.gdb_history
.sudo_as_admin_successful

# Windows
Thumbs.db
ehthumbs.db
Desktop.ini
*$Recycle.Bin*

# ===========================
# VS Code
# ===========================
.vscode/*
!.vscode/settings.json
!.vscode/tasks.json
!.vscode/launch.json
!.vscode/extensions.json
!.vscode/*.code-snippets
!.vscode/*.code-workspace
!.vscode/sftp.json

# ===========================
# JetBrains (IntelliJ, PyCharm, WebStorm, etc.)
# ===========================
.idea/
*.iws
*.iml
*.ipr
out/

# ===========================
# Cursor
# ===========================
.cursor/
.cursorrules
.cursorignore

# ===========================
# Claude Code
# ===========================
.claude/
!.claude/settings.json

# ===========================
# General
# ===========================
*.log
*.tmp
*.temp
*.bak
*.swp
*.swo
*.cache
*.pid
.env
.env.local
.env.*.local
*.env
```

#### Framework-specific sections (append based on user answers)

**Python:**
```gitignore
# ===========================
# Python
# ===========================
__pycache__/
*.py[cod]
*$py.class
*.so
*.egg-info/
*.egg
dist/
build/
.pytest_cache/
.coverage
htmlcov/
.venv/
venv/
env/
ENV/
mypy_cache/
.pyre/
```

**Node.js / JavaScript / TypeScript:**
```gitignore
# ===========================
# Node.js / JavaScript / TypeScript
# ===========================
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-debug.log*
bun.lockb
dist/
build/
.next/
*.tsbuildinfo
coverage/
.eslintcache
```

**Java / Kotlin:**
```gitignore
# ===========================
# Java / Kotlin
# ===========================
target/
bin/
obj/
*.class
*.jar
*.war
*.ear
rebel.xml
```

**Go:**
```gitignore
# ===========================
# Go
# ===========================
*.exe
*.test
*.out
vendor/
```

**Rust:**
```gitignore
# ===========================
# Rust
# ===========================
/target
**/*.rs.bk
Cargo.lock
```

**Ruby:**
```gitignore
# ===========================
# Ruby
# ===========================
.bundle/
Gemfile.lock
pkg/
coverage/
tmp/
log/
```

**C / C++:**
```gitignore
# ===========================
# C / C++
# ===========================
*.o
*.obj
*.lib
*.a
*.so
*.so.*
*.dylib
*.dll
*.pdb
*.out
build/
CMakeFiles/
CMakeCache.txt
```

**PHP:**
```gitignore
# ===========================
# PHP
# ===========================
/vendor/
composer.lock
```

**C# / .NET:**
```gitignore
# ===========================
# C# / .NET
# ===========================
bin/
obj/
*.suo
*.user
*.nupkg
packages/
```

**Swift:**
```gitignore
# ===========================
# Swift
# ===========================
build/
DerivedData/
*.xcuserdata
*.xcworkspace/
!*.xcworkspace/xcshareddata/
*.xcuserstate
```

**Docker:**
```gitignore
# ===========================
# Docker
# ===========================
.docker/
!docker-compose*.yml
!Dockerfile*
!docker-entrypoint*
```

**Terraform:**
```gitignore
# ===========================
# Terraform
# ===========================
.terraform/
*.tfstate*
*.tfvars
.terraform.lock.hcl
```

**Kubernetes:**
```gitignore
# ===========================
# Kubernetes
# ===========================
.kube-linter/
*.kubeconfig*
```

### Step 5: Create scaffold files

Create these files with minimal content — just enough to be useful as anchors. The project isn't detailed yet, so don't fill in dummy data that will be wrong until someone comes along and fixes it.

**README.md** — the bare essentials:

```markdown
# <Project Name>

<Description>
```

**VERSION:**

```text
0.1.0
```

**CHANGELOG.md** — header only:

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
```

**TODOS.md** — header only:

```markdown
# TODOS
```

**ARCHITECTURE.md** — header only:

```markdown
# Architecture
```

**PROJECT.md** — just what's known from the questions:

```markdown
# Project

- **Name:** <name>
- **Description:** <description>
- **Language:** <language>
- **License:** <license>
```

Fill in only the fields the user answered; omit a line if the answer was "none" or unclear.

**LICENSE** — the standard SPDX text for the chosen license. If MIT is selected:

```text
MIT License

Copyright (c) <year> <author>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

Use the user's gitconfig name for the author. If no license was selected, skip this file.

### Step 6: Initial commit

```bash
git add .
git commit -m "chore: initialize project"
```

Show the user a brief summary:

```
Project initialized!

Repository: <path>
Branch: main

Files created:
  - .gitignore
  - README.md
  - VERSION
  - CHANGELOG.md
  - TODOS.md
  - ARCHITECTURE.md
  - PROJECT.md
  - LICENSE          (if applicable)

Commit: chore: initialize project
```

---

## Edge Cases

**Existing .gitignore:** Offer to merge new entries rather than overwrite. Append missing categories below a `# --- existing content above ---` separator.

**User wants more files:** If the user asks for CHANGELOG, TODOS, or similar, create them on request — but keep the content minimal.

**~/.gitconfig missing:** If `git config --global user.name` or `git config --global user.email` returns empty, prompt the user to set them up before committing.

**User skips questions:** If the user wants a bare minimum ("just git init"), create only .gitignore with universal entries and commit.

**Shallow or bare repos:** If `--bare` is requested, skip file creation and only initialize the repo.

---

## Tips

- Use the user's `~/.gitconfig` (name, email) to pre-fill the LICENSE author and to skip the gitconfig edge-case prompt.
- Always show the .gitignore before writing it if it's complex (many framework sections).
