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

**Why Install Git?**

Git is a distributed version control system that allows you to:
- **Track changes** in your code over time
- **Collaborate** with other developers seamlessly
- **Revert** to previous versions when something breaks
- **Branch** and experiment without affecting the main codebase
- **Backup** your code to remote servers (GitHub, GitLab, etc.)

Without Git, managing code changes across a team becomes chaotic, and recovering from mistakes is nearly impossible. Git has become the industry standard for modern software development.

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

**Purpose**: Confirm Git is installed correctly and check the version number.

```bash
git --version
```

**Why This Matters**: Different Git versions have different features. Knowing your version helps when:
- Following tutorials that require specific features
- Troubleshooting compatibility issues
- Deciding whether to upgrade for new features (e.g., `git switch` was added in Git 2.23)

**Expected Output**: Should display something like `git version 2.43.0`

---

## Initial Configuration

**Why Configure Git?**

Before making your first commit, Git needs to know who you are. This information is embedded in every commit you make, creating an audit trail of who changed what. Proper configuration ensures:
- **Attribution**: Your commits show your name and email
- **Consistency**: Your commits look professional in team environments
- **Integration**: Proper setup with remote services like GitHub
- **Workflow Efficiency**: Correct editor, credential storage, and line ending settings prevent common frustrations

### Set Your Identity

**Purpose**: Identifies you as the author of commits. Every commit you make includes this information.

```bash
# Set global username
git config --global user.name "Your Name"

# Set global email
git config --global user.email "your.email@example.com"
```

**Why You Need This**:
- **Required for commits**: Git won't let you commit without this configuration
- **Collaboration**: Team members see who made each change
- **GitHub/GitLab integration**: Your commits link to your profile using your email
- **Legal/audit trails**: In corporate environments, this creates accountability

**Best Practices**:
- Use your real name for professional projects
- Use the same email as your GitHub/GitLab account for proper attribution
- For work projects, use your work email

**Verification**: Check your configuration with `git config user.name` and `git config user.email`

### Configure Default Branch Name

**Purpose**: Sets the name for the default branch when creating new repositories.

```bash
git config --global init.defaultBranch main
```

**Why You Need This**:
- **Modern standard**: The industry has moved from "master" to "main" as the default branch name
- **Consistency**: Ensures all your new repos use the same naming convention
- **Platform alignment**: GitHub and GitLab now default to "main"
- **Avoids confusion**: Without this, you might get "master" locally but "main" on remote platforms

**Historical Context**: The Git community adopted "main" as a more inclusive term. This command ensures your local setup matches current best practices.

### Configure Line Endings

**Purpose**: Handles differences in how operating systems represent line breaks in text files.

**The Problem**:
- **Windows** uses CRLF (Carriage Return + Line Feed: `\r\n`) to end lines
- **Linux/macOS** uses LF (Line Feed: `\n`) only
- Without configuration, files can show unnecessary changes when switching between systems

#### Linux/macOS
```bash
git config --global core.autocrlf input
```

**What This Does**:
- Converts CRLF to LF when you commit (input)
- Leaves LF unchanged when checking out files
- **Why**: Ensures your repository always has LF line endings, which is the Unix standard

#### Windows
```bash
git config --global core.autocrlf true
```

**What This Does**:
- Converts LF to CRLF when checking out code
- Converts CRLF to LF when committing
- **Why**: Allows Windows developers to work with CRLF locally (for compatibility with Windows tools) while keeping the repository standardized with LF

**Common Issue This Prevents**: Without proper line ending configuration, you'll see "phantom" changes in Git diff where files appear modified even though only line endings changed. This clutters pull requests and makes code review difficult.

### Set Default Text Editor

**Purpose**: Configures which text editor opens when Git needs you to write commit messages or resolve conflicts.

**When Git Uses Your Editor**:
- Writing detailed commit messages
- Editing during interactive rebase
- Resolving merge conflicts
- Editing configuration files

```bash
# Vim (powerful but steep learning curve)
git config --global core.editor "vim"

# Nano (beginner-friendly terminal editor)
git config --global core.editor "nano"

# VS Code (most popular for modern development - requires --wait flag)
git config --global core.editor "code --wait"

# Sublime Text (lightweight GUI editor)
git config --global core.editor "subl -n -w"

# Notepad++ (Windows - familiar interface)
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

**The `--wait` Flag**: Essential for GUI editors. It tells Git to wait until you close the editor before proceeding. Without it, Git thinks you're done immediately and may fail the operation.

**Recommendation for Beginners**:
- **Linux/macOS**: Use `nano` (easiest to learn)
- **Windows**: Use Notepad++ or VS Code (familiar interfaces)
- **All platforms**: VS Code with `--wait` is the modern standard

### Configure Credential Storage

**Purpose**: Saves your username and password so you don't have to type them for every push/pull operation.

**The Problem**: By default, Git asks for credentials every time you interact with a remote repository. This becomes tedious when you push/pull frequently.

#### Linux
```bash
# Cache credentials for 15 minutes (default)
git config --global credential.helper cache

# Cache for custom duration (in seconds, e.g., 1 hour below)
git config --global credential.helper 'cache --timeout=3600'
```

**How It Works**: Stores credentials in memory for the specified duration. After timeout, you'll need to enter them again.

**Security Note**: Credentials are stored in memory, not on disk, so they're more secure but temporary.

#### macOS
```bash
# Store in macOS Keychain
git config --global credential.helper osxkeychain
```

**How It Works**: Uses macOS's built-in Keychain to securely store credentials permanently.

**Why This is Best for macOS**: Keychain is encrypted and integrated with the OS, providing maximum security and convenience.

#### Windows
```bash
# Use Windows Credential Manager
git config --global credential.helper manager-core
```

**How It Works**: Integrates with Windows Credential Manager to securely store credentials permanently.

**Alternative**: The older `wincred` helper also works but `manager-core` is the modern, recommended option.

**Important for GitHub Users**: GitHub deprecated password authentication in 2021. You must use:
- **Personal Access Tokens (PAT)**: Generated in GitHub settings, used instead of password
- **SSH Keys**: More secure alternative (see GitHub docs for setup)

### View All Configuration

**Purpose**: View your current Git configuration to verify settings or troubleshoot issues.

```bash
# View all settings
git config --list

# View specific setting
git config user.name

# View with origin info (shows which config file each setting comes from)
git config --list --show-origin
```

**When To Use This**:
- Verifying your identity is set correctly
- Checking which editor is configured
- Troubleshooting why Git behaves unexpectedly
- Documenting your setup for team consistency

### Configuration Levels

**Purpose**: Git has three configuration levels that override each other in order of specificity.

```bash
# System-wide (affects all users on the machine)
git config --system
# Location: /etc/gitconfig

# User-level (affects current user - most common)
git config --global
# Location: ~/.gitconfig or ~/.config/git/config

# Repository-level (affects current repo only - highest priority)
git config --local
# Location: .git/config in the repository
```

**Priority Order**: Local > Global > System (local settings override global, global overrides system)

**Use Cases**:
- **System**: Rarely used, requires admin rights, for shared machines
- **Global**: Your default - use for your personal settings
- **Local**: Override global settings for specific projects (e.g., using work email for work repos)

**Example**: Set work email for a specific project:
```bash
cd work-project
git config --local user.email "you@company.com"
```

---

## Git Basics

**Understanding the Foundation**

Git is fundamentally different from older version control systems like SVN. Instead of storing deltas (differences between versions), Git takes snapshots of your entire project at each commit. This makes operations fast and enables powerful features like branching and merging.

### Core Concepts

**Repository (Repo)**:
- A `.git` directory containing your project's complete history
- Includes all commits, branches, and configuration
- Can exist locally (on your computer) and remotely (GitHub, GitLab, etc.)
- **Think of it as**: A database storing every version of your project

**Commit**:
- A snapshot of your entire project at a specific moment
- Contains: changes, author info, timestamp, unique SHA-1 hash
- Immutable: Once created, commits never change (this is crucial for integrity)
- **Think of it as**: A save point in a video game that you can return to

**Branch**:
- A movable pointer to a commit
- Allows parallel development without affecting other work
- Lightweight: Just a 41-byte file containing a commit hash
- **Think of it as**: An alternate timeline where you can experiment safely

**Remote**:
- A version of your repository hosted elsewhere (GitHub, GitLab, Bitbucket)
- Enables collaboration and backup
- Typically named `origin` by default
- **Think of it as**: A synchronized cloud backup of your project

**Working Directory**:
- Your actual project files as you see them in your file system
- Where you make your edits
- **Think of it as**: Your current workspace/desktop

**Staging Area (Index)**:
- An intermediate area between working directory and repository
- Lets you prepare commits by selecting which changes to include
- **Think of it as**: A shopping cart where you select items before checkout

**HEAD**:
- A pointer to your current position in the commit history
- Usually points to the latest commit on your current branch
- **Think of it as**: The "you are here" marker on a map

### Git Workflow

**The Three-Stage Workflow**:
```
Working Directory → (git add) → Staging Area → (git commit) → Local Repository → (git push) → Remote Repository
     ↑                                                                                            |
     |____________________________________________ (git pull) ___________________________________|
```

**Why This Workflow?**:

1. **Working Directory**: You edit files normally
   - Freedom to make multiple changes
   - No impact on Git until you explicitly add files

2. **Staging Area**: You select specific changes
   - **Purpose**: Group related changes into logical commits
   - **Example**: You fixed a bug and added a feature. Stage and commit them separately for clear history
   - This is Git's "killer feature" that other VCS systems lack

3. **Local Repository**: You commit to your local history
   - **Purpose**: Create save points with descriptive messages
   - Work offline, commit frequently
   - Full history available locally for speed

4. **Remote Repository**: You push to share with others
   - **Purpose**: Backup and collaboration
   - Only push when you're ready to share
   - Pull to get others' changes

### File States in Git

Git tracks files in four possible states:

- **Untracked**:
  - New files Git doesn't know about yet
  - Example: You just created `newfile.js`
  - Action needed: `git add newfile.js` to track it

- **Unmodified**:
  - Files that haven't changed since last commit
  - Git doesn't show these in `git status`
  - These are "clean" files

- **Modified**:
  - Tracked files that have changed
  - Changes exist in working directory but aren't staged
  - Example: You edited `app.js` but haven't run `git add` yet
  - Action needed: `git add` to stage changes

- **Staged**:
  - Changes selected for the next commit
  - Will be included when you run `git commit`
  - Can be unstaged if you change your mind

**Visual Representation**:
```
Untracked ──git add──> Staged ──git commit──> Unmodified
                          ↑                        |
                          |                        | (edit file)
                          |                        ↓
                          └────── Modified ────────┘
```

**Common Questions**:

**Q: Why not commit directly without staging?**
A: Staging lets you craft precise commits. You might fix multiple bugs but want separate commits for each, making history clearer.

**Q: Can I stage only part of a file?**
A: Yes! Use `git add -p` for interactive staging. Perfect when you made multiple unrelated changes in one file.

**Q: What if I want to undo a stage?**
A: Use `git restore --staged filename` (or `git reset HEAD filename` in older Git versions).

---

## Repository Setup

### Initialize a New Repository

**Purpose**: Transform a regular directory into a Git repository, enabling version control.

```bash
# Create new directory and initialize
mkdir my-project
cd my-project
git init

# Initialize with specific branch name
git init -b main
```

**What Happens**:
- Creates a `.git` directory containing all Git metadata
- Initializes the repository structure (objects, refs, config)
- No files are tracked yet - you need to explicitly add them

**When To Use**:
- Starting a new project from scratch
- Adding version control to an existing untracked project
- Creating a local repository before pushing to GitHub

**Best Practice**: Use `git init -b main` to avoid the old "master" default

**After Initialization**:
1. Create/add files: `git add .`
2. Make first commit: `git commit -m "Initial commit"`
3. Add remote: `git remote add origin <url>`
4. Push: `git push -u origin main`

### Clone an Existing Repository

**Purpose**: Download a complete copy of a remote repository to your local machine.

**HTTPS vs SSH**:
- **HTTPS**: `https://github.com/username/repo.git`
  - Easier to set up, works everywhere
  - Requires credentials for each push (unless cached)
  - Good for public repos or beginners

- **SSH**: `git@github.com:username/repo.git`
  - Requires SSH key setup (one-time configuration)
  - No password needed once set up
  - More secure, recommended for regular contributors

```bash
# Clone via HTTPS (easiest for beginners)
git clone https://github.com/username/repository.git

# Clone via SSH (best for regular use)
git clone git@github.com:username/repository.git

# Clone to specific directory (custom folder name)
git clone https://github.com/username/repository.git my-folder

# Clone specific branch only
git clone -b branch-name https://github.com/username/repository.git

# Shallow clone (saves bandwidth and disk space)
git clone --depth 1 https://github.com/username/repository.git
```

**What Clone Does**:
1. Creates a new directory with the repository name
2. Downloads all commits, branches, and tags
3. Sets up `origin` as the remote pointing to the source
4. Checks out the default branch (usually `main`)
5. Creates tracking relationship between local and remote branches

**Shallow Clone (`--depth 1`)**:
- **Purpose**: Download only recent history, not the entire project history
- **Benefits**: Faster clone, uses less disk space
- **Drawbacks**: Can't view old commits, limited Git operations
- **Use Cases**: CI/CD pipelines, quick code review, deploy scripts
- **Not Recommended For**: Active development work

**Common Issue**: If you clone via HTTPS but want to use SSH later, change the remote URL:
```bash
git remote set-url origin git@github.com:username/repository.git
```

### Connect to Remote Repository

**Purpose**: Link your local repository to a remote server (GitHub, GitLab, etc.) for backup and collaboration.

```bash
# Add remote
git remote add origin https://github.com/username/repository.git

# Verify remote (shows fetch and push URLs)
git remote -v

# Change remote URL
git remote set-url origin https://github.com/username/new-repository.git

# Remove remote
git remote remove origin

# Rename remote
git remote rename origin upstream
```

**Understanding `origin`**:
- Just a conventional name for your primary remote
- You can name it anything, but `origin` is the universal standard
- When you clone a repo, Git automatically creates an `origin` remote

**Why Multiple Remotes?**:

You can have multiple remotes for different purposes:
```bash
git remote add origin https://github.com/yourname/repo.git      # Your fork
git remote add upstream https://github.com/original/repo.git    # Original project
```

**Use Case**: Contributing to open source:
1. Fork the original repo to your account
2. Clone your fork: `git clone <your-fork-url>`
3. Add original as `upstream`: `git remote add upstream <original-url>`
4. Pull updates from original: `git pull upstream main`
5. Push your changes to your fork: `git push origin feature-branch`

**Verify Remotes**:
```bash
git remote -v
# Output:
# origin    https://github.com/you/repo.git (fetch)
# origin    https://github.com/you/repo.git (push)
# upstream  https://github.com/original/repo.git (fetch)
# upstream  https://github.com/original/repo.git (push)
```

---

## Branch Management

**Why Branches Matter**

Branches are Git's killer feature. They allow you to:
- **Isolate work**: Develop features without breaking the main codebase
- **Collaborate safely**: Multiple developers work in parallel without conflicts
- **Experiment freely**: Try new ideas that you can easily discard
- **Organize releases**: Maintain multiple versions (production, staging, development)

**The Cost**: Nearly zero! Creating a branch is just writing 41 bytes to disk. This is why Git encourages frequent branching, unlike older VCS systems.

**Mental Model**: Think of branches as lightweight pointers that move forward as you commit. They don't copy your entire project - they just point to commits.

### Creating Branches

**Purpose**: Create a new line of development to isolate your work from other branches.

```bash
# Create new branch (but stay on current branch)
git branch feature-branch

# Create and switch to new branch (most common)
git checkout -b feature-branch

# Modern way (Git 2.23+, clearer intent)
git switch -c feature-branch

# Create branch from specific commit
git branch feature-branch abc1234

# Create branch from remote branch (and track it)
git checkout -b feature-branch origin/feature-branch
```

**Command Breakdown**:

**`git branch feature-branch`**:
- **What it does**: Creates a branch but doesn't switch to it
- **When to use**: When you want to mark a point for later but continue current work
- **Example scenario**: "I want to remember this commit for a potential fix, but I'm not working on it now"

**`git checkout -b feature-branch`** (Most Common):
- **What it does**: Creates and immediately switches to the new branch
- **Why `-b` flag**: Stands for "branch" - tells checkout to create before switching
- **When to use**: Starting new feature or bug fix work
- **Example**: `git checkout -b feature/user-authentication`

**`git switch -c feature-branch`** (Modern Approach):
- **What it does**: Same as `checkout -b` but with clearer intent
- **Why it exists**: `git checkout` does many things (switch branches, restore files, etc.), which confuses beginners
- **Git 2.23+**: Introduced `switch` and `restore` to split `checkout`'s responsibilities
- **Best practice**: Use this for new projects; `checkout` for compatibility

**`git branch feature-branch abc1234`**:
- **What it does**: Creates branch pointing to a specific commit
- **When to use**:
  - Recovery: "I need to create a branch from where I was 3 commits ago"
  - Bug hunting: "I want to test the code from this specific commit"
- **Example**: `git branch hotfix 4a3b2c1` (creates hotfix branch at commit 4a3b2c1)

**Branch Naming Conventions**:
```bash
# Feature branches
git checkout -b feature/user-login
git checkout -b feat/shopping-cart

# Bug fixes
git checkout -b bugfix/header-alignment
git checkout -b fix/memory-leak

# Hotfixes (urgent production fixes)
git checkout -b hotfix/security-patch

# Experimental work
git checkout -b experiment/new-architecture
```

**Why Good Names Matter**:
- Team members instantly understand the purpose
- Easier to track in pull requests and project management tools
- Some workflows (like Git Flow) rely on naming patterns

### Switching Branches

**Purpose**: Move between different branches to work on different tasks.

```bash
# Switch to existing branch (old way)
git checkout branch-name

# Switch to existing branch (new way - Git 2.23+)
git switch branch-name

# Switch to previous branch (super useful!)
git checkout -
# or
git switch -

# Create and switch if branch doesn't exist (force create)
git switch -C branch-name
```

**What Happens When Switching**:
1. Git updates your working directory to match the target branch
2. HEAD moves to point to the new branch
3. Uncommitted changes come with you (if they don't conflict)

**Important Safety Feature**:
Git won't let you switch if you have uncommitted changes that would be overwritten. You must either:
- **Commit** your changes first
- **Stash** them: `git stash` (save for later)
- **Discard** them: `git restore .` (careful!)

**The Previous Branch Shortcut (`-`)**:
```bash
git switch main            # Switch to main
git switch feature-branch  # Switch to feature
git switch -               # Back to main
git switch -               # Back to feature
```

**When to use**: Perfect for quickly alternating between two branches, like switching between feature work and fixing a bug on main.

**Common Mistake**:
```bash
git switch non-existent-branch
# Error: pathspec 'non-existent-branch' did not match any file(s) known to git
```
**Solution**: Use `-c` to create: `git switch -c non-existent-branch`

### Viewing Branches

**Purpose**: List and inspect branches to understand your repository's structure.

```bash
# List local branches (shows current branch with *)
git branch

# List all branches (local and remote)
git branch -a

# List remote branches only
git branch -r

# List branches with last commit info
git branch -v

# List branches with tracking info (shows upstream)
git branch -vv

# List merged branches (safe to delete)
git branch --merged

# List unmerged branches (still contain unique work)
git branch --no-merged

# Show current branch name only
git branch --show-current
```

**Command Details**:

**`git branch`** (Basic List):
```
  feature/login
* main
  bugfix/header
```
- **`*` indicates** your current branch (where HEAD points)
- **Use case**: Quick check of available local branches

**`git branch -a`** (All Branches):
```
* main
  feature/login
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
  remotes/origin/develop
```
- **Red entries**: Remote branches
- **White entries**: Local branches
- **Use case**: See everything available in the repository

**`git branch -v`** (Verbose):
```
  feature/login   3a2b4c1 Add login form
* main            7f8e9d0 Update README
  bugfix/header   1c2d3e4 Fix header alignment
```
- Shows the last commit hash and message for each branch
- **Use case**: Quick overview of what's happening on each branch

**`git branch -vv`** (Very Verbose):
```
  feature/login   3a2b4c1 [origin/feature/login: ahead 2] Add login form
* main            7f8e9d0 [origin/main] Update README
  bugfix/header   1c2d3e4 Fix header alignment
```
- **[origin/main]**: Branch tracks origin/main and is up to date
- **[ahead 2]**: You have 2 commits not pushed yet
- **[behind 3]**: Remote has 3 commits you don't have
- **[ahead 1, behind 2]**: Both local and remote have unique commits
- **Use case**: Before pushing, check your sync status with remote

**`git branch --merged`**:
```
  feature/completed-feature
  bugfix/old-fix
* main
```
- **What it shows**: Branches fully merged into current branch
- **Why it matters**: These branches are safe to delete - their changes are already in your current branch
- **Use case**: Cleanup old branches: `git branch -d $(git branch --merged)`

**`git branch --no-merged`**:
```
  feature/work-in-progress
  experiment/new-idea
```
- **What it shows**: Branches with commits not in your current branch
- **Why it matters**: Deleting these would lose work
- **Use case**: See what work is still pending integration

**`git branch --show-current`** (Git 2.22+):
```
main
```
- **Output**: Just the branch name (perfect for scripts)
- **Use case**: Shell scripts, automation, CI/CD pipelines
- **Example**: `echo "Current branch: $(git branch --show-current)"`

### Renaming Branches

**Purpose**: Change a branch name to be more descriptive or fix typos.

```bash
# Rename current branch
git branch -m new-branch-name

# Rename different branch (while on another branch)
git branch -m old-name new-name

# Rename and sync with remote
git branch -m old-name new-name
git push origin -u new-name
git push origin --delete old-name
```

**Flag Explanation**:
- **`-m`**: "move" (rename) - safe, prevents overwriting
- **`-M`**: Force move - overwrites existing branch (dangerous!)

**Common Scenarios**:

**Scenario 1: Fix typo in current branch**
```bash
# You're on "feture-login" (typo!)
git branch -m feature-login
```

**Scenario 2: Branch name doesn't follow team convention**
```bash
# Change "login" to "feature/login"
git branch -m login feature/login
```

**Scenario 3: Already pushed to remote**
```bash
# 1. Rename locally
git branch -m old-name new-name

# 2. Push new branch and set upstream
git push origin -u new-name

# 3. Delete old branch from remote
git push origin --delete old-name
```

**Team Consideration**: If others are working on this branch, coordinate the rename! They'll need to:
```bash
git branch -m old-name new-name
git fetch origin
git branch -u origin/new-name new-name
```

### Deleting Branches

**Purpose**: Remove branches you no longer need to keep your repository clean.

```bash
# Delete local branch (safe - prevents deletion if unmerged)
git branch -d branch-name

# Force delete local branch (careful!)
git branch -D branch-name

# Delete remote branch (two methods)
git push origin --delete branch-name
# or (older syntax)
git push origin :branch-name

# Delete multiple branches at once
git branch -d branch1 branch2 branch3

# Delete all merged branches (cleanup after release)
git branch --merged | grep -v "^*" | grep -v "main" | xargs git branch -d
```

**Flag Explanation**:
- **`-d`**: "delete" - safe deletion, prevents loss of unmerged work
- **`-D`**: Force delete - deletes even if branch has unmerged changes (DANGEROUS!)

**When Git Prevents Deletion (`-d`)**:
```bash
git branch -d feature-branch
# Error: The branch 'feature-branch' is not fully merged.
```

**What this means**: The branch has commits that aren't in your current branch. Deleting would lose work.

**Your options**:
1. **Merge first**: `git merge feature-branch` then delete
2. **Force delete if you're sure**: `git branch -D feature-branch`
3. **Keep the branch**: Maybe you still need those changes

**Safe Workflow**:
```bash
# 1. List merged branches (safe to delete)
git branch --merged

# 2. Delete merged feature branch
git checkout main
git branch -d feature/completed-feature

# 3. Delete from remote too
git push origin --delete feature/completed-feature
```

**Deleting Remote Branches**:

**Modern syntax** (clear and explicit):
```bash
git push origin --delete feature-branch
```

**Legacy syntax** (works but confusing):
```bash
git push origin :feature-branch
```
- **Meaning**: "Push nothing to feature-branch" (which deletes it)
- **Mnemonics**: The `:` means "replace remote branch with nothing"

**Important**: Deleting a remote branch doesn't delete your local copy. Clean up local tracking branches:
```bash
# Remove stale remote-tracking branches
git fetch --prune
# or
git remote prune origin
```

**Bulk Cleanup Example**:
```bash
# Delete all local branches already merged into main
git checkout main
git branch --merged | grep -v "^\*" | grep -v "main" | xargs -n 1 git branch -d
```
- **`grep -v "^\*"`**: Excludes current branch
- **`grep -v "main"`**: Protects main branch
- **`xargs -n 1`**: Runs git branch -d for each branch name

### Tracking Remote Branches

**Purpose**: Link local branches with remote branches for simplified push/pull operations.

**What is Tracking?**
- Creates a relationship between a local branch and a remote branch
- Allows you to use `git push` and `git pull` without specifying branch names
- Shows ahead/behind status with `git status`

```bash
# Push and set upstream (most common - first push)
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

**The `-u` Flag** (upstream):
```bash
# First time pushing a new branch
git push -u origin feature-branch
```

**What `-u` does**:
1. Pushes your local branch to remote
2. Creates the remote branch if it doesn't exist
3. Sets up tracking relationship

**Benefits of tracking**:
```bash
# Without tracking:
git push origin feature-branch    # Must specify every time
git pull origin feature-branch

# With tracking (after git push -u):
git push                          # Automatic!
git pull
```

**Check Tracking Status**:
```bash
git branch -vv
```
Output:
```
* feature-login  3a2b4c1 [origin/feature-login: ahead 2] Add login form
  main           7f8e9d0 [origin/main] Update README
```

**Setting Upstream Later**:
```bash
# You pushed without -u
git push origin feature-branch

# Now you want to track it
git branch --set-upstream-to=origin/feature-branch

# Or short form
git branch -u origin/feature-branch
```

**Different Local and Remote Names** (rare but possible):
```bash
# Push local "feature" to remote "feature-branch"
git push origin feature:feature-branch

# Set tracking
git branch -u origin/feature-branch feature
```

**Fetching vs Pulling**:

**`git fetch --all`**:
- Downloads all branches from all remotes
- Updates your remote-tracking branches (origin/main, etc.)
- **Doesn't** modify your working directory
- Safe - lets you review before merging

**`git pull`**:
- Essentially `git fetch` + `git merge`
- Downloads AND merges into your current branch
- Can cause unexpected merge conflicts

**Best Practice**: Fetch first, review, then merge:
```bash
git fetch origin
git log origin/main        # Review what's new
git merge origin/main      # Merge when ready
```

**Cleaning Up Stale Branches**:

**Problem**: Teammates delete remote branches, but you still see them locally:
```bash
git branch -r
  origin/main
  origin/feature-old    # Deleted remotely but still shows
```

**Solution**:
```bash
git fetch --prune
# or
git remote prune origin
```

**What prune does**: Removes remote-tracking branches that no longer exist on the remote server.

**Automatic pruning**:
```bash
git config --global fetch.prune true
```
Now `git fetch` always prunes automatically.

### Branch Comparison

**Purpose**: Understand differences between branches before merging or reviewing work.

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

**The Daily Development Cycle**

This is the fundamental workflow you'll use dozens of times per day:

```bash
# 1. Check status (always start here!)
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

**Step-by-Step Explanation**:

**1. `git status` - Your Safety Check**
```bash
git status
```
**Output shows**:
- Current branch
- Files modified but not staged (red)
- Files staged for commit (green)
- Untracked files
- Ahead/behind status with remote

**Why always check first?**
- Prevents accidentally committing wrong files
- Shows if you're on the correct branch
- Reveals merge conflicts before they surprise you

**2. `git add` - Staging Files**

**`git add filename.txt`** (Specific files):
- **When**: You want precise control over what's committed
- **Why**: Keeps commits focused and atomic
- **Example**: Fixed a bug in `auth.js` but also experimented in `test.js`. Only add `auth.js` for a clean commit.

**`git add .`** (Current directory):
- **What it does**: Stages all changes in current directory and subdirectories
- **Common mistake**: Accidentally staging unwanted files
- **Best practice**: Run `git status` first to see what will be added

**`git add -A`** (Everything):
- **What it does**: Stages all changes in entire repository, including deletions
- **When to use**: Major refactoring or feature completion
- **Equivalent to**: `git add --all`

**`git add *.js`** (Pattern matching):
- **What it does**: Stages all JavaScript files
- **Use case**: You changed multiple JS files for one feature
- **Works with**: `*.css`, `src/**/*.py`, etc.

**`git add -p`** (Interactive patch mode):
```bash
git add -p config.js
```
**What happens**:
- Git shows each change (hunk) in the file
- You choose: `y` (yes), `n` (no), `s` (split), `q` (quit)
- Allows staging only parts of a file

**When to use**:
- You made multiple unrelated changes in one file
- You want to split into separate commits
- Example: Fixed bug + added feature in same file - stage separately

**3. `git commit` - Creating Snapshots**

**Commit message best practices**:
```bash
# Good: Clear, describes what and why
git commit -m "Fix login timeout by increasing session duration"

# Bad: Vague, no context
git commit -m "Fix bug"

# Good: Uses imperative mood
git commit -m "Add user authentication middleware"

# Bad: Past tense
git commit -m "Added user authentication middleware"
```

**Why good commit messages matter**:
- Future you will thank present you
- Code reviewers understand your intent
- `git log` becomes useful documentation
- Easy to find and revert specific changes

**Commit message format** (conventional commits):
```bash
type(scope): subject

body (optional)

footer (optional)
```

Examples:
```bash
git commit -m "feat(auth): add OAuth2 login support"
git commit -m "fix(api): resolve race condition in user endpoint"
git commit -m "docs(readme): update installation instructions"
git commit -m "refactor(db): optimize query performance"
```

**4. `git push` - Sharing Your Work**

```bash
# First push of new branch
git push -u origin feature-branch

# Subsequent pushes (if tracking is set)
git push

# Force push (DANGEROUS - use with caution)
git push --force origin branch-name
```

**When push fails**:
```bash
git push
# Error: Updates were rejected because the remote contains work that you do not have locally
```

**What happened**: Someone else pushed to the same branch.

**Solution**:
```bash
git pull --rebase origin feature-branch  # Get their changes
git push                                  # Now push yours
```

**The Shortcut: `git commit -am`**
```bash
git commit -am "Quick fix"
```
**What it does**: Stages all **tracked** files and commits in one command
**Limitation**: Doesn't add new (untracked) files
**When to use**: Quick fixes to existing files
**When NOT to use**: When you have new files or want selective staging

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

**Purpose**: Temporarily save uncommitted work without committing, so you can switch contexts.

**Real-World Scenario**:
You're halfway through a feature when your manager asks you to fix an urgent bug on the main branch. You can't commit your incomplete feature, and you can't switch branches with uncommitted changes. Solution: stash!

```bash
# Stash current changes (tracked files only)
git stash

# Stash with descriptive message (recommended!)
git stash save "WIP: user profile page layout"

# List all stashes
git stash list

# Apply most recent stash (keeps stash in list)
git stash apply

# Apply and remove most recent stash
git stash pop

# Apply specific stash
git stash apply stash@{2}

# Show stash contents (summary)
git stash show

# Show detailed stash diff
git stash show -p

# Drop specific stash (delete it)
git stash drop stash@{0}

# Clear all stashes (careful!)
git stash clear

# Stash including untracked files
git stash -u

# Stash everything including untracked and ignored files
git stash -a
```

**Complete Workflow Example**:

```bash
# You're working on feature-branch
git status
# Modified: feature.js, styles.css

# Urgent bug fix needed!
git stash save "WIP: new feature half done"

# Switch to main and fix bug
git switch main
git checkout -b hotfix/critical-bug
# ... fix bug ...
git commit -am "Fix critical security bug"
git push -u origin hotfix/critical-bug

# Back to your feature
git switch feature-branch
git stash pop                    # Restore your work
# Continue working...
```

**`git stash` vs `git stash save "message"`**:
```bash
# Basic stash (no message)
git stash

# With message (much better!)
git stash save "WIP: fixing login validation"
```
**Why messages matter**: When you have multiple stashes, messages help you remember what's what.

**Viewing Stashes**:
```bash
git stash list
```
Output:
```
stash@{0}: WIP on feature-branch: Add user profile
stash@{1}: WIP on main: Fix header styling
stash@{2}: WIP on develop: Refactor API calls
```

**`stash@{n}` Notation**:
- `stash@{0}` = most recent stash
- `stash@{1}` = second most recent
- Numbers increment as you add stashes

**`git stash apply` vs `git stash pop`**:

**`git stash pop`** (most common):
- Applies stashed changes
- Removes stash from the list
- **Use when**: You're done with the stash and want to clean up

**`git stash apply`**:
- Applies stashed changes
- **Keeps** stash in the list
- **Use when**: You want to apply the same stash to multiple branches
- Must manually delete later: `git stash drop stash@{0}`

**Stashing Untracked Files**:

**Problem**: By default, `git stash` only stashes tracked (previously committed) files.

```bash
# You have: modified file.js (tracked) + new_file.js (untracked)
git stash
# Only file.js is stashed! new_file.js remains
```

**Solution**:
```bash
# Stash tracked + untracked
git stash -u

# or
git stash --include-untracked
```

**Stash Everything** (including .gitignore files):
```bash
git stash -a
# or
git stash --all
```

**Viewing Stash Contents**:
```bash
# Summary
git stash show stash@{0}
```
Output:
```
 file.js | 15 ++++++---------
 1 file changed, 6 insertions(+), 9 deletions(-)
```

**Detailed diff**:
```bash
git stash show -p stash@{0}
```
Shows full diff with line-by-line changes.

**Common Mistakes**:

**Mistake 1**: Stashing then forgetting about it
```bash
# Days later...
git stash list  # Oh no, I have 10 stashes!
```
**Prevention**: Use descriptive messages and clean up regularly

**Mistake 2**: Popping stash with conflicts
```bash
git stash pop
# CONFLICT (content): Merge conflict in file.js
```
**What happened**: Files changed since you stashed
**Solution**: Resolve conflicts like a merge, or `git stash drop` and start over

**Mistake 3**: Stashing while on wrong branch
```bash
# On feature-branch but wanted to stash for main
git stash
git switch main
git stash pop    # Applying feature changes to main - oops!
```
**Prevention**: Always check `git status` before stashing

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
