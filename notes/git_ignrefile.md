# Git Expert Guide: 1. Git Ignore (`.gitignore`)

Let's learn `.gitignore` from beginner to interview level, with practical commands, real-world project examples, common mistakes, and frequently asked interview questions with answers.

## 1. What is Git Ignore?

`.gitignore` is a special file in Git that tells Git which untracked files or directories it should ignore.

In real projects, you often have files that shouldn't be committed to a repository, such as:

- `node_modules/` — installed Node.js dependencies.
- `.env` — environment variables and API keys.
- `*.log` — log files.
- `dist/` or `build/` — generated output.
- `.idea/` — IDE-specific configuration.

Instead of manually avoiding these files every time you commit, you can define rules in `.gitignore`.

### Real-world example

Imagine you're building a Node.js application:

my-app/

`app.js`

Commit

`package.json`

Commit

`.env`

Ignore

`node_modules/`

Ignore

`debug.log`

Ignore

You want teammates to receive your source code, but you don't want to upload hundreds of megabytes of dependencies, machine-specific files, or private credentials.

Create a `.gitignore` file in your project root:

gitignore
```
# Dependencies
node_modules/

# Environment variables and secrets
.env

# Log files
*.log

# Generated build output
dist/

# IDE settings
.idea/
.vscode/
```

Now Git ignores matching untracked files when determining what to add to version control.

## 2. How to create and use `.gitignore`

### Step 1: Create the file

bash
```
touch .gitignore
```

On Windows PowerShell, you can use:

powershell
```
New-Item .gitignore -ItemType File
```

### Step 2: Add ignore rules

For example:

gitignore
```
.env
node_modules/
*.log
```

### Step 3: Check Git's status

bash
```
git status
```

If the matching files are untracked, they should no longer appear as files to be added.

### Step 4: Commit the `.gitignore` file

bash
```
git add .gitignore
git commit -m "Add gitignore rules"
```

Important: Commit `.gitignore` itself. It contains project rules that other developers should use too.

## 3. Git ignore patterns you must know

These patterns are very commonly tested in interviews.

GitHub

+1

| Pattern | Meaning | Example |
| --- | --- | --- |
| `*.log` | Ignore matching files at any directory level | `debug.log`, `logs/app.log` |
| `node_modules/` | Ignore directories named `node_modules` at any level | `api/node_modules/` |
| `/config.json` | Ignore only the root-level file | `config.json` |
| `logs/debug.log` | Match this path relative to the `.gitignore` location | `logs/debug.log` |
| `**/temp.txt` | Match `temp.txt` anywhere | `src/temp.txt` |
| `build/` | Ignore the directory and its contents | `build/app.js` |
| `*.tmp` | Ignore files ending in `.tmp` | `cache.tmp` |
| `!important.log` | Re-include a previously ignored matching file | `important.log` |

### Understanding wildcards

- `*` matches zero or more characters, but not `/`.
- `?` matches a single character, but not `/`.
- `**` can match across directory levels in supported patterns.
- `!` reverses an earlier matching ignore rule, subject to directory restrictions.
- `/` anchors a pattern to the `.gitignore` file's directory when used at the beginning.

### Example 1: Ignore all log files

gitignore
```
*.log
```

This ignores files such as:

text
```
app.log
server.log
logs/error.log
src/debug.log
```

### Example 2: Ignore only the root-level `config.json`

gitignore
```
/config.json
```

This ignores:

text
```
config.json
```

But it does not ignore:

text
```
src/config.json
```

### Example 3: Ignore all temporary files except one

gitignore
```
*.tmp
!important.tmp
```

Result:

- `cache.tmp` — ignored.
- `important.tmp` — not ignored, provided it is not excluded by another applicable rule.

Interview tip: Pattern order matters. Generally, the last matching pattern at the same precedence level determines whether a path is ignored or re-included.

One important limitation: Git cannot re-include a file inside a parent directory that remains excluded. You must also make the necessary parent directory traversable by Git.

GitHub

## 4. Most important interview concept: `.gitignore` does not untrack files

This is probably the most important concept to understand.

Suppose you committed `.env` yesterday. Today, you add this rule:

gitignore
```
.env
```

Will Git stop tracking `.env`?

No. Git continues tracking it because it is already in the repository's index. Changes to the file can still appear in `git status`.

GitHub

+1

### Real interview scenario

You accidentally committed `debug.log` and now want Git to ignore it without deleting the local file.

Use these commands:

bash
```
# Add the ignore rule
echo "debug.log" >> .gitignore

# Stop tracking the file but keep it locally
git rm --cached debug.log

# Stage the ignore rule
git add .gitignore

# Commit the changes
git commit -m "Stop tracking debug log"
```

What happens?

Local file remains

`debug.log` stays on your computer.

Git stops tracking it

The next commit removes it from the repository's tracked files.

Future changes are ignored

Git will ignore it as long as the applicable rule remains in effect.

### Difference between `git rm` and `git rm --cached`

| Command | Local file | Git tracking |
| --- | --- | --- |
| `git rm file.txt` | Removes the file from the working tree | Stages removal |
| `git rm --cached file.txt` | Keeps the local file | Stages removal from tracking |

Note that these commands stage a change. The removal becomes part of the repository's history only when committed.

Security warning: If you committed an API key or password, `.gitignore` and `git rm --cached` do not erase it from previous commits. Revoke or rotate the exposed credential immediately, and consider history cleanup if necessary.

## 5. Where can you configure Git ignore rules?

Git supports three main places for ignore rules.

GitHub

+1

1. `.gitignore`

Project-level rules shared with the team when committed.

text
```
my-app/.gitignore
```

2. `.git/info/exclude`

Local rules for one repository; not committed as project files.

text
```
my-app/.git/info/exclude
```

3. Global ignore file

Personal rules that apply across your repositories.

bash
```
git config --global core.excludesFile ~/.config/git/ignore
```

Create the configured file and add rules such as:

gitignore
```
.DS_Store
*.swp
```

Interview question: Which file should you use for rules that every developer on the team needs?

Answer: `.gitignore`, because it can be committed and shared with everyone.

## 6. How to debug `.gitignore` problems

Sometimes you add a rule, but Git still shows a file or continues ignoring something unexpectedly.

### Command 1: Check why a file is ignored

bash
```
git check-ignore -v app.log
```

This reports the matching ignore file, line number, and pattern when the file is ignored by a rule.

### Command 2: Display ignored files

bash
```
git status --ignored
```

Useful when you want to see ignored files that ordinary `git status` hides.

### Command 3: List tracked files

bash
```
git ls-files
```

If a file appears here, it is tracked in the index. Adding it to `.gitignore` will not automatically untrack it.

### Command 4: Force-add an ignored file

bash
```
git add -f important.log
```

Use this when you deliberately want to add a file that matches an ignore rule. Be careful not to force-add secrets or generated files accidentally.

## 7. Top 12 Git Ignore interview questions and answers

These are useful for Git, DevOps, software developer, and CI/CD interviews.

### Q1. What is `.gitignore`?

It is a file containing patterns that tell Git which untracked files or directories to ignore.

Example:

gitignore
```
node_modules/
*.log
.env
```

### Q2. Does `.gitignore` work on already tracked files?

No. It affects files that are not already tracked. To stop tracking a file but keep it locally, use:

bash
```
git rm --cached filename
```

Add the appropriate ignore rule and commit the changes.

### Q3. What is the difference between `.gitignore` and `.git/info/exclude`?

- `.gitignore`: project rules that can be committed and shared.
- `.git/info/exclude`: local repository rules that are not shared through normal commits.

### Q4. How do you ignore all files with a particular extension?

Use a wildcard.

gitignore
```
*.log
```

This ignores matching `.log` files throughout the repository, including nested directories.

### Q5. What does `!` mean in `.gitignore`?

It negates a previous ignore rule and can re-include a matching path.

gitignore
```
*.log
!important.log
```

`important.log` can be included while other matching logs remain ignored, provided no other applicable rule excludes it.

### Q6. What is the difference between `*` and `**`?

- `*` matches characters within a single path component.
- `**` can match across multiple directory levels in supported patterns.

gitignore
```
*.log
**/logs/**
```

The second pattern ignores contents beneath directories named `logs` at any depth.

### Q7. How do you remove a file from Git tracking without deleting it locally?

bash
```
git rm --cached config.local
```

Add `config.local` to `.gitignore` if it should stay ignored, then commit the change.

### Q8. How do you find which rule is ignoring a file?

bash
```
git check-ignore -v path/to/file
```

This helps identify the ignore file and matching line.

### Q9. Can you force Git to add an ignored file?

Yes.

bash
```
git add -f file.log
```

This overrides the ignore rule for that add operation. Use it intentionally.

### Q10. What happens if multiple `.gitignore` files exist?

Git can use `.gitignore` files at different directory levels. Rules are evaluated relative to their location, and a more specific lower-level file can override a parent rule. Within the same precedence level, the last matching pattern wins.

### Q11. How do you ignore a file only in the root directory?

Add a leading slash.

gitignore
```
/config.json
```

This matches the root-level `config.json`, not `src/config.json`.

### Q12. Is `.gitignore` a security mechanism for passwords?

No. It helps prevent accidental commits of untracked files, but it does not protect tracked secrets or remove credentials from Git history.

Store secrets securely, keep `.env` out of version control, commit a safe `.env.example` instead, and rotate any exposed credentials.

## 8. Practical interview exercise

Try solving this real-world scenario before revealing the answer.

Your project has these files:

text
```
my-app/
├── .gitignore
├── app.js
├── .env
├── error.log
├── important.log
├── node_modules/
└── src/
    └── debug.log
```

Your requirements are:

- Ignore `.env`.
- Ignore `node_modules/`.
- Ignore all `.log` files except `important.log`.
- Keep `app.js` and source code tracked normally.

What should your `.gitignore` contain?

 Reveal solution

## 9. Quick revision sheet

| Task | Command or pattern |
| --- | --- |
| Ignore all log files | `*.log` |
| Ignore a directory | `node_modules/` |
| Ignore a root-level file | `/config.json` |
| Re-include a file | `!important.log` |
| Stop tracking but keep locally | `git rm --cached file` |
| Find the matching ignore rule | `git check-ignore -v file` |
| View ignored files | `git status --ignored` |
| Force-add an ignored file | `git add -f file` |
| Configure a global ignore file | `git config --global core.excludesFile ~/.config/git/ignore` |

For further study, refer to the official [Git ignore documentation](<https://git-scm.com/docs/gitignore?utm_source=chatgpt.com>) and [GitHub's guide to ignoring files](<https://docs.github.com/en/get-started/git-basics/ignoring-files?utm_source=chatgpt.com>).

GitHub

+1

The key interview takeaway: `.gitignore` prevents matching untracked files from being added accidentally. It does not untrack existing files, delete them, or remove secrets from Git history.