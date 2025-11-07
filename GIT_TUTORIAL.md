# Complete Git Setup & Management Tutorial

A comprehensive guide covering Git installation, configuration, and branch management for Linux, Windows, and macOS.

## Table of Contents
1. [Installation](#installation)
2. [Initial Configuration](#initial-configuration)
3. [Git Basics](#git-basics)
4. [Repository Setup](#repository-setup)
5. [Branch Management](#branch-management)
6. [Common Workflows](#common-workflows)
7. [Advanced Operations](#advanced-operations)
8. [Troubleshooting](#troubleshooting)
9. [Useful Resources](#useful-resources)

---

## Installation

### Linux

#### Ubuntu/Debian
```bash
sudo apt update
sudo apt install git
```

#### Fedora/RHEL/CentOS
```bash
# Fedora
sudo dnf install git

# RHEL/CentOS 7
sudo yum install git

# RHEL/CentOS 8+
sudo dnf install git
```

#### Arch Linux
```bash
sudo pacman -S git
```

#### From Source (All Linux)
```bash
# Install dependencies first
sudo apt install build-essential libssl-dev libcurl4-gnutls-dev libexpat1-dev gettext unzip

# Download and install
wget https://github.com/git/git/archive/v2.43.0.tar.gz
tar -xf v2.43.0.tar.gz
cd git-2.43.0
make prefix=/usr/local all
sudo make prefix=/usr/local install
```

### macOS

#### Using Homebrew (Recommended)
```bash
# Install Homebrew if not already installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Git
brew install git
```

#### Using Xcode Command Line Tools
```bash
xcode-select --install
```

#### Using MacPorts
```bash
sudo port install git
```

### Windows

#### Using Git for Windows (Recommended)
1. Download from: https://git-scm.com/download/win
2. Run the installer
3. Follow the installation wizard with recommended settings

#### Using Winget
```powershell
winget install --id Git.Git -e --source winget
```

#### Using Chocolatey
```powershell
choco install git
```

#### Using Scoop
```powershell
scoop install git
```

### Verify Installation
```bash
git --version
```

---

## Initial Configuration

### Set Your Identity
```bash
# Set global username
git config --global user.name "Your Name"

# Set global email
git config --global user.email "your.email@example.com"
```

### Configure Default Branch Name
```bash
git config --global init.defaultBranch main
```

### Configure Line Endings

#### Linux/macOS
```bash
git config --global core.autocrlf input
```

#### Windows
```bash
git config --global core.autocrlf true
```

### Set Default Text Editor
```bash
# Vim
git config --global core.editor "vim"

# Nano
git config --global core.editor "nano"

# VS Code
git config --global core.editor "code --wait"

# Sublime Text
git config --global core.editor "subl -n -w"

# Notepad++ (Windows)
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

### Configure Credential Storage

#### Linux
```bash
# Cache credentials for 15 minutes
git config --global credential.helper cache

# Cache for custom duration (in seconds)
git config --global credential.helper 'cache --timeout=3600'
```

#### macOS
```bash
# Store in macOS Keychain
git config --global credential.helper osxkeychain
```

#### Windows
```bash
# Use Windows Credential Manager
git config --global credential.helper manager-core
```

### View All Configuration
```bash
# View all settings
git config --list

# View specific setting
git config user.name

# View with origin info
git config --list --show-origin
```

### Configuration Levels
```bash
# System-wide (affects all users)
git config --system

# User-level (affects current user)
git config --global

# Repository-level (affects current repo only)
git config --local
```

---

## Git Basics

### Core Concepts

**Repository (Repo)**: A directory containing your project files and Git's version history

**Commit**: A snapshot of your project at a specific point in time

**Branch**: An independent line of development

**Remote**: A version of your repository hosted on the internet or network

**Working Directory**: Your local files where you make changes

**Staging Area (Index)**: A holding area for changes before committing

**HEAD**: Pointer to the current branch/commit you're working on

### Git Workflow
```
Working Directory → (git add) → Staging Area → (git commit) → Local Repository → (git push) → Remote Repository
```

### File States in Git
- **Untracked**: New files Git doesn't know about
- **Unmodified**: Files that haven't changed since last commit
- **Modified**: Changed files not yet staged
- **Staged**: Files ready to be committed

---

## Repository Setup

### Initialize a New Repository
```bash
# Create new directory and initialize
mkdir my-project
cd my-project
git init

# Initialize with specific branch name
git init -b main
```

### Clone an Existing Repository
```bash
# Clone via HTTPS
git clone https://github.com/username/repository.git

# Clone via SSH
git clone git@github.com:username/repository.git

# Clone to specific directory
git clone https://github.com/username/repository.git my-folder

# Clone specific branch
git clone -b branch-name https://github.com/username/repository.git

# Shallow clone (limited history)
git clone --depth 1 https://github.com/username/repository.git
```

### Connect to Remote Repository
```bash
# Add remote
git remote add origin https://github.com/username/repository.git

# Verify remote
git remote -v

# Change remote URL
git remote set-url origin https://github.com/username/new-repository.git

# Remove remote
git remote remove origin

# Rename remote
git remote rename origin upstream
```

---

## Branch Management

### Creating Branches

```bash
# Create new branch
git branch feature-branch

# Create and switch to new branch
git checkout -b feature-branch

# Modern way (Git 2.23+)
git switch -c feature-branch

# Create branch from specific commit
git branch feature-branch abc1234

# Create branch from remote branch
git checkout -b feature-branch origin/feature-branch
```

### Switching Branches

```bash
# Switch to existing branch (old way)
git checkout branch-name

# Switch to existing branch (new way - Git 2.23+)
git switch branch-name

# Switch to previous branch
git checkout -
# or
git switch -

# Create and switch if branch doesn't exist
git switch -C branch-name
```

### Viewing Branches

```bash
# List local branches
git branch

# List all branches (local and remote)
git branch -a

# List remote branches only
git branch -r

# List branches with last commit info
git branch -v

# List branches with tracking info
git branch -vv

# List merged branches
git branch --merged

# List unmerged branches
git branch --no-merged

# Show current branch
git branch --show-current
```

### Renaming Branches

```bash
# Rename current branch
git branch -m new-branch-name

# Rename different branch
git branch -m old-name new-name

# Rename and push to remote
git branch -m old-name new-name
git push origin -u new-name
git push origin --delete old-name
```

### Deleting Branches

```bash
# Delete local branch (safe - prevents deletion if unmerged)
git branch -d branch-name

# Force delete local branch
git branch -D branch-name

# Delete remote branch
git push origin --delete branch-name
# or
git push origin :branch-name

# Delete multiple branches
git branch -d branch1 branch2 branch3

# Delete all local branches except current
git branch | grep -v "^*" | xargs git branch -d
```

### Tracking Remote Branches

```bash
# Push and set upstream
git push -u origin branch-name

# Set upstream for existing branch
git branch --set-upstream-to=origin/branch-name

# Push to different remote branch name
git push origin local-branch:remote-branch

# Fetch all remote branches
git fetch --all

# Prune deleted remote branches
git fetch --prune
# or
git remote prune origin
```

### Branch Comparison

```bash
# Show commits in branch1 not in branch2
git log branch1 ^branch2

# Show differences between branches
git diff branch1..branch2

# Show files changed between branches
git diff --name-only branch1 branch2

# Show commits different between branches
git log --oneline --graph branch1..branch2
```

---

## Common Workflows

### Basic Git Workflow

```bash
# 1. Check status
git status

# 2. Add files to staging
git add filename.txt          # Add specific file
git add .                     # Add all files in current directory
git add -A                    # Add all changes (new, modified, deleted)
git add *.js                  # Add all JS files
git add -p                    # Interactive staging (patch mode)

# 3. Commit changes
git commit -m "Descriptive commit message"

# 4. Push to remote
git push origin branch-name

# Shortcut: Add and commit in one step (tracked files only)
git commit -am "Commit message"
```

### Viewing History

```bash
# Show commit history
git log

# One line per commit
git log --oneline

# Show last N commits
git log -n 5

# Show with graph
git log --oneline --graph --all

# Show commits by author
git log --author="Name"

# Show commits in date range
git log --since="2024-01-01" --until="2024-12-31"

# Show commits affecting specific file
git log -- filename.txt

# Show commit details
git show commit-hash

# Show changes in commit
git show commit-hash --stat
```

### Undoing Changes

```bash
# Discard changes in working directory
git checkout -- filename.txt
# or (Git 2.23+)
git restore filename.txt

# Unstage file (keep changes)
git reset HEAD filename.txt
# or (Git 2.23+)
git restore --staged filename.txt

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# Undo specific commit (create new commit)
git revert commit-hash

# Amend last commit
git commit --amend -m "New message"

# Amend without changing message
git commit --amend --no-edit
```

### Stashing Changes

```bash
# Stash current changes
git stash

# Stash with message
git stash save "Work in progress"

# List stashes
git stash list

# Apply most recent stash
git stash apply

# Apply and remove most recent stash
git stash pop

# Apply specific stash
git stash apply stash@{2}

# Show stash contents
git stash show

# Show detailed stash diff
git stash show -p

# Drop specific stash
git stash drop stash@{0}

# Clear all stashes
git stash clear

# Stash including untracked files
git stash -u
```

### Merging Branches

```bash
# Merge branch into current branch
git merge branch-name

# Merge with commit message
git merge branch-name -m "Merge message"

# Merge without fast-forward (creates merge commit)
git merge --no-ff branch-name

# Abort merge in case of conflicts
git merge --abort

# Continue merge after resolving conflicts
git merge --continue
```

### Rebasing

```bash
# Rebase current branch onto main
git rebase main

# Interactive rebase (last 3 commits)
git rebase -i HEAD~3

# Continue rebase after resolving conflicts
git rebase --continue

# Skip current commit during rebase
git rebase --skip

# Abort rebase
git rebase --abort

# Rebase onto remote branch
git pull --rebase origin main
```

### Cherry-Picking

```bash
# Apply specific commit to current branch
git cherry-pick commit-hash

# Cherry-pick multiple commits
git cherry-pick commit1 commit2 commit3

# Cherry-pick without committing
git cherry-pick --no-commit commit-hash

# Continue cherry-pick after resolving conflicts
git cherry-pick --continue

# Abort cherry-pick
git cherry-pick --abort
```

### Tagging

```bash
# Create lightweight tag
git tag v1.0.0

# Create annotated tag
git tag -a v1.0.0 -m "Version 1.0.0"

# Tag specific commit
git tag -a v1.0.0 commit-hash

# List all tags
git tag

# List tags matching pattern
git tag -l "v1.*"

# Show tag info
git show v1.0.0

# Push tag to remote
git push origin v1.0.0

# Push all tags
git push origin --tags

# Delete local tag
git tag -d v1.0.0

# Delete remote tag
git push origin --delete v1.0.0
```

---

## Advanced Operations

### Resolving Merge Conflicts

```bash
# 1. Check conflicted files
git status

# 2. View conflicts
git diff

# 3. Open and edit conflicted files
# Look for conflict markers:
<<<<<<< HEAD
Your changes
=======
Their changes
>>>>>>> branch-name

# 4. After resolving, add files
git add filename.txt

# 5. Complete merge
git commit
```

### Submodules

```bash
# Add submodule
git submodule add https://github.com/user/repo.git path/to/submodule

# Initialize submodules after clone
git submodule init
git submodule update

# Clone with submodules
git clone --recursive https://github.com/user/repo.git

# Update submodules
git submodule update --remote

# Remove submodule
git submodule deinit path/to/submodule
git rm path/to/submodule
rm -rf .git/modules/path/to/submodule
```

### Git Bisect (Find Bug Introduction)

```bash
# Start bisect
git bisect start

# Mark current commit as bad
git bisect bad

# Mark known good commit
git bisect good commit-hash

# Git will checkout commits for you to test
# After each test:
git bisect good    # if bug not present
git bisect bad     # if bug present

# End bisect
git bisect reset
```

### Git Hooks

Common hooks location: `.git/hooks/`

```bash
# Pre-commit hook example
# .git/hooks/pre-commit
#!/bin/sh
npm test

# Make executable
chmod +x .git/hooks/pre-commit
```

### .gitignore Examples

```bash
# Create .gitignore file
touch .gitignore

# Common patterns:
# Ignore all .log files
*.log

# Ignore node_modules
node_modules/

# Ignore environment files
.env
.env.local

# Ignore OS files
.DS_Store
Thumbs.db

# Ignore IDE folders
.vscode/
.idea/

# Ignore build directories
dist/
build/
out/

# But keep specific file
!important.log

# View ignored files
git status --ignored
```

### Git Aliases

```bash
# Create aliases
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual 'log --oneline --graph --all'

# Use alias
git co main
git st
```

---

## Troubleshooting

### Common Issues and Solutions

#### Accidentally committed to wrong branch
```bash
# Create new branch with current changes
git branch feature-branch

# Reset current branch
git reset --hard HEAD~1

# Switch to new branch
git checkout feature-branch
```

#### Committed with wrong email/name
```bash
# Amend last commit author
git commit --amend --author="Name <email@example.com>"
```

#### Forgot to add files to last commit
```bash
git add forgotten-file.txt
git commit --amend --no-edit
```

#### Need to undo pushed commit
```bash
# Create reverse commit (safe)
git revert commit-hash
git push origin branch-name

# Force reset (dangerous, use with caution)
git reset --hard commit-hash
git push -f origin branch-name
```

#### Merge conflicts during pull
```bash
# Option 1: Resolve manually
git pull
# Fix conflicts in files
git add .
git commit

# Option 2: Use rebase
git pull --rebase

# Option 3: Abort and try differently
git merge --abort
```

#### Accidentally deleted uncommitted changes
```bash
# Check reflog
git reflog

# Restore from reflog
git checkout -b recovery commit-hash
```

#### Large file committed by mistake
```bash
# Remove from last commit
git rm --cached large-file
git commit --amend

# Remove from history (use BFG Repo-Cleaner)
# Download from: https://rtyley.github.io/bfg-repo-cleaner/
java -jar bfg.jar --delete-files large-file repo.git
```

#### Detached HEAD state
```bash
# Create branch from current state
git checkout -b new-branch

# Or return to previous branch
git checkout main
```

---

## Useful Resources

### Official Documentation
- Git Official Website: https://git-scm.com/
- Git Documentation: https://git-scm.com/doc
- Git Reference Manual: https://git-scm.com/docs
- Pro Git Book (Free): https://git-scm.com/book/en/v2

### Interactive Learning
- Learn Git Branching: https://learngitbranching.js.org/
- Git Immersion: http://gitimmersion.com/
- Katacoda Git Scenarios: https://www.katacoda.com/courses/git
- Git Exercises: https://gitexercises.fracz.com/

### Cheat Sheets
- GitHub Git Cheat Sheet: https://education.github.com/git-cheat-sheet-education.pdf
- Atlassian Git Cheat Sheet: https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet
- GitLab Git Cheat Sheet: https://about.gitlab.com/images/press/git-cheat-sheet.pdf

### GUI Tools
- GitKraken: https://www.gitkraken.com/
- SourceTree: https://www.sourcetreeapp.com/
- GitHub Desktop: https://desktop.github.com/
- Git Extensions (Windows): https://gitextensions.github.io/
- Tower: https://www.git-tower.com/

### GitHub/GitLab/Bitbucket
- GitHub Docs: https://docs.github.com/
- GitHub CLI: https://cli.github.com/
- GitLab Docs: https://docs.gitlab.com/
- Bitbucket Docs: https://support.atlassian.com/bitbucket-cloud/

### Advanced Topics
- Git Workflows: https://www.atlassian.com/git/tutorials/comparing-workflows
- Git Hooks: https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks
- Git Internals: https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain

### Best Practices
- Conventional Commits: https://www.conventionalcommits.org/
- Git Flow: https://nvie.com/posts/a-successful-git-branching-model/
- GitHub Flow: https://guides.github.com/introduction/flow/
- Semantic Versioning: https://semver.org/

### Troubleshooting
- Oh Shit, Git!?: https://ohshitgit.com/
- Dangit, Git!?: https://dangitgit.com/ (PG version)
- Flight Rules for Git: https://github.com/k88hudson/git-flight-rules

### Video Tutorials
- Git & GitHub Crash Course (Traversy Media): https://www.youtube.com/watch?v=SWYqp7iY_Tc
- Git Tutorial for Beginners (Programming with Mosh): https://www.youtube.com/watch?v=8JJ101D3knE
- Advanced Git Tutorials (Atlassian): https://www.youtube.com/playlist?list=PLu-nSsOS6FRIg52MWrd7C_qSnQp3ZoHwW

---

## Quick Reference Commands

### Setup & Configuration
```bash
git config --global user.name "Name"
git config --global user.email "email@example.com"
git config --list
```

### Repository Operations
```bash
git init
git clone <url>
git remote add origin <url>
```

### Basic Commands
```bash
git status
git add <file>
git add .
git commit -m "message"
git push origin <branch>
git pull origin <branch>
```

### Branching
```bash
git branch                    # List branches
git branch <name>            # Create branch
git checkout <branch>        # Switch branch
git checkout -b <branch>     # Create and switch
git merge <branch>           # Merge branch
git branch -d <branch>       # Delete branch
```

### History & Inspection
```bash
git log
git log --oneline
git diff
git show <commit>
```

### Undo Changes
```bash
git checkout -- <file>       # Discard changes
git reset HEAD <file>        # Unstage
git reset --soft HEAD~1      # Undo commit (keep changes)
git reset --hard HEAD~1      # Undo commit (discard changes)
```

---

## Common Git Workflows

### Feature Branch Workflow
```bash
# 1. Create feature branch
git checkout -b feature/new-feature

# 2. Make changes and commit
git add .
git commit -m "Add new feature"

# 3. Push to remote
git push -u origin feature/new-feature

# 4. Create pull request on GitHub/GitLab

# 5. After merge, cleanup
git checkout main
git pull origin main
git branch -d feature/new-feature
```

### Hotfix Workflow
```bash
# 1. Create hotfix branch from main
git checkout main
git pull origin main
git checkout -b hotfix/critical-bug

# 2. Fix and commit
git add .
git commit -m "Fix critical bug"

# 3. Push and merge quickly
git push -u origin hotfix/critical-bug
# Create and merge PR

# 4. Update local main
git checkout main
git pull origin main
git branch -d hotfix/critical-bug
```

---

## License
This tutorial is provided as-is for educational purposes.

## Contributing
Feel free to suggest improvements or report issues!

Last Updated: 2025
