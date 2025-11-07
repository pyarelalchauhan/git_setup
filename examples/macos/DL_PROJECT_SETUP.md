# Deep Learning Project Setup with Git - macOS Guide

A complete guide for setting up and managing a Deep Learning project with Git on macOS systems.

## Table of Contents
1. [Environment Setup](#environment-setup)
2. [Project Initialization](#project-initialization)
3. [Git Configuration for DL](#git-configuration-for-dl)
4. [Project Structure](#project-structure)
5. [Daily Workflow](#daily-workflow)
6. [Managing Experiments](#managing-experiments)
7. [Collaboration](#collaboration)
8. [Best Practices](#best-practices)

---

## Environment Setup

### 1. Install Git

**Option 1: Using Xcode Command Line Tools (Easiest)**
```bash
xcode-select --install
```

**Option 2: Using Homebrew (Recommended)**
```bash
# Install Homebrew if not already installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Git
brew install git
```

**Option 3: Using MacPorts**
```bash
sudo port install git
```

**Verify Installation:**
```bash
git --version
```

### 2. Install Python and Deep Learning Tools

**macOS comes with Python, but install a separate version:**

**Using Homebrew (Recommended):**
```bash
# Install Python 3
brew install python@3.11

# Verify installation
python3 --version
pip3 --version

# Add to PATH (if needed)
echo 'export PATH="/usr/local/opt/python@3.11/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

**Using Official Installer:**
1. Download from: https://www.python.org/downloads/macos/
2. Run installer
3. Verify: `python3 --version`

### 3. Create Virtual Environment

**Why Virtual Environments?**
- Isolates project dependencies
- Prevents conflicts between projects
- Makes environment reproducible
- Essential for DL projects with specific library versions

```bash
# Navigate to your projects directory
cd ~/projects

# Create virtual environment
python3 -m venv dl_env

# Activate virtual environment
source dl_env/bin/activate

# Your prompt should now show (dl_env)
```

**Add activation to shell profile for convenience:**
```bash
# For zsh (default on macOS Catalina+)
echo 'alias dlenv="source ~/projects/dl_env/bin/activate"' >> ~/.zshrc
source ~/.zshrc

# For bash
echo 'alias dlenv="source ~/projects/dl_env/bin/activate"' >> ~/.bash_profile
source ~/.bash_profile
```

### 4. Install Deep Learning Frameworks

```bash
# Activate environment first
source ~/projects/dl_env/bin/activate

# For Apple Silicon Macs (M1, M2, M3)
# Install PyTorch with Metal Performance Shaders (MPS) support
pip install torch torchvision torchaudio

# For Intel Macs
# Install PyTorch (CPU version)
pip install torch torchvision torchaudio

# OR Install TensorFlow
# For Apple Silicon (M1/M2/M3)
pip install tensorflow-macos tensorflow-metal

# For Intel Macs
pip install tensorflow

# Install common DL libraries
pip install numpy pandas matplotlib scikit-learn jupyter notebook

# Install additional tools
pip install tensorboard wandb pillow opencv-python tqdm
```

**Verify Installation:**

**For Apple Silicon (M1/M2/M3):**
```bash
python3 << 'EOF'
import torch
print(f'PyTorch: {torch.__version__}')
print(f'MPS (Metal) Available: {torch.backends.mps.is_available()}')
print(f'MPS Built: {torch.backends.mps.is_built()}')
EOF
```

**For Intel Macs:**
```bash
python3 -c "import torch; print(f'PyTorch: {torch.__version__}')"
```

---

## Project Initialization

### 1. Create Project Directory

```bash
# Create project directory
mkdir -p ~/projects/image-classifier
cd ~/projects/image-classifier

# Activate virtual environment
source ~/projects/dl_env/bin/activate
```

### 2. Initialize Git Repository

```bash
# Initialize Git repo
git init -b main

# Configure identity (if not already done globally)
git config user.name "Your Name"
git config user.email "your.email@example.com"

# Configure credential storage for macOS Keychain
git config --global credential.helper osxkeychain

# Verify
git config --list | grep user
```

### 3. Create Initial Project Structure

```bash
# Create directory structure
mkdir -p data/{raw,processed,external}
mkdir -p models/{checkpoints,saved_models}
mkdir -p notebooks
mkdir -p src/{data,models,training,utils}
mkdir -p configs
mkdir -p logs
mkdir -p results/{figures,predictions}

# Create __init__.py files for Python packages
touch src/__init__.py
touch src/data/__init__.py
touch src/models/__init__.py
touch src/training/__init__.py
touch src/utils/__init__.py

# Create README
touch README.md
```

**Directory Structure:**
```
image-classifier/
├── data/                   # Data files (excluded from Git)
│   ├── raw/               # Original, immutable data
│   ├── processed/         # Cleaned, transformed data
│   └── external/          # External datasets
├── models/                # Model files (excluded from Git)
│   ├── checkpoints/       # Training checkpoints
│   └── saved_models/      # Final trained models
├── notebooks/             # Jupyter notebooks for exploration
├── src/                   # Source code
│   ├── data/             # Data loading and processing
│   ├── models/           # Model architectures
│   ├── training/         # Training scripts
│   └── utils/            # Utility functions
├── configs/              # Configuration files (YAML, JSON)
├── logs/                 # Training logs (excluded from Git)
├── results/              # Results and visualizations
│   ├── figures/          # Plots and graphs
│   └── predictions/      # Model predictions
├── requirements.txt      # Python dependencies
├── .gitignore           # Git ignore rules
└── README.md            # Project documentation
```

---

## Git Configuration for DL

### 1. Create .gitignore

```bash
cat > .gitignore << 'EOF'
# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
env/
venv/
dl_env/
ENV/
*.egg-info/
dist/
build/

# Jupyter Notebook
.ipynb_checkpoints
*.ipynb_checkpoints

# Virtual Environment
venv/
ENV/
env/

# Deep Learning Specific
# Large data files
data/raw/*
data/processed/*
data/external/*
!data/raw/.gitkeep
!data/processed/.gitkeep
!data/external/.gitkeep

# Model files (large)
models/checkpoints/*
models/saved_models/*.pth
models/saved_models/*.h5
models/saved_models/*.pb
models/saved_models/*.ckpt
!models/checkpoints/.gitkeep
!models/saved_models/.gitkeep

# Logs
logs/*
*.log
!logs/.gitkeep
tensorboard_logs/
wandb/
runs/

# Large result files
results/predictions/*
!results/predictions/.gitkeep

# IDE
.vscode/
.idea/
*.swp
*.swo
*~

# macOS
.DS_Store
.AppleDouble
.LSOverride
._*
.Spotlight-V100
.Trashes

# Temporary files
*.tmp
*.temp
*.cache

# Weights & Biases
wandb/

# MLflow
mlruns/

# DVC (Data Version Control)
.dvc/cache
EOF
```

### 2. Create .gitkeep Files

```bash
# Create .gitkeep files to preserve empty directories
touch data/raw/.gitkeep
touch data/processed/.gitkeep
touch data/external/.gitkeep
touch models/checkpoints/.gitkeep
touch models/saved_models/.gitkeep
touch logs/.gitkeep
touch results/predictions/.gitkeep
```

### 3. Create requirements.txt

```bash
# Generate requirements.txt
pip freeze > requirements.txt

# OR create manually with specific versions
cat > requirements.txt << 'EOF'
torch==2.1.0
torchvision==0.16.0
numpy==1.24.3
pandas==2.0.3
matplotlib==3.7.2
scikit-learn==1.3.0
jupyter==1.0.0
tensorboard==2.14.0
Pillow==10.0.0
tqdm==4.66.1
pyyaml==6.0.1
EOF
```

---

## Daily Workflow

### 1. Starting a New Work Session

```bash
# Navigate to project
cd ~/projects/image-classifier

# Activate virtual environment
source ~/projects/dl_env/bin/activate
# Or use the alias if you created one
dlenv

# Update from remote (if collaborating)
git pull origin main

# Check current status
git status

# Check current branch
git branch --show-current
```

### 2. Making Changes

```bash
# Edit file (use your preferred editor)
# VS Code
code src/data/preprocessing.py

# Or use nano
nano src/data/preprocessing.py

# Or use vim
vim src/data/preprocessing.py

# Check what changed
git status
git diff src/data/preprocessing.py

# Stage changes
git add src/data/preprocessing.py

# Commit
git commit -m "feat(data): add image augmentation function"
```

### 3. Working with Jupyter Notebooks

```bash
# Activate environment
source ~/projects/dl_env/bin/activate

# Start Jupyter
jupyter notebook

# Or use JupyterLab
jupyter lab

# After working in notebook
git add notebooks/experiment_001_baseline.ipynb
git commit -m "exp: baseline CNN architecture exploration"
```

---

## Managing Experiments

### Apple Silicon (M1/M2/M3) Specific

**Using Metal Performance Shaders (MPS) for GPU Acceleration:**

```python
# In your Python training script
import torch

# Check for MPS availability
if torch.backends.mps.is_available():
    device = torch.device("mps")
    print("Using Apple Silicon GPU (MPS)")
elif torch.cuda.is_available():
    device = torch.device("cuda")
    print("Using NVIDIA GPU")
else:
    device = torch.device("cpu")
    print("Using CPU")

# Move model and data to device
model = model.to(device)
inputs = inputs.to(device)
```

### Experiment Branch Workflow

### 1. Creating Experiment Branch

```bash
# Create and switch to experiment branch
git checkout -b experiment/resnet-transfer-learning

# Verify
git branch --show-current
```

### 2. Training on macOS

**For Apple Silicon:**
```bash
# Training will use MPS automatically if available
python src/training/train.py --config configs/experiment_002_resnet.yaml --device mps
```

**Monitor GPU Usage:**
```bash
# Install powermetrics wrapper
sudo powermetrics --samplers gpu_power -i 1000

# Or use Activity Monitor app
# Open Activity Monitor > Window > GPU History
```

### 3. Tracking Results

```bash
# Create results file
cat > results/experiment_002_results.txt << 'EOF'
Experiment 002: ResNet Transfer Learning
----------------------------------------
Date: 2025-01-15
Device: Apple M2 (MPS)
Config: configs/experiment_002_resnet.yaml

Results:
- Best Validation Accuracy: 92.3%
- Final Test Accuracy: 91.8%
- Training Time: 1.8 hours (with MPS acceleration)
- Model Size: 97.8 MB

Notes:
- MPS acceleration provided 3x speedup over CPU
- Transfer learning significantly improved accuracy
- Consider fine-tuning more layers
EOF

# Commit results
git add results/experiment_002_results.txt
git commit -m "exp: ResNet experiment results - 92.3% val acc on M2"
```

### 4. Merging Successful Experiment

```bash
# Switch to main
git checkout main

# Merge experiment
git merge experiment/resnet-transfer-learning -m "Merge ResNet experiment"

# Push to remote
git push origin main

# Delete experiment branch (optional)
git branch -d experiment/resnet-transfer-learning
```

---

## Collaboration

### 1. Setting Up SSH Keys for GitHub/GitLab

**Generate SSH Key:**
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your.email@example.com"

# Press Enter to accept default location
# Enter passphrase (optional but recommended)

# Add to SSH agent
eval "$(ssh-agent -s)"

# For macOS Monterey (12.0) or later
cat > ~/.ssh/config << 'EOF'
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
EOF

ssh-add --apple-use-keychain ~/.ssh/id_ed25519

# Copy public key to clipboard
pbcopy < ~/.ssh/id_ed25519.pub

# Add to GitHub: Settings → SSH and GPG keys → New SSH key
```

### 2. Clone and Setup

```bash
# Clone via SSH
git clone git@github.com:username/image-classifier.git
cd image-classifier

# Setup environment
python3 -m venv dl_env
source dl_env/bin/activate
pip install -r requirements.txt
```

### 3. Pull Request Workflow

```bash
# Create feature branch
git checkout -b feature/data-augmentation-pipeline

# Make changes, commit
git add .
git commit -m "feat(data): implement advanced augmentation"

# Push
git push -u origin feature/data-augmentation-pipeline

# Create PR using GitHub CLI (install: brew install gh)
gh auth login
gh pr create --title "Add data augmentation" --body "Implements mixup and cutmix"
```

---

## Best Practices

### 1. macOS-Specific Considerations

**Prevent .DS_Store in Git:**
```bash
# Already in .gitignore, but also set globally
echo ".DS_Store" >> ~/.gitignore_global
git config --global core.excludesfile ~/.gitignore_global
```

**Use Keychain for Git Credentials:**
```bash
# Already configured in setup, but verify
git config --global credential.helper osxkeychain
```

### 2. Optimizing for Apple Silicon

**Environment Variables:**
```bash
# Add to ~/.zshrc for better performance
cat >> ~/.zshrc << 'EOF'

# PyTorch MPS optimization
export PYTORCH_ENABLE_MPS_FALLBACK=1

# TensorFlow Metal optimization
export TF_METAL_DEVICE_VERBOSE=1
EOF

source ~/.zshrc
```

**Check Architecture:**
```bash
# Verify you're running native ARM code
uname -m
# Should output: arm64

# Check if Python is native
python3 -c "import platform; print(platform.machine())"
# Should output: arm64
```

### 3. Using Homebrew for Development Tools

```bash
# Install useful tools
brew install tree        # View directory structure
brew install htop        # System monitoring
brew install gh          # GitHub CLI
brew install dvc         # Data version control

# Example: View project structure
tree -L 3 -I '__pycache__|*.pyc|dl_env'
```

### 4. Shell Configuration

**Add useful aliases to ~/.zshrc:**
```bash
cat >> ~/.zshrc << 'EOF'

# Git aliases
alias gs='git status'
alias ga='git add'
alias gc='git commit -m'
alias gp='git push'
alias gl='git log --oneline -10'
alias gco='git checkout'

# Project aliases
alias dlenv='source ~/projects/dl_env/bin/activate'
alias cdproj='cd ~/projects/image-classifier'
alias nb='jupyter notebook'

# Utility
alias ll='ls -lah'
alias tree2='tree -L 2 -I "__pycache__|*.pyc|dl_env"'
EOF

# Reload
source ~/.zshrc
```

### 5. VS Code Integration

**Install VS Code:**
```bash
brew install --cask visual-studio-code
```

**Recommended Extensions:**
- Python
- Jupyter
- GitLens
- GitHub Copilot
- PyTorch Snippets

**Open project:**
```bash
cd ~/projects/image-classifier
code .
```

---

## Troubleshooting

### Issue 1: MPS Not Available on Apple Silicon

**Check:**
```bash
python3 << 'EOF'
import torch
print(f"MPS Available: {torch.backends.mps.is_available()}")
print(f"MPS Built: {torch.backends.mps.is_built()}")
EOF
```

**Solution:**
```bash
# Reinstall PyTorch
pip uninstall torch torchvision torchaudio
pip install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cpu
```

### Issue 2: Permission Denied (SSH)

**Error:**
```
Permission denied (publickey)
```

**Solution:**
```bash
# Check SSH agent
eval "$(ssh-agent -s)"

# Add key again
ssh-add --apple-use-keychain ~/.ssh/id_ed25519

# Test connection
ssh -T git@github.com
```

### Issue 3: Rosetta 2 Required (Intel Apps on Apple Silicon)

**If you need Intel-only software:**
```bash
# Install Rosetta 2
softwareupdate --install-rosetta

# Run x86_64 shell
arch -x86_64 zsh

# Install Homebrew for x86_64
arch -x86_64 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Issue 4: Command Not Found After Installing

**Solution:**
```bash
# Reload shell configuration
source ~/.zshrc

# Or check PATH
echo $PATH

# Add to PATH if needed
echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.zshrc
```

---

## Performance Tips for macOS

### 1. Apple Silicon Optimization

**PyTorch:**
```python
# Use MPS data type for better performance
import torch

device = torch.device("mps")
model = model.to(device).to(torch.float32)  # Use float32 for MPS
```

**TensorFlow:**
```python
import tensorflow as tf

# Enable Metal
print("GPUs Available:", tf.config.list_physical_devices('GPU'))
```

### 2. Activity Monitor

**Monitor Training:**
- Open Activity Monitor
- Window → GPU History
- Monitor GPU utilization during training

### 3. Energy Efficiency

**For long training runs on battery:**
```bash
# Prevent sleep
caffeinate -i python src/training/train.py

# Or use screen/tmux
screen -S training
python src/training/train.py
# Detach: Ctrl+A, D
# Reattach: screen -r training
```

---

## Quick Reference

### Essential macOS Commands

```bash
# Navigation
cd ~/projects/image-classifier       # Go to project
pwd                                   # Print working directory
ls -la                               # List all files

# File operations
touch file.py                        # Create file
rm file.py                           # Delete file
cp file.py backup.py                 # Copy file
mv old.py new.py                     # Rename/move
pbcopy < file.txt                    # Copy to clipboard
pbpaste > file.txt                   # Paste from clipboard

# Git commands
git status                            # Check status
git add .                            # Stage all
git commit -m "message"              # Commit
git push origin main                 # Push
git pull origin main                 # Pull

# Environment
source ~/projects/dl_env/bin/activate  # Activate venv
deactivate                             # Deactivate venv
pip freeze > requirements.txt          # Save dependencies
```

### Keyboard Shortcuts

- **Command + Space**: Spotlight Search
- **Command + Tab**: Switch apps
- **Command + `**: Switch windows within app
- **Command + Q**: Quit app
- **Command + W**: Close window/tab
- **Command + C/V**: Copy/Paste
- **Command + Shift + .**: Show hidden files in Finder

---

## Next Steps

1. ✅ Set up Git and Python environment
2. ✅ Create project structure
3. ✅ Configure for macOS (Keychain, .DS_Store)
4. ✅ Optimize for Apple Silicon (if applicable)
5. ✅ Start baseline experiment
6. 📚 Learn [DVC](https://dvc.org) for data versioning
7. 📚 Explore [MLflow](https://mlflow.org) for tracking
8. 📚 Try [Weights & Biases](https://wandb.ai)

---

**Happy Coding on macOS! 🚀**
