# 🧑‍💻 Git Feature Branch Workflow (Rebase-First, Team-Safe)

## Step 1: Always Start Fresh From Working Branch (I.e. staging, dev)
```bash
git checkout dev
git pull origin dev --rebase
```

## Step 2: Create a New Feature Branch
```bash
git checkout -b feat/my-new-feature
```

| Branch   | Purpose                  |
|----------|--------------------------|
| main or prod    | Production-ready code     |
| staging or dev  | Pre-release QA branch     |
| feat/*          | New features              |
| fix/*           | Bug fixes                 |
| chore/*         | Setup or config work      |

## Step 3: Work on Your Feature
```bash
git add .
git commit -m "add my feature" (make sure it fits CI check's requirements i.e all lowercase, capitalize, etc.)
```
> Even if not done with work, it's good CI/CD principles to push code at the end of the day regardless of its state.
> You can say it in your commit message as work in progress: `wip: updating auth handling`

## Step 4: Push to GitHub (First Time)
```bash
git push -u origin feat/my-new-feature —force-with-lease
```

---

## Step 5: Pull Coworker's Changes into Your Feature Branch (If Necessary, BUT Make Sure You Do)

### If You **Already Made Commits**:
```bash
git fetch origin
git rebase origin/feat/my-new-feature
git push --force-with-lease
```

### If You **Haven't Made Changes Or Committed Yet**:
```bash
git pull origin feat/my-new-feature
```
> ✅ This will fast-forward your branch cleanly without merge commits.

---

## Step 6: Sync with Latest Working Branch (Every Morning Or When Working Branch Updates)

### If You **Already Made Commits**:
```bash
git fetch origin
git rebase origin/dev
git push --force-with-lease
```

### If You **Made Changes Or Committed Yet**:
```bash
git pull origin dev --rebase
```
> ✅ This updates your branch cleanly before pushing to PR.

---

## Step 7: Push Rebasing Changes to GitHub
```bash
git push --force-with-lease
```
> ⚠️ Always use `--force-with-lease` after a rebase since rebasing rewrites history.

---

# 🚀 Key Rules Summary
- ✅ Always rebase onto origin/dev before making a PR.
- ❌ Never git merge into your feature branch since git merge will make a copy of the other branch’s commit history and pollute it.
- ✅ Always use --force-with-lease after rebasing to overwrite the divergent history.
- ✅ When collaborating on the same branch:
  - Always pull before committing for a clean fast-forward.
  - Rebase + force-push if you and your coworker have diverging commits. (Please communicate first, this will overwrite any commits that do not exist in your local history.)
  - Force pushing is technically only needed after rebasing but still good practice to —-force-with-lease every push
