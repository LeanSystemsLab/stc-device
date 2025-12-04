# Git Workflow Guide

This document defines the standard workflow for working with feature branches in this project.

## 1. Start Working on a Feature

Create a new branch for each task or ticket.  
Branch names should reflect the ticket ID and topic (e.g., `92-add-sensor`).

```bash
# Always start from latest main
git checkout main
git pull origin main

# Create and switch to a new feature branch
git switch -c 92-add-sensor
````

If you already made local changes before creating the branch:

```bash
git stash -u
git switch -c 92-add-sensor
git stash pop
```

## 2. Working and Committing

Commit often. Small commits make it easier to roll back or isolate issues.
Use clear, consistent messages — preferably following the **Conventional Commits** style:

```
feat(sensor): add temperature reading
fix(ui): correct label alignment
refactor(core): simplify data parser
```

Typical workflow:

```bash
git add -A
git commit -m "feat(sensor): add temperature reading
Related to #92" # Optionally adds information to issue.
git push -u origin 92-add-sensor   # only necessary the first time, after that:
git push # once remote branch exists, this is sufficient.
```

If you make small fixups after earlier commits, use autosquash markers:

```bash
git commit --fixup <commit-hash>
```


## 3. Preparing for Merge

Before merging, rebase your branch onto the latest `main` to ensure a clean history.

```bash
git fetch origin
git rebase origin/main
```

Resolve any conflicts, test again, and push the updated branch:

```bash
git push --force-with-lease
```

Create a **Pull Request** (or **Merge Request**) and request review.

## 4. Merging into Main

After approval, merge as **squash** (which is preferred, **fast-forward** is alternatively allowed)

### Preferred: Squash-merge (single commit on main)

In the Git hosting UI, choose *“Squash and merge”*.

```bash
git checkout main
git merge --squash 92-add-sensor
git commit -m "feat(sensor): add temperature reading
Fixes #92"
git push origin main
```

### Option A: Fast-forward (keeps commit history)

```bash
git checkout main
git pull origin main
git merge --ff-only 92-add-sensor
git push origin main
```

## 5. Cleanup

After merging, remove the feature branch locally and remotely.

```bash
git branch -d 92-add-sensor
git push origin --delete 92-add-sensor
```

If deletion fails due to unmerged commits, use:

```bash
git branch -D add-sensor
```

Your host (GitHub, GitLab, etc.) can also **auto-delete branches** after merge.