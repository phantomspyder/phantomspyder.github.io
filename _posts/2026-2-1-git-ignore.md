```markdown
---
title: "Understanding .gitignore in a .NET Web App: What It Is, How It Works, and Syntax"
description: "A practical guide to .gitignore for ASP.NET Core / .NET web apps: what it does, how Git applies rules, and the patterns you actually need."
---

# Understanding `.gitignore` in a .NET Web App

A `.gitignore` file is Git’s “do not track this” list. In a **.NET web app**, it’s especially handy because builds, tooling, and IDEs generate plenty of files that are **safe to regenerate** and **shouldn’t be committed** (build output, caches, user settings, OS noise).

This post covers:

- What `.gitignore` is (and isn’t)
- How Git applies ignore rules
- The syntax you need
- Practical ignore patterns for ASP.NET Core / MVC apps

---

## What `.gitignore` does (and does not do)

### ✅ What it does

- Prevents **untracked** matching files from appearing in `git status`
- Prevents `git add .` from staging those files
- Keeps your repo clean and your diffs meaningful

### ❌ What it does *not* do

- It does **not** stop tracking files that are already committed.

If you accidentally committed build output like `bin/` or `obj/`, adding it to `.gitignore` won’t remove it from the repository. You must untrack it:
```
bash
git rm -r --cached bin obj
git commit -m "Stop tracking build output"
```
---

## Where ignore rules come from

Git can read ignore rules from multiple places:

1. **Project `.gitignore` files** (committed, shared with the team)
2. **`.git/info/exclude`** (local to your clone, not committed)
3. **Global ignore file** (your machine), configured with:
```
bash
git config --global core.excludesFile ~/.gitignore_global
```
---

## How `.gitignore` rules are applied

A few rules of thumb that make `.gitignore` behavior predictable:

- Git processes patterns **line by line**
- **Later rules win** if multiple patterns match
- A `.gitignore` in a **subfolder** can override a parent folder’s rules
- Negation (`!`) can re-include something previously ignored (with a caveat)

---

## `.gitignore` syntax basics

A `.gitignore` file is plain text: **one pattern per line**.

### Comments and blank lines

Lines that start with `#` are comments. Blank lines are ignored.
```
gitignore
# Ignore build output
bin/
obj/
```
If you need a literal `#` at the beginning of a pattern, escape it:
```
gitignore
\#literal-file.txt
```
### Wildcards: `*`, `?`, and `[]`

- `*` matches any characters except `/`
- `?` matches a single character except `/`
- `[]` matches a character set/range

Examples:
```
gitignore
*.log        # any .log file
file?.txt    # file1.txt, fileA.txt (not file10.txt)
file[0-9].txt
```
### Directories (trailing slash)

A trailing slash means “directory only”:
```
gitignore
node_modules/
bin/
obj/
```
### Root-anchored patterns (leading slash)

Without a leading slash, patterns can match anywhere in the repo:
```
gitignore
bin/     # ignores bin/ anywhere
```
Anchor to the repository root with `/`:
```
gitignore
/bin/    # ignores only the root bin/
```
### `**` for “any depth”

`**` can match across directory boundaries. This is common in larger solutions:
```
gitignore
**/bin/
**/obj/
```
### Negation (`!`) — “un-ignore” patterns

Use `!` to re-include something previously ignored:
```
gitignore
*.json
!appsettings.json
```
**Important caveat:** If a directory is ignored, Git won’t usually scan inside it to find re-included files unless the directory itself is un-ignored too. Example:
```
gitignore
secrets/
!secrets/
!secrets/README.md
```
If you need a literal `!` at the start of a pattern, escape it:
```
gitignore
\!literal-leading-bang.txt
```
---

## A practical `.gitignore` baseline for .NET web apps

This is a solid starting point for **ASP.NET Core / MVC** projects:
```
gitignore
# Build output
bin/
obj/
artifacts/
publish/

# User-specific / IDE files
.vs/
.vscode/
.idea/
*.user
*.suo
*.rsuser

# Logs
*.log

# OS noise
.DS_Store
Thumbs.db
```
---

## What about `appsettings.json`?

This is a common team decision.

### Option A (common): commit defaults; ignore only local development overrides
```
gitignore
appsettings.Development.json
```
### Option B: don’t commit real settings; commit a template instead
```
gitignore
appsettings.json
!appsettings.example.json
```
Pick one approach and write it down in your `README.md` so teammates (and future you) don’t have to guess.

---

## Debugging: “Why is this file ignored?”

When `.gitignore` doesn’t behave how you expect, this command tells you exactly **which rule** matched and where it came from:
```
bash
git check-ignore -v path/to/file
```
---

## Final tips

- Keep shared rules in a **root `.gitignore`**
- Ignore generated output (`bin/`, `obj/`, `publish/`) and IDE caches
- Remember: `.gitignore` is about cleanliness, not security—don’t commit secrets in the first place

---
```
