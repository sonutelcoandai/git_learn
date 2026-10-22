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