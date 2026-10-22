# Git Branches: Complete Interview Guide with Real Examples

A Git branch is one of the most important topics for Git interviews. Interviewers often ask about branch creation, switching, merging, rebasing, conflicts, remote branches, and how to recover from mistakes.

I'll explain each concept in simple language, with practical commands, real project examples, and commonly asked interview questions with answers.

## 1. What is a Git branch?

A branch is a movable pointer to a commit in Git. It lets you work on a feature, bug fix, or experiment without directly changing another branch's work.

Imagine you are developing an e-commerce application.

Example: Branching in an e-commerce project

main

Stable production code

feature/cart

Develop shopping cart functionality

bugfix/login

Fix login validation issues

Both branches can be developed independently and later merged into main.

For example, your team has three tasks:

- Developer A builds the shopping cart on `feature/cart`.
- Developer B fixes a login bug on `bugfix/login`.
- The team keeps `main` stable until the changes are reviewed and merged.

This is why branches are essential for collaborative development.

## 2. Essential Git branch commands

These are the commands you should know before attending a Git interview.

| Command | Purpose |
| --- | --- |
| `git branch` | List local branches |
| `git branch -a` | List local and remote-tracking branches |
| `git branch feature/cart` | Create a branch |
| `git switch feature/cart` | Switch to an existing branch |
| `git switch -c feature/cart` | Create and switch to a new branch |
| `git branch -d feature/cart` | Delete a merged local branch |
| `git branch -D feature/cart` | Force-delete a local branch |
| `git status` | Show current branch and working-tree state |
| `git log --oneline --graph --all` | Visualize commit history and branches |

Older Git workflows also use `git checkout` for switching and creating branches. Modern Git provides `git switch` for switching and `git restore` for restoring files, making their purposes clearer.

### Real example: Create your first feature branch

bash
```
# 1. Clone the project
git clone https://example.com/shop.git
cd shop

# 2. Check your current branch
git branch

# 3. Create and switch to a feature branch
git switch -c feature/cart

# 4. Make a change and commit it
# Edit cart.py, for example
git add cart.py
git commit -m "Add shopping cart feature"

# 5. View the branches and history
git log --oneline --graph --all
```

The URL above is illustrative; use your actual repository URL.

Notice that `git switch -c feature/cart` creates the branch at your current commit and switches to it. The new commit is made on `feature/cart`, not on `main`.

## 3. Types of branches in Git

Git does not enforce a fixed set of branch types. Teams choose branch names and workflows according to their needs.

| Branch type | Example | Purpose |
| --- | --- | --- |
| Main branch | `main` | Stable or production-ready code |
| Feature branch | `feature/payment` | Develop a new feature |
| Bug-fix branch | `bugfix/login-error` | Fix a defect |
| Release branch | `release/2.1.0` | Prepare a release |
| Hotfix branch | `hotfix/payment-crash` | Fix an urgent production issue |
| Remote-tracking branch | `origin/main` | Track the state of a remote branch |

A common naming convention is `feature/`, `bugfix/`, `hotfix/`, and `release/`. These prefixes are conventions, not special Git syntax.

## 4. Create, switch, and delete branches

### Create a branch

bash
```
git branch feature/profile
```

This creates a branch but does not switch to it.

### Switch branches

bash
```
git switch feature/profile
```

This moves your working context to the existing branch.

### Create and switch in one command

bash
```
git switch -c feature/profile
```

This is the most convenient option when starting a new task.

### Delete a branch

bash
```
git switch main
git branch -d feature/profile
```

Git normally prevents `-d` from deleting a branch whose changes have not been merged into its upstream or current branch.

bash
```
git branch -D feature/profile
```

`-D` forces deletion even if the branch contains unmerged commits. Use it carefully: it removes the branch reference, although the commits may remain recoverable for a while.

Interview question: What is the difference between `git branch` and `git switch`?

Answer:

- `git branch` creates, lists, or deletes branch references.
- `git switch` changes the currently checked-out branch.
- `git switch -c` performs both creation and switching.

## 5. Git merge — one of the most asked interview topics

Merging combines changes from one branch into another.

Suppose you have this history:

text
```
A---B---C  main
     \
      D---E  feature/cart
```

You want to bring the cart feature into `main`.

bash
```
git switch main
git merge feature/cart
```

Git may create a merge commit if the histories have diverged.

text
```
A---B---C-------M  main
     \         /
      D---E---    feature/cart
```

Here, `M` is the merge commit.

### What is a fast-forward merge?

If `main` has not advanced since the feature branch was created, Git can simply move the `main` pointer forward.

Before:

text
```
A---B  main
     \
      C---D  feature/cart
```

After merging into `main`:

text
```
A---B---C---D  main, feature/cart
```

No merge commit is necessary.

### Real-world workflow

bash
```
# Start a feature
git switch main
git switch -c feature/payment

# Make and commit changes
git add .
git commit -m "Add payment integration"

# Merge the feature
git switch main
git merge feature/payment

# Push the updated main branch
git push origin main
```

In a team, you would normally push the feature branch and merge it through a pull request or merge request, according to your team's review process.

Interview question: What is the difference between fast-forward and three-way merge?

Answer:

- Fast-forward merge: Git moves the target branch pointer forward because no new target-branch commits need to be reconciled.
- Three-way merge: Git uses the common ancestor and the two branch tips to combine changes. A merge commit is normally created unless a special strategy or option changes the result.

A useful command is:

bash
```
git merge --no-ff feature/payment
```

This requests a merge commit even when a fast-forward is possible, which can preserve a visible feature-branch boundary in the history.

## 6. Git rebase — another favorite interview topic

Rebase moves or replays your branch's commits onto a new base commit.

Suppose `main` has received new changes while you are working on a feature.

Before:

text
```
A---B---C  main
     \
      D---E  feature/cart
```

Run:

bash
```
git switch feature/cart
git rebase main
```

After a successful rebase:

text
```
A---B---C  main
         \
          D'---E'  feature/cart
```

`D'` and `E'` represent rewritten commits. They have new commit IDs because their parent history has changed.

### Merge versus rebase

### Merge

Preserves existing commit identities and branch history; may create a merge commit.

bash
```
git switch feature/cart
git merge main
```

### Rebase

Replays your feature commits on top of the latest base, creating new commit identities.

bash
```
git switch feature/cart
git rebase main
```

### When should you use each?

- Use merge when you want to integrate histories without rewriting existing commits.
- Use rebase to keep a feature branch's history linear and up to date.
- Avoid rebasing commits that other people already depend on, unless the team has explicitly coordinated the rewrite.

Interview question: Why is rebasing a shared branch dangerous?

Answer: Rebasing rewrites commit history. Other developers may still have the original commits, so their histories can diverge and require reconciliation. For a private feature branch, rebase is often convenient. For shared branches, coordinate before rewriting history.

## 7. Merge conflicts: how to resolve them

A merge conflict happens when Git cannot automatically reconcile changes. For example, two developers modify the same lines of `app.py`.

Git may display:

python
```
<<<<<<< HEAD
message = "Welcome"
=======
message = "Welcome back"
>>>>>>> feature/greeting
```

The conflict markers identify the competing versions. You must decide what the final code should be.

### Steps to resolve a conflict during a merge

bash
```
git switch main
git merge feature/greeting

# Edit app.py and remove conflict markers
# Keep the correct final implementation

git add app.py
git commit
```

Depending on your Git version and workflow, `git merge --continue` can also be used after resolving and staging the conflict.

### If the conflict happens during a rebase

bash
```
# Resolve the conflicted files
git add app.py
git rebase --continue
```

To abandon the rebase:

bash
```
git rebase --abort
```

To abandon a merge that is still in progress:

bash
```
git merge --abort
```

Interview question: Does Git always create conflicts when two people edit the same file?

Answer: No. Git can automatically combine changes in the same file when they affect separate, compatible areas. Conflicts happen when Git cannot safely determine how to combine the changes, often because edits overlap.

## 8. Remote branches: `origin/main`, `fetch`, and `pull`

A local branch exists in your local repository. A remote-tracking branch records your repository's view of a branch on a remote such as GitHub or GitLab.

For example:

- `main` — your local branch.
- `origin/main` — your local record of the remote's `main` branch.
- `origin` — a conventional name for a remote repository.

### Important commands

bash
```
# List local branches
git branch

# List remote-tracking branches
git branch -r

# List all branches
git branch -a

# Download remote updates without integrating them
git fetch origin

# Download updates and integrate them into the current branch
git pull origin main

# Publish your feature branch
git push -u origin feature/cart
```

### What is the difference between fetch and pull?

`git fetch` downloads new objects and updates remote-tracking references. It does not merge or rebase those changes into your current branch.

`git pull` fetches updates and then integrates them into the current branch, normally using merge or a configured rebase strategy.

Example:

text
```
Before fetch:

Local main:   A---B
Origin main:  A---B---C

After fetch:

Local main:   A---B
Origin main:  A---B---C
```

Your local `main` has not moved yet. After you integrate the changes, it can advance to commit `C`.

Interview question: What does `git push -u origin feature/cart` do?

Answer: It pushes your local feature branch to the remote and sets an upstream tracking relationship. Afterward, you can usually run `git push` and `git pull` on that branch without specifying the remote and branch every time.

## 9. Delete a remote branch

After a feature has been merged, teams often delete its remote branch to keep the repository organized.

bash
```
# Delete the remote feature branch
git push origin --delete feature/cart

# Delete the local feature branch
git branch -d feature/cart
```

These are separate operations. Deleting a local branch does not delete the remote branch.

Interview question: If I delete a branch, do I lose all its commits?

Answer: Deleting a branch removes the reference pointing to its latest commit. Commits that are reachable through other references remain accessible. Otherwise-unreachable commits may remain recoverable for a period through Git's object storage and reflog, but you should not rely on that indefinitely.

## 10. Cherry-pick: take one commit from another branch

Suppose a developer fixes a critical login bug on `feature/login`, but you need that exact fix in `release/2.1.0` without merging all the feature work.

Use `git cherry-pick`.

bash
```
# Find the required commit
git log --oneline feature/login

# Switch to the release branch
git switch release/2.1.0

# Apply the selected commit
git cherry-pick abc1234
```

`abc1234` is an illustrative commit ID.

Git creates a new commit on the current branch that applies the selected change. Its commit ID will normally differ from the original.

Interview question: What is the difference between merge and cherry-pick?

- Merge: Integrates a branch's history, including its relevant commits.
- Cherry-pick: Applies the changes introduced by selected commit(s) to the current branch.

Cherry-picking is useful for targeted fixes, but it can create duplicate changes in history if used carelessly.

## 11. Stash: switch branches without committing unfinished work

Imagine you are halfway through a feature and your manager asks you to fix a production bug immediately.

You have modified files but are not ready to commit them.

bash
```
# Temporarily save tracked-file changes
git stash

# Switch to the hotfix branch
git switch hotfix/login-crash

# Make and commit the urgent fix
git add .
git commit -m "Fix login crash"

# Return to your feature branch
git switch feature/cart

# Restore the stashed changes
git stash pop
```

Useful related commands:

bash
```
git stash list
git stash apply
git stash drop
git stash -u
```

- `git stash apply` restores a stash without removing it from the stash list.
- `git stash pop` applies a stash and normally removes it if application succeeds.
- `git stash -u` also includes untracked files.

Interview question: Is stash the same as commit?

Answer: No. A commit becomes part of branch history. A stash temporarily stores working-tree changes outside the normal branch commit history.

## 12. `HEAD`, branch pointers, and detached HEAD

`HEAD` normally points to your currently checked-out branch, which in turn points to a commit.

For example:

text
```
HEAD -> main -> C
```

If you switch to `feature/cart`:

text
```
HEAD -> feature/cart -> E
```

You can inspect the current position using:

bash
```
git status
git log --oneline -5
git rev-parse HEAD
```

### What is detached HEAD?

You are in a detached HEAD state when you check out a commit directly rather than a branch.

bash
```
git switch --detach abc1234
```

This is useful for inspecting old versions or testing a past commit. If you make commits here, they will not automatically belong to a named branch.

To preserve any new work, create a branch:

bash
```
git switch -c experiment/old-version
```

Interview question: Is detached HEAD an error?

Answer: No. It is a valid Git state. The risk is making commits and then losing track of them because no branch points to them. Creating a branch preserves the work under a named reference.

## 13. Recover from mistakes when working with branches

This is an important interview area because interviewers often ask scenario-based questions.

### Scenario A: I created a branch from the wrong branch. How do I fix it?

Suppose you accidentally created `feature/report` from an outdated branch.

First, check whether the branch has commits you need to preserve.

If it has no unique commits and no work you need to keep:

bash
```
git switch main
git branch -D feature/report
git switch -c feature/report
```

If it already has useful commits, do not delete it blindly. You can create a backup and rebase it onto the correct base:

bash
```
git branch backup/report
git switch feature/report
git rebase --onto main old-base feature/report
```

Replace `old-base` with the actual commit that represents the old starting point. The exact rebase command depends on your history.

### Scenario B: I committed to the wrong branch. What should I do?

Suppose you committed a new feature directly to `main`, but the change belongs on `feature/cart`.

For a recent commit that has not been shared, you can create a feature branch at the current commit and move `main` back one commit:

bash
```
# Preserve the commit on a new branch
git branch feature/cart

# Move main back one commit
git reset --hard HEAD~1
```

Warning: `git reset --hard` discards tracked working-tree changes and moves the current branch. Do not use it if you have other uncommitted work or have already shared the commit without understanding the consequences.

For a shared commit, a safer alternative may be to use `git revert` rather than rewriting history.

### Scenario C: I accidentally deleted a branch. Can I recover it?

Often, yes, if the branch's last commit is still available.

bash
```
# Find recent branch positions
git reflog

# Re-create the branch at the recovered commit
git branch feature/cart abc1234
```

Replace `abc1234` with the actual commit ID from the reflog.

### Scenario D: I want to undo a commit without deleting history.

Use:

bash
```
git revert abc1234
```

This creates a new commit that reverses the changes introduced by the selected commit, subject to possible conflicts.

Compare the two:

| Command | Effect |
| --- | --- |
| `git revert` | Creates a new commit that undoes a change |
| `git reset` | Moves the current branch reference |
| `git reflog` | Shows recent movements of references such as HEAD |
| `git restore` | Restores file contents in the working tree or index |

Interview question: What is the safest way to undo a commit that has already been pushed?

Answer: Usually `git revert`, because it preserves existing shared history and creates a reversing commit. `git reset` followed by a force push can disrupt other developers' work.

## 14. Branch protection and pull requests

In professional projects, developers usually should not push directly to production branches.

A typical workflow looks like this:

1. Create a feature branch

`git switch -c feature/payment`

2. Commit and push changes

`git push -u origin feature/payment`

3. Open a pull request

Code review, tests, and approval

4. Merge into main

Use the team's approved merge strategy

Branch protection rules can require:

- Pull-request reviews.
- Passing automated tests.
- Successful CI/CD checks.
- Restrictions on direct pushes.
- Additional approvals for sensitive branches.

Interview question: What is the difference between a branch and a pull request?

Answer: A branch is a Git reference used to organize work. A pull request is a collaboration feature provided by platforms such as GitHub or GitLab to review and discuss proposed changes before merging them.

## 15. Most-asked Git branch interview questions and answers

Use this section for quick revision.

1\. What is a Git branch?A lightweight, movable reference to a commit. It allows independent development without immediately changing another branch.  2\. How do you create and switch to a branch?  3\. What is the difference between main and origin/main?  4\. What is the difference between merge and rebase?  5\. What is a fast-forward merge?  6\. What is a merge conflict?  7\. How do you list all branches?  8\. How do you delete a local branch?  9\. How do you delete a remote branch?  10\. What is git fetch versus git pull?  11\. What is cherry-pick?  12\. What is detached HEAD?  13\. How do you recover a deleted branch?  14\. What does git push -u do?  15\. What is the difference between git reset and git revert?  16\. Can two branches point to the same commit?  17\. Does creating a branch copy all files?  18\. What is the best practice for feature branches?  19\. What happens if two branches modify the same file?  20\. What is the difference between checkout and switch?

## 16. Practice exercise: simulate a real project

Try this workflow in a test repository. It covers branch creation, merging, remote publishing, and cleanup.

bash
```
# 1. Create a repository
git init branch-practice
cd branch-practice

# 2. Create the first commit
git switch -c main
echo "Online Store" > README.md
git add README.md
git commit -m "Initial commit"

# 3. Create a feature branch
git switch -c feature/cart
echo "Shopping cart feature" > cart.txt
git add cart.txt
git commit -m "Add shopping cart"

# 4. Switch back and add another change
git switch main
echo "Login feature" > login.txt
git add login.txt
git commit -m "Add login"

# 5. Merge the feature
git merge feature/cart

# 6. Visualize the history
git log --oneline --graph --all

# 7. Delete the merged branch
git branch -d feature/cart
```

You should see `main` contain both the login and shopping cart changes. Because `main` and `feature/cart` diverged, Git will normally create a merge commit in this exercise.

## 17. Quick revision cheat sheet

| If the interviewer asks... | Remember |
| --- | --- |
| Create a branch | `git switch -c feature/x` |
| Switch a branch | `git switch main` |
| Merge a branch | `git merge feature/x` |
| Rebase a branch | `git rebase main` |
| Update remote references | `git fetch origin` |
| Publish a branch | `git push -u origin feature/x` |
| Resolve a merge conflict | Edit files, `git add`, complete merge |
| Temporarily save work | `git stash` |
| Apply one commit | `git cherry-pick <commit>` |
| Recover a lost branch | `git reflog` |
| Undo a shared commit | Usually `git revert <commit>` |
| See branch history | `git log --oneline --graph --all` |

### What should you learn next?

After branches, focus on merge vs rebase, conflict resolution, reset vs revert, and reflog recovery. These topics are especially useful for practical Git interviews because they test whether you understand Git history, not just command syntax.