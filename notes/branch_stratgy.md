# Git Branching Strategy — Complete Guide for Interviews

A Git branching strategy is a set of rules that defines how developers create branches, develop features, fix bugs, release software, and merge code into shared branches.

As a Git expert, you should understand not only the Git commands but also when to use each branching strategy, why teams choose it, and how to explain it in interviews with real project examples.

## 1. What is a Git branch?

A branch is a lightweight pointer to a commit in Git. It lets you develop a feature or fix a bug without directly changing another branch's work.

Branching & Merging | GitHub | QuickLearnPro

For example, suppose you are developing an e-commerce application.

- `main` contains production-ready code.
- `feature/payment` contains payment gateway development.
- `bugfix/login-error` contains a fix for a login issue.
- `release/2.0` contains preparation for version 2.0.

Each branch lets developers work independently and combine their changes when ready.

### Essential Git commands

bash
```
# Check the current branch
git branch

# Create and switch to a new branch
git switch -c feature/payment

# See all local and remote branches
git branch -a

# Switch back to main
git switch main

# Merge a feature branch into main
git merge feature/payment

# Delete a merged local branch
git branch -d feature/payment
```

Interview tip: A branch is not a separate copy of the entire repository. It is a movable reference to a commit, while Git's commit graph records the history.

## 2. The major Git branching strategies

There are five approaches you should know for interviews.

Mastering Git Commands for Seamless Releases Across DevOps Environments | by Hayanan | Javarevisited | Jan, 2026 | Medium

### 1. Git Flow

Structured releases · Multiple long-lived branches

Uses `main`, `develop`, `feature/*`, `release/*`, and `hotfix/*`. Useful when releases need formal preparation and maintenance.

GitHub - regnaer/Release-Based-Workflow

### 2. GitHub Flow

Simple workflow · Continuous deployment

Developers create short-lived feature branches, open pull requests, run checks, and merge into `main`.

Git Branching Strategies - by Dr Milan Milanović

### 3. GitLab Flow

Environment or release-oriented workflow

Combines feature branches and merge requests with approaches for managing environments or release branches.

Understanding the Trunk-Based Development Workflow | Tower Blog

### 4. Trunk-Based Development

Frequent integration · Short-lived branches

Developers integrate small changes into a shared trunk, usually `main`, frequently. Strong automated testing is important.

What is Source Code Management? - ARCAD

### 5. Release Branching

Versioned releases · Parallel maintenance

A team creates branches such as `release/2.1` to stabilize one version while development continues for the next release.

### Quick comparison

| Strategy | Long-lived branches | Best suited for |
| --- | --- | --- |
| Git Flow | `main`, `develop`, often release branches | Scheduled releases, formal QA |
| GitHub Flow | Usually `main` | Web apps, continuous deployment |
| GitLab Flow | Depends on chosen variant | Environment and release management |
| Trunk-Based | One primary trunk | Frequent delivery, CI/CD |
| Release Branching | `main` plus supported release branches | Multiple maintained versions |

There is no universally best strategy. The right choice depends on deployment frequency, team size, release requirements, testing maturity, and the need to support older versions.

## 3. Git Flow — explained with a real project

Git Flow is a structured branching model associated with Vincent Driessen. It is often discussed in interviews because it has clearly defined branch types and release processes.

AWS Prescriptive Guidance

+1

Imagine your company develops an e-commerce application and releases a new version every two weeks.

### The five main branch types

`main`

Contains production releases. Released commits are commonly tagged, for example `v2.0.0`.

`develop`

Integration branch where completed features are combined for the next release.

`feature/*`

Used for new functionality, such as `feature/payment-gateway`. Created from `develop` and normally merged back into it.

`release/*`

Used to stabilize a release, such as `release/2.0`. Usually branched from `develop`.

`hotfix/*`

Used to urgently fix a production problem, such as `hotfix/payment-failure`. Created from `main`.

### Git Flow lifecycle

`main`

Production version 1.0

`feature/payment-gateway`

Developer implements payment integration

`develop`

Feature merged and integration tests run

`release/2.0`

QA, regression testing, release preparation

`main` — tag `v2.0.0`

Version 2.0 released to production

The release branch is also merged back into `develop` so release fixes are not lost.

### Practical commands

Assume `main` and `develop` already exist.

Step 1: Create a feature branch

bash
```
git switch develop
git pull origin develop
git switch -c feature/payment-gateway
```

Step 2: Develop and commit the feature

bash
```
git add .
git commit -m "feat: integrate payment gateway"
git push -u origin feature/payment-gateway
```

Open a pull request or merge request targeting `develop`. After code review and testing, merge it.

Step 3: Prepare the release

bash
```
git switch develop
git pull origin develop
git switch -c release/2.0
```

The QA team tests this release. If a bug is found, fix it on `release/2.0` and commit the fix.

Step 4: Release to production

After approval, merge the release branch into `main`, create a version tag, and merge the release changes back into `develop`.

bash
```
git switch main
git merge --no-ff release/2.0
git tag -a v2.0.0 -m "Release version 2.0.0"
git push origin main --tags

git switch develop
git merge --no-ff release/2.0
git push origin develop
```

In a team environment, use reviewed pull requests and the team's release pipeline. The commands illustrate the Git operations, not a replacement for required approvals.

Step 5: Fix a critical production bug

Suppose customers cannot complete payments in version 2.0. Create a hotfix from `main`:

bash
```
git switch main
git pull origin main
git switch -c hotfix/payment-failure

# Fix the issue in the application
git add .
git commit -m "fix: handle payment gateway timeout"
```

After testing and review, merge the hotfix into `main`, publish a patch release such as `v2.0.1`, and merge the fix into `develop` as well. If a release branch is active, ensure the fix is included there too.

### Advantages and disadvantages of Git Flow

Advantages

- Clear separation between ongoing development and production releases.
- Supports scheduled releases and formal QA.
- Offers a defined process for urgent production fixes.
- Helps teams manage release stabilization.

Disadvantages

- More branches and merging overhead.
- Changes may take longer to reach production.
- Long-lived branches can drift apart and cause conflicts.
- Often unnecessarily complex for teams deploying several times a day.

## 4. GitHub Flow — practical example

GitHub Flow is simpler: create a short-lived branch from `main`, make changes, open a pull request, pass checks, and merge. It is designed for teams that deploy regularly.

GitHub Docs

+1

Imagine you need to add a coupon feature to an online shopping site.

`main`

Stable

`feature/coupon-code`

- Implement coupon validation.
- Add tests.
- Push changes and open a pull request.
- Resolve review comments.

CI checks and code review

Build, unit tests, security checks and any required approvals.

Merge into `main`

Deploy through the release pipeline

Commands:

bash
```
git switch main
git pull origin main
git switch -c feature/coupon-code

# Implement and test the feature
git add .
git commit -m "feat: add coupon code support"
git push -u origin feature/coupon-code
```

After review, merge the pull request into `main`. Delete the feature branch once it is no longer needed.

Important: Merging into `main` does not automatically mean production deployment. That depends on how the team's CI/CD pipeline is configured.

### When would you choose GitHub Flow?

Choose it when the team has a reliable automated test suite, frequent deployments, short-lived feature branches, and a process to disable or roll back a problematic feature.

## 5. Trunk-Based Development

In trunk-based development, developers integrate their changes into a shared branch (`main` or `trunk`) frequently. They may commit directly or use very short-lived branches and pull requests.

This approach emphasizes continuous integration and keeping the codebase releasable.

AWS Prescriptive Guidance

+1

### Real example: An online banking application

Three developers work on separate changes:

- Developer A: improve the account dashboard.
- Developer B: add transaction search.
- Developer C: improve API response time.

Instead of keeping branches open for several weeks, each developer integrates small, tested changes frequently. If transaction search is not ready for customers, a feature flag can keep it disabled while its code is already integrated.

text
```
main
  ├── Small change A → integrate
  ├── Small change B → integrate
  └── Small change C → integrate
```

Example:

bash
```
git switch main
git pull origin main
git switch -c feature/transaction-search

# Make a small, tested change
git add .
git commit -m "feat: add transaction search API"
git push -u origin feature/transaction-search
```

Open a pull request, run the checks, and merge it quickly rather than leaving the branch open for a long period.

### Advantages

- Integration problems are discovered earlier.
- Less branch divergence.
- Supports continuous integration and delivery.
- Makes small changes easier to review and troubleshoot.

### Challenges

- Requires reliable automated tests.
- Requires discipline to keep changes small.
- Feature flags and backward-compatible changes may be needed.
- Incomplete work must not break the shared branch.

## 6. How to choose a strategy in a real company

| Situation | Reasonable starting choice |
| --- | --- |
| Small team, one web application, daily releases | GitHub Flow |
| Mature CI/CD team deploying many times a day | Trunk-Based Development |
| Enterprise application with scheduled releases and QA | Git Flow |
| Product maintaining versions 2.0, 2.1 and 3.0 simultaneously | Release branches |
| Multiple deployment environments with promotion controls | GitLab Flow or a tailored environment-based workflow |

These are starting recommendations, not rigid rules. A team can combine short-lived feature branches with release branches when it needs both fast integration and support for older versions.

GitLab Docs

+2

## 7. Most asked Git branching interview questions and answers

These are representative questions commonly used in Git, DevOps, and software engineering interviews, from basic concepts to real-world scenarios.

### Q1. What is a branching strategy, and why do we need one?

Answer: A branching strategy defines how developers create, name, merge, and manage branches during software development and releases.

It helps teams:

- Work on multiple features in parallel.
- Protect production code from unreviewed changes.
- Manage releases and urgent fixes.
- Integrate changes consistently and reduce conflicts.

Real example: In an e-commerce company, one developer works on payments while another fixes login. Both use separate branches and submit pull requests rather than independently modifying production code.

### Q2. What is the difference between Git Flow and GitHub Flow?

Answer:

| Git Flow | GitHub Flow |
| --- | --- |
| Uses `main` and `develop` | Usually uses `main` as the primary branch |
| Has feature, release and hotfix branches | Uses short-lived feature or bugfix branches |
| Suited to structured release cycles | Suited to frequent delivery |
| More process overhead | Simpler workflow |

Real example: A banking product with monthly release approvals might use Git Flow. A SaaS website deploying small improvements every day might use GitHub Flow.

### Q3. What is the difference between `main` and `develop`?

Answer:

- `main` represents production releases in a traditional Git Flow setup.
- `develop` integrates features intended for a future release.

The `develop` branch is a convention used by Git Flow, not a mandatory Git branch.

Real example: Payment and coupon features are merged into `develop` during development. After regression testing, the team releases them through `release/2.0` and merges that release into `main`.

### Q4. What is the difference between a feature branch and a release branch?

Answer:

- A feature branch is for developing a particular change.
- A release branch is for stabilizing a planned version, including bug fixes and release preparation.

Real example:

text
```
feature/wallet-payment
release/2.4
```

The first adds wallet payments. The second stabilizes version 2.4 while developers may continue building version 2.5.

### Q5. What is a hotfix branch? From which branch should it be created?

Answer: A hotfix branch addresses an urgent production issue. In Git Flow, it is created from `main` because that branch represents the production release.

bash
```
git switch main
git pull origin main
git switch -c hotfix/login-crash
```

After the fix, testing, and review, merge it into `main`, publish the patch release, and integrate the fix into `develop` and any affected supported release branches.

Real example: Users cannot log in after a production deployment. You create `hotfix/login-crash`, fix the issue, test it, and release version `v2.4.1`.

### Q6. What happens if a developer merges a feature directly into `main` instead of `develop` in Git Flow?

Answer: The feature bypasses the intended integration and release process. It might reach production without the normal release testing or approvals.

The appropriate recovery depends on whether the change was actually released. If it was not released, the team may revert the merge or use its release process to restore the intended branch state. If it was released, coordinate a tested corrective release rather than blindly rewriting shared history.

Interview tip: Explain that branch protection and pull request rules can prevent unauthorized merges.

### Q7. What is a merge conflict, and how do you resolve it?

Answer: A merge conflict occurs when Git cannot automatically reconcile changes, often because two branches modify overlapping lines of the same file.

Example: Two developers change the same payment validation method.

bash
```
git switch feature/payment
git fetch origin
git merge origin/main
```

If a conflict occurs:

1. Run `git status` to identify conflicted files.
2. Open each file and review the conflict markers.
3. Choose or combine the correct changes.
4. Remove the markers and test the result.
5. Stage the resolved files and complete the merge.

bash
```
git add src/payment.js
git commit
```

For a merge conflict, Git may prepare the merge commit automatically after the files are staged; if so, complete it with the appropriate commit command.

Important: Do not resolve a conflict by blindly accepting all changes from one side. Understand the intended application behavior first.

### Q8. What is the difference between `git merge` and `git rebase`?

Answer:

- `git merge` integrates another branch's changes and may create a merge commit.
- `git rebase` reapplies commits on top of a new base, creating new commit IDs.

Example:

bash
```
# Merge main into the feature branch
git switch feature/payment
git merge main
```

Alternatively:

bash
```
# Rebase feature commits onto main
git switch feature/payment
git rebase main
```

Merging generally preserves the branch integration history. Rebasing can produce a more linear history.

Interview tip: Avoid rebasing commits that other people are already depending on unless the team has explicitly coordinated the rewrite.

### Q9. How do you keep a long-running feature branch updated with `main`?

Answer: Regularly integrate the latest changes from `main`, resolve conflicts early, and run tests.

Merge-based approach:

bash
```
git fetch origin
git switch feature/payment
git merge origin/main
```

Rebase-based approach for a private or coordinated feature branch:

bash
```
git fetch origin
git switch feature/payment
git rebase origin/main
```

Real example: A payment feature takes two weeks to implement, while other developers change the order service. Updating the feature branch regularly helps discover integration problems before the final pull request.

### Q10. What is trunk-based development, and how does it differ from feature branching?

Answer: Trunk-based development emphasizes integrating small changes into the shared trunk frequently. Feature branches, when used, are very short-lived.

Traditional feature branching can allow a branch to remain separate for days or weeks. Trunk-based development aims to minimize that separation.

Real example: A developer merges a small, tested API change today and a UI improvement tomorrow instead of waiting until a large feature is completely finished.

### Q11. What is a feature flag, and why is it useful for branching?

Answer: A feature flag is a runtime control that enables or disables functionality without requiring a separate long-lived Git branch.

Real example: A company integrates a new payment method into `main`, but enables it only for internal testers. Once the feature is validated, the team turns it on for customers.

Feature flags reduce the need to maintain large, long-running branches, but they must be managed carefully to avoid accumulating obsolete flags.

### Q12. How do you release a hotfix when a release branch already exists?

Answer: First identify the affected production version and release branch. In Git Flow, create the hotfix from the production `main` branch, test and release it, and then ensure the fix reaches `develop` and the active release branch if that release also needs it.

Real example:

text
```
main:           v2.4.0 → hotfix → v2.4.1
develop:        future version 2.5
release/2.5:    testing version 2.5
```

The fix must not remain only on `main` if later versions still contain the bug.

### Q13. What is the difference between `git revert` and `git reset` when a bad change is merged?

Answer:

- `git revert` creates a new commit that reverses an earlier commit. It is generally safer for shared branches.
- `git reset` moves a branch reference and may change the index or working tree depending on the option. It can rewrite local history.

Example:

bash
```
# Reverse a bad commit safely on a shared branch
git revert <commit-hash>
```

For a merge commit, you may need to specify the mainline parent using `-m`.

Real example: A broken feature has already been merged into a shared production branch. Reverting it preserves the existing history and provides a clear record of the corrective action.

### Q14. How do you handle multiple versions of the same application?

Answer: Use release branches for versions that need separate maintenance, together with tags to identify exact releases.

text
```
main
 ├── release/2.0
 ├── release/2.1
 └── release/3.0
```

For example, customers still using version 2.1 may require a security patch while other customers use version 3.0. The team applies and tests the fix on every affected supported version.

A branch is movable; a tag normally marks a specific release commit.

### Q15. Which branching strategy would you recommend for a new project?

Answer: I would begin with GitHub Flow or a trunk-based approach if the team has reliable automated tests and can deploy frequently. For a project requiring formal release stabilization, scheduled releases, and parallel version support, I would consider Git Flow or release branches.

I would also establish:

- Protected production branches.
- Pull request reviews and automated checks.
- Consistent branch naming.
- Short-lived feature branches.
- A documented hotfix and rollback process.

Why this is a strong interview answer: It demonstrates that you choose a strategy based on business and engineering needs instead of claiming one strategy is always the best.

## 8. Real-world scenario: You are the Git expert on a project

Imagine a team of five developers building an e-commerce platform.

The project needs a new payment gateway, a coupon feature, a production bug fix, and a release next Friday.

Here is how I would manage it using Git Flow.

Monday: Parallel development

Developer A creates `feature/payment-gateway`; Developer B creates `feature/coupon-code`. Both branch from `develop`.

Tuesday–Wednesday: Integration

Developers open pull requests. Reviewers check the code, CI runs tests, and approved changes are merged into `develop`.

Thursday: Release preparation

The team creates `release/2.5`, performs regression tests, and fixes release-specific bugs.

Production incident

If the current production payment system breaks, the team creates `hotfix/payment-failure` from `main`, validates the fix, and publishes a patch release.

Friday: Release

After approval, the team merges the release branch into `main`, tags `v2.5.0`, deploys, and integrates release fixes back into `develop`.

### What would you say if the interviewer asks why you chose Git Flow?

You could answer:

> "I chose Git Flow because our project has scheduled releases, a QA stabilization phase, and a need to handle urgent production fixes separately from ongoing feature development. Feature branches allow developers to work independently, `develop` integrates upcoming features, release branches provide a controlled stabilization window, and hotfix branches allow us to patch production without waiting for the next planned release. If the team moved to continuous deployment, I would reassess whether a simpler GitHub Flow or trunk-based workflow would be more efficient."

That answer demonstrates both practical experience and an understanding of trade-offs.

## 9. Branch naming conventions and best practices

Consistent names make it easier to identify why a branch exists.

| Purpose | Example |
| --- | --- |
| Feature | `feature/PROJ-101-payment` |
| Bug fix | `bugfix/PROJ-205-login-error` |
| Production hotfix | `hotfix/PROJ-310-payment-timeout` |
| Release | `release/2.5.0` |
| Experiment | `experiment/new-search` |

Best practices:

- Keep branches focused on one logical task.
- Keep feature branches short-lived.
- Pull or fetch recent changes before integrating work.
- Require reviews and automated checks for important branches.
- Avoid committing secrets or credentials.
- Tag releases using meaningful version numbers.
- Delete obsolete branches after merging.
- Never force-push shared branches without explicit coordination.
- Document how to handle releases, hotfixes, and rollbacks.

## 10. Practice lab: Test your understanding

1. In Git Flow, where does a feature branch normally start?

main

develop

release/2.0

hotfix/2.0

2. A production payment service is broken. Which branch type is appropriate in Git Flow?

feature/payment

release/payment

hotfix/payment-failure

experiment/payment

3. Which strategy emphasizes integrating small changes into a shared trunk frequently?

Git Flow

Trunk-Based Development

Long-lived feature branching

Release branching only

4. What is generally safer for undoing a bad commit on a shared branch?

git reset --hard

git revert

Delete the repository

Force-push a rewritten history

 Reset quiz

## 11. Official resources for further learning

- [Git branching basics — Pro Git book](<https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell?utm_source=chatgpt.com>)
- [Git workflows — GitHub documentation](<https://docs.github.com/en/get-started/git-basics/git-workflows?utm_source=chatgpt.com>)
- [Git branching strategies — AWS Prescriptive Guidance](<https://docs.aws.amazon.com/prescriptive-guidance/latest/choosing-git-branch-approach/git-branching-strategies.html?utm_source=chatgpt.com>)
- [Branching strategies — GitLab documentation](<https://docs.gitlab.com/user/project/repository/branches/strategies/?utm_source=chatgpt.com>)

My recommendation for interview preparation: First master Git Flow, GitHub Flow, trunk-based development, merge vs. rebase, conflict resolution, hotfixes, and release management. Practice explaining each with a real scenario and the commands you would execute. Those skills are more valuable than simply memorizing branch definitions.

[quicklearnpro.com](<https://quicklearnpro.com/domains/developer-tools/github/branching-and-merging?utm_source=chatgpt.com>)