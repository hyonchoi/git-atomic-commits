---
name: git-init
description: >
  Initialize a new project as a git repository with a comprehensive .gitignore and essential project scaffold files.

  INVOKE THIS SKILL for any request to initialize, set up, or start a new git repository: "init git", "git init", "initialize project", "set up repo", "new project", "start a new project", "bootstrap project", "create a new repo", "init a new project", or any other phrasing that asks to begin a new git-tracked project.

  Also invoke for: "add .gitignore", "generate gitignore", "set up .gitignore", "create default project files", "scaffold project files", or any other phrasing that asks to create the standard project boilerplate.
triggers:
  - git init
  - init git
  - initialize project
  - new project
  - set up repo
  - bootstrap project
---

# Git Init

Initialize a new git repository with a comprehensive .gitignore and essential project files.

## Core Principle

Every project should start with a solid foundation: version control, a robust .gitignore covering all platforms and editors, and the standard documentation files that keep a project organized from day one.

---

## Workflow

### Step 1: Confirm the project root

Determine the correct directory to initialize. If the user specified a path, use it. Otherwise, use the current working directory.

```bash
pwd
```

Check if the directory already has a `.git` directory:
```bash
test -d .git && echo "git repo exists" || echo "not a git repo"
```

If it's already a git repo, inform the user and ask whether to proceed with adding missing files and updating .gitignore.

### Step 2: Initialize the repository

```bash
git init -b main
```

Use `main` as the default branch name. If the user has a different preference, use that instead.

### Step 3: Determine the project framework

Ask the user what kind of project this is so the .gitignore can include framework-specific entries:

```
What kind of project is this? (Select all that apply)

- Language/Runtime:
  [ ] Python
  [ ] Node.js / JavaScript / TypeScript
  [ ] Java / Kotlin
  [ ] Go
  [ ] Rust
  [ ] Ruby
  [ ] C / C++
  [ ] PHP
  [ ] C# / .NET
  [ ] Swift
  [ ] Other (specify)

- Package Manager:
  [ ] npm / yarn / pnpm / bun
  [ ] pip / poetry / uv
  [ ] Cargo
  [ ] Maven / Gradle
  [ ] Composer
  [ ] Go modules
  [ ] Other (specify)

- Container / Infrastructure:
  [ ] Docker
  [ ] Kubernetes
  [ ] Terraform
```

Wait for the user's selections before proceeding.

### Step 4: Generate .gitignore

Create a comprehensive .gitignore with entries organized by category. **Always** include the cross-platform, editor, and IDE sections below regardless of project type. Then append the framework-specific sections based on the user's selections.

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
# IDE-common
.settings/
.project
.classpath

# Windows
Thumbs.db
Thumbs.db:encryptable
ehthumbs.db
ehthumbs_v100.db
Desktop.ini
*$Recycle.Bin*
*.cab
*.msi
*.msix
*.msm
*.msp
*.lnk
*.pidb
*.stackdump

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
.idea_modules/
*.sln.iml

# ===========================
# Cursor
# ===========================
.cursor/
.cursorrules
.cursorignore
.cursor-template*
.cursor-workspace/

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
*.swn
*.cache
*.pid
*.seed
*.orig
.env
.env.local
.env.*.local
*.env
```

#### Framework-specific sections (append based on user selections)

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
.tox/
.venv/
venv/
env/
ENV/
.venv/
mypy_cache/
.dmypy.json
.pyre/
.pydbschema
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
lerna-debug.log*
bun.lockb
dist/
build/
.next/
*.tsbuildinfo
coverage/
.nyc_output/
.eslintcache
.parcel-cache/
```

**Java / Kotlin:**
```gitignore
# ===========================
# Java / Kotlin
# ===========================
target/
bin/
obj/
classes/
*.class
*.jar
*.war
*.ear
*.nar
rebel.xml
.mvn/wrapper/maven-wrapper.jar
!**/src/main/**/wrapper/
!**/src/test/**/wrapper/
```

**Go:**
```gitignore
# ===========================
# Go
# ===========================
/go
*.exe
*.exe~
*.test
*.out
vendor/
.gomodcache/
```

**Rust:**
```gitignore
# ===========================
# Rust
# ===========================
/target
**/*.rs.bk
**/*.rs.bak
**/*.rbj
Cargo.lock
rustfmt.toml
.rustfmt.toml
```

**Ruby:**
```gitignore
# ===========================
# Ruby
# ===========================
.bundle/
Gemfile.lock
pkg/
.yardoc/
.racc/
coverage/
/.resultset.json
/.coverage.*
/.simplecov
/coverage/
stored_responses/
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
*.dylib.*
*.dll
*.dll.a
*.pdb
*.out
*.app
Makefile
cmake/
CMakeFiles/
CMakeCache.txt
compile_commands.json
cmake_install.cmake
Makefile
build/
*.vcxproj
*.vcxproj.filters
*.vcxproj.user
ipch/
*.sdf
*.suo
*.opensdf
```

**PHP:**
```gitignore
# ===========================
# PHP
# ===========================
/vendor/
composer.lock
*.log
.phpunit.result.cache
.php-version
.phpunit.result.cache
.cache/
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
*.userosscache
*.sln.docstates
*.nupkg
*.snupkg
[pP]ublish*
appsettings.json
appsettings.*.json
packages/
*.cache
```

**Swift:**
```gitignore
# ===========================
# Swift
# ===========================
build/
DerivedData/
*.xcuserdata
*.xcscmblueprint
*.xcworkspace/
!*.xcworkspace/xcshareddata/
*.xccheckout
*.moved-aside
*.xcuserstate
profile
*.moved-aside
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
override.tf
override.tf.json
*_override.tf
*_override.tf.json
```

**Kubernetes:**
```gitignore
# ===========================
# Kubernetes
# ===========================
.kube-linter/
*.kubeconfig*
.kubectl-backup
```

**Package Managers:**
```gitignore
# npm / yarn / pnpm / bun
package-lock.json
yarn.lock
pnpm-lock.yaml
bun.lock

# pip / poetry / uv
pip-log.txt
pip-delete-this-directory.txt
poetry.lock
uv.lock
pip-tools.lock

# Maven
*.iml
.mvn/
pom.xml.tag
pom.xml.releaseBackup
pom.xml.versionsBackup
pom.xml.next
release.properties
dependency-reduced-pom.xml
buildNumber.properties
.env

# Gradle
.gradletasknamecache
.gradle/

# Composer
composer.phar
```

### Step 5: Create essential project files

Create the following files with sensible defaults. For each file, ask the user for the project name and a brief description, then generate the content.

**README.md:**
```markdown
# <Project Name>

<Description>

## Getting Started

### Prerequisites

- List prerequisites here

### Installation

```bash
# Installation instructions here
```

### Usage

```bash
# Usage examples here
```

## Development

### Setup

```bash
# Development setup instructions
```

### Testing

```bash
# Test commands
```

## Contributing

Please read [CONTRIBUTING](CONTRIBUTING.md) for details on the process for submitting pull requests.

## License

[MIT](LICENSE)
```

**VERSION:**
```text
0.1.0
```

**CHANGELOG.md:**
```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Initial project setup
```

**LICENSE:**
Ask the user which license to use:
```
Which license would you like to use?
1. MIT (Recommended - permissive, widely used)
2. Apache-2.0 (Patent grant, attribution required)
3. GPL-3.0 (Copyleft - derivatives must be open source)
4. BSD-2-Clause (Simple, permissive)
5. BSD-3-Clause (Non-endorsement clause)
6. Unlicense (Public domain dedication)
7. None (I'll add one later)
```

Use the standard SPDX license text for the chosen license. If MIT is selected, use:
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

**TODOS.md:**
```markdown
# TODOS

## High Priority

- [ ] Define project scope and goals
- [ ] Set up CI/CD pipeline
- [ ] Add project dependencies

## Medium Priority

- [ ] Write initial tests
- [ ] Add documentation
- [ ] Set up logging and monitoring

## Low Priority

- [ ] Performance profiling
- [ ] Code coverage targets
- [ ] Internationalization support
```

**ARCHITECTURE.md:**
```markdown
# Architecture

## Overview

<A brief description of the project architecture>

## Directory Structure

```
<project-root>/
├── src/          # Source code
├── tests/        # Test files
├── docs/         # Documentation
├── scripts/      # Build/utility scripts
├── config/       # Configuration files
├── README.md
├── CHANGELOG.md
├── VERSION
└── ARCHITECTURE.md
```

## Key Components

- **Component 1**: <Description>
- **Component 2**: <Description>

## Data Flow

<A brief description of how data flows through the system>

## Design Decisions

| Decision | Rationale |
|----------|-----------|
| <Decision> | <Reason> |

## Dependencies

- List major dependencies and their purpose
```

**PROJECT.md:**
```markdown
# Project

## Name

<Project name>

## Description

<Brief description of what the project does>

## Goals

- <Goal 1>
- <Goal 2>
- <Goal 3>

## Non-Goals

- <Things this project explicitly will not do>

## Tech Stack

- <Language/Runtime>
- <Framework>
- <Database>
- <Infrastructure>

## Team

- <Owner/maintainer>

## Communication

- Issues: <GitHub Issues or other tracker>
- Discussion: <Slack/Discord/etc.>

## Links

- Repository: <URL>
- Documentation: <URL>
- Deployment: <URL>
```

### Step 6: Initial commit

Stage all files and create the initial commit:

```bash
git add .
git commit -m "chore: initialize project with scaffold"
```

Show the user a summary of what was created:

```
Project initialized!

Repository: <path>
Branch: main

Files created:
  - .gitignore
  - README.md
  - VERSION
  - CHANGELOG.md
  - LICENSE
  - TODOS.md
  - ARCHITECTURE.md
  - PROJECT.md

Commit: chore: initialize project with scaffold
```

---

## Edge Cases

**Directory already contains files:**
Inform the user that the directory is not empty. List the existing files and ask whether to proceed with initialization anyway or choose a different directory.

**~/.gitconfig missing:**
If `git config --global user.name` or `git config --global user.email` returns empty, prompt the user to set them up first:
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

**Existing .gitignore:**
If a .gitignore already exists, offer to merge the new entries with the existing one rather than overwriting it. Append missing categories and prepend `# --- existing content above ---` as a separator.

**User selects "None" for framework:**
Generate a .gitignore with only the universal sections (macOS/Linux/Windows/VSCode/JetBrains/Cursor/General).

**User declines certain default files:**
If the user explicitly asks not to create certain files (e.g., "skip LICENSE" or "don't create PROJECT.md"), honor that and skip them.

**Shallow or bare repos:**
If `git init` is run inside a shallow clone or the user requests `--bare`, skip the file creation step and only initialize the repo.

---

## Tips

- Always show the user the generated .gitignore before writing it so they can verify the entries are appropriate.
- When generating project files, use information from the user's `~/.gitconfig` (name, email) if available to pre-fill author fields.
- For monorepo setups, mention that the .gitignore covers the root and suggest creating workspace-specific entries in sub-projects.
