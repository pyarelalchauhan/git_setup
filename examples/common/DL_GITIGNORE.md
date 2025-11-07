# Universal .gitignore for Deep Learning Projects

A comprehensive `.gitignore` template for Deep Learning projects that works across Linux, Windows, and macOS.

## Quick Start

Copy this entire file to create your `.gitignore`:

```bash
# In your project directory
curl https://raw.githubusercontent.com/your-repo/git_setup/main/examples/common/DL_GITIGNORE.md > .gitignore
```

Or manually create `.gitignore` and paste the template below.

---

## Complete .gitignore Template

```gitignore
# ============================================================================
# DEEP LEARNING PROJECT .GITIGNORE
# ============================================================================
# This file covers Python, Deep Learning frameworks, data files, models,
# and common development tools across Linux, Windows, and macOS
# ============================================================================

# ============================================================================
# PYTHON
# ============================================================================

# Byte-compiled / optimized / DLL files
__pycache__/
*.py[cod]
*$py.class

# C extensions
*.so

# Distribution / packaging
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
share/python-wheels/
*.egg-info/
.installed.cfg
*.egg
MANIFEST

# PyInstaller
*.manifest
*.spec

# Installer logs
pip-log.txt
pip-delete-this-directory.txt

# Unit test / coverage reports
htmlcov/
.tox/
.nox/
.coverage
.coverage.*
.cache
nosetests.xml
coverage.xml
*.cover
*.py,cover
.hypothesis/
.pytest_cache/
cover/

# Translations
*.mo
*.pot

# Django stuff:
*.log
local_settings.py
db.sqlite3
db.sqlite3-journal

# Flask stuff:
instance/
.webassets-cache

# Scrapy stuff:
.scrapy

# Sphinx documentation
docs/_build/

# PyBuilder
.pybuilder/
target/

# Jupyter Notebook
.ipynb_checkpoints
*.ipynb_checkpoints/

# IPython
profile_default/
ipython_config.py

# pyenv
#   For a library or package, you might want to ignore these files since the code is
#   intended to run in multiple environments; otherwise, check them in:
# .python-version

# pipenv
#   According to pypa/pipenv#598, it is recommended to include Pipfile.lock in version control.
#   However, in case of collaboration, if having platform-specific dependencies or dependencies
#   having no cross-platform support, pipenv may install dependencies that don't work, or not
#   install all needed dependencies.
#Pipfile.lock

# poetry
#   Similar to Pipfile.lock, it is generally recommended to include poetry.lock in version control.
#   This is especially recommended for binary packages to ensure reproducibility, and is more
#   commonly ignored for libraries.
#   https://python-poetry.org/docs/basic-usage/#commit-your-poetrylock-file-to-version-control
#poetry.lock

# pdm
#   Similar to Pipfile.lock, it is generally recommended to include pdm.lock in version control.
#pdm.lock
#   pdm stores project-wide configurations in .pdm.toml, but it is recommended to not include it
#   in version control.
#   https://pdm.fming.dev/#use-with-ide
.pdm.toml

# PEP 582; used by e.g. github.com/David-OConnor/pyflow and github.com/pdm-project/pdm
__pypackages__/

# Celery stuff
celerybeat-schedule
celerybeat.pid

# SageMath parsed files
*.sage.py

# Environments
.env
.venv
env/
venv/
ENV/
env.bak/
venv.bak/
dl_env/
.conda/

# Spyder project settings
.spyderproject
.spyproject

# Rope project settings
.ropeproject

# mkdocs documentation
/site

# mypy
.mypy_cache/
.dmypy.json
dmypy.json

# Pyre type checker
.pyre/

# pytype static type analyzer
.pytype/

# Cython debug symbols
cython_debug/

# ============================================================================
# DEEP LEARNING FRAMEWORKS & TOOLS
# ============================================================================

# PyTorch
*.pth
*.pt
*.bin
checkpoints/
lightning_logs/

# TensorFlow / Keras
*.h5
*.hdf5
*.pb
*.ckpt
*.ckpt.data-*
*.ckpt.index
*.ckpt.meta
saved_model/
checkpoint

# ONNX
*.onnx

# TensorRT
*.engine
*.plan

# Model Files (General)
models/checkpoints/*
models/saved_models/*.pth
models/saved_models/*.h5
models/saved_models/*.pb
models/saved_models/*.ckpt
models/saved_models/*.pkl
models/saved_models/*.joblib
!models/checkpoints/.gitkeep
!models/saved_models/.gitkeep

# Weights & Biases
wandb/
*.wandb

# MLflow
mlruns/
mlartifacts/

# TensorBoard
tensorboard_logs/
runs/
events.out.tfevents.*

# DVC (Data Version Control)
.dvc/cache/
.dvc/tmp/

# Sacred
sacred_logs/

# Optuna
optuna_study/

# Ray Tune
ray_results/

# ============================================================================
# DATA FILES
# ============================================================================

# Data directories (keep structure but not content)
data/raw/*
data/processed/*
data/external/*
data/interim/*
!data/raw/.gitkeep
!data/processed/.gitkeep
!data/external/.gitkeep
!data/interim/.gitkeep

# Common data file formats
*.csv
*.tsv
*.dat
*.npy
*.npz
*.pkl
*.pickle
*.h5
*.hdf5
*.parquet
*.feather
*.arrow

# Image files (if large datasets)
# Uncomment if you don't want to track images
#*.jpg
#*.jpeg
#*.png
#*.gif
#*.bmp
#*.tiff
#*.svg

# Video files
*.mp4
*.avi
*.mov
*.mkv
*.flv
*.wmv
*.webm

# Audio files
*.wav
*.mp3
*.flac
*.aac
*.ogg
*.m4a

# Compressed files
*.zip
*.tar
*.gz
*.bz2
*.xz
*.7z
*.rar

# Database files
*.db
*.sqlite
*.sqlite3

# ============================================================================
# LOGS & OUTPUTS
# ============================================================================

# Log files
logs/*
*.log
!logs/.gitkeep

# Output directories
outputs/*
results/predictions/*
results/submissions/*
!outputs/.gitkeep
!results/predictions/.gitkeep
!results/submissions/.gitkeep

# Temporary files
*.tmp
*.temp
*.bak
*.swp
*.swo
*~

# Cache
*.cache
cache/
.cache/

# ============================================================================
# JUPYTER NOTEBOOKS
# ============================================================================

# Notebook checkpoints
.ipynb_checkpoints/
*/.ipynb_checkpoints/*

# Jupyter Notebook metadata
.jupyter/

# VS Code Jupyter extension
.vscode/settings.json

# ============================================================================
# IDE & EDITORS
# ============================================================================

# Visual Studio Code
.vscode/
.history/
*.code-workspace

# PyCharm
.idea/
*.iml
*.iws
*.ipr

# Sublime Text
*.sublime-project
*.sublime-workspace
*.tmlanguage.cache
*.tmPreferences.cache
*.stTheme.cache

# Vim
[._]*.s[a-v][a-z]
[._]*.sw[a-p]
[._]s[a-rt-v][a-z]
[._]ss[a-gi-z]
[._]sw[a-p]
Session.vim
Sessionx.vim
.netrwhist
tags

# Emacs
*~
\#*\#
/.emacs.desktop
/.emacs.desktop.lock
*.elc
auto-save-list
tramp
.\#*

# Eclipse
.project
.classpath
.c9/
*.launch
.settings/
*.pydevproject

# Spyder
.spyderproject
.spyproject

# ============================================================================
# OPERATING SYSTEMS
# ============================================================================

# macOS
.DS_Store
.AppleDouble
.LSOverride
._*
.DocumentRevisions-V100
.fseventsd
.Spotlight-V100
.TemporaryItems
.Trashes
.VolumeIcon.icns
.com.apple.timemachine.donotpresent
.AppleDB
.AppleDesktop
Network Trash Folder
Temporary Items
.apdisk

# Windows
Thumbs.db
Thumbs.db:encryptable
ehthumbs.db
ehthumbs_vista.db
*.stackdump
[Dd]esktop.ini
$RECYCLE.BIN/
*.cab
*.msi
*.msix
*.msm
*.msp
*.lnk

# Linux
.directory
.Trash-*
.nfs*

# ============================================================================
# VERSION CONTROL
# ============================================================================

# Git
*.orig
*.patch
*.diff

# Mercurial
.hg/
.hgignore
.hgsigs
.hgsub
.hgsubstate
.hgtags

# SVN
.svn/

# ============================================================================
# CLOUD & CONTAINERS
# ============================================================================

# Docker
.dockerignore
docker-compose.override.yml

# Kubernetes
*.kubeconfig

# AWS
.aws/

# Google Cloud
.gcloud/
*-credentials.json
application_default_credentials.json

# Azure
.azure/

# ============================================================================
# CI/CD
# ============================================================================

# GitHub Actions
# (Keep .github/workflows/ if you want to track CI/CD)

# Travis CI
.travis.yml

# CircleCI
.circleci/config.yml

# GitLab CI
# .gitlab-ci.yml

# ============================================================================
# MISCELLANEOUS
# ============================================================================

# Credentials & Secrets
*.key
*.pem
*.p12
*.pfx
*.keystore
*.jks
secrets.yml
.secrets/
credentials.json
.credentials

# Configuration (keep templates, ignore local configs)
config.local.yml
config.local.json
.env.local
.env.*.local

# Package manager lock files (decide based on team preference)
# package-lock.json
# yarn.lock
# Pipfile.lock
# poetry.lock

# Node modules (if using JavaScript tools)
node_modules/

# R
.Rhistory
.RData
.Rproj.user

# MATLAB
*.asv
*.m~

# ============================================================================
# PROJECT-SPECIFIC
# ============================================================================
# Add any project-specific files or patterns below
# Examples:
# my_custom_data/
# specific_model_*.pth
# experiment_results_20*.txt

```

---

## Explanation of Key Sections

### Python Section
Excludes all Python-generated files:
- `__pycache__/`: Compiled Python files
- `*.pyc`, `*.pyo`: Bytecode files
- `venv/`, `env/`: Virtual environments
- `.pytest_cache/`: Test cache

### Deep Learning Frameworks
Excludes large model files and training artifacts:
- `*.pth`, `*.pt`: PyTorch models
- `*.h5`, `*.hdf5`: Keras/TensorFlow models
- `*.ckpt`: TensorFlow checkpoints
- `wandb/`: Weights & Biases logs
- `runs/`: TensorBoard logs

### Data Files
Excludes data but preserves directory structure:
- `data/raw/*`: But keeps `data/raw/.gitkeep`
- `*.csv`, `*.npy`, `*.pkl`: Common data formats
- `*.mp4`, `*.jpg`: Large media files

### IDE & Editors
Excludes IDE-specific settings:
- `.vscode/`: VS Code settings
- `.idea/`: PyCharm settings
- Vim, Emacs swap files

### Operating Systems
Platform-specific files to ignore:
- `.DS_Store`: macOS Finder metadata
- `Thumbs.db`: Windows thumbnail cache
- `.directory`: Linux file manager metadata

---

## Customization Guide

### 1. Keep Small Sample Data

If you want to track small sample datasets:

```gitignore
# Override data exclusion for small samples
!data/raw/sample_tiny.csv
!data/raw/test_images/*.jpg
```

### 2. Track Specific Model Files

If you want to track final models (< 100MB):

```gitignore
# Track final production model
!models/saved_models/final_model_v1.pth
```

### 3. Project-Specific Additions

Add at the end:

```gitignore
# Project-specific
experimental_code/
old_experiments/
scratch/
notes.txt
TODO.md
```

### 4. Team Configurations

If sharing IDE settings with team:

```gitignore
# Keep .vscode settings for team
!.vscode/
!.vscode/settings.json
!.vscode/launch.json

# But ignore personal workspace
.vscode/*.code-workspace
```

---

## Usage Tips

### Checking What's Ignored

```bash
# Check if a file would be ignored
git check-ignore -v path/to/file

# List all ignored files
git status --ignored

# List all ignored files in directory
git ls-files --others --ignored --exclude-standard
```

### Adding Exceptions

Use `!` to exclude from ignoring:

```bash
# Ignore all .txt files
*.txt

# Except README.txt
!README.txt
```

### Global .gitignore

For system-wide ignores:

```bash
# Linux/macOS
echo ".DS_Store" >> ~/.gitignore_global
git config --global core.excludesfile ~/.gitignore_global

# Windows
echo "Thumbs.db" >> %USERPROFILE%\.gitignore_global
git config --global core.excludesfile %USERPROFILE%\.gitignore_global
```

---

## Pre-commit Hooks

Auto-clean notebooks before committing:

```bash
# Install nbstripout
pip install nbstripout

# Setup
nbstripout --install

# Now notebooks are automatically cleaned on commit
```

---

## Related Tools

### Git LFS
For tracking large files that must be in Git:
```bash
git lfs track "*.pth"
git lfs track "models/final_model.h5"
```

### DVC
For versioning data separately:
```bash
dvc add data/raw/dataset.zip
git add data/raw/dataset.zip.dvc
```

---

## Common Patterns by Project Type

### Computer Vision Project
```gitignore
# Additional for CV
datasets/
*.jpg
*.png
*.tif
annotations/
```

### NLP Project
```gitignore
# Additional for NLP
corpora/
*.txt
*.jsonl
embeddings/
*.vec
*.bin
```

### Reinforcement Learning
```gitignore
# Additional for RL
replays/
*.replay
videos/
*.mp4
gym_logs/
```

---

## Troubleshooting

### File Already Tracked

If a file is already tracked but now in .gitignore:

```bash
# Remove from Git but keep locally
git rm --cached path/to/file

# Remove directory recursively
git rm -r --cached path/to/directory

# Commit the removal
git commit -m "Remove tracked files now in .gitignore"
```

### Accidentally Committed Large File

```bash
# Remove from last commit
git rm --cached large_file.pth
git commit --amend --no-edit

# If already pushed (dangerous!)
git push --force origin branch-name

# Better: Use BFG Repo-Cleaner
# https://rtyley.github.io/bfg-repo-cleaner/
```

---

## Template Downloads

### Download via curl
```bash
curl -O https://raw.githubusercontent.com/github/gitignore/main/Python.gitignore
```

### GitHub's Official Templates
- Python: https://github.com/github/gitignore/blob/main/Python.gitignore
- TensorFlow: https://github.com/github/gitignore/blob/main/community/TensorFlow.gitignore
- PyTorch: https://github.com/github/gitignore/blob/main/community/PyTorch.gitignore

---

## Quick Reference

### Most Important for DL Projects

```gitignore
# Essential for Deep Learning
__pycache__/
*.py[cod]
venv/
data/
models/*.pth
models/*.h5
logs/
wandb/
.DS_Store
Thumbs.db
.vscode/
.idea/
```

### Minimal .gitignore

If you prefer minimal:

```gitignore
__pycache__/
venv/
data/
models/*.pth
models/*.h5
logs/
.DS_Store
```

---

**Remember**: It's better to ignore too much than too little. You can always force-add specific files with `git add -f file`.
