
# Git & GitHub — Practical Learning and Interview Guide

> Learn Git through hands-on practice, understand real-world workflows, and prepare for technical interviews with confidence.

A practical reference for developers who want to master version control, troubleshoot common Git problems, and collaborate effectively using GitHub.

**Learn by doing. Practice with purpose. Contribute to open source.**

<p align="center">
  <a href="https://github.com/sonutelcoandai/git_learn">
    <img src="https://img.shields.io/badge/Git-Learning-181717?style=for-the-badge&logo=git&logoColor=white" alt="Git Learning">
  </a>
  <a href="https://github.com/sonutelcoandai/git_learn/fork">
    <img src="https://img.shields.io/badge/Fork-This%20Project-2ea44f?style=for-the-badge&logo=github" alt="Fork this project">
  </a>
  <a href="https://github.com/sonutelcoandai/git_learn/stargazers">
    <img src="https://img.shields.io/badge/Star-Support%20Learning-yellow?style=for-the-badge&logo=github" alt="Star the repository">
  </a>
</p>

---

## Why This Repository?

Git is an essential tool in modern software development. Knowing commands is only the beginning; understanding version history, branching strategies, conflict resolution, and collaborative workflows is what makes you effective.

This repository is designed to help you:

- **Learn Git systematically** through concise explanations and practical examples.
- **Build real-world skills** by practicing common development workflows.
- **Prepare for technical interviews** with fundamental concepts and scenario-based questions.
- **Solve Git problems independently** using inspection, debugging, and recovery techniques.
- **Collaborate confidently** through forks, branches, pull requests, and code reviews.

Whether you're learning Git for the first time or revising before an interview, use these notes as a practical reference.

## What You'll Learn

| Area | Topics |
|---|---|
| Fundamentals | Repositories, commits, snapshots, working tree, staging area |
| Essential Commands | `add`, `commit`, `status`, `diff`, `log` |
| Branch Management | Branch creation, switching, merging, deletion |
| Remote Operations | `fetch`, `pull`, `push`, upstream tracking |
| History Management | `show`, `blame`, `reflog`, commit inspection |
| Undo and Recovery | `restore`, `reset`, `revert`, `stash` |
| Advanced Workflows | Rebasing, cherry-picking, merge strategies |
| Team Collaboration | Forks, pull requests, reviews, conflict resolution |
| Interview Preparation | Conceptual questions, practical exercises, troubleshooting scenarios |

Browse the [notes directory](https://github.com/sonutelcoandai/git_learn/tree/main/notes) to explore the available learning materials.

## How to Use This Repository

Follow this workflow for each topic:

1. **Read** the concept and understand the problem it solves.
2. **Practice** the commands in a local test repository.
3. **Inspect** the result using `git status`, `git diff`, and `git log`.
4. **Experiment** with different scenarios and observe the behavior.
5. **Explain** the concept in your own words, as you would in an interview.
6. **Revise** the commands and concepts you find difficult.

**Recommended learning strategy:** Spend less time memorizing commands and more time understanding their effects.

---

## Quick Start: Your First Git Workflow

Use the following commands to inspect, stage, commit, and review changes.

```bash
# Check repository status
git status

# Stage a specific file
git add README.md

# Review staged changes
git diff --staged

# Commit the changes
git commit -m "docs: update learning notes"

# Review recent commits
git log --oneline -5
```

Push your commits to the configured remote branch when you're ready:

```bash
git push
```

### Understand the Git Workflow

| Component | Purpose |
|---|---|
| Working tree | Contains the files you're editing |
| Staging area | Holds changes selected for the next commit |
| Local repository | Stores commits and project history |
| Remote repository | Shares commits with other collaborators |

**Key distinction:** `git add` stages changes, `git commit` records them locally, and `git push` publishes commits to a remote repository.

---

## Practical Scenarios Every Developer Should Know

### 1. Create a Feature Branch

```bash
git switch -c feature/user-authentication
```

**Use case:** Develop a feature independently without making changes directly on the main branch.

### 2. Inspect Remote Updates

```bash
git fetch origin
git status
```

**Use case:** Retrieve information about remote changes before deciding how to integrate them.

To integrate changes from your configured upstream branch:

```bash
git pull
```

### 3. Undo a Commit Safely

```bash
git revert <commit-hash>
```

**Use case:** Reverse a commit while preserving the existing history. This is often preferable for changes already shared with a team.

### 4. Recover an Earlier Commit

```bash
git reflog
```

**Use case:** Inspect recent movements of local references to locate commits that may no longer be visible in the current branch history.

### 5. Resolve a Merge Conflict

```bash
git status
```

Open the conflicted files, resolve the conflicting sections, and stage the resolved files.

```bash
git add <resolved-file>
```

If you're completing a merge, finish it with:

```bash
git commit
```

If you're resolving a rebase conflict instead, continue with:

```bash
git rebase --continue
```

**Use case:** Understand how Git handles incompatible changes and how to complete the operation in progress.

---

## Git Interview Preparation

Preparing for a software engineering, DevOps, or developer interview? Start with these foundational questions and practice answering them without memorizing definitions.

These are representative technical interview questions, not a verified list attributed to specific companies.

### Essential Interview Questions

**1. What is Git, and how is it different from GitHub?**

Git is a distributed version control system that tracks changes to files. GitHub is a platform for hosting Git repositories and collaborating on projects.

**2. What is the difference between `git fetch` and `git pull`?**

`git fetch` retrieves remote updates without integrating them into the current branch. `git pull` fetches updates and then integrates them according to the configured strategy.

**3. What is the difference between merging and rebasing?**

- `git merge` integrates histories and may create a merge commit.
- `git rebase` replays commits onto another base and creates new commit identities for the replayed commits.

**4. What is the difference between `git reset` and `git revert`?**

`git reset` moves a branch reference and may also change the staging area or working tree. `git revert` creates a new commit that reverses an earlier commit.

**5. What is the Git staging area?**

The staging area, also called the index, records the changes selected for the next commit.

**6. What is a detached HEAD state?**

HEAD points directly to a commit instead of a local branch. Commits created in this state may be difficult to find later unless you preserve them with a branch or another reference.

**7. What is `git cherry-pick` used for?**

It applies the changes introduced by selected commits to the current branch, creating new commit identities.

**8. How do you temporarily save uncommitted changes?**

Use `git stash` to save eligible working-tree changes temporarily and restore them later.

**9. How can you recover a commit after an accidental reset?**

Use `git reflog` to inspect recent reference movements and identify a potentially recoverable commit.

**10. How do you resolve a merge conflict?**

Inspect the conflict with `git status`, edit the affected files, stage the resolutions, and complete the merge or rebase operation.

### Scenario-Based Interview Practice

| Interview scenario | Concepts to demonstrate |
|---|---|
| You committed to the wrong branch | Branch management and cherry-picking |
| You need to undo a pushed commit | Revert and shared-history safety |
| Your feature branch is behind `main` | Fetch, merge, rebase, and conflict handling |
| You staged the wrong file | `git restore --staged` |
| You need one commit from another branch | `git cherry-pick` |
| A commit disappeared after a reset | `git reflog` |
| You need to inspect an unfamiliar change | `git show`, `git diff`, and `git log` |
| You need to combine several local commits | Interactive rebase and history management |

**Interview tip:** Explain the problem, choose the appropriate command, describe its effect on history, and identify any risks. Demonstrating sound judgment is more valuable than simply recalling syntax.

---

## Hands-On Practice Checklist

Use a disposable repository and complete these exercises yourself.

- [ ] Initialize a repository and create your first commit.
- [ ] Stage one file without staging other changes.
- [ ] Create a feature branch and merge it into another branch.
- [ ] Reproduce and resolve a merge conflict.
- [ ] Undo a commit with `git revert`.
- [ ] Compare `git reset --soft`, `--mixed`, and `--hard`.
- [ ] Save and restore changes with `git stash`.
- [ ] Investigate commit history using `git reflog`.
- [ ] Rebase a feature branch in a test repository.
- [ ] Explain the difference between fetching, pulling, and pushing.

Verify your results using:

```bash
git status
git diff
git log --oneline --graph --all
```

**Challenge yourself:** Complete each exercise without looking at the solution, then explain why your chosen approach works.

---

## Contributing to This Project

Contributions from learners and experienced developers are welcome. Help make these notes more accurate, practical, and accessible to the community.

### What Can You Contribute?

- Clearer explanations of Git concepts.
- Reproducible command-line examples.
- Interview questions with accurate, concise answers.
- Real-world troubleshooting guides.
- Improvements to documentation and navigation.
- Corrections to errors or outdated practices.

### Contribution Workflow

**Step 1 — Star the repository**

If this project helps you learn Git or prepare for an interview, consider starring it.

[Star this repository](https://github.com/sonutelcoandai/git_learn/stargazers)

**Step 2 — Fork the repository**

Create your own copy so you can develop improvements independently.

[Fork git_learn](https://github.com/sonutelcoandai/git_learn/fork)

**Step 3 — Clone your fork**

Replace `<your-username>` with your GitHub username.

```bash
git clone https://github.com/<your-username>/git_learn.git
cd git_learn
```

**Step 4 — Create a branch**

```bash
git switch -c docs/improve-git-notes
```

**Step 5 — Make and review your changes**

```bash
git status
git diff
```

Test your examples and verify that your explanations accurately describe Git behavior.

**Step 6 — Commit and push**

```bash
git add notes/
git commit -m "docs: improve Git learning notes"
git push -u origin docs/improve-git-notes
```

**Step 7 — Open a pull request**

Visit your fork on GitHub and submit a pull request against the original repository's `main` branch. Describe your changes and explain how they improve the learning experience.

See the [official GitHub guide to pull requests from forks](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork).

### Contribution Standards

- Keep documentation clear, concise, and technically accurate.
- Prefer practical examples over unexplained command lists.
- Explain expected results and important risks.
- Avoid duplicating existing material unnecessarily.
- Never include credentials, access tokens, or other secrets.

---

## Start Learning. Start Practicing. Start Contributing.

If you want to learn Git or prepare for a technical interview, put these notes into practice.

**Pick a concept. Run the commands. Understand the result. Solve a real problem.**

Found this useful? Give the repository a star.

Know something that could improve it? Fork the project and contribute.

Every useful example, correction, and explanation helps someone become a better developer.

**Learn today. Build confidently. Help others grow.**

---

**Repository:** [sonutelcoandai/git_learn](https://github.com/sonutelcoandai/git_learn)  
**Learning materials:** [Explore the notes](https://github.com/sonutelcoandai/git_learn/tree/main/notes)  
**Contribute:** [Fork the repository](https://github.com/sonutelcoandai/git_learn/fork)
