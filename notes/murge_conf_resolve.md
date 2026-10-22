# Git Interview Guide: Resolve Merge Conflicts

I'll explain this topic as a Git expert would in a real development team, starting with the basics, then practical commands, real project examples, common mistakes, and frequently asked interview questions with answers.

## 1. What is a merge conflict in Git?

A merge conflict happens when Git cannot automatically combine changes from two branches because it cannot determine which changes should be kept.

Imagine you and your teammate are working on the same project.

- You modify a line in `app.js` on the `feature-login` branch.
- Your teammate modifies the same line on the `main` branch.
- When you merge the branches, Git needs you to decide how to combine those changes.

Git can automatically merge many changes, but when it cannot determine the correct result, it reports a merge conflict.

### Real-world example

Suppose your project has this file:

`app.js`

javascript
```
function getMessage() {
    return "Hello";
}
```

Two developers make different changes.

main

Developer A

Changes the message to:

javascript
```
return "Welcome to our app";
```

feature

Developer B

Changes the same line to:

javascript
```
return "Hello, user!";
```

Now, when Git tries to merge `feature` into `main`, it cannot automatically choose between the two return statements. This creates a conflict.

## 2. How does a merge conflict look?

Git may mark the conflicted file like this:

javascript
```
function getMessage() {
<<<<<<< HEAD
    return "Welcome to our app";
=======
    return "Hello, user!";
>>>>>>> feature
}
```

Understand each marker:

| Marker | Meaning |
| --- | --- |
| `<<<<<<< HEAD` | Beginning of the current branch's conflicting content |
| `=======` | Separates the two versions |
| `>>>>>>> feature` | End of the incoming branch's conflicting content |

When merging, `HEAD` generally represents the branch you are currently on, while `feature` is the branch being merged into it.

Important: These markers are temporary conflict indicators. You must remove them when resolving the conflict.

## 3. Step-by-step: How to resolve a merge conflict

Let's solve the conflict from the example above.

### Step 1: Check your current branch

bash
```
git branch
```

Or:

bash
```
git status
```

Suppose you want to merge `feature` into `main`.

bash
```
git switch main
git merge feature
```

Git may display an error similar to:

text
```
Auto-merging app.js
CONFLICT (content): Merge conflict in app.js
Automatic merge failed; fix conflicts and then commit the result.
```

This means Git needs your help.

### Step 2: Identify conflicted files

bash
```
git status
```

Example output:

text
```
On branch main
You have unmerged paths.

Unmerged paths:
  both modified: app.js
```

To list only files that still have unresolved conflicts:

bash
```
git diff --name-only --diff-filter=U
```

### Step 3: Open the conflicted file

Open `app.js` in your editor. You will see the conflict markers shown earlier.

Decide which version is correct for your application.

You have three common options:

- Keep the current branch's changes.
- Keep the incoming branch's changes.
- Combine both changes into a new, correct implementation.

For our example, let's combine the intended meaning into one final result:

javascript
```
function getMessage() {
    return "Welcome to our app, user!";
}
```

Notice that we removed all conflict markers and kept only valid JavaScript.

### Step 4: Stage the resolved file

After editing and saving the file:

bash
```
git add app.js
```

Staging tells Git that you have resolved the conflict in this file.

Check the status:

bash
```
git status
```

Git should no longer list `app.js` as unmerged.

Remember: `git add` marks a conflict as resolved, but it does not prove your code is logically correct. You still need to review and test it.

### Step 5: Review and test your changes

Inspect what you have staged:

bash
```
git diff --cached
```

Run the relevant tests for your project. For example, in a JavaScript project:

bash
```
npm test
```

Use the test command configured by your actual project.

### Step 6: Complete the merge

If you used `git merge`, finish with:

bash
```
git commit -m "Resolve merge conflict in app.js"
```

Git may provide a default merge commit message; you can keep it or edit it.

Finally:

bash
```
git status
git log --oneline --graph --decorate -5
```

You can now verify the working tree and inspect the resulting history.

## 4. Complete real-world terminal example

Here is the whole workflow in one place.

bash
```

# 1. Switch to the target branch
git switch main

# 2. Merge the feature branch
git merge feature

# 3. Find unresolved conflicts
git status
git diff --name-only --diff-filter=U

# 4. Edit conflicted files manually in your editor
# Remove conflict markers and keep the correct code

# 5. Stage resolved files
git add app.js

# 6. Review the staged changes
git diff --cached

# 7. Run your project's tests
npm test

# 8. Finish the merge
git commit -m "Resolve merge conflict"

# 9. Verify
git status
git log --oneline --graph --decorate -5
```

The example assumes you are merging `feature` into `main`. The file names and test command will vary by project.

## 5. How to choose which changes to keep

Sometimes you want only one side of the conflict.

### Option A: Keep the current branch's version

During a normal merge:

bash
```
git checkout --ours app.js
git add app.js
```

Here, `ours` means the version from the branch you are merging into — in our example, `main`.

### Option B: Keep the incoming branch's version

bash
```
git checkout --theirs app.js
git add app.js
```

Here, `theirs` means the version from the branch being merged in — in our example, `feature`.

You can also use newer Git syntax:

bash
```
git restore --ours app.js
git restore --theirs app.js
```

These commands replace the working copy of the file with one side's version. Any useful edits from the other side may be lost, so inspect the result before proceeding.

### Option C: Keep both changes

This is often the best choice when both developers added useful functionality.

For example, one branch adds email validation and the other adds password validation. You should normally preserve both validations in the final implementation, rather than blindly selecting one side.

Interview tip: Never say you always choose `ours` or always choose `theirs`. The correct choice depends on the application's requirements.

## 6. Merge conflict vs. rebase conflict

This is one of the most commonly tested Git interview topics.

Both `git merge` and `git rebase` can cause conflicts, but you complete the operation differently.

| Operation | How to complete it after resolving conflicts |
| --- | --- |
| `git merge` | `git add` followed by `git commit` (or Git's indicated merge completion command) |
| `git rebase` | `git add` followed by `git rebase --continue` |

### Example: Conflict during rebase

Suppose you are working on `feature` and want to update it with the latest changes from `main`.

bash
```
git switch feature
git fetch origin
git rebase origin/main
```

If a conflict occurs:

bash
```
git status
```

Edit the conflicted file, then run:

bash
```
git add app.js
git rebase --continue
```

If another commit causes another conflict, repeat the process.

To cancel the rebase and return to its pre-rebase state:

bash
```
git rebase --abort
```

Why is rebase different? Rebase replays your commits on top of another base, potentially creating a conflict for each replayed commit. Merge combines branch histories and normally concludes with a merge commit when a non-fast-forward merge is needed.

## 7. How to abort a merge conflict

An interviewer may ask: "What if you started a merge but don't want to continue?"

If the merge is still in progress and you want to cancel it:

bash
```
git merge --abort
```

This attempts to restore the state from before the merge began.

For example:

bash
```
git switch main
git merge feature

# Conflict occurs

git merge --abort
```

You can then review the branches and try again.

Caution: Git warns that aborting may not fully reconstruct pre-merge changes in every situation, particularly if you had uncommitted changes before starting. A clean working tree before merging is a good practice.

### Related commands interviewers expect you to know

| Command | Purpose |
| --- | --- |
| `git status` | Identify unresolved files and the current operation |
| `git diff` | Inspect unstaged changes |
| `git diff --cached` | Inspect staged changes |
| `git diff --merge` | Inspect the conflict-related working-tree diff |
| `git merge --abort` | Cancel an in-progress merge |
| `git rebase --abort` | Cancel an in-progress rebase |
| `git mergetool` | Open a configured merge-resolution tool |
| `git log --oneline --graph --all` | Visualize branch history |

## 8. Common types of merge conflicts

Pair Programming & TDD in 2025: Evolving or Obsolete in an AI‑First Era | by Pravir Raghu | Medium

1. Content conflict

Two branches modify overlapping lines in the same file.

Typical solution: inspect both versions and edit the final content.

Copilot Edits: Edit multiple files and enhance your coding workflow ✨

2. Modify/delete conflict

One branch modifies a file while another deletes it.

Typical solution: decide whether the file is still needed, then keep or delete it accordingly.

Close-up of software developer pointing at source code on large monitor, reviewing programming details and debugging software issues to improve system reliability and development workflow.

3. Rename conflict

Two branches rename the same file differently, or rename a file while the other branch modifies it.

Typical solution: choose the intended filename and preserve all required content.

Hotel Booking App Development Company | Devsrank

4. Structural or semantic conflict

Git may merge the text without reporting a conflict, but the combined code or configuration is incorrect.

Typical solution: review the behavior, configuration and tests, not just Git's conflict markers.

One important distinction: Git's text conflict markers do not detect every logical conflict. For example, two changes to different lines may merge cleanly but produce incompatible API behavior.

## 9. Advanced topic: The three versions of a conflicted file

During a typical merge conflict, Git's index can store three versions of a file:

- Base (stage 1): the common ancestor's version.
- Ours (stage 2): the current branch's version.
- Theirs (stage 3): the incoming branch's version.

You can inspect them with:

bash
```
git show :1:app.js
git show :2:app.js
git show :3:app.js
```

These commands work when the corresponding stages exist for that conflicted path.

You can also use:

bash
```
git ls-files -u
```

This lists unmerged index entries.

Why does the base matter? It helps you understand what changed on each branch compared with the shared ancestor.

Advanced interview trap: During a rebase, `ours` and `theirs` can feel reversed compared with a normal merge. In a rebase, `ours` generally refers to the new base and commits already replayed, while `theirs` is the commit currently being replayed. Always inspect the actual versions rather than relying on the labels alone.

## 10. Most-asked Git merge conflict interview questions and answers

These are useful for fresher, intermediate, and experienced developer interviews.

QUESTION 1 · BASIC

What is a merge conflict in Git?

Answer: A merge conflict occurs when Git cannot automatically reconcile changes from different branches. It commonly happens when two branches modify the same lines differently or when one modifies a file that the other deletes.

Example: `main` changes `return "Hello"` to `return "Welcome"` while `feature` changes it to `return "Hi"`.

QUESTION 2 · BASIC

How do you resolve a merge conflict?

Answer: I check the conflicting files using `git status`, inspect and edit the files to preserve the intended changes, stage the resolved files with `git add`, run tests, and complete the merge with `git commit`.

bash
```
git status
# Edit the conflicted file
git add app.js
git diff --cached
git commit
```

QUESTION 3 · BASIC

What do `<<<<<<<`, `=======`, and `>>>>>>>` mean?

Answer: They mark the conflicting sections in a file. The first marker starts the current-side section, the separator divides the versions, and the last marker ends the incoming-side section. They must be removed when manually resolving the conflict.

QUESTION 4 · BASIC

What is the difference between `--ours` and `--theirs`?

Answer: During a normal merge, `--ours` selects the current branch's version, and `--theirs` selects the incoming branch's version.

bash
```
git checkout --ours app.js
git checkout --theirs app.js
```

These are alternatives, not commands you should run one after the other blindly. Choose the correct version for your requirements.

QUESTION 5 · BASIC

How do you cancel a merge conflict?

Answer: If a merge is in progress, use:

bash
```
git merge --abort
```

This attempts to restore the state from before the merge began. For an in-progress rebase, use `git rebase --abort`.

QUESTION 6 · INTERMEDIATE

What is the difference between `git merge` and `git rebase` when resolving conflicts?

Answer: A merge combines branch histories, while rebase replays commits on a new base. After resolving a merge conflict, stage the files and complete the merge. After resolving a rebase conflict, stage the files and use `git rebase --continue`.

QUESTION 7 · INTERMEDIATE

Does `git add` resolve a conflict?

Answer: It marks the selected file as resolved in Git's index. It does not automatically fix the code or confirm its correctness. You must edit the file first, stage it, and verify the result.

QUESTION 8 · INTERMEDIATE

Can Git create a merge conflict even if two developers edit different lines?

Answer: Yes, depending on the surrounding changes and how Git matches the edited regions. However, edits in clearly separate, non-overlapping regions often merge automatically. Also, changes to different lines can create logical bugs without producing any textual conflict.

QUESTION 9 · INTERMEDIATE

How do you find all conflicted files?

Answer:

bash
```
git diff --name-only --diff-filter=U
```

Alternatively, use `git status` to see unresolved paths and the state of the ongoing operation.

QUESTION 10 · INTERMEDIATE

What happens if you forget to remove conflict markers?

Answer: The markers can cause syntax errors or invalid configuration. In programming languages, the build or test process may fail. In some plain-text formats, they may remain as unwanted content. Always review the final file and run appropriate checks.

QUESTION 11 · INTERMEDIATE

What is the difference between a merge conflict and a merge commit?

Answer: A merge conflict is an issue that requires manual resolution. A merge commit is a commit that records the integration of histories. A merge can complete without conflicts, and a fast-forward merge may not create a merge commit.

QUESTION 12 · ADVANCED

How do you resolve conflicts in a shared file such as `package.json`?

Answer: I compare both versions and preserve compatible changes, such as dependencies added by both developers. I also check lockfile consistency and run the project's package-manager validation or tests. I don't blindly choose one side because that could remove required dependencies.

QUESTION 13 · ADVANCED

Can Git report no conflicts even when the final code is broken?

Answer: Yes. Git merges text, not business requirements. Two changes can be textually compatible but logically incompatible. Code review, automated tests, integration tests, and sometimes manual testing are needed to catch such problems.

QUESTION 14 · ADVANCED

What are the advantages of using a merge tool?

Answer: A configured tool such as VS Code's merge editor or another three-way merge tool helps compare the base, current, and incoming versions visually. It can make complicated conflicts easier to understand, but the developer is still responsible for the correctness of the final result.

QUESTION 15 · SCENARIO-BASED

A teammate's changes disappear after you resolve a conflict. What would you do?

Answer: I would inspect the staged diff and branch history before proceeding. If the missing changes are still available in the other branch or its commits, I would restore the necessary parts, resolve any new conflicts, and run tests.

bash
```
git diff --cached
git log --oneline --graph --all
git show <commit-hash>
```

I would avoid destructive commands until I understood which changes were missing.

## 11. Real interview scenario: Two developers change the same feature

Interviewer: You are working on an e-commerce application. You modify the product price calculation in `main`, while another developer modifies it in `feature-discount`. A conflict occurs when you merge. What will you do?

Strong interview answer:

"I'll first inspect `git status` to identify the conflicted files. I'll examine both versions and understand the business requirements before editing the final implementation. Since price calculation and discounts are involved, I'll ensure both changes are preserved correctly rather than selecting one side blindly. I'll remove the conflict markers, stage the resolved file, review the staged diff, run relevant tests, and complete the merge. Finally, I'll verify the repository status and merge history."

For example, the two developers might have written:

`main`

javascript
```
const total = price * quantity;
```

`feature-discount`

javascript
```
const total =
  price * quantity * (1 - discount);
```

A possible resolved implementation is:

javascript
```
function calculateTotal(price, quantity, discount = 0) {
    return price * quantity * (1 - discount);
}
```

This preserves the quantity calculation and discount behavior, assuming `discount` is a valid fraction between `0` and `1`. In production, you should also validate inputs and follow the application's currency-rounding rules.

## 12. Best practices to avoid merge conflicts

- Pull or fetch frequently: Keep your feature branch reasonably up to date with the target branch.
- Keep commits focused: Small, logically grouped changes are easier to review and resolve.
- Coordinate shared files: Communicate when several developers are editing the same critical file.
- Avoid unnecessary formatting changes: Large formatting-only changes can make genuine conflicts harder to understand.
- Use automated tests: A conflict-free merge is not necessarily a correct merge.
- Review before staging: Check that the final code contains the intended changes from both branches.
- Don't rewrite shared history casually: Rebasing commits that others already depend on requires care.

## 13. Quick revision sheet

| If the interviewer asks… | Remember this |
| --- | --- |
| Find the conflict | `git status` |
| List unresolved paths | `git diff --name-only --diff-filter=U` |
| Keep current side | `git checkout --ours file` |
| Keep incoming side | `git checkout --theirs file` |
| Finish a merge | `git add file` then `git commit` |
| Continue a rebase | `git add file` then `git rebase --continue` |
| Cancel a merge | `git merge --abort` |
| Cancel a rebase | `git rebase --abort` |
| Review staged changes | `git diff --cached` |
| Inspect conflict stages | `git show :1:file`, `:2:file`, `:3:file` |

## 14. Test your interview knowledge

## Git conflict quiz

0 of 4

1. You resolved app.js and ran git add app.js during a merge. What is the next usual step?

Run git rebase --continue

Review the changes and commit the merge

Run git merge --abort

2. During a normal merge from feature into main, what does --theirs usually refer to?

The main branch

The feature branch being merged in

The common ancestor

3. Does a conflict-free merge guarantee correct application behavior?

Yes, always

Only if Git creates a merge commit

No, tests and review are still needed

4. Which command cancels an in-progress rebase?

git merge --abort

git rebase --abort

git reset --merge-conflict

More interview questions  Reset quiz

The main point to remember for your interview: Resolving a merge conflict is not just removing Git's markers. It is understanding both changes, choosing the correct final behavior, validating the result, and completing the Git operation safely.