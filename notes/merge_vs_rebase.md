# Git Merge vs Rebase — Complete Interview Guide

Think of me as your Git mentor. Let's learn `git merge` vs `git rebase` from the basics to real-world scenarios, with diagrams, commands, practical examples, common mistakes, and frequently asked interview questions with answers.

## 1. What are Git Merge and Git Rebase?

Both commands help you integrate changes from one branch into another, but they handle Git history differently.

- `git merge` combines changes from two branches and usually creates a merge commit when the branches have diverged.
- `git rebase` moves your branch's commits onto a new base commit, creating new commit IDs and producing a more linear history.

Imagine you are working on a feature while another developer adds changes to the `main` branch.

## Git Merge

Preserves the original branch history.

What is Git merge and how does it work?

text
```
A---B---C---------M  main
     \           /
      D---E-----/
           feature
```

The merge commit `M` joins the histories of both branches.

## Git Rebase

Replays your feature commits on top of the latest base.

Git rebase isn’t scary. You just never had it explained right | by \<devtips/\> | Run.it.Bare | Medium

text
```
A---B---C---D'---E'  feature
        main
```

Here, `D'` and `E'` are new commits based on `C`. The original commits `D` and `E` are replaced in the resulting branch history.

The key interview point: merge preserves branch history; rebase rewrites the commits being replayed to create a new history.

## 2. Understand the real scenario

Suppose your team is developing an e-commerce application.

- The `main` branch contains the latest production-ready code.
- You create a feature branch called `feature/cart`.
- You add two commits: one for adding a shopping cart and another for calculating the total.
- Meanwhile, another developer adds a payment validation feature to `main`.

Your history initially looks like this:

text
```
          D---E  feature/cart
         /
A---B---C---F    main
```

- `A`, `B`, `C`: existing project commits.
- `D`, `E`: your feature commits.
- `F`: another developer's new commit.

Your feature branch is now behind `main`. You want to integrate `F` into your feature branch before opening or completing a pull request.

There are two common ways to do that.

## 3. Git Merge: step-by-step with a real example

### What does `git merge` do?

Suppose you are currently on `feature/cart` and want to integrate the latest changes from `main`.

bash
```
git switch feature/cart
git merge main
```

Git takes the changes from `main` and integrates them into your current branch.

### Before merge

text
```
          D---E  feature/cart
         /
A---B---C---F    main
```

### After merge

text
```
          D---E-------M  feature/cart
         /           /
A---B---C---F--------   main
```

The new commit `M` is the merge commit. It connects the two histories.

### Try it yourself

bash
```
# Create a feature branch
git switch -c feature/cart

# Make a change and commit it
echo "Shopping cart added" > cart.txt
git add cart.txt
git commit -m "Add shopping cart"

# Imagine another developer has committed to main.
# Switch back to your feature branch.
git switch feature/cart

# Integrate main into your feature branch
git merge main

# Inspect the history
git log --oneline --graph --decorate --all
```

The commands assume that the relevant `main` commits already exist locally.

### Advantages of merge

- Preserves the original commit history.
- Clearly records when branches were integrated.
- Is generally safe for shared branches because it does not rewrite existing commits.
- Works well when the team wants to preserve the full development history.

### Disadvantages of merge

- Can produce many merge commits in a busy repository.
- The history can become harder to read when many branches are merged frequently.
- You may need to resolve merge conflicts.

Interview tip: A merge commit typically has two parents when merging two diverged branches. However, if the target branch is already an ancestor of your current branch, Git can perform a fast-forward merge without creating a merge commit.

### What is a fast-forward merge?

Before:

text
```
A---B---C  main
         \
          D---E  feature
```

If you merge `feature` into `main` and `main` has not diverged, Git can simply move the `main` pointer forward:

text
```
A---B---C---D---E  main
```

No merge commit is required.

To force a merge commit even when fast-forwarding is possible:

bash
```
git merge --no-ff feature
```

This is useful when you want to retain an explicit record of a feature branch integration.

## 4. Git Rebase: step-by-step with a real example

### What does `git rebase` do?

Rebase takes the commits unique to your current branch and replays them on top of another base commit.

Suppose the history is:

text
```
          D---E  feature/cart
         /
A---B---C---F    main
```

You are on `feature/cart` and run:

bash
```
git switch feature/cart
git rebase main
```

Git replays your feature commits on top of `F`.

After rebase:

text
```
A---B---C---F---D'---E'  feature/cart
            ^
           main
```

The commits `D'` and `E'` contain the replayed changes from `D` and `E`, but they have new commit IDs because their parent history has changed.

### Try it yourself

bash
```
git switch feature/cart
git rebase main

git log --oneline --graph --decorate --all
```

Now your feature branch includes the latest `main` history without a merge commit.

### Advantages of rebase

- Creates a clean, linear commit history.
- Makes it easier to follow the sequence of changes.
- Helps prepare a feature branch before merging it into `main`.
- Can simplify code review by removing unnecessary merge commits.

### Disadvantages of rebase

- Rewrites commit history for the commits being replayed.
- Can cause confusion if other developers depend on the original commits.
- May require resolving conflicts repeatedly, potentially once for each conflicting commit.
- Requires extra care when used on published or shared branches.

Interview tip: Rebase does not simply move existing commits. It creates replacement commits for the commits it replays, so their hashes change.

## 5. Git Merge vs Rebase: comparison table

| Feature | `git merge` | `git rebase` |
| --- | --- | --- |
| Purpose | Integrates branch histories | Replays commits onto a new base |
| History | Preserves original branch topology | Produces a more linear history |
| Merge commit | Usually, if branches diverged | Not created by rebase itself |
| Commit hashes | Existing commits remain unchanged | Replayed commits get new hashes |
| Conflict resolution | Usually during the merge operation | During the rebase, potentially at multiple commits |
| Shared branches | Generally safer | Risky if others rely on commits being rewritten |
| Best use | Integrating completed work | Updating a private feature branch |
| Undo | `git merge --abort` during an unfinished merge | `git rebase --abort` during an unfinished rebase |

One important nuance: a merge does not rewrite the existing commits, but it can still create a new commit. A rebase can preserve the changes while changing the identities and ancestry of the replayed commits.

## 6. Real-world workflow: which command should you use?

Suppose you are working on a team project with branches `main` and `feature/cart`.

### Option A: Update your feature branch using merge

bash
```
git switch feature/cart
git fetch origin
git merge origin/main
```

Use this if you want to integrate the latest remote `main` changes without rewriting your existing feature commits.

### Option B: Update your feature branch using rebase

bash
```
git switch feature/cart
git fetch origin
git rebase origin/main
```

Use this if you want a linear history before submitting a pull request.

### Option C: Finish the feature and merge it into main

Once your feature is reviewed and ready:

bash
```
git switch main
git pull --ff-only origin main
git merge --no-ff feature/cart
git push origin main
```

This creates an explicit merge commit if the merge succeeds and the branches have diverged. In a team repository, follow the team's pull-request and branch-protection process rather than pushing directly to `main` if that is required.

### My recommendation

- Private feature branch: Rebase frequently to keep it current.
- Shared or public branch: Prefer merge unless the team explicitly coordinates a history rewrite.
- Pull requests: Follow the repository's policy. A team may prefer a merge commit, squash merge, or rebase merge.

## 7. Most asked Git Merge vs Rebase interview questions and answers

These are the questions I would prioritize when preparing for a Git interview.

### Q1. What is the difference between Git merge and Git rebase?

Answer: Both integrate changes from different branches, but they handle history differently.

- `git merge` combines branch histories and may create a merge commit.
- `git rebase` replays commits onto a new base, creating replacement commits with new hashes.

Example:

bash
```
git switch feature
git merge main
```

This integrates `main` into `feature`, preserving the existing branch topology.

bash
```
git switch feature
git rebase main
```

This replays the feature commits on top of `main`, creating a linear history when possible.

### Q2. Does rebase change commit hashes?

Answer: Yes, for commits that are replayed, because the parent commit and therefore the commit content used to calculate its hash changes.

For example:

text
```
Before: A---B---C
              \
               D---E

After:  A---B---C---D'---E'
```

`D'` and `E'` represent the rebased commits. They can have different hashes from `D` and `E`.

Commits that are not rewritten generally keep their original hashes.

### Q3. Does merge change commit hashes?

Answer: Merge does not rewrite the existing commits from the two branches. It may create a new merge commit with its own hash.

Example:

text
```
A---B---C------M
     \        /
      D------E
```

The original commits remain unchanged, and `M` records the integration.

### Q4. What is a fast-forward merge?

Answer: A fast-forward merge occurs when the target branch has not diverged from the branch being merged. Git can move the target branch pointer forward without creating a merge commit.

bash
```
git switch main
git merge feature
```

To prevent fast-forwarding and retain a merge commit:

bash
```
git merge --no-ff feature
```

### Q5. Can you rebase a shared branch?

Answer: Technically, yes, but you should generally avoid rebasing a branch that other developers use.

Rebasing rewrites commit history. Other developers may have the original commits locally, resulting in divergent histories and extra reconciliation work.

Best practice: Rebase private feature branches; coordinate carefully before rewriting shared history.

### Q6. What happens when a conflict occurs during merge or rebase?

Answer: Git pauses the operation so you can resolve conflicting changes.

For a merge:

bash
```
git merge main

# Resolve the conflicts in your editor
git add <resolved-file>
git commit
```

For a rebase:

bash
```
git rebase main

# Resolve the conflicts in your editor
git add <resolved-file>
git rebase --continue
```

If you want to cancel the operation:

bash
```
git merge --abort
```

or

bash
```
git rebase --abort
```

The key difference is that a rebase may stop at multiple commits, so you may need to resolve conflicts more than once.

### Q7. How do you undo a rebase?

Answer: If the rebase is still running, use:

bash
```
git rebase --abort
```

If it has completed, you may be able to return to the previous branch position using the reflog:

bash
```
git reflog
git reset --hard <previous-commit>
```

Be careful: `git reset --hard` can discard uncommitted changes. Check your working tree and save any work you need before using it.

### Q8. What is the difference between `git rebase` and `git rebase -i`?

Answer: A regular rebase replays commits onto another base. Interactive rebase lets you inspect and edit a sequence of commits.

bash
```
git rebase -i HEAD~3
```

You can use it to:

- `pick`: Keep a commit.
- `reword`: Change its commit message.
- `squash`: Combine it with the previous commit and edit the combined message.
- `fixup`: Combine it with the previous commit without retaining its message.
- `drop`: Remove a commit.

Example: You have three commits that make small changes to a shopping cart. You can squash them into one clean commit before opening a pull request.

Do not casually rewrite commits that others already depend on.

### Q9. Which is better: merge or rebase?

Answer: Neither is universally better. It depends on the workflow.

- Choose merge when you want to preserve branch topology and integration history.
- Choose rebase when you want a linear history and are updating a private branch.
- Follow team conventions when integrating code through pull requests.

A strong interview response explains the trade-off rather than claiming that one is always superior.

### Q10. What is the difference between merge and squash merge?

Answer: A regular merge can preserve all the individual feature commits and may create a merge commit. A squash merge combines the feature branch's changes into a single new commit on the target branch.

For example:

text
```
Feature: A---B---C
Main:    M

Squash result: M---S
```

Here, `S` contains the combined changes from the feature commits. The original individual commits are not added to the target branch's history as separate commits.

Squash merging is commonly available in pull-request workflows.

### Q11. What is the difference between `git merge` and `git pull`?

Answer: `git merge` integrates another branch into the current branch. `git pull` first fetches remote changes and then integrates them using the configured pull strategy, commonly merge or rebase.

bash
```
git pull origin main
```

This is conceptually similar to fetching updates and then merging them, unless the pull configuration or command specifies a different integration strategy.

You can explicitly choose:

bash
```
git pull --rebase origin main
```

### Q12. Can rebase be used to update a feature branch with the latest main branch?

Answer: Yes. This is one of the most common uses of rebase.

bash
```
git fetch origin
git switch feature/cart
git rebase origin/main
```

This replays your feature commits on top of the latest fetched `main` branch.

Remember that `git fetch` updates your remote-tracking references; it does not automatically merge those changes into your current branch.

### Q13. What is the difference between `git rebase` and `git reset`?

Answer: Rebase replays commits onto a different base. Reset moves the current branch pointer to another commit and, depending on the mode, changes the index and working tree.

bash
```
git reset --soft HEAD~1
```

Moves the branch back one commit while keeping the changes staged.

bash
```
git reset --mixed HEAD~1
```

Moves the branch back one commit and leaves the changes unstaged.

bash
```
git reset --hard HEAD~1
```

Moves the branch back one commit and resets the index and working tree, potentially discarding changes.

Don't confuse reset with rebase: they solve different problems.

### Q14. What does `git push --force-with-lease` do, and why is it used after rebase?

Answer: After rebasing a branch that has already been pushed, your local history may no longer match the remote history. A normal push may be rejected.

If rewriting that remote branch is intentional and coordinated, you might use:

bash
```
git push --force-with-lease origin feature/cart
```

`--force-with-lease` checks that the remote branch is still at the expected state before replacing its history. It is safer than `--force`, but it is not risk-free and does not replace coordination with collaborators.

Avoid force-pushing shared branches unless your team explicitly permits it.

## 8. Practical interview challenge

Let's test your understanding with a real situation.

Your branch history is:

text
```
          D---E  feature
         /
A---B---C---F    main
```

You are currently on `feature`. You want to incorporate `F` while keeping your feature history linear.

What command should you run?

A. `git merge main`

B. `git rebase main`

C. `git reset --hard main`

D. `git push --force origin feature`

 Check answer

## 9. Common mistakes to avoid in interviews and real projects

- Rebasing a shared branch without coordinating with other developers.
- Assuming every merge creates a merge commit; fast-forward merges do not.
- Assuming rebase changes the actual code changes in every commit; it replays changes, although conflicts can require manual decisions.
- Confusing `git pull`, `git fetch`, `git merge`, and `git rebase`.
- Using `git push --force` without considering other developers' work.
- Using `git reset --hard` without checking for uncommitted changes.
- Thinking that a clean, linear history is always preferable to preserving the full branch history.

## 10. Quick revision sheet

| Command | What it does |
| --- | --- |
| `git merge main` | Integrates `main` into the current branch |
| `git rebase main` | Replays current branch commits on top of `main` |
| `git merge --abort` | Cancels an unfinished merge |
| `git rebase --abort` | Cancels an unfinished rebase |
| `git rebase --continue` | Continues a paused rebase after resolving conflicts |
| `git rebase -i HEAD~3` | Interactively edits the last three commits |
| `git log --oneline --graph --all` | Visualizes branch history |
| `git reflog` | Shows recent movements of local references |
| `git merge --no-ff feature` | Forces a merge commit when a fast-forward would be possible |
| `git push --force-with-lease` | Safely guards against overwriting an unexpected remote update when rewriting history intentionally |

The three things to remember for an interview are:

1. Merge preserves existing commits and branch topology; rebase rewrites the commits it replays.
2. Merge is usually the safer choice for shared history; rebase is useful for cleaning up private feature branches.
3. Both can encounter conflicts. Resolve them carefully, and never rewrite shared history without understanding the consequences.

[graphite.dev](<https://graphite.dev/guides/git-merge-how-does-it-work?utm_source=chatgpt.com>)

[medium.com](<https://medium.com/devlink-tips/git-rebase-isnt-scary-you-just-never-had-it-explained-right-18cb866ce0a2?utm_source=chatgpt.com>)