---
name: git-atomic-commits
description: >
  Stage and commit git changes atomically — one logical topic per commit — by analyzing a dirty working tree, proposing groupings, and using line-level staging to separate changes that belong to different topics even within the same file.

  INVOKE THIS SKILL for ANY commit request, no matter how short or simple: "commit", "commit changes", "commit this", "commit it", "make a commit", "git commit", "please commit", "go ahead and commit", "commit the changes", or any other phrasing that asks for a commit to be created. Short bare requests like "commit changes" are the most common trigger — do not miss them.

  Also invoke for: "stage changes", "clean up commits", "atomic commits", "split commits", "organize my commits", "I have a bunch of changes and want to commit them properly", or "help me stage my changes".

  This skill must be used for every commit operation — never run git commit directly without going through this skill's propose-then-stage workflow.
triggers:
  - commit
  - commit changes
  - git commit
  - stage changes
  - atomic commits
  - split commits
---

# Git Atomic Commits

Organize a dirty working tree into clean, single-topic commits using line-level staging.

## Core Principle

Each commit should do exactly one thing. If file X has changes for topic A and topic C, those lines must be staged and committed separately — A in one commit, C in another.

---

## Workflow

### Step 1: Inspect the working tree

```bash
git --no-pager diff          # unstaged changes
git --no-pager diff --cached # already staged (should be empty at start)
git --no-pager status        # overview
```

Read the full diff carefully. Understand every changed hunk before proposing anything.

### Step 2: Check for an active plan

Before proposing groupings, check if `docs/plans/` contains a plan document whose scope matches the current changes. If one exists:
- Use the plan's todo list as the primary grouping structure — one commit per todo where possible
- Align commit messages with the todo titles
- If multiple todos are truly inseparable (e.g., share a file with interleaved lines), combine them with a note

If no matching plan exists, derive groupings from the changes themselves.

### Step 3: Propose groupings to the user

Present a clear, numbered list of proposed commits. For each group:
- Give it a short, descriptive commit message (imperative mood, ≤72 chars)
- List which files are affected
- Note if only *some lines* of a file belong to this group

**Order commits by dependency — independent commits first.**

Before finalizing the order, map out dependencies between proposed commits:
- A commit B *depends on* A if checking out B without A would leave the repo in a broken or incomplete state (missing imports, unresolved references, abstract methods without implementations, etc.)
- Commits with no dependencies on others go first
- When multiple orderings satisfy the dependency constraints, prefer the one that puts foundational/shared code before the code that uses it

Example: commits A, B, C, D where A needs B and D, B needs D, C needs B → valid orders are `D→B→C→A` or `D→B→A→C`.

**Rename detection: pair deletions with their replacements.**

When a file is moved or substantially rewritten as a new file at a different path, stage the deletion and the new file in the *same* commit. Git's similarity-based rename detection will then show it as `renamed old/path → new/path` in `git log --stat` and `git diff`, rather than a separate delete + add. This preserves history and makes the move intent clear.

**Example proposal format:**

Show the actual lines of code that belong to each commit, using a mini diff format. This makes it unambiguous exactly which lines go where.

```
Proposed commits:

1. "Add JWT-based authentication and token validation"
   src/auth.js (partial):
     + const jwt = require('jsonwebtoken');
     + if (!result) throw new Error('Invalid credentials');
     + return jwt.sign({ user }, process.env.SECRET);
     + session.clearCookie('token');
     + function validateToken(token) {
     +   return jwt.verify(token, process.env.SECRET);
     + }
   src/middleware/auth.js (entire new file)

2. "Add login rate limiting"
   src/auth.js (partial):
     + const rateLimit = require('express-rate-limit');
     + rateLimitCheck(user);
     + function rateLimitCheck(user) {
     +   if (loginAttempts[user] > 5) throw new Error('Too many attempts');
     +   loginAttempts[user] = (loginAttempts[user] || 0) + 1;
     + }

3. "Update README with setup instructions"
   README.md (entire file)

Does this look right? Any changes before I start staging?
```

Use `+` for added lines, `-` for removed lines, and plain text for context. Always show the actual code — not just descriptions.

**Wait for user approval before touching anything.**

If the user wants to adjust groupings, revise and re-propose. Do not proceed until the user explicitly confirms.

### Step 4: Stage and commit each group

Work through commits one at a time, in order.

#### Staging whole files
```bash
git add <file>
```

#### Staging specific lines (partial file)

Use `git add -p` (interactive) or `git add -e` (manual edit) depending on hunk structure.

**When hunks align cleanly with topic boundaries → use `git add -p`:**
```bash
git add -p <file>
# y = stage this hunk
# n = skip this hunk
# s = split hunk into smaller pieces
# e = manually edit the hunk
```

**When lines interleave within a single hunk → use `git add -e`:**

`git add -e <file>` opens the raw patch in your editor. You manually remove lines that don't belong to this commit before saving.

**Three rules for editing the patch:**
- `+` line you want to **exclude** → delete the entire line
- `-` line you want to **exclude** → change `-` to ` ` (a space) — this "un-removes" it for now; it will be removed in a later commit
- Context lines (` ` lines, no prefix) → leave them alone

**Concrete example** — staging only the JWT lines, excluding rate-limit lines:

Before editing (what git opens):
```diff
+const jwt = require('jsonwebtoken');
+const rateLimit = require('express-rate-limit');
+
 function login(user, pass) {
+  rateLimitCheck(user);
+  const result = db.query('SELECT * FROM users WHERE user=?', [user]);
+  if (!result) throw new Error('Invalid credentials');
+  return jwt.sign({ user }, process.env.SECRET);
 }
+
+function validateToken(token) {
+  return jwt.verify(token, process.env.SECRET);
+}
+
+function rateLimitCheck(user) {
+  if (loginAttempts[user] > 5) throw new Error('Too many attempts');
+}
```

After editing (rate-limit lines removed or neutralized):
```diff
+const jwt = require('jsonwebtoken');

 function login(user, pass) {
+  const result = db.query('SELECT * FROM users WHERE user=?', [user]);
+  if (!result) throw new Error('Invalid credentials');
+  return jwt.sign({ user }, process.env.SECRET);
 }
+
+function validateToken(token) {
+  return jwt.verify(token, process.env.SECRET);
+}
```

What changed: deleted `+const rateLimit...`, deleted `+  rateLimitCheck(user);`, deleted the entire `rateLimitCheck` function block.

Save and close the editor. Git stages only what remains in the edited patch.

After staging, verify before committing:
```bash
git diff --cached   # confirm only the right lines are staged
git diff            # confirm the rest is still unstaged
```

#### Commit
```bash
git commit -m "<commit message from the proposal>"
```

Repeat for each proposed group.

### Step 5: Verify

After all commits:
```bash
git log --oneline -<N>   # show the last N commits
git status               # should be clean (or only untracked files)
```

Show the user the final commit list and confirm everything looks right.

---

## Edge Cases

**Hunk too large to split with `s` in `git add -p`:**
Fall back to `git add -e`. Edit the diff patch directly.

**Lines from two topics are truly adjacent (no blank line between them):**
Use `git add -e` and manually trim the staged patch. Add a note in the commit message if context is needed.

**New untracked files:**
Use `git add <file>` normally — they can only be staged as a whole.

**User changes the grouping mid-way:**
`git reset HEAD` to unstage everything, then restart from Step 3 with the revised plan.

**Merge conflicts or rebase in progress:**
Pause and notify the user. Do not stage during a conflicted state.

**Submodule pointer changes in a monorepo root:**
If the only changes in the root repo are submodule pointer updates (i.e., `git diff` in the root shows only `Subproject commit ...` lines and no other modifications), do **not** propose or create a root repo commit. Submodule pointer bumps caused solely by committing inside a submodule are noise — the user will decide if and when to record them in the root. Only include submodule pointer changes in a root commit when the user explicitly asks for it.

**Already-staged deletions that need to be paired with new files for rename detection:**
Use `git reset HEAD <file>` to unstage the deletion, then re-stage it together with the new file in the correct commit. Never let a paired deletion and addition land in different commits if rename detection is desired.

**Abstract base class and its implementations changed together:**
If a base class introduces new abstract methods and its subclasses implement them,
the base class and all its implementations **must land in the same commit** — otherwise
the intermediate state has unimplemented abstract methods and won't compile.

Group by subsystem (e.g., all PostgreSQL files together, all InfluxDB files together)
rather than by layer (e.g., all base classes first, all implementations second).

Exception: classes that are logically independent of the abstraction change (e.g.,
`@Deprecated` standalone classes that happen to use the same column names but don't
extend the base class) can and should be in their own separate commit.

---

## Commit Message Style

- Imperative mood: "Add", "Fix", "Refactor", "Remove" — not "Added" or "Adds"
- ≤72 characters for the subject line
- No period at the end
- Be specific: "Fix null check in user session expiry" not "Fix bug"
