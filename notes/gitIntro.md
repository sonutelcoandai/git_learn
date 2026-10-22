Prompt:--   you are git expert so explain me topics {} include most asked interview question  on given topics and answers with real example on topics.

# Git & GitHub Complete Guide for Beginners + Interview Preparation

This guide explains Git, GitHub, GitHub repository structure, GitHub Desktop, installation and setup, practical commands, real-world examples, and frequently asked interview questions with answers.

The goal is to help you understand Git practically and confidently answer Git interview questions for fresher and junior developer roles.

## 1. What is Git and GitHub? What is the key difference?

Hatem Soliman | Systems Engineer

## Git

Git is a distributed version control system that tracks changes in your code.

- Works on your computer.
- Maintains code history.
- Helps create branches.
- Allows you to restore older versions.

Devika AI Pricing & Reviews 2025 | Techjockey.com

## GitHub

GitHub is an online platform that hosts Git repositories and supports collaboration.

- Stores code remotely.
- Enables pull requests and code reviews.
- Manages issues and project discussions.
- Supports team collaboration.

### Key differences

| Feature | Git | GitHub |
| --- | --- | --- |
| Type | Version control software | Online collaboration platform |
| Where it works | Primarily on your local computer | Cloud / web, with integrations |
| Main purpose | Track code changes | Host and collaborate on Git projects |
| Internet required? | No, for most local operations | Generally required for remote operations |
| History | Stored in local repository | Stored in the hosted repository too |
| Examples | `git commit`, `git branch` | Pull requests, issues, code reviews |

Interview answer: Git is a distributed version control system used to track changes in source code. GitHub is a platform that hosts Git repositories and provides collaboration features such as pull requests, code reviews, and issue tracking.

### Real-life example

Imagine you are developing a student management application.

- Git records changes when you add login functionality.
- Git lets you create a separate branch to develop a registration page.
- GitHub hosts your project online so your teammates can access it.
- You open a pull request on GitHub so your teammates can review your changes before merging them.

Remember: Git is the version control tool; GitHub is one place to host and collaborate on Git repositories. Other hosting platforms include GitLab and Bitbucket.

## 2. What is GitHub? Important terminology

Before learning commands, understand these basic terms.

Repository (repo)

A project folder whose files and change history are tracked by Git.

Example: `student-management-system`

Commit

A saved snapshot of changes, identified by a unique hash.

Example: `Add student login page`

Branch

An independent line of development.

Example: `feature/student-login`

Pull request (PR)

A GitHub request to review and merge changes from one branch into another.

Merge

Combines changes from one branch into another.

Push and pull

`push` uploads local commits to a remote repository; `pull` fetches remote changes and integrates them into your current branch.

## 3. GitHub repository folder structure

A GitHub repository can contain almost any folder or file structure. Git does not require one particular project layout.

For example, a simple web application might look like this:

student-management-system/

`src/`

`index.html` — page structure

`style.css` — page styling

`app.js` — application logic

`tests/`

`app.test.js` — automated tests

`README.md`

Project documentation

`.gitignore`

Files Git should ignore

`package.json`

Node.js project metadata

`.git/`

Local Git history and metadata

Note: `.git/` is normally hidden inside your local repository. It is not usually displayed as a normal project folder on GitHub.

### Important files and folders

| File or folder | Purpose |
| --- | --- |
| `README.md` | Explains what the project does, how to install it and how to run it |
| `.gitignore` | Lists untracked files Git should ignore |
| `.git/` | Stores local repository data, commits, refs and configuration |
| `src/` | Contains application source code |
| `tests/` | Contains automated tests |
| `package.json` | Defines Node.js project metadata, dependencies and scripts |
| `LICENSE` | Specifies permissions and conditions for using the code |
| `docs/` | Contains additional documentation, if needed |

### What should go into `.gitignore`?

For a Node.js project, you might use:

gitignore
```
node_modules/
.env
dist/
coverage/
.DS_Store
```

- `node_modules/`: usually recreated by installing dependencies.
- `.env`: often contains API keys or passwords.
- `dist/`: generated build output, depending on your project.
- `coverage/`: generated test reports.

Important: `.gitignore` does not automatically stop tracking a file that Git already tracks. Remove a tracked file from the index with `git rm --cached filename` if you need Git to stop tracking it while retaining the local file. If a secret has already been pushed, revoke or rotate it; ignoring it afterward does not remove it from history.

### Interview question: What is the difference between `.git` and `.gitignore`?

Answer: `.git` is the internal directory that stores Git repository information and history. `.gitignore` is a text file that specifies patterns for untracked files Git should ignore.

## 4. Git installation and setup

You need to install Git before using Git commands in a terminal. GitHub itself does not require Git to browse repositories, but Git is needed for normal local command-line workflows.

### Step 1: Download and install Git

How To Install Git On Windows 10 | Tutorials24x7

Git — official download

Download Git for your operating system.

Windows, macOS and Linux

[Download Git from git-scm.com](<https://git-scm.com/install/?utm_source=chatgpt.com>)

- Windows: Run the installer. The default options are generally suitable for beginners. Ensure Git is available from the command line.
- macOS: Install using the official installer or, if you use Homebrew, run `brew install git`.
- Ubuntu/Debian Linux: Run `sudo apt update` followed by `sudo apt install git`.

### Step 2: Verify installation

Open Terminal, PowerShell, Git Bash or your IDE's terminal and run:

bash
```
git --version
```

Example output:

text
```
git version 2.51.0
```

The exact version may differ. If a version is displayed, Git is installed and accessible.

### Step 3: Configure your identity

Git records an author name and email address in your commits.

bash
```
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Replace these example values with your preferred name and email. If you want your commits associated with your GitHub account, use an email address associated with that account or a GitHub-provided private email address.

Check the settings:

bash
```
git config --global --list
```

Interview tip: `git config --global` applies a setting to your user account on that computer. A repository-specific setting, configured without `--global` while inside that repository, can override it.

### Step 4: Create your first Git repository

Let's create a simple project called `student-app`.

bash
```
mkdir student-app
cd student-app
git init
```

Create a file named `README.md` and add:

markdown
```
# Student App

A simple project for managing student records.
```

Now check the repository:

bash
```
git status
```

Git should show `README.md` as an untracked file.

### Step 5: Make your first commit

bash
```
git add README.md
git commit -m "Add project README"
```

What happened?

1. `git add` staged the file.
2. `git commit` saved the staged change as a new commit in local history.
3. The commit message describes what you changed.

Verify the result:

bash
```
git log --oneline
```

Example:

text
```
a1b2c3d Add project README
```

The hash shown in your terminal will be different.

## 5. Understand the Git workflow

The following is one of the most important concepts for Git interviews: the relationship between the working directory, staging area, local repository and remote repository.

1. Working directory

Edit files in your project

2. Staging area

Choose changes with `git add`

3. Local repository

Save a snapshot with `git commit`

4. Remote repository

Upload commits with `git push`

Example workflow:

bash
```
git status
git add .
git commit -m "Add student registration"
git push
```

The final `git push` works when your local branch has an appropriate remote and upstream configured. Otherwise, you may need to configure the remote or use `git push -u origin main`.

Remember the difference:

- `git add` does not create a commit.
- `git commit` does not upload changes to GitHub.
- `git push` uploads local commits to a remote repository.

## 6. Most important Git commands with real examples

| Command | What it does | Example use |
| --- | --- | --- |
| `git init` | Creates a local repository | Start tracking a new project |
| `git clone URL` | Copies a remote repository locally | Download a teammate's project |
| `git status` | Shows current changes and staging state | Check what needs committing |
| `git add .` | Stages eligible changes in the current directory tree | Prepare several files for commit |
| `git commit -m "message"` | Saves staged changes | Save a feature |
| `git log --oneline` | Shows concise commit history | Find a previous commit |
| `git diff` | Shows unstaged changes | Review your edits |
| `git branch` | Lists local branches | Check your current branch |
| `git switch -c feature/login` | Creates and switches to a branch | Start a new feature |
| `git merge feature/login` | Merges a branch into the current branch | Integrate a feature |
| `git pull` | Fetches and integrates remote changes | Update your local branch |
| `git push` | Uploads commits to a remote | Share your changes |
| `git stash` | Temporarily shelves working changes | Switch tasks without committing |
| `git revert HASH` | Creates a new commit that reverses another commit | Safely undo a shared change |
| `git reset` | Moves the current branch reference and can change staging or working files | Undo local commits or unstage files, depending on options |

### Real-world example: Develop a login feature

Assume the main branch contains your stable application. You want to implement a login page without directly changing `main`.

bash
```
git switch main
git pull
git switch -c feature/login
```

Edit your login files, then run:

bash
```
git status
git add src/login.js
git commit -m "Add login functionality"
git push -u origin feature/login
```

Now open GitHub and create a pull request from `feature/login` into `main`. After review and any required checks, merge the pull request.

This is a common team development workflow.

## 7. What is GitHub Desktop?

GitHub Desktop | Simple collaboration from your desktop

GitHub Desktop is a graphical application for working with Git repositories without typing every command.

You can use it to view changes, stage files, commit, create branches, push and pull, and work with GitHub pull requests.

Official download: [desktop.github.com](<https://desktop.github.com/?utm_source=chatgpt.com>)

### Git vs GitHub Desktop

| Git | GitHub Desktop |
| --- | --- |
| Command-line version control tool | Graphical interface for Git |
| Uses terminal commands | Uses menus, buttons and visual diffs |
| Supports scripting and advanced workflows | Makes common workflows easier to see |
| Works without GitHub | Designed to work with local Git repositories and GitHub |

GitHub Desktop does not replace the underlying Git concepts. Learning both the GUI and the command line is useful.

### How to use GitHub Desktop

GitHub Setup | PUI-Spr23

Step 1: Install and sign in

Download the application and sign in to GitHub when prompted. You can also use it with local repositories.

New repo: Add description field · Issue #2719 · desktop/desktop · GitHub

Step 2: Create or clone a repository

Choose Create a New Repository for a new project or Clone a Repository to download an existing project.

Intro to Marine Data Science – Module Setup and Submission

Step 3: Edit, review and commit

Modify your project files. GitHub Desktop displays changed files and line-by-line differences. Select the changes to include and enter a commit summary.

Version Control with Git and GitHub — Version Control with Git and GitHub

Step 4: Publish or push

Publish a new local repository to GitHub, or push commits from an existing repository to its remote.

### Practical GitHub Desktop example

Suppose you update `README.md` to explain how to run your student app.

1. Open the repository in GitHub Desktop.
2. Edit `README.md` in your editor.
3. Return to GitHub Desktop and inspect the Changes tab.
4. Enter a summary such as `Improve setup instructions`.
5. Click Commit to main if that is the intended branch.
6. Click Push origin to upload the commit, if a remote is configured.

The commit is local until you push it. Always check the current branch before committing.

## 8. How to connect your local project to GitHub

This is a common practical interview task: create a project locally and upload it to GitHub.

### Step 1: Create a repository on GitHub

1. Sign in at [github.com](<https://github.com/?utm_source=chatgpt.com>).
2. Click the + menu and choose New repository.
3. Enter a name, such as `student-app`.
4. Choose Public or Private.
5. If your local project already contains files, creating the GitHub repository without initializing extra files such as a README makes the initial push simpler.
6. Click Create repository.

### Step 2: Connect the local repository

If you already initialized and committed your local project, copy the repository URL from GitHub and run:

bash
```
git remote add origin https://github.com/YOUR-USERNAME/student-app.git
git branch -M main
git push -u origin main
```

Replace `YOUR-USERNAME` with your actual GitHub username.

What each command means:

- `git remote add origin URL`: connects your local repository to a remote named `origin`.
- `git branch -M main`: renames the current branch to `main`.
- `git push -u origin main`: uploads the branch and sets its upstream for future pushes and pulls.

You may need to authenticate through a browser or credential manager. A GitHub account password generally cannot be used as Git's HTTPS password; use a supported authentication method.

### Step 3: Verify the remote

bash
```
git remote -v
git status
git log --oneline
```

Refresh your repository page on GitHub to see the uploaded files and commit history.

Interview question: What is `origin`?

`origin` is the conventional default name for a remote repository. It is simply a name, not a special GitHub command or a mandatory name.

## 9. Most asked Git and GitHub interview questions with answers

These are useful questions to practise for fresher, junior developer and internship interviews.

### Beginner-level questions

Q1. What is version control?

Version control is a system that tracks changes to files over time. It helps developers see what changed, who changed it, and when, and lets them restore or compare earlier versions.

Example: If a new feature breaks your application, you can inspect the previous commit to find the change that introduced the problem.

Q2. What is the difference between Git and GitHub?

Git is the version control system that tracks changes and manages commits. GitHub hosts Git repositories online and adds collaboration features such as pull requests, code reviews and issue tracking.

Q3. What is a repository?

A repository is a Git-managed project with its tracked files and commit history.

Example: `student-app` can be a repository containing application code, tests and documentation.

Q4. What is a commit?

A commit is a recorded snapshot of staged changes. Each commit has a unique hash and metadata, including the author and commit message.

bash
```
git add .
git commit -m "Add student dashboard"
```

Q5. What is a branch?

A branch is a movable reference to a commit that supports a separate line of development.

Example: A developer creates `feature/payment` to implement payment functionality without making those changes directly on `main`.

Q6. What is the difference between `git add` and `git commit`?

`git add` stages selected changes for the next commit. `git commit` records the staged changes in the local repository.

bash
```
git add app.js
git commit -m "Fix app validation"
```

Q7. What is the difference between `git fetch` and `git pull`?

`git fetch` downloads remote updates and updates remote-tracking references without integrating them into your current branch. `git pull` fetches updates and then integrates them, usually by merging or rebasing depending on configuration and options.

Q8. What is the difference between `git clone` and `git init`?

- `git init` initializes a new local repository in a directory.
- `git clone` copies an existing repository into a new local directory, normally including its history and remote configuration.

### Intermediate-level questions

Q9. What is a merge conflict? How do you resolve it?

A merge conflict occurs when Git cannot automatically reconcile competing changes, such as when two branches modify the same lines.

Example: One developer changes a student's status to `active`, while another changes the same line to `inactive`.

A typical resolution process:

1. Run the merge or pull operation.
2. Run `git status` to identify conflicted files.
3. Open each file and choose the correct final content.
4. Remove conflict markers such as `<<<<<<<`, `=======` and `>>>>>>>`.
5. Stage the resolved files and complete the merge.

bash
```
git add student.js
git commit -m "Resolve student status conflict"
```

For a merge already in progress, the final commit completes the merge if Git requires one. Some pull strategies may instead require completing a rebase.

Q10. What is the difference between `git merge` and `git rebase`?

- Merge: Integrates histories, generally preserving the branch structure and potentially creating a merge commit.
- Rebase: Replays commits onto a new base, creating new commit hashes and often producing a more linear history.

Example:

bash
```
git switch feature/login
git fetch origin
git rebase origin/main
```

Rebase is often useful for cleaning up a feature branch. Avoid rebasing commits other people already depend on unless your team has agreed to rewrite that history.

Q11. What is the difference between `git reset` and `git revert`?

`git reset` moves the current branch reference and, depending on the options, changes the staging area and working files. `git revert` creates a new commit that reverses the changes introduced by an earlier commit.

Example of reverting a commit:

bash
```
git revert COMMIT_HASH
```

Revert is generally the safer choice for undoing changes in shared history because it does not remove the original commit.

Q12. What is `git stash`?

It temporarily saves working changes so you can switch tasks without creating a normal commit.

bash
```
git stash
git switch main
git switch feature/login
git stash pop
```

`git stash pop` attempts to reapply the saved changes and removes that stash entry if the application succeeds. Conflicts may need to be resolved manually.

Q13. What is a pull request?

A pull request (PR) is a GitHub feature for proposing, reviewing and discussing changes before merging one branch into another.

Example: You push `feature/login` and open a PR to merge it into `main`. A teammate reviews the code and requests a fix before approving it.

Q14. What is `.gitignore`?

`.gitignore` specifies patterns for untracked files that Git should ignore, such as generated files, dependency folders or local configuration files.

gitignore
```
node_modules/
.env
dist/
```

It does not automatically untrack files already committed.

Q15. What is HEAD in Git?

`HEAD` normally identifies the currently checked-out commit, typically through the current branch reference.

Example: If you switch to `feature/login`, `HEAD` normally points to that branch's current commit. In detached HEAD state, it can point directly to a commit rather than a branch.

Q16. What is the staging area?

The staging area, also called the index, records the changes prepared for the next commit. It allows you to commit only selected changes rather than everything in your working directory.

Q17. What is a remote repository?

A remote repository is another copy of a Git repository that your local repository can communicate with.

Example: `origin` may refer to your project's GitHub repository.

bash
```
git remote -v
git push origin main
```

### Scenario-based interview questions

Q18. You accidentally committed the wrong file. What would you do?

If you have not pushed the commit, you can amend it or create another commit to correct it, depending on the situation.

To remove a file from the most recent commit while keeping it on disk:

bash
```
git rm --cached secrets.txt
git commit --amend
```

This assumes `secrets.txt` was included in the last commit and you have removed it from the index before amending. If the file contains a real secret, revoke or rotate the secret immediately. Amending a commit does not guarantee that the secret is erased from all copies or history.

Q19. Your teammate pushed changes, and your push is rejected. What do you do?

Your local branch may be behind the remote branch. Inspect the situation and integrate the remote changes before pushing again.

bash
```
git fetch origin
git status
git pull --rebase
git push
```

This example assumes the current branch tracks the correct remote branch. Resolve any conflicts if prompted. Avoid force-pushing as a first response.

Q20. How do you undo changes in a file before committing?

To discard unstaged edits to a file:

bash
```
git restore app.js
```

To unstage a file while keeping its edits:

bash
```
git restore --staged app.js
```

Warning: Restoring unstaged edits discards those edits. Inspect `git status` and `git diff` first.

Q21. How do you find who changed a particular line?

Use:

bash
```
git blame app.js
```

It displays the last commit attributed to each line, along with author and commit information. It is useful for investigating code history, although it does not necessarily reveal the full reasoning behind a change.

Q22. How do you work with multiple developers on the same project?

A common approach is to keep a stable `main` branch, create feature branches, push work to GitHub, open pull requests, review code, run automated tests and merge approved changes.

This reduces direct changes to the shared stable branch and makes collaboration easier to review.

## 10. Git command cheat sheet

Keep this list for daily practice.

| Goal | Command |
| --- | --- |
| Check Git version | `git --version` |
| Configure name | `git config --global user.name "Your Name"` |
| Create repository | `git init` |
| Copy repository | `git clone URL` |
| Check changes | `git status` |
| Review unstaged edits | `git diff` |
| Stage one file | `git add file.js` |
| Commit changes | `git commit -m "Message"` |
| View history | `git log --oneline` |
| List branches | `git branch` |
| Create and switch branch | `git switch -c feature/name` |
| Switch branches | `git switch main` |
| Download remote updates | `git fetch` |
| Integrate remote changes | `git pull` |
| Upload commits | `git push` |
| Show remote URLs | `git remote -v` |
| Temporarily save changes | `git stash` |
| Undo a commit safely | `git revert HASH` |

## 11. Test your interview knowledge

## Git & GitHub quiz

0 of 5 answered

1. Which command saves staged changes in local Git history?

git add

git commit

git push

git fetch

2. Which tool hosts Git repositories and provides pull requests?

Git Bash

Git

GitHub

README.md

3. What does git fetch do?

Deletes a branch

Downloads remote updates without integrating them

Creates a commit

Uploads local commits

4. Which command creates and switches to a new branch?

git branch -d feature/login

git merge feature/login

git switch -c feature/login

git push feature/login

5. Which command is usually safest to undo a commit already shared with a team?

git revert

git reset --hard

Delete the .git folder

git clean -fd

Score: 0/5

 Reset quiz

## 12. Recommended learning plan

1. Day 1: Understand Git vs GitHub, install Git and configure your identity.
2. Day 2: Practise `init`, `status`, `add`, `commit`, `log` and `diff`.
3. Day 3: Create a GitHub account, create a repository and push a local project.
4. Day 4: Learn branches, merging, pull requests and conflict resolution.
5. Day 5: Install GitHub Desktop and practise the same workflow using its interface.
6. Day 6: Practise `fetch`, `pull`, `push`, `stash`, `restore`, `revert` and `reset`.
7. Day 7: Answer the interview questions aloud and build a small project with a feature branch and pull request.

For hands-on learning, use the official [Git documentation](<https://git-scm.com/doc?utm_source=chatgpt.com>), [GitHub Skills tutorials](<https://skills.github.com/?utm_source=chatgpt.com>) and [GitHub Desktop documentation](<https://docs.github.com/en/desktop?utm_source=chatgpt.com>).

Final interview tip: Don't just memorize command definitions. Be prepared to explain a real workflow: create a repository → modify files → stage → commit → branch → push → open a pull request → review → merge. If you can perform and explain that workflow yourself, you will have a strong foundation for most beginner Git and GitHub interview questions.