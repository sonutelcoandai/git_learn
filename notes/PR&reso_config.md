# Git Interview Guide: Pull Requests (PR) and PR Conflicts

I'll explain these topics from a Git expert and interview perspective, starting with the basics, then practical commands, real-world examples, conflict resolution, and frequently asked interview questions with answers.

## 1. What is a Pull Request (PR)?

A Pull Request is a request to merge changes from one branch into another branch. It allows developers to review code, discuss changes, run automated tests, and approve the changes before merging.

Important: A Pull Request is a feature provided by platforms such as GitHub, GitLab, and Bitbucket. Git itself does not have a native PR command.

### Real-world example

Imagine you're working on an e-commerce application.

- `main` — contains stable production code.
- `feature/login` — contains your new login functionality.
- You finish writing and testing the login code.
- You create a PR to merge `feature/login` into `main`.
- Your teammate reviews your code and approves it.
- The PR is merged into `main`.

feature/login

Write code · Commit · Push

Create Pull Request

Code Review + CI Tests

Review comments · Fix issues · Approve

Merge PR

main

Login functionality is integrated

### How to create a PR step by step

Assume your team uses GitHub.

Step 1: Get the latest code

bash
```
git switch main
git pull origin main
```

Step 2: Create a feature branch

bash
```
git switch -c feature/login
```

Step 3: Make changes and commit

bash
```
git add .
git commit -m "Add login functionality"
```

Step 4: Push the branch to the remote repository

bash
```
git push -u origin feature/login
```

- `origin` is the usual name for the remote repository.
- `-u` sets the upstream branch so future pushes can usually use `git push`.

Step 5: Create the PR

On GitHub, open the repository and click Compare & pull request. Choose:

- Base branch: `main` — where you want the changes to go.
- Compare branch: `feature/login` — where your changes come from.

Step 6: Review and merge

Address review comments, ensure required checks pass, obtain approval, and merge according to your team's rules.

## 2. Important PR terminology

| Term | Meaning |
| --- | --- |
| Base branch | Destination branch, such as `main` |
| Head/source branch | Branch containing your changes, such as `feature/login` |
| Reviewer | Person who examines your code |
| Approval | Reviewer agrees that the changes can be merged |
| CI/CD checks | Automated builds, tests, linting, and other checks |
| Merge | Integrate the PR's changes into the target branch |
| Draft PR | PR opened for early feedback, not yet ready for final review |
| Merge conflict | Git cannot automatically reconcile competing changes |
| Review comments | Feedback asking questions or suggesting changes |

Interview tip: A PR is not the same thing as a Git merge. The PR is the review and collaboration process; merging is the operation that integrates the changes.

## 3. What is a PR conflict?

A PR conflict occurs when Git cannot automatically combine the changes in your source branch with the target branch.

### Real-world example

Suppose two developers work on the same project.

The `main` branch contains:

javascript
```
function getDiscount() {
    return 10;
}
```

Developer A creates `feature/discount` and changes the value to:

javascript
```
function getDiscount() {
    return 20;
}
```

Meanwhile, Developer B changes the same line directly on `main`:

javascript
```
function getDiscount() {
    return 15;
}
```

Now Developer A opens a PR from `feature/discount` into `main`.

Git cannot know whether the final discount should be `20` or `15`. This is a merge conflict.

Git may show the following in the conflicting file:

javascript
```
function getDiscount() {
<<<<<<< HEAD
    return 15;
=======
    return 20;
>>>>>>> feature/discount
}
```

What do these markers mean?

- `<<<<<<< HEAD` — starts one version of the conflicting code. In a normal merge into the checked-out `main` branch, this is the current `main` version.
- `=======` — separates the two versions.
- `>>>>>>> feature/discount` — ends the incoming branch's version.

The markers are temporary. You must remove them when resolving the conflict.

For example, after discussing the business requirement with your teammate, you decide the discount should be `20`:

javascript
```
function getDiscount() {
    return 20;
}
```

Then you save the file, stage the resolution, and complete the merge.

## 4. How to resolve a PR conflict step by step

There are two common approaches: resolve it locally using Git, or use the conflict-resolution interface on GitHub when it supports the changes involved.

### Method A: Resolve the conflict using Git commands

Suppose your PR is from `feature/discount` into `main`.

Step 1: Switch to your feature branch

bash
```
git switch feature/discount
```

Step 2: Fetch the latest remote changes

bash
```
git fetch origin
```

This downloads remote updates without merging them into your current branch.

Step 3: Merge the latest target branch into your feature branch

bash
```
git merge origin/main
```

If the conflicting changes cannot be merged automatically, Git pauses and reports the conflict.

Step 4: Find and resolve conflicts

bash
```
git status
```

Open each conflicted file, decide which changes to keep, and remove all conflict markers.

Step 5: Stage the resolved files and complete the merge

bash
```
git add src/discount.js
git commit -m "Resolve merge conflict with main"
```

Replace `src/discount.js` with the actual conflicted file path. A successful merge requires a merge commit only when Git has not already completed the merge automatically.

Step 6: Push the updated feature branch

bash
```
git push origin feature/discount
```

The PR updates automatically on GitHub. Run the relevant tests and check the PR status before merging.

Why merge `origin/main` into the feature branch?

Because your feature branch needs to incorporate the latest target-branch changes. You resolve the conflict on your branch, then push the updated branch. This avoids directly changing `main`.

### Method B: Resolve the conflict using GitHub

1. Open the PR.
2. Check whether GitHub reports a merge conflict.
3. If the Resolve conflicts option is available, open the conflict editor.
4. Choose or combine the correct changes.
5. Mark the conflicts as resolved and commit the resolution to the PR branch.
6. Rerun or verify the required checks, then merge when eligible.

GitHub's web editor does not support every type of conflict. Complex conflicts, binary files, or conflicts needing local tests often require the command-line method.

## 5. Merge versus rebase when fixing PR conflicts

Another common interview question is whether to merge `main` into your feature branch or rebase your feature branch onto `main`.

| Feature | Merge | Rebase |
| --- | --- | --- |
| Example command | `git merge origin/main` | `git rebase origin/main` |
| History | Preserves branch history and may create a merge commit | Replays your commits on top of the latest base |
| Commit IDs | Existing commits are generally preserved | Replayed commits get new IDs |
| Conflict handling | Resolve conflicts, then complete the merge | Resolve conflicts, then continue the rebase |
| Shared branch safety | Usually safer for a shared feature branch | Requires care if other people depend on the branch |

### Rebase example

bash
```
git switch feature/discount
git fetch origin
git rebase origin/main
```

If a conflict occurs:

bash
```
# Edit the conflicting files, then:
git add src/discount.js
git rebase --continue
```

Repeat as needed until the rebase completes. To abandon the rebase and return to the pre-rebase state:

bash
```
git rebase --abort
```

After a successful rebase, push the updated branch:

bash
```
git push --force-with-lease origin feature/discount
```

Rebasing rewrites commit history, so coordinate with teammates if the branch is shared. Prefer `--force-with-lease` over `--force`, and only force-push when permitted by your team's workflow.

Interview tip: If the interviewer asks which is better, answer that it depends on the team's history policy. Merge preserves the original branching history; rebase creates a more linear history.

## 6. Most asked Git PR and conflict interview questions

Here are the questions you should be able to answer in an interview, including scenario-based questions.

### Q1. What is a Pull Request, and why do we use it?

Answer: A Pull Request is a request to integrate changes from one branch into another. It enables code review, collaboration, automated testing, and approval before merging.

Example: I create `feature/payment`, push my changes, and open a PR into `main`. My teammate reviews the code, CI tests run, and the team merges it after approval.

### Q2. What is the difference between `git pull` and a Pull Request?

Answer:

- `git pull` downloads remote changes and integrates them into your current local branch.
- A Pull Request is a platform-based workflow for reviewing and merging changes between branches.

bash
```
git pull origin main
```

This command updates your local branch. It does not create a PR.

### Q3. What causes a merge conflict?

Answer: A conflict happens when Git cannot determine how to combine changes automatically.

Common causes include:

- Two developers modify the same lines differently.
- One branch deletes a file while another modifies it.
- Both branches add files with the same path.
- Complex renames or overlapping structural changes make automatic merging ambiguous.

Example: Developer A changes `price = 100` to `price = 120`, while Developer B changes it to `price = 150`. Git needs a decision about the final value.

### Q4. How do you resolve a conflict in a PR?

Answer: I update my feature branch with the latest target branch, inspect the conflicted files, resolve the changes, test the result, and push the updated branch.

bash
```
git switch feature/payment
git fetch origin
git merge origin/main
git status
```

After editing the conflicted files:

bash
```
git add .
git commit -m "Resolve merge conflict"
git push origin feature/payment
```

If the merge completed without conflicts, Git may already have created the merge commit, so an additional `git commit` is unnecessary.

### Q5. What is the difference between `git fetch` and `git pull`?

Answer:

| `git fetch` | `git pull` |
| --- | --- |
| Downloads remote updates | Downloads and integrates remote updates |
| Does not change the current branch's files through integration | Can change files and create conflicts |
| Useful for inspecting changes before integrating | Useful when you want to update your current branch |

Example:

bash
```
git fetch origin
git log --oneline HEAD..origin/main
```

The second command shows commits present in `origin/main` but not in your current `HEAD`.

### Q6. What if the same file has changes in different lines?

Answer: Git can usually merge the changes automatically if they do not overlap and the surrounding context allows a safe merge.

For example, one developer changes a function at the top of a file and another adds a separate function at the bottom. A conflict is not inevitable just because the file is the same.

Important: A conflict is about whether Git can reconcile changes, not simply whether two people edited the same file.

### Q7. What is the difference between a merge conflict and a PR review comment?

Answer:

- A merge conflict is a technical integration problem that Git cannot resolve automatically.
- A review comment is feedback from a reviewer about correctness, readability, security, design, or maintainability.

A PR can have review comments without any conflicts, and a PR can have conflicts even when the code review is complete.

### Q8. What happens if `main` changes after I create my PR?

Answer: My PR may become out of date. Depending on the changes and repository settings, the PR may still be mergeable or may require an update.

I can update my branch with:

bash
```
git fetch origin
git switch feature/payment
git merge origin/main
```

Then I resolve any conflicts, run tests, and push the branch.

Some teams use rebase instead, according to their branching policy.

### Q9. What is the difference between `git merge --abort` and `git rebase --abort`?

Answer:

- `git merge --abort` attempts to cancel an in-progress merge and restore the pre-merge state.
- `git rebase --abort` cancels an in-progress rebase and returns the branch to its pre-rebase state.

Example:

bash
```
git merge --abort
```

Use these when you want to stop the operation rather than resolve its conflicts. They are not general-purpose undo commands for already completed operations.

### Q10. How do you know which version is yours during conflict resolution?

Answer: It depends on the operation.

During a normal merge into your checked-out branch:

- Ours generally means the current branch.
- Theirs generally means the branch being merged in.

During a rebase, the interpretation can be counterintuitive: "ours" is typically the branch being rebased onto, while "theirs" is the commit being replayed.

You can inspect the conflict markers and compare the actual code rather than blindly choosing one side.

Useful commands:

bash
```
git status
git diff
git diff --ours -- src/discount.js
git diff --theirs -- src/discount.js
```

For a conflicted file, `git diff --ours` and `git diff --theirs` help inspect each version; check the output and operation context before choosing.

### Q11. Can we merge a PR without resolving a conflict?

Answer: If GitHub reports that a PR has merge conflicts preventing the selected merge method, those conflicts must be resolved before merging through that method.

However, not every PR requires manual conflict resolution. Git can merge compatible changes automatically. Repository rules and required checks may also prevent merging for reasons unrelated to conflicts.

### Q12. What is a squash merge?

Answer: A squash merge combines the changes from a PR into one new commit on the target branch, rather than preserving every source-branch commit as a separate commit.

Example: A feature branch has five commits for a login feature. Squashing can put the final changes into one commit such as:

text
```
Add login functionality
```

This keeps the target branch history simpler. Squash merging does not, by itself, resolve conflicts that prevent integration.

### Q13. What would you do if your conflict resolution broke existing functionality?

Answer: I would not merge immediately. I would inspect the final diff, run relevant unit and integration tests, verify the business logic with the other developer if necessary, and fix the issue before pushing the updated branch.

If the problem is already merged, I would follow the team's incident and rollback procedure rather than blindly rewriting shared history.

### Q14. Can two developers work on the same branch?

Answer: Yes, Git supports it, but it requires coordination because both developers may push changes to the same remote branch. Feature branches are often kept separate per task or developer to reduce accidental overwrites and make code review easier.

If multiple developers share a branch, they should regularly fetch and integrate updates, avoid careless force-pushing, and coordinate any history rewriting.

## 7. Practice a real PR conflict scenario

Try answering this as if you are in a technical interview.

Scenario-based interviewSituation: You created `feature/cart` from `main`. You changed the cart tax calculation. Meanwhile, another developer changed the same calculation directly on `main`. Your GitHub PR now says, "This branch has conflicts that must be resolved."

What would you do first?

Force-push my branch to overwrite `main`.

fetch Fetch the latest changes and integrate `main` into my feature branch.

Delete my feature branch and close the PR.

Merge my feature branch directly into `main` without checking the conflict.

 Check answer

## 8. Quick revision sheet

| Topic | Remember this |
| --- | --- |
| PR | Code review and proposed integration of branch changes |
| Base branch | Destination branch |
| Source branch | Branch containing your proposed changes |
| Conflict | Git cannot automatically reconcile changes |
| `git fetch` | Download remote updates without integrating them |
| `git pull` | Fetch and integrate remote updates |
| `git merge` | Combine branch histories |
| `git rebase` | Replay commits on a new base |
| `git merge --abort` | Cancel an in-progress merge |
| `git rebase --abort` | Cancel an in-progress rebase |
| `git add` | Stage resolved files |
| `git push` | Publish local commits to a remote branch |
| Squash merge | Combine a PR's changes into one commit on the target branch |

The best interview answer for a PR conflict: "I fetch the latest target branch, integrate it into my feature branch using the team's preferred merge or rebase strategy, inspect and resolve the conflicts carefully, run tests, and push the updated branch. Then I verify the PR checks and review requirements before merging."

That answer demonstrates both Git command knowledge and a safe, collaborative development workflow.