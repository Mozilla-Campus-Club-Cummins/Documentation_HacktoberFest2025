# Git & Version Control

Git is a distributed version control system designed to track changes in your codebase, collaborate with multiple developers, and manage project history efficiently. This document explains Git concepts, commands, branching, merge conflicts, pull requests, and contributing to open source in a detailed and beginner-friendly way.

Version control is a way to track changes made to a project, especially code. Without it, you could accidentally overwrite your work or lose track of changes. Git solves this by keeping a history of everything you do, so you can go back to any previous version anytime.

When we say Git is distributed, it means that every developer’s computer has a full copy of the project’s history, including who changed what and when. This allows features like git blame to identify exactly who made a change.

## Checking Git Version

Before you start, make sure Git is installed:

```bash
git --version
# This will show the version installed on your system.
```

## Setting Up Git

Before using Git for the first time, you need to set up some basic configuration like username and email address. Git uses this information to track who made each commit. These settings are stored in Git's global configuration, which means they apply to all the repositories on your computer.

```bash
git config --global user.name 'Gunn'

git config --global user.email 'gunn@gmail.com'
```

You can check your settings anytime:

```bash
git config --list
```

## File States in Git

1. Untracked : The file exists in your working directory but has not been added to Git.
2. Tracked : The file is already under Git’s version control.
    - Unmodified → The file has not changed since the last commit.
    - **Modified** → The file has been changed but not yet staged. It is the one that you have changed but haven't told Git to save yet. Git knows the file exists, but it doesn’t track the new changes yet.
    - Staged → A staged file means you have told Git, “Hey, I want to save this change in the next snapshot (commit).”
3. Ignoring Files : Some files should not be tracked by Git (e.g., logs, compiled files, sensitive data). You can create a `.gitignore` file and list the files or directories to exclude.

**`git status`** shows you which files have been modified, which files are staged for the next commit, and which are untracked.

```bash
git status     # Shows the status of files (staged, modified, etc.)
git diff       # Shows the differences between the working directory and the staged version

git add file_name      # staging the file
git add .              # Now all the files will be tracked by git
```

## Creating a Repository

A repository is a place where Git stores all your project files and history.

### Initialize a New Repo

```bash
mkdir my-project
cd my-project
git init
```

This creates a `.git` folder inside your project. Your project is now a local Git repository.

### Local V/S Remote Git Repositories

Git projects exist in two main places :

-   Local Repository -> on your computer.
-   Remote Repository -> on a server like GitHub, GitLab or BitBucket.

</br>

***Local Repository*** is the copy of the project on your computer. It keeps track of every change you make including:

-   File modifications
-   New Files added
-   Branches and commits

**_> How to Create a Local Repo:_**

```bash
mkdir my-project
cd my-project
git init # initializes a new local Git repository
# creates a hidden .git folder containing the entire history and metadata.
```

*Workflow in Local Repository_

1. Working Directory -> where you edit files
2. Staging Area -> where you prepare changes for commit (git add).
3. Repository -> commits are saved here (git commit).

Example:

```pgsql
Working Directory: index.html modified
          ↓ git add index.html
Staging Area: index.html staged
          ↓ git commit -m "Update index.html"
Repository: index.html saved in commit history
```

Steps to Save Changes:

```bash
git status                # See which files are modified
git add index.html        # Stage a file
git commit -m "Added homepage"  # Commit the staged file
```

- git add → tells Git which changes to include in the next commit.
- git commit → records a snapshot in the repository.

```pgsql
Local Repo (on your computer)
 ┌───────────────┐
 │ Working Dir   │ <-- Edit files here
 │ Staging Area  │ <-- git add
 │ Commits       │ <-- git commit
 └───────┬───────┘
         │ push
         ▼
Remote Repo (GitHub/GitLab)
 ┌───────────────┐
 │ Branches      │
 │ Pull Requests │ <-- Review & Merge
 └───────────────┘
```

### Cloning an Existing Repository

If a project already exists on GitHub, you can clone it:

```bash
git clone https://github.com/username/repo.git
```

This creates a full local copy of the repository. The original repository is called remote usually referred to as origin.

## Branching

Branches allow you to work on features separately without affecting the main project.

### Create a Branch

```bash
# Creates a new branch (if it doesn't exist before) and switches to it.
git checkout -b feature-login
# All commits now will go to this branch locally until you push it to remote repository
```

Default Branch of git is main. In git, Head is a pointer pointing to the latest commit you created.

Detached Head → a state where head no longer points to the latest commit.

```pgsql
main
 ├─●──●──●─────────────
     \
feature-login
      ●──●             <-- Work happens here
```

- main → stable branch
- feature-login → your local branch
- Commits on feature branch don’t affect main until merged

### Switch between Branches

```bash
git checkout main
```

### Merge a Branch

```bash
git merge feature-login
```

Example:

```mathematica
main:      A --- B --- C
feature:           \
                     D --- E
```

- A, B, C → commits on main
- D, E → commits on feature branch

### Local To Remote Repository

Once your local repo is ready, you can connect it to GitHub:

```bash
git remote add origin https://github.com/username/repo.git
git push -u origin main
```

- origin : name for your remote repository
- -u : sets the branch to track the remote repository.

Local ↔ Remote Workflow

```sql
Local Repository
   ├── Working Directory
   ├── Staging Area
   └── Repository (commits)
        │
        │ push
        ↓
Remote Repository (GitHub)
        ↑
        │ pull / fetch
```

- push => upload commits from local repo to remote.
- pull/fetch => download commits from remote to local.

## How to Collaborate with a team?

1. Clone the repo (copy of remote repo locally):

```bash
git clone https://github.com/org/repo.git
```

2. Create a feature branch:

```bash
git checkout -b feature-login
```

3. Make Changes -> Stage -> Commit:

```bash
git add .
git commit -m "Added login form"
```

4. Push branches to remote

```bash
git push origin feature-login
```

5. Open a Pull Request on GitHub

6. Maintainers review → merge → branch integrated into main

## Local V/S Remote Repo Quick Table & Workflow

| Feature   | Local Repo                      | Remote Repo                           |
| --------- | ------------------------------- | ------------------------------------- |
| Location  | On your computer                | On GitHub/GitLab/Bitbucket            |
| Purpose   | Track changes locally           | Central hub for collaboration         |
| Branching | Local branches                  | Remote branches track pushed branches |
| Commands  | git add, git commit, git branch | git push, git pull, git fetch         |
| Safety    | All changes local               | Shared with team, requires review     |

Visual WorkFlow:

```rust
[Local Repo]
Working Dir --> Staging Area --> Commits
       │                       │
       └---- push ------------> [Remote Repo (GitHub)]
                               │
       <---- pull/fetch -------┘

Branching:
main --> feature-login --> commits
feature-login --> push --> remote feature-login branch
```

## Merge Conflict

A merge conflict occurs when Git tries to merge two branches, but the same lines of a file have been modified in both branches differently. Git cannot decide which change to keep, so it pauses and asks you to resolve it manually.

Happens during: git merge or git rebase

Common scenario: Two developers edit the same function or line in the same file

```pgsql
main branch          feature branch
     ●                   ●
     │                   │
     │                   ▼
     │                modified line
     ▼
     conflict occurs here
```

main branch has a line of code like:

```js
console.log("Hello World!");
```

feature branch has changed the same line:

```js
console.log("Hello, Git!");
```

Now when you try git merge feature into main, Git sees two different changes in the same line → conflict.

1. Run git merge feature-branch in main and Git output will say something like:

```pgsql
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

2. git status

```pgsql
both modified: index.html
```

Git tells you which files have conflicts and Conflicted files have special markers inserted:

```html
<<<<<<< HEAD
console.log("Hello World!");
=======
console.log("Hello, Git!");
>>>>>>> feature-branch
```

Decide which changes to keep. You have three options:

Keep your branch version:

```js
console.log("Hello World!");
```

Keep the feature branch version:

```js
console.log("Hello, Git!");
```

Combine them (merge manually):

```js
console.log("Hello World & Git!");
```

After deciding, remove the conflict markers (<<<<<<<, =======, >>>>>>>)

3. Stage the resolved file

```bash
git add index.html
```

4. Commit the merge

```bash
git commit -m "Resolved merge conflict between main and feature-branch"
```

Workflow:

```pgsql
Step 1: Conflict detected
main:     ●──●
feature:      ●──●
                │
                ▼
Conflict occurs here

Step 2: Open file & resolve
<<<<<<< HEAD
main version
=======
feature version
>>>>>>> feature-branch
↓
Choose final version

Step 3: Stage resolved file
git add <file>

Step 4: Commit resolution
git commit
```

### Tips For Avoiding Conflicts

1. Pull often before starting new work

```bash
git checkout main
git pull origin main
```

2. Keep changes small and focused → smaller changes = fewer conflicts

3. Communicate with teammates if multiple people are editing the same files

4. Use feature branches → main branch stays stable.

## Pull Requests

Pull Requests are how you propose changes to a repository.

Workflow:

1. Fork the repository
2. Clone your fork
3. Create a new branch and make changes.
4. Push the branch to your fork
5. Open a Pull Request on GitHub
6. Maintainers review your PR and merge it.

## Rebasing

Rebasing is a way to move or “reapply” commits from one branch onto another branch, creating a clean, linear history instead of a “branchy” history you get with a regular merge.

### Why Use Rebasing?

- Keeps history linear and readable
- Makes it easier to follow the project history
- Useful before merging a feature branch into main

Note: Avoid rebasing branches that are shared with others because it rewrites history.

### Merge V/S Rebase

| Feature      | Merge                     | Rebase                                                |
| ------------ | ------------------------- | ----------------------------------------------------- |
| History      | Shows branches as merges  | Linear history, as if branch started from latest main |
| Commit graph | `●──●──●` + merge commits | `●──●──●` straight line                               |
| Conflicts    | Happens at merge point    | Happens at each commit being replayed                 |

### Rebasing Workflow

Imagine:

```mathematica
main branch:    A---B---C
feature branch:      D---E
```

- Commits D & E were made before main had C
- You want to apply D & E on top of C

```bash
git checkout feature
git rebase main
```

Git moves your commits D and E after C:

```mathematica
main branch:    A---B---C
feature branch:           D'---E'
```

Notice D and E become D' and E' — Git reapplies them as new commits The history now looks like a straight line.

### Handling Conflicts During Rebase

- If a commit has conflicts, Git stops:

```bash
git status
# Shows files with conflicts
```

- Resolve conflict in file

- Stage resolved file:

```bash
git add <file>
```

- Continue rebase:

```bash
git rebase --continue
```

- To abort rebase if stuck:

```bash
git rebase --abort
```