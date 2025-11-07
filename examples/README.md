# Deep Learning Project Examples

Platform-specific guides for setting up and managing Deep Learning projects with Git.

## Overview

This directory contains comprehensive, platform-specific tutorials showing how to use Git for Deep Learning projects on different operating systems. Each guide provides real-world workflows, commands, and best practices tailored to the specific platform.

## Available Guides

### 📁 [Linux](linux/DL_PROJECT_SETUP.md)
Complete guide for Ubuntu, Debian, Fedora, Arch Linux, and other Linux distributions.

**Key Topics:**
- Installing Git and Python via package managers
- Creating virtual environments with venv
- PyTorch/TensorFlow installation (CPU/CUDA)
- Project structure for DL projects
- Daily development workflow
- Managing experiments with branches
- Handling large files (.gitignore, Git LFS, DVC)

**Best for:** Ubuntu, Debian, Fedora, Arch, RHEL, CentOS

### 📁 [Windows](windows/DL_PROJECT_SETUP.md)
Complete guide for Windows 10/11 with PowerShell, Command Prompt, and Git Bash.

**Key Topics:**
- Installing Git for Windows
- PowerShell vs Command Prompt vs Git Bash
- Virtual environment setup and activation
- PyTorch/TensorFlow with CUDA support
- Path handling (forward slashes vs backslashes)
- Windows-specific issues (long paths, execution policies)
- GPU setup with NVIDIA CUDA

**Best for:** Windows 10, Windows 11

### 📁 [macOS](macos/DL_PROJECT_SETUP.md)
Complete guide for macOS with special focus on Apple Silicon (M1/M2/M3).

**Key Topics:**
- Installing Git via Homebrew/Xcode
- Python setup with Homebrew
- Apple Silicon optimizations (MPS/Metal)
- SSH key setup with macOS Keychain
- .DS_Store and macOS-specific configurations
- Performance monitoring with Activity Monitor
- zsh shell configuration

**Best for:** macOS Ventura, Sonoma (Intel and Apple Silicon)

---

## Quick Platform Comparison

| Feature | Linux | Windows | macOS |
|---------|-------|---------|-------|
| **Package Manager** | apt/dnf/pacman | winget/choco | Homebrew |
| **Virtual Env** | venv | venv | venv |
| **GPU Support** | NVIDIA CUDA | NVIDIA CUDA | Apple Metal (M1/M2/M3) |
| **Shell** | bash/zsh | PowerShell/CMD/Git Bash | zsh |
| **Path Separator** | / | \\ (or /) | / |
| **Git Credential** | cache | manager-core | osxkeychain |
| **Line Endings** | LF | CRLF→LF | LF |

---

## Common Deep Learning Project Structure

All guides use this recommended structure:

```
project-name/
├── data/                   # Data files (not tracked in Git)
│   ├── raw/               # Original, immutable data
│   ├── processed/         # Cleaned, transformed data
│   └── external/          # External datasets
├── models/                # Model checkpoints and saved models
│   ├── checkpoints/       # Training checkpoints
│   └── saved_models/      # Final trained models
├── notebooks/             # Jupyter notebooks for exploration
├── src/                   # Source code (tracked in Git)
│   ├── data/             # Data loading and processing
│   ├── models/           # Model architectures
│   ├── training/         # Training scripts
│   └── utils/            # Utility functions
├── configs/              # Configuration files (YAML, JSON)
├── logs/                 # Training logs (not tracked)
├── results/              # Results and visualizations
│   ├── figures/          # Plots and graphs
│   └── predictions/      # Model predictions
├── requirements.txt      # Python dependencies
├── .gitignore           # Git ignore rules
└── README.md            # Project documentation
```

---

## Universal .gitignore for DL Projects

See [common/DL_GITIGNORE.md](common/DL_GITIGNORE.md) for a comprehensive `.gitignore` template that works across all platforms.

**Key exclusions:**
- ✅ Python cache files (`__pycache__`, `*.pyc`)
- ✅ Virtual environments (`venv/`, `env/`)
- ✅ Large data files (`data/raw/*`, `data/processed/*`)
- ✅ Model checkpoints (`*.pth`, `*.h5`, `*.ckpt`)
- ✅ Training logs (`logs/*`, `wandb/`, `tensorboard_logs/`)
- ✅ OS-specific files (`.DS_Store`, `Thumbs.db`)
- ✅ IDE configurations (`.vscode/`, `.idea/`)

---

## Common Git Workflows for DL

### 1. Daily Development Cycle

```bash
# 1. Start work session
git pull origin main
git status

# 2. Make changes
# Edit code, train models, run experiments

# 3. Stage and commit
git add src/models/new_model.py
git commit -m "feat(models): add ResNet architecture"

# 4. Push to remote
git push origin main
```

### 2. Experiment Branch Workflow

```bash
# 1. Create experiment branch
git checkout -b experiment/resnet-transfer-learning

# 2. Run experiment
python src/training/train.py --config configs/experiment_002.yaml

# 3. Track results
git add results/experiment_002_results.txt
git commit -m "exp: ResNet experiment - 92.3% val acc"

# 4. Merge if successful
git checkout main
git merge experiment/resnet-transfer-learning
git branch -d experiment/resnet-transfer-learning
```

### 3. Collaboration Workflow

```bash
# 1. Clone repository
git clone <repository-url>
cd project-name

# 2. Setup environment
python3 -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows
pip install -r requirements.txt

# 3. Create feature branch
git checkout -b feature/data-augmentation

# 4. Make changes and push
git add .
git commit -m "feat(data): implement advanced augmentation pipeline"
git push -u origin feature/data-augmentation

# 5. Create pull request (on GitHub/GitLab)
```

---

## What to Track in Git (Universal Rules)

### ✅ DO Track

- **Source code**: All `.py`, `.R`, `.jl` files
- **Configuration files**: `*.yaml`, `*.json`, `*.toml`
- **Requirements**: `requirements.txt`, `environment.yml`, `Pipfile`
- **Documentation**: `README.md`, docstrings, `*.md` files
- **Scripts**: Training scripts, data processing scripts
- **Notebooks**: Jupyter notebooks (with outputs cleared)
- **Small sample data**: Test datasets < 1MB

### ❌ DON'T Track

- **Large datasets**: Raw data, processed data (> 10MB)
- **Model files**: Trained weights, checkpoints (> 100MB)
- **Virtual environments**: `venv/`, `env/`, `dl_env/`
- **Generated files**: `__pycache__/`, `*.pyc`, `*.so`
- **Logs**: TensorBoard logs, training logs
- **IDE settings**: `.vscode/`, `.idea/` (unless shared with team)
- **OS files**: `.DS_Store`, `Thumbs.db`, `desktop.ini`

---

## Managing Large Files

### Option 1: Git LFS (Large File Storage)

```bash
# Install Git LFS
# Linux: sudo apt install git-lfs
# macOS: brew install git-lfs
# Windows: included in Git for Windows

# Initialize
git lfs install

# Track large files
git lfs track "*.pth"
git lfs track "*.h5"
git lfs track "models/saved_models/*"

# Commit .gitattributes
git add .gitattributes
git commit -m "chore: setup Git LFS"
```

### Option 2: DVC (Data Version Control)

```bash
# Install DVC
pip install dvc

# Initialize
dvc init

# Track data directory
dvc add data/raw/dataset.zip

# Commit DVC files (not actual data)
git add data/raw/dataset.zip.dvc .dvc/config
git commit -m "data: track dataset with DVC"

# Setup remote storage
dvc remote add -d storage s3://my-bucket/dvc-storage
dvc push
```

### Option 3: External Storage

- Store large files on cloud storage (S3, Google Cloud Storage, Azure Blob)
- Keep download scripts in Git
- Document where to get data in README

---

## Platform Selection Guide

### Choose Linux if:
- 🎯 You need maximum compatibility with DL libraries
- 🎯 You're deploying to Linux servers
- 🎯 You want best GPU support (NVIDIA)
- 🎯 You prefer package managers and terminal workflows

### Choose Windows if:
- 🎯 You're most comfortable with Windows
- 🎯 You need Windows-specific software
- 🎯 You have NVIDIA GPU and want CUDA support
- 🎯 You prefer GUI tools and IDEs like Visual Studio

### Choose macOS if:
- 🎯 You have a Mac (especially Apple Silicon)
- 🎯 You want Unix-like terminal with GUI conveniences
- 🎯 You're developing for iOS/macOS deployment
- 🎯 You want excellent battery life for portable work

---

## Getting Started

1. **Choose your platform**: Select Linux, Windows, or macOS guide
2. **Follow the setup**: Install Git, Python, and DL frameworks
3. **Create project structure**: Use the recommended directory layout
4. **Configure .gitignore**: Exclude data files and models
5. **Start experimenting**: Use branches for different experiments
6. **Collaborate**: Push to GitHub/GitLab and work with team

---

## Additional Resources

### Git Learning
- [Main Git Tutorial](../GIT_TUTORIAL.md) - Comprehensive Git guide
- [Pro Git Book](https://git-scm.com/book/en/v2) - Free online book
- [Learn Git Branching](https://learngitbranching.js.org/) - Interactive tutorial

### Deep Learning
- [PyTorch Tutorials](https://pytorch.org/tutorials/)
- [TensorFlow Tutorials](https://www.tensorflow.org/tutorials)
- [Fast.ai Course](https://www.fast.ai/)

### Experiment Tracking
- [DVC](https://dvc.org) - Data version control
- [MLflow](https://mlflow.org) - Experiment tracking
- [Weights & Biases](https://wandb.ai) - Visualization and tracking
- [TensorBoard](https://www.tensorflow.org/tensorboard) - Training visualization

---

## Contributing

Found an error or have a suggestion? Please:
1. Open an issue
2. Submit a pull request
3. Share feedback

---

## FAQ

**Q: Which platform is best for deep learning?**
A: Linux offers the best compatibility and GPU support, but all platforms work well. Choose based on your comfort and requirements.

**Q: Should I track model files in Git?**
A: No, use Git LFS for models or store them separately. Regular Git is not optimized for large binary files.

**Q: How do I share datasets with my team?**
A: Use DVC with cloud storage (S3, GCS) or shared network storage. Don't commit large datasets to Git.

**Q: What about Conda vs venv?**
A: Both work! Conda is popular in data science, venv is Python's built-in. These guides use venv for simplicity.

**Q: How often should I commit?**
A: Commit frequently (after each logical change). Push less frequently (after completing a feature or at end of day).

**Q: Should I commit Jupyter notebooks?**
A: Yes, but clear outputs first. Use `nbstripout` to automatically clean notebooks before committing.

---

**Happy Deep Learning! 🚀🤖**
