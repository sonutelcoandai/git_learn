Sign up for free
You said:
you are git expert so explain me topics {1. new repo, 2. git clone, 3. branch , 4. branches in git hub 5. git pull, 6. git forking} include most asked interview question  on given topics and answers with real example on topics.

ChatGPT said:
Git and GitHub Interview Guide: From Basics to Real-World Examples
I'll explain all 6 topics from a beginner's perspective, with practical commands, real-world examples, common mistakes, and frequently asked interview questions with answers.

First, understand the difference between Git and GitHub.

Git

A version control system installed on your computer. It tracks changes, creates commits, manages branches, and helps you restore previous versions of your code.

GitHub

An online platform where you host Git repositories, collaborate with developers, review pull requests, and manage projects.

For example, imagine you are developing a website for an online shopping application.

Git tracks changes to your website code.

GitHub stores a remote copy of your repository.

Branches let you develop a new feature without disturbing the main code.

Pull requests let your teammates review your changes.

Forking lets you create your own copy of someone else's GitHub repository.

1. Creating a new Git repository
A repository (repo) is a project folder that Git tracks. It contains your files and their version history.

There are two common ways to start a repository:

Create a new local repository using git init.

Create a repository on GitHub and then clone it to your computer.

Real-world example
You are starting a new project called shopping-app.

Step 1 — Create a project folder


bash
mkdir shopping-app
cd shopping-app
Step 2 — Initialize Git


bash
git init
This creates a hidden .git directory that stores Git's tracking information and history.

Step 3 — Create a file


bash
echo "# Shopping App" > README.md
Step 4 — Check the status


bash
git status
Git will show README.md as an untracked file.

Step 5 — Stage the file


bash
git add README.md
Staging means selecting changes to include in your next commit.

Step 6 — Commit the changes


bash
git commit -m "Initial commit"
A commit is a saved snapshot of your project's tracked changes.

The workflow is:

Working directory

Edit README.md

Staging area

git add README.md

Local repository

git commit -m "Initial commit"

To connect your local repository to a new GitHub repository, create an empty repository on GitHub, then use its actual URL:


bash
git remote add origin https://github.com/your-username/shopping-app.git
git branch -M main
git push -u origin main
Replace your-username with your GitHub username. The commands assume your initial commit exists and that the remote repository is empty.

Frequently asked interview questions
Q1. What is a Git repository?

A repository is a project managed by Git. It stores version history, commits, branches, and other metadata.

Q2. What is the difference between git init and git clone?

git init initializes a new repository in a directory.

git clone downloads an existing repository, including its history, into a new local directory.

Q3. What is the difference between git add and git commit?

git add stages selected changes.

git commit saves the staged changes as a snapshot in local history.

Q4. What is the purpose of the .git folder?

It stores Git's internal repository data, including commit history, references, configuration, and object data. Deleting it can remove the repository's local tracking information.

Q5. What is the difference between a local and a remote repository?

A local repository is on your computer. A remote repository is hosted elsewhere, often on GitHub, for backup and collaboration.

2. Git clone
git clone copies an existing Git repository from a remote location to your computer.

It normally downloads the project files, commit history, branches' references, and remote configuration.

Real-world example
Your teammate has created a shopping application on GitHub:

https://github.com/team-company/shopping-app.git

You want to work on the project on your own laptop.


bash
git clone https://github.com/team-company/shopping-app.git
cd shopping-app
git status
Check the remote connection:


bash
git remote -v
You will typically see origin pointing to the repository you cloned.

Now you can edit files, create commits, and work on branches.

GitHub repository

Shared project online

Your computer

Local working copy

Command: git clone URL

Common clone variations
Clone a repository:


bash
git clone https://github.com/user/project.git
Clone into a custom folder:


bash
git clone https://github.com/user/project.git my-project
Clone only the latest snapshot with limited history:


bash
git clone --depth 1 https://github.com/user/project.git
The last command creates a shallow clone, useful when you do not need the full commit history.

Frequently asked interview questions
Q1. What happens when you run git clone?

Git creates a local copy of a remote repository, downloads its history according to the clone options, checks out the selected branch, and configures a remote—usually named origin.

Q2. Does cloning automatically give you permission to push changes?

No. You need appropriate write access to the remote repository. If you only have read access, you can often clone it but cannot push directly to it.

Q3. What is origin in Git?

origin is the conventional default name for the remote repository from which a repository was cloned. It is a nickname, not a special GitHub server.

Q4. What is the difference between git clone and git pull?

git clone creates a local copy of a repository for the first time. git pull updates an existing local branch using changes from a remote repository.

Q5. Can you clone a private GitHub repository?

Yes, if you have permission and authenticate using an appropriate method, such as SSH or HTTPS with a supported credential.

3. Branches in Git
A branch is an independent line of development. It allows you to work on a feature or bug fix without making changes directly to the main development line.

Think of a shopping application where the main website is already working. You need to add a payment feature. You can create a separate branch, implement the payment feature, test it, and merge it into main after it is ready.

Git Worktree: Zero to Hero—Mastering Multiple Workspaces in Git
Important branch commands
Command

Purpose

git branch

List local branches

git branch feature-payment

Create a branch

git switch feature-payment

Switch to a branch

git switch -c feature-payment

Create and switch to a branch

git merge feature-payment

Merge a branch into the current branch

git branch -d feature-payment

Delete a merged local branch

Real-world example: Adding a payment feature
Start on main:


bash
git switch main
git switch -c feature-payment
You are now working on feature-payment. Create a file and commit your work:


bash
echo "Payment processing module" > payment.txt
git add payment.txt
git commit -m "Add payment module"
Switch back to the main branch:


bash
git switch main
The new file is not present on main yet because you committed it on feature-payment.

Merge the feature into main:


bash
git merge feature-payment
Now the changes from the feature branch are integrated into main.

Branch workflow

main — stable application

feature-payment — implement and test payment

Merge into main after review and testing

Local branches versus remote branches
Local branch: A branch in your local repository, such as feature-payment.

Remote-tracking branch: Your local record of a branch on a remote, such as origin/main.

Remote branch: A branch hosted on the remote repository, such as the main branch on GitHub.

To publish your new branch to GitHub:


bash
git push -u origin feature-payment
The -u option sets the upstream relationship so future git push and git pull commands can usually omit the remote and branch names.

Frequently asked interview questions
Q1. Why do we use branches in Git?

Branches allow developers to work on different features, bug fixes, or experiments independently without directly changing the stable branch.

Q2. What is the difference between git branch and git switch?

git branch lists, creates, or deletes branches. git switch changes the branch you are working on and can also create a new branch with -c.

Q3. What happens when you merge a branch?

Git integrates changes from the specified branch into your current branch. Depending on the history, Git may perform a fast-forward merge or create a merge commit.

Q4. What is a merge conflict?

A merge conflict happens when Git cannot automatically reconcile changes, commonly because different branches modify overlapping parts of a file.

Example: two developers change the same line in payment.py. Git may ask you to resolve the conflict manually, stage the resolved file, and complete the merge.


bash
git status
# Edit the conflicted file and resolve the conflict
git add payment.py
git commit
The final git commit is appropriate when you are completing a merge that requires a merge commit.

Q5. Can two developers work on different branches at the same time?

Yes. Each developer can work on a separate branch and later integrate their changes through a merge or pull request. Conflicts are possible if their changes overlap.

Q6. What is the difference between merging and rebasing?

git merge integrates changes and can preserve branch history with a merge commit.

git rebase replays commits onto a new base, creating new commit IDs for the replayed commits.

Rebasing is often used to make a feature branch's history more linear. Avoid rebasing shared commits unless the team has agreed on the workflow.

4. Branches on GitHub
GitHub branches are branches in a repository hosted on GitHub. They let teams collaborate, push code, review changes, and merge features using pull requests.

The key difference is that creating a local branch does not automatically create a branch on GitHub. You normally need to push it.

Real-world example: A team of three developers
Suppose your team is developing an e-commerce website.

Developer

Branch

Task

Developer A

feature-login

User login

Developer B

feature-payment

Payment integration

Developer C

fix-cart-bug

Fix shopping cart bug

The shared GitHub repository may contain:

main — the stable code.

feature-login — the login implementation.

feature-payment — the payment implementation.

fix-cart-bug — the bug fix.

Each developer pushes their branch to GitHub. Once the work is ready, they open pull requests so teammates can review the changes before merging them.

How to create and publish a GitHub branch
Create a local branch:


bash
git switch -c feature-login
Make and commit changes:


bash
git add .
git commit -m "Add login feature"
Push the branch to GitHub:


bash
git push -u origin feature-login
Next, open your GitHub repository in a browser and create a pull request from feature-login into main.

After the review and required checks pass, merge the pull request according to your team's process.

What is a pull request (PR)?
A pull request is a GitHub collaboration feature for proposing changes from one branch into another.

For example:

feature-login → main

A pull request allows the team to:

Review the code.

Discuss suggested changes.

Run automated tests and checks.

Approve the changes.

Merge them into the target branch.

A pull request is not the same as git pull. The similar wording often causes confusion in interviews.

What is a protected branch?
A protected branch has repository rules that restrict certain operations. For example, a team might protect main by requiring pull requests, approvals, or passing status checks before changes can be merged.

Frequently asked interview questions
Q1. What is the difference between a Git branch and a GitHub branch?

A Git branch is part of Git's version-control model and can exist locally or remotely. A GitHub branch is a branch stored on GitHub's hosted repository.

Q2. How do you push a local branch to GitHub?


bash
git push -u origin feature-login
This publishes the branch to origin and configures its upstream.

Q3. What is the difference between a pull request and a merge?

A pull request is a collaboration and review workflow on GitHub. A merge is the operation that integrates changes from one branch into another. A pull request may be merged after approval.

Q4. What is the default branch on GitHub?

It is the branch GitHub uses as the repository's primary branch for operations such as opening the repository's main code view and choosing the default target for new pull requests. It is commonly named main, but repository owners can configure a different name.

Q5. Can a GitHub branch be deleted after merging?

Yes. Teams often delete feature branches after merging to keep the repository organized. The merged commits remain accessible through the target branch, though deleting a branch can remove that branch reference.

Q6. What is a branch protection rule?

A rule that enforces repository policies, such as required reviews, status checks, restrictions on direct pushes, or other safeguards, depending on the repository's configuration.

5. Git pull
git pull updates your current local branch with changes from a remote repository.

In standard configurations, it performs two main operations:

git fetch — downloads new commits and updates remote-tracking references.

Integrates those changes into your current branch, usually by merging or, if configured, rebasing.

Real-world example: Your teammate updated GitHub
You and your teammate are working on the same shopping application.

Your teammate adds a new product page and pushes it to GitHub. Your local copy is now behind the remote branch.

You can retrieve the latest changes using:


bash
git switch main
git pull origin main
This fetches the latest changes from origin/main and integrates them into your local main branch.

Before pull

GitHub has the new product page; your local branch is outdated.

git pull origin main

After pull

Your local branch incorporates the remote changes.

Difference between git fetch and git pull
git fetch

git pull

Downloads remote updates

Downloads remote updates and integrates them

Does not merge into your current branch by itself

Usually merges or rebases into your current branch

Lets you inspect changes before integrating them

Convenient when you're ready to integrate changes

Example:


bash
git fetch origin
git log --oneline HEAD..origin/main
The log command displays commits reachable from origin/main that are not reachable from your current HEAD, assuming the remote-tracking reference is up to date.

When you're ready to integrate the updates:


bash
git merge origin/main
Alternatively, you can use git pull directly.

What if git pull causes a conflict?
Imagine your teammate changes the product price in product.py, and you have also changed the same line locally.

Git may report a merge conflict.

A typical recovery workflow is:


bash
git status
Open the conflicted file and look for markers like these:


python
<<<<<<< HEAD
price = 100
=======
price = 120
>>>>>>> origin/main
These markers show the competing versions. Decide which change to keep or combine them, then remove the markers and save the file.


bash
git add product.py
git commit
If the pull was configured to rebase instead of merge, resolve the conflict, stage the resolution, and continue with git rebase --continue instead. The exact recovery steps depend on the operation in progress.

Frequently asked interview questions
Q1. What is git pull?

It fetches changes from a remote repository and integrates them into the current branch, typically using merge or rebase.

Q2. What is the difference between git pull and git fetch?

Fetch downloads updates without integrating them into the current branch. Pull fetches and then integrates the updates.

Q3. What happens if you run git pull when your local branch has uncommitted changes?

It may succeed if the changes do not interfere with the update. If the update could overwrite or conflict with local changes, Git may refuse or report conflicts. Commit or stash your changes first when appropriate.

Q4. How do you pull changes from a specific remote branch?


bash
git pull origin feature-payment
This attempts to fetch and integrate the specified remote branch into your current branch. Switch to the intended target branch first.

Q5. Why does Git say there are divergent branches?

Your local and remote branches have commits that the other side does not have. Git needs an integration strategy, such as merge or rebase.

For example:


bash
git pull --no-rebase origin main
uses merge-based integration, while:


bash
git pull --rebase origin main
uses rebase-based integration.

Q6. What is the difference between git pull and git pull origin main?

git pull uses the current branch's configured upstream and pull settings. git pull origin main explicitly specifies the remote and branch to fetch and integrate. The exact effect depends on your current branch and Git configuration.

6. Git forking
A fork is your own copy of another user's or organization's repository on GitHub. You can make changes to your fork without having direct write access to the original repository.

Forking is especially useful for contributing to open-source projects.

Real-world example: Contributing to an open-source shopping application
Suppose an open-source project exists at:

https://github.com/open-source-team/shopping-app

You discover a bug in its shopping cart, but you do not have permission to push to the original repository.

Your workflow is:

Original repository

open-source-team/shopping-app

Fork on GitHub

Your fork

your-username/shopping-app

Clone, edit, commit and push

Pull request

Propose your fix to the original project

Step-by-step fork workflow
Step 1: Fork the repository

Open the original repository on GitHub and click Fork. GitHub creates a copy under your account.

Step 2: Clone your fork


bash
git clone https://github.com/your-username/shopping-app.git
cd shopping-app
Step 3: Add the original repository as upstream


bash
git remote add upstream https://github.com/open-source-team/shopping-app.git
git remote -v
Now you typically have two remotes:

origin — your fork.

upstream — the original repository.

Step 4: Create a branch for your fix


bash
git switch -c fix-cart-bug
Step 5: Fix the bug and commit


bash
git add .
git commit -m "Fix shopping cart calculation"
Step 6: Push your branch to your fork


bash
git push -u origin fix-cart-bug
Step 7: Open a pull request

On GitHub, create a pull request from your fork's fix-cart-bug branch to the original repository's appropriate target branch.

The original maintainers can review, request changes, and merge your contribution.

How to update your fork with the latest original changes
Suppose the original repository has received new commits since you forked it.

Fetch the updates:


bash
git fetch upstream
Update your local main branch:


bash
git switch main
git merge upstream/main
Push the updated branch to your fork:


bash
git push origin main
This example assumes the original repository's target branch is named main. If the original uses a different default branch, use that branch name instead.

Fork versus clone
Fork

Clone

Creates a separate repository on GitHub under your account

Creates a local working copy on your computer

Commonly used for contributing without direct write access

Used to work locally on a repository

Usually performed through GitHub's interface

Performed with git clone URL

Can be cloned after it is created

Does not automatically create a fork

You can fork a repository and then clone your fork. They are complementary operations, not alternatives that exclude each other.

Frequently asked interview questions
Q1. What is forking in GitHub?

Forking creates a separate GitHub repository under your account based


You said:
you are git expert so explain me topics {1. PR and how to resolve PR conflicts} include most asked interview question  on given topics and answers with real example on topics.

ChatGPT said:
Git Interview Preparation: Pull Requests (PRs) and Resolving PR Conflicts
I'll explain this as a Git expert preparing you for real-world development and technical interviews. We'll cover the concepts, practical commands, real project examples, common mistakes, and frequently asked interview questions with answers.

1. What is a Pull Request (PR)?
A Pull Request (PR) is a request to merge changes from one branch into another branch. It allows other developers to review your code, suggest improvements, run automated tests, and approve the changes before merging.

For example, in a team project:

main contains stable production-ready code.

You create a feature branch to implement a login API.

You commit and push your changes.

You create a PR to merge your feature branch into main.

Your teammates review the code, and after approval, it gets merged.

Pull Request workflow
main branch

Stable code

Create feature branch

feature/login-api

Code → Commit → Push

Upload your changes to GitHub/GitLab

Create PR → Review → Approve

Merge into main

Feature becomes part of the target branch

Important: Is a PR a Git command?
No. A Pull Request is a collaboration feature provided by platforms such as GitHub, GitLab, and Bitbucket. Git itself provides the commands to create branches, commit changes, push code, merge branches, and resolve conflicts.

GitHub uses the term Pull Request (PR); GitLab commonly uses Merge Request (MR). They serve essentially the same purpose.

2. Real-world example: Creating a PR
Suppose you're working on an e-commerce application, and your task is to add a login API.

Your repository looks like this:

text
ecommerce-app/
├── src/
│   ├── auth/
│   └── products/
└── README.md
Your team asks you to implement POST /api/login.

Step 1: Get the latest code
bash
git switch main
git pull origin main
git switch main switches to the main branch.

git pull origin main fetches and integrates the latest changes from the remote main branch.

Step 2: Create a feature branch
bash
git switch -c feature/login-api
This creates and switches to a new branch called feature/login-api.

Why do we create a feature branch? Because you should develop your feature separately without directly modifying the shared main branch.

Step 3: Implement and commit your code
After adding the login API:

bash
git status
git add src/auth/login.js
git commit -m "feat: add login API"
What these commands do:

git status: Shows changed files.

git add: Stages changes for the next commit.

git commit: Records a snapshot of the staged changes.

Step 4: Push the branch to the remote repository
bash
git push -u origin feature/login-api
The -u option sets the upstream tracking branch, so future pushes can usually use git push.

Step 5: Open the PR
On GitHub:

Open your repository.

Select Pull requests.

Click New pull request.

Set the base branch to main.

Set the compare branch to feature/login-api.

Review the diff and add a title and description.

Create the PR.

Example PR description:

PR #42: Add login API
Base: main ← Compare: feature/login-api

Description

Implemented POST /api/login.

Added request validation.

Added invalid-credentials handling.

Added unit tests for successful and failed login attempts.

Testing

Login with valid credentials: passed.

Login with invalid credentials: passed.

Missing required fields: passed.

After opening the PR, teammates can comment on your code, request changes, and approve it. Once the required checks and approvals pass, you can merge it according to your team's rules.

3. What is a PR conflict?
A PR conflict occurs when Git cannot automatically combine changes from two branches.

It commonly happens when:

Two developers modify the same lines in the same file.

One developer changes a line while another deletes it.

A feature branch falls behind the target branch, and new changes overlap with its changes.

Git cannot automatically reconcile different file histories or changes.

Important interview point: Different changes in the same file do not necessarily cause a conflict. Git can often merge them automatically if the changes do not overlap.

Real-world conflict example
Suppose the main branch contains this code in src/config.js:

javascript
const appName = "ShopApp";
const timeout = 3000;
You create a branch called feature/login-api and change the timeout:

javascript
const appName = "ShopApp";
const timeout = 5000;
Meanwhile, another developer changes the same line on main:

javascript
const appName = "ShopApp";
const timeout = 10000;
Now your PR tries to merge feature/login-api into main.

Git cannot determine whether the correct timeout should be 5000 or 10000. Both branches changed the same line differently, so a conflict may occur.

Git marks the conflicting section like this:

javascript
const appName = "ShopApp";

<<<<<<< HEAD
const timeout = 10000;
=======
const timeout = 5000;
>>>>>>> feature/login-api
What do these markers mean?

Marker

Meaning

<<<<<<< HEAD

Start of the current checked-out branch's version

=======

Separates the two versions

>>>>>>> feature/login-api

End of the incoming branch's version

When using git merge, HEAD usually represents the branch receiving the merge. When using git rebase, the labels and meanings can differ, so don't assume HEAD always represents the target branch.

Git has not broken your code; it is asking you to decide which changes should be preserved.

4. How to resolve PR conflicts step by step
There are two common approaches: merge the latest target branch into your feature branch, or rebase your feature branch onto the latest target branch.

We'll first use the merge approach because it is straightforward and common in team environments.

Method 1: Resolve conflicts using git merge
Assume:

Target branch: main

Your branch: feature/login-api

Conflict file: src/config.js

Fetch the latest remote changes.

bash     git fetch origin     

This downloads the latest remote branch information without merging it into your current branch.

Switch to your feature branch.

bash     git switch feature/login-api     

Merge the target branch into your feature branch.

bash     git merge origin/main     

If the same lines have conflicting changes, Git pauses the merge and reports the conflicted files.

Identify the conflicted files.

bash     git status     

Example output:

```text
On branch feature/login-api
You have unmerged paths.

both modified: src/config.js
```

Open src/config.js in your editor and inspect the conflict markers.

Resolve the conflict manually.

Suppose the team decides the timeout should be 5000 milliseconds. Update the file to:

javascript     const appName = "ShopApp";     const timeout = 5000;     

Remove all conflict markers and preserve the correct business logic. Do not simply choose one side without understanding the intended behavior.

Stage the resolved file and complete the merge.

bash     git add src/config.js     git commit -m "resolve merge conflict in config"     

When you merge with the default Git behavior, Git generally creates a merge commit after you stage the resolved files and commit.

Test and push your changes.

bash     npm test     git push origin feature/login-api     

Run the relevant tests for your project. If they pass, push your branch. The existing PR will normally update automatically.

Note: The example uses npm test for a JavaScript project. Use your project's actual test command.

What happens after resolving the conflict?
Conflict resolved locally

The file contains the intended final code.

Changes committed and pushed

The remote feature branch is updated.

PR checks run again

Review the diff and verify that CI checks pass.

A conflict resolution does not automatically mean the PR is approved. Reviewers may still request changes, and automated checks may still fail.

5. Method 2: Resolve PR conflicts using git rebase
Rebase is another common approach. It reapplies your feature branch's commits on top of the latest target branch.

Use the same example:

bash
git fetch origin
git switch feature/login-api
git rebase origin/main
If a conflict occurs, Git pauses and asks you to resolve it.

Open the conflicted file.

Edit it to contain the correct final code.

Stage the resolved file.

Continue the rebase.

bash
git add src/config.js
git rebase --continue
Repeat the resolution process if another commit introduces a conflict.

After the rebase completes, run tests and push:

bash
npm test
git push --force-with-lease origin feature/login-api
Why --force-with-lease? Rebasing rewrites commit history. If the feature branch was already pushed, a normal push may be rejected because the remote history no longer matches. --force-with-lease is safer than --force because it checks that the remote branch has not changed unexpectedly.

Only rewrite a shared branch's history when your team agrees. If others are working on the same branch, coordinate before rebasing and force-pushing.

Merge vs. rebase: Interview comparison
Feature

Merge

Rebase

What it does

Combines branch histories

Replays commits on a new base

Commit history

Preserves branch history; may create a merge commit

Produces a more linear history

Rewrites commits

Normally no

Yes, the replayed commits get new IDs

Conflict handling

Resolve, stage, then complete the merge

Resolve, stage, then continue the rebase

Best use

Preserve shared branch history

Keep private or coordinated feature history linear

Shared branch safety

Generally safer for existing history

Avoid rewriting shared history without coordination

Neither method is universally better. Follow your team's branching and merge policy.

6. Useful commands for PR conflicts
Command

Purpose

git status

See the current branch and conflicted files

git diff

Inspect unstaged changes

git diff --merge

Inspect conflict-related differences in supported merge states

git diff --ours --theirs

Not a standard combined command; use the separate commands below instead

git diff --ours -- path/to/file

Inspect the current side during an unresolved merge

git diff --theirs -- path/to/file

Inspect the incoming side during an unresolved merge

git merge --abort

Abort an in-progress merge and return to its pre-merge state when possible

git rebase --abort

Abort an in-progress rebase and return to its pre-rebase state

git add filename

Mark a file's conflicts as resolved after editing

git merge --continue

Continue a merge if Git requires that command for the current merge state

git rebase --continue

Continue after resolving a rebase conflict

git log --oneline --graph --all

Visualize branch history

git push

Upload commits to the remote branch

Interview tip: git add does not resolve the conflict itself. You must edit the file correctly first, then stage it.

7. How to resolve PR conflicts directly on GitHub
Sometimes GitHub shows a message such as:

This branch has conflicts that must be resolved.

If the conflict is simple and GitHub enables its conflict editor, you can resolve it in the browser.

Open the PR.

Click Resolve conflicts.

Edit the conflicted files and remove the conflict markers.

Click Mark as resolved for each resolved file.

Click Commit merge to commit the resolution.

GitHub's browser editor has limitations. For complex conflicts, binary files, or conflicts requiring testing, resolve them locally instead.

8. What if you want to cancel a conflict resolution?
Suppose you started merging main into your feature branch but realize you should not have started the merge.

For an in-progress merge:

bash
git merge --abort
For an in-progress rebase:

bash
git rebase --abort
These commands attempt to return your working tree to the state before the operation started. They are not interchangeable.

Interview question: What is the difference between git merge --abort and git rebase --abort?

Answer: git merge --abort cancels a merge in progress; git rebase --abort cancels a rebase in progress. Both are used to abandon the respective operation, not to fix conflicts.

9. Most-asked PR interview questions and answers
Beginner-level questions
Q1. What is a Pull Request?
Answer: A Pull Request is a request to review and integrate changes from one branch into another. It supports code reviews, discussions, automated testing, and approval before merging.

Example: I create feature/payment-api, push my changes, and open a PR targeting main. My teammates review the implementation before it is merged.

Q2. What is the difference between a branch and a PR?
Answer:

A branch is a separate line of development in Git.

A PR is a platform-level request to review and merge the changes from a source branch into a target branch.

A branch can exist without a PR, and creating a PR does not itself merge the branch.

Q3. What are the base branch and compare branch in a PR?
Answer:

Base branch: The destination that receives the changes.

Compare branch: The source containing the proposed changes.

Example:

text
base:    main
compare: feature/login-api
This means you want to merge changes from feature/login-api into main.

Q4. What happens after creating a PR?
Answer: Usually, the team reviews the diff, automated CI checks run, reviewers leave comments or approve, the author addresses requested changes, and the PR is merged after all required conditions are satisfied.

The exact process depends on the repository's rules.

Q5. Can we update a PR after creating it?
Answer: Yes. Commit and push additional changes to the same source branch.

bash
git add .
git commit -m "fix login validation"
git push origin feature/login-api
The existing PR updates automatically. You generally do not need to create a new PR.

Q6. What is a code review?
Answer: Code review is the process of checking another developer's changes for correctness, readability, maintainability, security, performance, and compliance with project standards.

Q7. Can we merge a PR without approval?
Answer: Technically, sometimes yes. But repository rules may require one or more approvals, passing CI checks, resolved conversations, or other conditions. Follow the repository's protection rules.

Intermediate-level questions
Q8. What causes a PR conflict?
Answer: A conflict occurs when Git cannot automatically reconcile changes in the source and target histories.

For example, both developers modify this line differently:

javascript
const maxRetries = 3;
One changes it to 5, and another changes it to 10. Git needs a human or another deliberate resolution strategy to determine the correct result.

Q9. How do you resolve a PR conflict?
Answer: My usual approach is:

Fetch the latest target branch.

Switch to my feature branch.

Merge the target branch into my feature branch.

Inspect the conflicted files.

Edit the files to preserve the correct logic.

Stage and commit the resolutions.

Run tests and push the updated feature branch.

Verify the PR checks and review status.

Example:

bash
git fetch origin
git switch feature/login-api
git merge origin/main

# Resolve the files manually.

git add src/config.js
git commit -m "resolve merge conflict"
git push origin feature/login-api
Q10. What is the difference between a merge conflict and a PR review comment?
Answer:

A merge conflict means Git cannot automatically combine changes.

A review comment is feedback from a reviewer about your code.

A PR can have review comments without any merge conflicts, and a PR can have conflicts even when the reviewer has no comments.

Q11. What is the difference between merge and rebase?
Answer: Merge combines branch histories and can create a merge commit. Rebase replays commits onto a new base and rewrites their commit IDs.

I use merge when preserving existing shared history matters and rebase when a linear feature-branch history is desired and rewriting that branch is safe.

Q12. What is squash merging?
Answer: Squash merging combines all the PR's changes into a single commit on the target branch, rather than preserving each source-branch commit separately.

For example, your feature branch contains:

text
commit 1: create login endpoint
commit 2: fix validation
commit 3: fix tests
A squash merge can put the combined changes into one commit:

text
feat: implement login endpoint
This creates a cleaner target-branch history, but the original feature-branch commits are not preserved as separate commits on the target branch.

Q13. What is the difference between a merge commit and a squash merge?
Answer:

Merge commit: Preserves the branch history and records the integration of the branches.

Squash merge: Combines the PR's changes into one commit on the target branch.

Both are valid strategies. The team's history and release-management policies determine which is preferred.

Q14. Can two PRs conflict with each other?
Answer: Yes. Suppose Developer A creates a PR that changes src/config.js, while Developer B creates another PR that changes the same lines. When one PR is merged, the other may become conflicted or require updating.

The correct fix is to synchronize the second feature branch with the updated target branch, resolve conflicts, run tests, and update its PR.

Q15. What should you do if CI fails after resolving a conflict?
Answer: Investigate the failed job, check whether the resolution introduced a bug or invalidated tests, correct the code, rerun the relevant tests, commit and push the fix, and verify the CI result.

Resolving a textual conflict does not guarantee the resulting code is logically correct.

Advanced interview questions
Q16. What is the difference between git fetch and git pull?
Answer:

git fetch downloads remote updates but does not integrate them into your current branch.

git pull fetches updates and then integrates them, typically using merge or rebase depending on configuration and options.

Example:

bash
git fetch origin
git log --oneline HEAD..origin/main
This lets you