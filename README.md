# Git Dictionary

A complete reference of Git commands in a direct and straightforward way.

## Table of Contents

- [Basic Commands](#basic-commands)
- [Repository Setup](#repository-setup)
- [File Operations](#file-operations)
- [Branching](#branching)
- [Remote Repositories](#remote-repositories)
- [Viewing History](#viewing-history)
- [Undoing Changes](#undoing-changes)
- [Merging and Rebasing](#merging-and-rebasing)
- [Stashing](#stashing)
- [Tagging](#tagging)
- [Configuration](#configuration)
- [Gitignore](#gitignore)
- [Advanced Commands](#advanced-commands)
- [Useful Aliases](#useful-aliases)

---

## Basic Commands

Essential Git commands you'll use every day:

| Command | Meaning |
|---------|---------|
| `git init` | initialize new repository |
| `git clone <url>` | clone repository from remote |
| `git status` | show working directory status |
| `git add <file>` | add file to staging area |
| `git add .` | add all changes to staging area |
| `git commit -m "message"` | commit changes with message |
| `git push` | push commits to remote repository |
| `git pull` | pull latest changes from remote |

**Example:**
```bash
git init                          # initialize new repository
git add README.md                 # add file to staging
git commit -m "initial commit"    # commit with message
git remote add origin <url>       # connect to remote
git push -u origin main          # push to remote repository
```

---

## Repository Setup

Commands to set up and initialize repositories:

| Command | Meaning |
|---------|---------|
| `git init` | create new repository |
| `git init --bare` | create bare repository (no working directory) |
| `git clone <url>` | clone existing repository |
| `git clone <url> <folder>` | clone into specific folder |
| `git remote add <name> <url>` | add remote repository |
| `git remote -v` | list all remotes |
| `git remote remove <name>` | remove remote |
| `git remote rename <old> <new>` | rename remote |

**Example:**
```bash
# Start new project
git init my-project               # create new repository
cd my-project
git remote add origin https://github.com/user/repo.git

# Clone existing project
git clone https://github.com/user/repo.git
git clone https://github.com/user/repo.git my-folder
```

---

## File Operations

Managing files and changes:

| Command | Meaning |
|---------|---------|
| `git add <file>` | add specific file |
| `git add .` | add all changes |
| `git add -A` | add all changes including deletions |
| `git add -u` | add only modified files |
| `git rm <file>` | remove file from repository |
| `git rm --cached <file>` | remove from repository but keep locally |
| `git mv <old> <new>` | rename/move file |
| `git restore <file>` | restore file to last commit |
| `git restore --staged <file>` | unstage file |

**Example:**
```bash
git add index.html               # add specific file
git add src/                     # add entire folder
git add *.js                     # add all JavaScript files
git rm old-file.txt              # remove file
git mv old-name.txt new-name.txt # rename file
git restore broken-file.js       # restore file
```

---

## Branching

Working with branches:

| Command | Meaning |
|---------|---------|
| `git branch` | list all branches |
| `git branch <name>` | create new branch |
| `git checkout <branch>` | switch to branch |
| `git checkout -b <name>` | create and switch to new branch |
| `git switch <branch>` | switch to branch (newer command) |
| `git switch -c <name>` | create and switch to new branch |
| `git branch -d <name>` | delete branch |
| `git branch -D <name>` | force delete branch |
| `git branch -m <old> <new>` | rename branch |

**Example:**
```bash
git branch                       # list branches
git branch feature-login        # create branch
git checkout feature-login      # switch to branch
git checkout -b bug-fix          # create and switch
git switch main                  # switch to main
git branch -d feature-login     # delete branch
```

---

## Remote Repositories

Working with remote repositories:

| Command | Meaning |
|---------|---------|
| `git push` | push to default remote |
| `git push <remote> <branch>` | push specific branch |
| `git push -u <remote> <branch>` | push and set upstream |
| `git push --all` | push all branches |
| `git pull` | pull from default remote |
| `git pull <remote> <branch>` | pull specific branch |
| `git fetch` | download changes without merging |
| `git fetch --all` | fetch from all remotes |

**Example:**
```bash
git push origin main             # push to origin/main
git push -u origin feature       # push and set upstream
git pull origin main             # pull from origin/main
git fetch                        # download latest changes
git fetch --all                  # fetch from all remotes
```

---

## Viewing History

Exploring repository history:

| Command | Meaning |
|---------|---------|
| `git log` | show commit history |
| `git log --oneline` | show condensed history |
| `git log --graph` | show branch graph |
| `git log -n <number>` | show last n commits |
| `git log --author="name"` | show commits by author |
| `git log --since="date"` | show commits since date |
| `git show <commit>` | show specific commit details |
| `git diff` | show unstaged changes |
| `git diff --staged` | show staged changes |
| `git diff <branch1>..<branch2>` | compare branches |

**Example:**
```bash
git log --oneline                # condensed history
git log --graph --all            # visual branch history
git log -5                       # last 5 commits
git log --author="John"          # commits by John
git log --since="2 weeks ago"    # recent commits
git diff                         # see current changes
git diff main..feature           # compare branches
```

---

## Undoing Changes

Fixing mistakes and reverting changes:

| Command | Meaning |
|---------|---------|
| `git restore <file>` | restore file to last commit |
| `git restore --staged <file>` | unstage file |
| `git reset <file>` | unstage file (alternative) |
| `git reset --soft HEAD~1` | undo last commit, keep changes staged |
| `git reset --mixed HEAD~1` | undo last commit, unstage changes |
| `git reset --hard HEAD~1` | undo last commit, lose all changes |
| `git revert <commit>` | create new commit that undoes changes |
| `git checkout <commit> -- <file>` | restore file from specific commit |

**Example:**
```bash
git restore index.html           # restore file
git restore --staged .           # unstage all files
git reset --soft HEAD~1          # undo commit, keep changes
git reset --hard HEAD~1          # undo commit, lose changes
git revert abc123                # create revert commit
```

---

## Merging and Rebasing

Combining branches:

| Command | Meaning |
|---------|---------|
| `git merge <branch>` | merge branch into current |
| `git merge --no-ff <branch>` | merge with merge commit |
| `git merge --squash <branch>` | merge and squash commits |
| `git rebase <branch>` | rebase current branch onto another |
| `git rebase -i <commit>` | interactive rebase |
| `git cherry-pick <commit>` | apply specific commit |
| `git merge --abort` | abort merge in progress |
| `git rebase --abort` | abort rebase in progress |

**Example:**
```bash
git checkout main                # switch to main
git merge feature-branch         # merge feature into main
git rebase main                  # rebase current branch onto main
git cherry-pick abc123           # apply specific commit
git merge --abort                # cancel merge
```

---

## Stashing

Temporarily saving work:

| Command | Meaning |
|---------|---------|
| `git stash` | stash current changes |
| `git stash push -m "message"` | stash with message |
| `git stash list` | list all stashes |
| `git stash show` | show stash contents |
| `git stash pop` | apply and remove last stash |
| `git stash apply` | apply stash without removing |
| `git stash drop` | delete last stash |
| `git stash clear` | delete all stashes |

**Example:**
```bash
git stash                        # stash current work
git stash push -m "WIP: feature" # stash with message
git stash list                   # see all stashes
git stash pop                    # apply latest stash
git stash apply stash@{1}        # apply specific stash
git stash drop                   # delete latest stash
```

---

## Tagging

Creating release versions:

| Command | Meaning |
|---------|---------|
| `git tag` | list all tags |
| `git tag <name>` | create lightweight tag |
| `git tag -a <name> -m "message"` | create annotated tag |
| `git tag -d <name>` | delete local tag |
| `git push origin <tag>` | push specific tag |
| `git push --tags` | push all tags |
| `git checkout <tag>` | checkout specific tag |

**Example:**
```bash
git tag v1.0.0                   # create lightweight tag
git tag -a v1.0.0 -m "Release 1.0.0" # create annotated tag
git push origin v1.0.0           # push tag
git push --tags                  # push all tags
git tag -d v1.0.0               # delete tag locally
```

---

## Configuration

Setting up Git preferences:

| Command | Meaning |
|---------|---------|
| `git config --global user.name "Name"` | set global username |
| `git config --global user.email "email"` | set global email |
| `git config --list` | show all configurations |
| `git config user.name` | show current username |
| `git config --global init.defaultBranch main` | set default branch name |
| `git config --global core.editor "code"` | set default editor |
| `git config --global --unset <key>` | remove configuration |

**Example:**
```bash
git config --global user.name "John Doe"
git config --global user.email "john@example.com"
git config --global init.defaultBranch main
git config --global core.editor "code --wait"
git config --list                # see all settings
```

---

## Gitignore

Managing ignored files:

| Pattern | Meaning |
|---------|---------|
| `file.txt` | ignore specific file |
| `*.log` | ignore all .log files |
| `folder/` | ignore entire folder |
| `!important.log` | don't ignore this file (exception) |
| `temp/*.tmp` | ignore .tmp files in temp folder |
| `**/logs` | ignore logs folders anywhere |
| `doc/**/*.pdf` | ignore all PDFs in doc subfolders |

**Common .gitignore patterns:**
```bash
# Dependencies
node_modules/
vendor/

# Build outputs
dist/
build/
*.exe
*.dll

# Environment files
.env
.env.local
.env.production

# IDE files
.vscode/
.idea/
*.swp
*.swo

# Operating System
.DS_Store
Thumbs.db

# Logs
logs/
*.log
npm-debug.log*

# Runtime data
pids/
*.pid
*.seed

# Coverage directory used by tools like istanbul
coverage/

# Database
*.db
*.sqlite

# Temporary files
*.tmp
*.temp
temp/
```

**Gitignore commands:**
```bash
# Create gitignore file
touch .gitignore

# Remove already tracked files
git rm --cached file-to-ignore.txt
git rm -r --cached folder-to-ignore/

# Check if file is ignored
git check-ignore -v file.txt

# Show ignored files
git ls-files --others --ignored --exclude-standard
```

---

## Advanced Commands

More powerful Git operations:

| Command | Meaning |
|---------|---------|
| `git bisect start` | start binary search for bug |
| `git bisect good <commit>` | mark commit as good |
| `git bisect bad <commit>` | mark commit as bad |
| `git grep "pattern"` | search for text in repository |
| `git blame <file>` | show who changed each line |
| `git reflog` | show reference log |
| `git clean -f` | remove untracked files |
| `git clean -fd` | remove untracked files and directories |
| `git archive` | create archive of repository |

**Example:**
```bash
git bisect start                 # start bug hunting
git bisect bad HEAD              # current commit is bad
git bisect good v1.0             # v1.0 was good
git grep "TODO"                  # find all TODOs
git blame index.html             # see who changed what
git clean -fd                    # clean untracked files
```

---

## Useful Aliases

Common Git aliases to save time:

**Setup aliases:**
```bash
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'
```

**Useful aliases:**
```bash
# Short status
git config --global alias.s "status -s"

# Pretty log
git config --global alias.lg "log --oneline --graph --all"

# Show branches
git config --global alias.br "branch -v"

# Undo last commit
git config --global alias.undo "reset --soft HEAD~1"

# Show what would be pushed
git config --global alias.ready "diff --name-status origin/main"

# Quick commit
git config --global alias.cm "commit -m"

# Quick add and commit
git config --global alias.ac "!git add -A && git commit -m"
```

**Usage after setup:**
```bash
git st                           # git status
git co main                      # git checkout main
git br                           # git branch -v
git lg                           # git log --oneline --graph --all
git cm "fix bug"                 # git commit -m "fix bug"
git ac "quick fix"               # git add -A && git commit -m "quick fix"
```

---

## Git Workflow Examples

### Basic Workflow
```bash
# Start working
git pull origin main             # get latest changes
git checkout -b feature-branch   # create feature branch

# Work on feature
git add .                        # stage changes
git commit -m "add new feature"  # commit changes
git push -u origin feature-branch # push branch

# Merge back
git checkout main                # switch to main
git pull origin main             # get latest main
git merge feature-branch         # merge feature
git push origin main             # push to main
git branch -d feature-branch     # delete local branch
```

### Emergency Hotfix
```bash
git checkout main                # switch to main
git pull origin main             # get latest
git checkout -b hotfix-bug       # create hotfix branch
# Fix the bug
git add .
git commit -m "fix critical bug"
git push -u origin hotfix-bug
# Create pull request, merge, then:
git checkout main
git pull origin main
git branch -d hotfix-bug
```

### Working with Forks
```bash
# Fork on GitHub, then:
git clone https://github.com/youruser/repo.git
cd repo
git remote add upstream https://github.com/originaluser/repo.git

# Before working, sync with upstream
git fetch upstream
git checkout main
git merge upstream/main
git push origin main

# Create feature
git checkout -b new-feature
# Work, commit, push
git push -u origin new-feature
# Create pull request on GitHub
```

---

## Troubleshooting

Common problems and solutions:

### Merge Conflicts
```bash
# When merge conflicts occur:
git status                       # see conflicted files
# Edit files to resolve conflicts
git add .                        # stage resolved files
git commit                       # complete merge
```

### Accidental Commits
```bash
# Undo last commit but keep changes
git reset --soft HEAD~1

# Undo last commit and lose changes
git reset --hard HEAD~1

# Change last commit message
git commit --amend -m "new message"
```

### Lost Work
```bash
# Find lost commits
git reflog                       # see all recent actions
git checkout <commit-hash>       # recover lost work
git checkout -b recovery-branch  # save recovered work
```

### Large Files
```bash
# Remove large file from history
git filter-branch --force --index-filter \
'git rm --cached --ignore-unmatch large-file.zip' \
--prune-empty --tag-name-filter cat -- --all
```

---

## How to Use This Dictionary

### Quick Search

Use Ctrl+F (or Cmd+F on Mac) to quickly find any command.

### Learning Path

1. **Start with basics**: init, add, commit, push, pull
2. **Learn branching**: branch, checkout, merge
3. **Master undoing**: reset, revert, restore
4. **Explore advanced**: rebase, stash, cherry-pick
5. **Customize**: aliases and configuration

### Best Practices

- **Commit often** with clear messages
- **Use branches** for features and experiments
- **Keep commits small** and focused
- **Write good commit messages** (present tense, clear)
- **Pull before pushing** to avoid conflicts
- **Use .gitignore** to keep repository clean

---

## License

This dictionary is open source and can be used freely in your projects and studies.

---

**Last updated:** July 2025  
**Version:** 1.0

*"Git is powerful but complex. This dictionary helps you navigate it with confidence!"*