# Git — Complete Revision Notes

## 1. Git Fundamentals

- **Git** → Distributed version control system that tracks changes in files.
- **Repository** → Project tracked by Git (`.git` directory).
- **Working directory** → Files you actively edit.
- **Staging area (Index)** → Changes prepared for the next commit.
- **Commit** → Snapshot of staged changes.
- **Local repository** → Git history stored on your computer.
- **Remote repository** → Repository hosted on GitHub or another server.

**Basic Git flow:**

```text
Working Directory → Staging Area → Local Repository → Remote Repository
        git add        git commit           git push
```

## 2. Making Commits — Chapter 3

| Command | Purpose |
|---|---|
| `git init` | Initialize Git repository |
| `git status` | Check working directory and staging area |
| `git add file` | Stage a file |
| `git add .` | Stage changes under current directory |
| `git commit -m "msg"` | Save staged changes |
| `git log` | View commit history |
| `git log --oneline` | Compact commit history |
| `git diff` | View unstaged changes |
| `git diff --staged` | View staged changes |

```bash
git status
git add README.md
git commit -m "Add README"
git log --oneline
```

**Remember:** `git add` stages changes; `git commit` records them in local history.

## 3. Branches — Chapter 4

- **Branch** → Movable pointer to a commit.
- Allows independent development without directly modifying `main`.
- **HEAD** → Usually points to the currently checked-out branch.

| Command | Purpose |
|---|---|
| `git branch` | List local branches |
| `git branch feature` | Create branch |
| `git switch feature` | Switch branch |
| `git switch -c feature` | Create and switch |
| `git branch -d feature` | Delete merged branch |
| `git log --oneline --graph --all` | Visualize history |

```bash
git switch -c feature/login
# Make changes
git add .
git commit -m "Add login feature"
```

**Remember:** A commit links to its parent commit, forming Git history.

## 4. Merging — Chapter 5

**Merge** → Integrates changes from one branch into another.

Two types:

| Type | Meaning |
|---|---|
| Fast-forward | Target branch moves forward; no separate merge commit |
| Three-way merge | Combines divergent histories using a common ancestor; usually creates a merge commit |

```bash
git switch main
git merge feature/login
```

**Remember:** Merge changes the branch you are currently on.

## 5. Remote Repositories — Chapter 7

- **Remote** → Another repository, commonly hosted on GitHub.
- **origin** → Conventional name for the default remote.
- **Push** → Upload local commits to remote.
- **Fetch** → Download remote history without merging.
- **Pull** → Fetch + integrate changes.

| Command | Purpose |
|---|---|
| `git remote -v` | View remote URLs |
| `git remote add origin URL` | Add remote |
| `git push -u origin branch` | Push and set upstream |
| `git push` | Push to configured upstream |
| `git fetch origin` | Download remote updates |
| `git pull` | Fetch and integrate updates |
| `git clone URL` | Download and initialize a local copy |

```bash
git remote add origin https://github.com/user/repo.git
git push -u origin feature/login
```

**Remember:** `-u` sets the upstream branch, making future `git push` and `git pull` simpler.

## 6. Three-Way Merges — Chapter 9

- Occurs when branches have **diverged** (different commits after a common ancestor).
- Git uses the common ancestor and both branch tips to combine changes.
- Usually creates a merge commit with two parents.

**Fetch vs Pull:**

```bash
git fetch origin      # Download without integrating
git pull origin main  # Download + integrate main
```

**Remember:** `git pull` may merge or rebase depending on Git configuration.

## 7. Merge Conflicts — Chapter 10

**Merge conflict** → Git cannot automatically reconcile certain changes.

Common causes:
- Both branches modify overlapping parts of the same file.
- One branch deletes a file while another modifies it.

**Conflict markers:**

```text
<<<<<<< HEAD
Your changes
=======
Incoming changes
>>>>>>> feature/login
```

**Resolving conflicts:**

```bash
git status
# Edit conflicted files and remove markers
git add conflicted-file.txt
git commit
```

For a merge in progress, `git commit` completes the merge after resolution.

**Abort merge:**

```bash
git merge --abort
```

**Remember:** Resolve content → Stage resolved files → Complete merge.

## 8. Rebasing — Chapter 11

**Rebase** → Reapplies commits from your branch on top of another branch.

```bash
git switch feature/login
git rebase main
```

**Merge vs Rebase:**

| Merge | Rebase |
|---|---|
| Preserves existing commit history | Rewrites the rebased commits |
| May create merge commit | Creates a more linear history |
| Good for shared history | Useful for updating local feature branches |

**Resolve conflicts during rebase:**

```bash
git status
# Fix conflicted files
git add conflicted-file.txt
git rebase --continue
```

Other commands:

```bash
git rebase --abort      # Cancel rebase
git rebase --skip       # Skip current commit
```

**Golden Rule:** Avoid rebasing commits already shared with others, because rebase changes commit hashes.

## 9. Important Git Concepts

| Concept | Meaning |
|---|---|
| Untracked | File not tracked by Git |
| Modified | Tracked file changed |
| Staged | Changes added to index |
| Committed | Changes saved in history |
| HEAD | Current checkout reference |
| Commit hash | Unique identifier of a commit |
| Upstream | Remote branch linked to local branch |
| Merge commit | Commit joining multiple histories |
| Fast-forward | Move branch pointer without a merge commit |
| Rebase | Replay commits on another base |

## 10. Everyday Git Workflow

```bash
# Start from updated main
git switch main
git pull origin main

# Create feature branch
git switch -c feature/task

# Work and commit
git status
git add .
git commit -m "Complete task"

# Push branch
git push -u origin feature/task
```

Then create a **Pull Request (PR)** on GitHub to merge into `main`.

**Note:** If `main` is protected, you cannot push directly to it. You must raise a PR.

## 11. Quick Recall

- `git init` → Initialize.
- `git clone` → Copy repository.
- `git status` → Check changes.
- `git add` → Stage.
- `git commit` → Save snapshot.
- `git log` → History.
- `git diff` → Changes.
- `git branch` → Manage branches.
- `git switch` → Change branches.
- `git merge` → Combine branches.
- `git remote -v` → Remotes.
- `git fetch` → Download changes.
- `git pull` → Download + integrate.
- `git push` → Upload commits.
- `git rebase` → Replay commits.
- `git merge --abort` → Cancel merge.
- `git rebase --continue` → Continue rebase.
- `git rebase --abort` → Cancel rebase.
- `git log --oneline --graph --all` → Visualize history.

**Core takeaway:** Git tracks changes through staging and commits. Branches enable independent work, merging integrates changes, and rebasing rewrites commit history. Master the daily workflow: **status → add → commit → push → PR**.