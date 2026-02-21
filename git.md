# Git Workflow (Lazygit + Conventional Commits + PR Safety)

This document describes the day‑to‑day workflow for making, validating, and merging changes.

---

## Tooling

* **lazygit** for staging, reviewing changes, and branching
* **External editor** (VSCode/Neovim/Nano) for writing commit messages
* **Global commit template** for Conventional Commits
* **`commit-msg` hook** to enforce commit format
* **GitHub PR template** for reviewability
* **Danger.js** to enforce PR safety rules

---

## Branching Rules

1. Work always starts from `main`.
2. Every change must be tied to a GitHub issue.
3. Branch name must include the issue number.

Examples:

```
feat/42-cors-support
fix/88-sqs-retry
refactor/12-memory-allocator
```

Create branch:

```
git switch main
git pull
git switch -c feat/<issue>-short-description
```

---

## Making Changes

1. Edit code
2. Open lazygit

```
lazygit
```

3. Stage files (press `space` on files)
4. Review diff inside lazygit

---

## Committing (Required Process)

> The inline `c` commit in lazygit is disabled.
> All commits must use the external editor.

Inside lazygit:

```
Shift + C
```

This opens the configured Git editor with the commit template.

### Commit Message Format (Conventional Commits)

```
type(scope): short description
```

Examples:

```
feat(api): add cors headers
fix(auth): prevent token leak
refactor(memory): simplify allocator
```

The `commit-msg` hook validates this automatically. Invalid commits are rejected before creation.

---

## Commit Template Sections

The editor will include prompts for:

* Why is this change needed?
* What changed?
* How was this tested?
* Risks / side effects

Fill them out concisely. They improve future debugging and code review.

---

## Pushing Changes

After committing:

```
git push -u origin <branch>
```

Subsequent pushes:

```
git push
```

---

## Pull Request Process

Create PR:

```
gh pr create
```

The PR template must be completed.

### Required for PR to pass Danger checks

* Linked issue (`Closes #123`)
* Branch includes issue number
* Conventional Commit title
* "How can a reviewer verify?" section with numbered steps
* At least one "System Impact" checkbox selected

---

## Review Philosophy

A PR is not just code — it is a **change proposal**.

A reviewer must be able to answer:

1. What behavior changed?
2. How do I run it?
3. What could break?

---

## Merge

After CI passes and review is approved:

* Squash merge into `main`
* Issue automatically closes via `Closes #...`

---

## Summary Flow

```
Issue → Branch → Code → Lazygit Stage → External Commit Editor → Hook Validation → Push → PR Template → Danger → Review → Merge
```

This workflow ensures:

* Traceable history
* Reproducible reviews
* Safe merges
* High‑quality commit history
