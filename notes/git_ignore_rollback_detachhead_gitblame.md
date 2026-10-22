# Git Interview Preparation: Rollback, Detached HEAD, Git Blame, and Git Ignore

I'll explain these four Git topics from a practical, interview-focused perspective, with real-world examples, commands, common mistakes, and frequently asked interview questions with answers.

## 1. Rolling Back in Git

Git rollback means undoing changes in your code or repository. The correct command depends on what you want to undo and whether the changes have been committed or pushed.

### The 4 most important rollback commands

`git restore` — discard file changes

Use when you want to discard uncommitted changes in a file.

bash
```
git restore app.js
```

`git reset` — move the branch backward

Use to undo commits locally or change what is staged.

bash
```
git reset --soft HEAD~1
```

`git revert` — undo a commit safely

Creates a new commit that reverses the effects of an earlier commit.

bash
```
git revert a1b2c3d
```

`git reflog` — find previous HEAD positions

Useful when you accidentally reset, switch branches, or lose track of a commit.

bash
```
git reflog
```

### A. Real example: undo uncommitted changes

Suppose your `app.js` contains:

javascript
```
console.log("Hello World");
```

You change it to:

javascript
```
console.log("Buggy code");
```

But you haven't committed the change and want to restore the last committed version.

bash
```
git status
git restore app.js
```

Result: Your working copy of `app.js` returns to its last committed contents.

Important: `git restore app.js` discards the unstaged changes in that file. They may be difficult or impossible to recover. Check `git diff` before running it.

To unstage a file without discarding its edits:

bash
```
git restore --staged app.js
```

### B. Real example: undo your last commit

Suppose you made a commit but realized you forgot to finish the feature.

bash
```
git log --oneline -3
```

Example output:

text
```
a1b2c3d Add login validation
e4f5g6h Create login page
i7j8k9l Initial commit
```

If you want to remove the last commit but keep its changes staged:

bash
```
git reset --soft HEAD~1
```

The commit is removed from the current branch history, but its changes remain staged.

Compare the three reset modes:

| Command | Commit removed from branch? | Changes kept? |
| --- | --- | --- |
| `git reset --soft HEAD~1` | Yes | Staged |
| `git reset --mixed HEAD~1` | Yes | Unstaged |
| `git reset --hard HEAD~1` | Yes | Discarded from working tree and index |

`--mixed` is the default mode of `git reset`.

Warning: `git reset --hard` can destroy uncommitted work. Avoid it unless you're certain you want to discard those changes.

### C. Real example: undo a pushed commit

Imagine you pushed a bug into the shared `main` branch. Other developers may already have pulled it.

bash
```
git log --oneline
git revert a1b2c3d
git push origin main
```

Git creates a new commit that reverses the changes introduced by `a1b2c3d`.

Why prefer `revert` here? It preserves the existing commit history and avoids rewriting history other developers may depend on.

### D. Recover a lost commit with reflog

Suppose you accidentally ran a reset and can no longer see your commit in `git log`.

bash
```
git reflog
```

Example:

text
```
9ab1234 HEAD@{0}: reset: moving to HEAD~1
7cd5678 HEAD@{1}: commit: Finish payment feature
```

Recover the previous commit by creating a new branch:

bash
```
git branch recovery 7cd5678
```

You can then inspect the recovered work with:

bash
```
git switch recovery
```

Reflog records recent local movements of `HEAD` and references. It is not a permanent backup; unreachable commits can eventually be garbage-collected.

### Frequently asked interview questions: Rolling back

Q1. What is the difference between `git reset` and `git revert`?

Answer:

- `git reset` moves the current branch reference to another commit and can change the staging area or working tree.
- `git revert` creates a new commit that reverses an earlier commit.
- Use `reset` primarily for local history cleanup and `revert` for undoing commits in shared history.

Q2. What is the difference between `git reset --soft`, `--mixed`, and `--hard`?

Answer: They differ in what they preserve after moving the branch backward: `--soft` keeps changes staged, `--mixed` keeps them unstaged, and `--hard` resets the index and working tree to the target commit.

Q3. I accidentally deleted a commit. How can I recover it?

Answer: First inspect `git reflog`, find the commit hash, and create a branch at that hash:

bash
```
git reflog
git branch recovery <commit-hash>
```

Q4. Can I safely undo a commit that has already been pushed?

Answer: Usually, use `git revert <commit-hash>` and push the resulting commit. Resetting and force-pushing can overwrite shared history and disrupt collaborators.

Q5. What is `HEAD~1`?

Answer: It refers to the first parent of the current `HEAD` commit. Therefore, `git reset HEAD~1` moves the current branch back one first-parent step.

## 2. Fixing Detached HEAD in Git

### What is HEAD?

In Git, `HEAD` usually points to the branch you're currently working on. The branch points to the latest commit on that branch.

For example:

text
```
main ── A ── B ── C  ← HEAD
```

Here, `HEAD` points to the `main` branch, and `main` points to commit `C`.

### What is a detached HEAD?

A detached HEAD means that `HEAD` points directly to a commit instead of pointing to a branch.

This often happens when you check out a specific commit:

bash
```
git checkout a1b2c3d
```

Or with the modern command:

bash
```
git switch --detach a1b2c3d
```

Your repository might look like this:

Example Git history

HEAD → commit B

Detached

Commit B

The commit you checked out

Commit A

Meanwhile, the main branch may still point to a later commit, C.

You can inspect code, run tests, and even create commits in detached HEAD state. The risk is that new commits aren't automatically attached to a branch. If you switch away without saving a reference to them, they can become difficult to find later.

### A. How to identify detached HEAD

Run:

bash
```
git status
```

You might see:

text
```
HEAD detached at a1b2c3d
nothing to commit, working tree clean
```

You can also run:

bash
```
git branch --show-current
```

If the output is empty, you may be in detached HEAD state.

### B. How to fix detached HEAD

The correct fix depends on whether you've made new changes or commits.

Case 1: You haven't made any changes

Simply return to your branch:

bash
```
git switch main
```

Or switch to your feature branch:

bash
```
git switch feature-login
```

Your HEAD is now attached to the selected branch.

Case 2: You made code changes but haven't committed

If you want to keep your work and start a branch from the current commit:

bash
```
git switch -c fix-detached-work
```

This creates a new branch at the current commit and switches to it. Your existing uncommitted changes remain in your working tree.

You can then commit normally:

bash
```
git add .
git commit -m "Fix login validation"
```

Case 3: You already made commits in detached HEAD

This is the most important interview scenario.

Suppose you ran:

bash
```
git switch --detach a1b2c3d
# Make changes
git add .
git commit -m "Fix payment bug"
```

You now have a new commit, but no branch points to it yet.

Create a branch immediately:

bash
```
git switch -c fix-payment-bug
```

Your new branch now points to the commit you created. Your work is preserved.

If you already switched away, check `git reflog` to locate the commit, then create a branch using its hash:

bash
```
git reflog
git branch fix-payment-bug <commit-hash>
git switch fix-payment-bug
```

### C. Real-world scenario

Imagine you're investigating a production bug reported against a release from last month.

bash
```
git log --oneline --all
git switch --detach <old-release-commit>
```

You test the old version and discover the bug. You make a fix and commit it.

bash
```
git add .
git commit -m "Fix null pointer in payment service"
git switch -c hotfix/payment-null-pointer
```

Now the fix is preserved on a named branch, ready for testing and review.

### Frequently asked interview questions: Detached HEAD

Q1. What is a detached HEAD in Git?

Answer: It is a state in which `HEAD` points directly to a commit rather than to a branch reference. It is useful for inspecting historical commits, but new commits aren't automatically associated with a named branch.

Q2. Is detached HEAD an error?

Answer: No. It's a valid Git state. It becomes risky only if you create commits and later leave them without creating a branch or another reference to preserve them.

Q3. How do you fix detached HEAD?

Answer: If you don't need any new work, switch back to a branch using `git switch main`. If you need to preserve work, create a branch using `git switch -c my-branch`.

Q4. What happens if I commit while in detached HEAD?

Answer: Git creates a normal commit, but no branch automatically advances to it. The commit can still be recovered through its hash or the reflog, provided it hasn't been garbage-collected.

Q5. What is the difference between `git checkout <commit>` and `git switch <branch>`?

Answer: Checking out a commit generally puts you in detached HEAD state. Switching to a branch attaches HEAD to that branch. `git switch` makes branch switching and detached checkout more explicit.

Q6. Can I create a branch from a detached HEAD?

Answer: Yes. Run:

bash
```
git switch -c new-feature
```

This is usually the simplest way to preserve work made in detached HEAD state.

## 3. Git Blame

### What is `git blame`?

`git blame` helps you identify who last changed each line of a file, which commit introduced the current line, and when the change occurred.

Despite its name, it is a debugging and code-history tool, not a command for determining who is at fault.

### A. Basic example

Suppose your `PaymentService.java` contains:

java
```
public class PaymentService {
    public void processPayment() {
        validateUser();
        chargeCard();
        sendReceipt();
    }
}
```

You want to know who introduced `chargeCard()`.

Run:

bash
```
git blame PaymentService.java
```

Example output:

text
```
a1b2c3d (Amit 2026-08-10 10:30:00) public class PaymentService {
a1b2c3d (Amit 2026-08-10 10:30:00)     public void processPayment() {
e4f5g6h (Priya 2026-09-02 14:15:00)         validateUser();
i7j8k9l (Rahul 2026-09-12 09:45:00)         chargeCard();
m2n3o4p (Amit 2026-09-15 16:00:00)         sendReceipt();
```

This tells you that, according to the repository history, Rahul's commit `i7j8k9l` last changed the current `chargeCard()` line.

It does not necessarily mean Rahul originally wrote the method from scratch; someone else may have introduced it and Rahul may have modified it later.

### B. Find who changed a particular line

If you want to investigate only lines 3 through 5:

bash
```
git blame -L 3,5 PaymentService.java
```

This is helpful in large source files.

### C. Investigate a previous version of a file

You can inspect blame at a particular commit:

bash
```
git blame a1b2c3d -- PaymentService.java
```

This shows the line attribution for the file as it existed at the specified revision.

You can also inspect a particular line's history and context:

bash
```
git show i7j8k9l
```

Or compare the current file with a previous version:

bash
```
git diff i7j8k9l^ i7j8k9l -- PaymentService.java
```

### D. Real-world debugging workflow

Imagine your application suddenly starts charging customers twice.

1. Locate the suspicious code in `PaymentService.java`.
2. Run `git blame PaymentService.java` to find the commit that last changed the relevant lines.
3. Inspect the commit with `git show <commit-hash>` to understand the exact changes and commit message.
4. Review the surrounding code, tests, and related commits to determine whether the change caused the bug.
5. Write a fix and add a regression test.

Useful options:

bash
```
# Attribute a range of lines
git blame -L 10,25 app.js

# Follow line history across file renames
git blame --follow -- app.js

# Ignore a commit that only changed formatting
git blame --ignore-rev <commit-hash> app.js
```

`--ignore-rev` is particularly useful when a large formatting-only commit makes blame output less informative. You can also configure ignored revisions for a repository with an ignore-revisions file.

### Frequently asked interview questions: Git Blame

Q1. What is `git blame` used for?

Answer: It identifies the commit that last modified each line of a file, along with attribution and date information. It helps trace code history during debugging.

Q2. Does `git blame` show the original author of every line?

Answer: No. It normally shows the commit that last modified each line in the selected revision. The person who last modified a line may not be its original author.

Q3. How do you check who changed line 20 in a file?

Answer:

bash
```
git blame -L 20,20 app.js
```

Q4. What is the difference between `git blame` and `git log`?

Answer: `git blame` focuses on line-by-line attribution. `git log` displays commit history and can show changes across files or the entire repository.

Q5. Can `git blame` help investigate a bug?

Answer: Yes. Use it to find the relevant commit, then use `git show <commit-hash>` to inspect the patch and understand what changed.

Q6. What happens if a file was renamed?

Answer: Git can often detect renames while tracing history. The `--follow` option helps trace a single file's history across renames, although rename detection has limitations.

## 4. Git Ignore (`.gitignore`)

### What is `.gitignore`?

A `.gitignore` file tells Git which untracked files and directories should not be included in normal version-control workflows.

It is commonly used to exclude:

- Build output and compiled files.
- Dependencies such as `node_modules/`.
- Environment files containing local configuration or secrets.
- Logs, temporary files, and editor-specific files.
- Test reports and generated artifacts.

### A. Real-world example

Suppose your Node.js project looks like this:

text
```
my-project/
├── src/
│   └── app.js
├── node_modules/
├── dist/
├── .env
├── debug.log
├── package.json
└── .gitignore
```

You want Git to track the source code and `package.json`, but ignore generated files and local secrets.

Create a `.gitignore` file in the repository root:

gitignore
```
# Dependencies
node_modules/

# Build output
dist/

# Environment variables and secrets
.env
.env.*

# Keep the example environment file
!.env.example

# Log files
*.log

# Operating system files
.DS_Store

# Editor configuration
.vscode/
```

Now check:

bash
```
git status
```

If these files are untracked, Git should omit the ignored files from its normal status output.

Important: `.gitignore` does not automatically stop tracking a file that has already been committed.

### B. How to ignore a file that Git already tracks

Suppose `.env` was accidentally committed.

Adding this line alone won't stop Git from tracking it:

gitignore
```
.env
```

You must remove it from Git's index while keeping the local file:

bash
```
git rm --cached .env
git add .gitignore
git commit -m "Stop tracking environment file"
```

Now `.env` remains on your machine but is no longer tracked by the repository.

If it contained an actual password, API key, or other secret, removing it from the latest commit is not sufficient to erase it from Git history. Rotate the exposed secret and consider history cleanup if necessary.

### C. Common `.gitignore` patterns

| Pattern | Meaning |
| --- | --- |
| `*.log` | Ignore files ending in `.log` at any directory depth |
| `node_modules/` | Ignore directories named `node_modules` |
| `/build/` | Ignore the `build` directory at the repository root |
| `temp.txt` | Ignore files or directories matching that name at any depth |
| `!important.txt` | Re-include a matching path that was previously ignored, subject to parent-directory rules |
| `config/*.json` | Ignore matching JSON files directly inside `config/` |

Remember that a pattern such as `*.log` matches log files throughout the repository, not just in its root.

### D. How to debug `.gitignore`

Suppose you added `dist/` to `.gitignore`, but Git still shows a file inside `dist/`.

First, check whether Git already tracks it:

bash
```
git ls-files dist/
```

If the file is tracked, remove it from the index while keeping your local copy:

bash
```
git rm -r --cached dist/
```

Then commit the change.

If the file is untracked but is not being ignored as expected, run:

bash
```
git check-ignore -v --no-index dist/app.js
```

This helps identify the matching ignore rule and the file containing that rule. The `--no-index` option is useful when checking paths that Git already tracks.

### E. Where should `.gitignore` be placed?

- Repository-level: Use `.gitignore` files in the repository or individual subdirectories for project-specific rules.
- Global: Use a global ignore file for personal files that you don't want in any repository, such as operating-system or editor-generated files.

To configure a global ignore file:

bash
```
git config --global core.excludesfile ~/.gitignore_global
```

Create `~/.gitignore_global` and add patterns for personal, machine-specific files.

### Frequently asked interview questions: Git Ignore

Q1. What is the purpose of `.gitignore`?

Answer: It prevents matching untracked files from being included in normal Git status and staging workflows. Typical examples are build output, dependencies, temporary files, and local configuration.

Q2. Does `.gitignore` affect files already tracked by Git?

Answer: No. Git continues tracking files that are already in its index. Use `git rm --cached <file>` to stop tracking a file while retaining the local copy.

Q3. What is the difference between `.gitignore` and `.git/info/exclude`?

Answer: A `.gitignore` file can be committed and shared with the team. `.git/info/exclude` is local to one repository and is not normally shared with other developers.

Q4. Can you ignore a file globally?

Answer: Yes. Configure `core.excludesfile` with `git config --global` to use a personal ignore file across repositories.

Q5. How do you find out why a file is ignored?

Answer:

bash
```
git check-ignore -v app.log
```

This shows the ignore rule and its source for a matching untracked path. Add `--no-index` when you need to diagnose a path that is already tracked.

Q6. What is the difference between `.gitignore` and `git rm --cached`?

Answer: `.gitignore` defines which untracked paths Git should ignore. `git rm --cached` removes a tracked path from the index without deleting the local file.

Q7. Should `.env` be committed?

Answer: A real `.env` file containing credentials or secrets generally should not be committed. Ignore it and commit a safe `.env.example` containing placeholder values instead.

## 5. Quick interview revision: compare all four topics

| Topic | Main purpose | Important command |
| --- | --- | --- |
| Rolling back | Undo changes or commits | `git revert`, `git reset` |
| Detached HEAD | Work at a specific commit without being on a branch | `git switch -c new-branch` |
| Git blame | Find which commit last changed each line | `git blame -L 10,20 app.js` |
| Git ignore | Exclude matching untracked files | `.gitignore`, `git check-ignore -v` |

## 6. Practice scenarios for interviews

Try answering these before revealing the suggested solution.

## Git interview quiz

0 of 5 answered

1. You pushed a faulty commit to a shared main branch. What should you normally use?

git reset --hard HEAD\~1

git revert \<commit-hash\>

git restore .

2. You committed work in detached HEAD and want to preserve it. What should you do?

Delete the repository

Run git switch -c recovery-branch

Run git reset --hard

3. You want to find the last commit that changed line 15 of app.js. Which command is appropriate?

git status app.js

git blame -L 15,15 app.js

git ignore app.js

4. A .env file is already tracked, but you added it to .gitignore. What else is required?

git rm --cached .env

git reset --hard

git blame .env

5. You want to undo your last commit but keep all its changes staged. Which command should you use?

git reset --hard HEAD\~1

git reset --soft HEAD\~1

git revert HEAD

Try again  More interview questions

## 7. What interviewers expect you to remember

- Rollback: Understand the difference between undoing uncommitted changes, resetting local history, and reverting a commit in shared history.
- Detached HEAD: Know why it occurs and how to preserve commits with `git switch -c`.
- Git blame: Know how to trace a line to its last modifying commit and investigate that commit using `git show`.
- Git ignore: Know that ignored files already tracked by Git remain tracked until removed from the index.

My recommendation: practice each command in a disposable Git repository. Deliberately make a commit, undo it, check out an old commit, run blame, and configure `.gitignore`. Being able to explain what changes in the working tree, staging area, and commit history is what makes your interview answers strong.