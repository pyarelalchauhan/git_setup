# Deep Learning Project Setup with Git - Windows Guide

A complete guide for setting up and managing a Deep Learning project with Git on Windows systems.

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

**Option 1: Git for Windows (Recommended)**
1. Download from: https://git-scm.com/download/win
2. Run installer
3. Select options:
   - ✅ Git Bash
   - ✅ Git from the command line and also from 3rd-party software
   - ✅ Use bundled OpenSSH
   - ✅ Use the OpenSSL library
   - ✅ Checkout Windows-style, commit Unix-style line endings

**Option 2: Using Winget**
```powershell
winget install --id Git.Git -e --source winget
```

**Option 3: Using Chocolatey**
```powershell
choco install git
```

**Verify Installation:**
```powershell
git --version
```

### 2. Install Python and Deep Learning Tools

**Install Python:**
1. Download from: https://www.python.org/downloads/windows/
2. Run installer
3. ✅ Check "Add Python to PATH"
4. Install

**Or using Winget:**
```powershell
winget install -e --id Python.Python.3.11
```

**Verify:**
```powershell
python --version
pip --version
```

### 3. Create Virtual Environment

**Why Virtual Environments?**
- Isolates project dependencies
- Prevents conflicts between projects
- Makes environment reproducible
- Essential for DL projects with specific library versions

**Using Command Prompt:**
```cmd
:: Navigate to your projects directory
cd C:\Users\YourName\projects

:: Create virtual environment
python -m venv dl_env

:: Activate virtual environment
dl_env\Scripts\activate

:: Your prompt should now show (dl_env)
```

**Using PowerShell:**
```powershell
# Navigate to your projects directory
cd C:\Users\YourName\projects

# Create virtual environment
python -m venv dl_env

# Activate virtual environment
dl_env\Scripts\Activate.ps1

# If you get execution policy error:
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

# Your prompt should now show (dl_env)
```

### 4. Install Deep Learning Frameworks

```powershell
# Activate environment first
dl_env\Scripts\activate

# Install PyTorch (CPU version)
pip install torch torchvision torchaudio

# OR Install PyTorch (GPU version - CUDA 11.8)
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118

# OR Install TensorFlow
pip install tensorflow

# Install common DL libraries
pip install numpy pandas matplotlib scikit-learn jupyter notebook

# Install additional tools
pip install tensorboard wandb pillow opencv-python tqdm
```

**Verify Installation:**
```powershell
python -c "import torch; print(f'PyTorch: {torch.__version__}'); print(f'CUDA Available: {torch.cuda.is_available()}')"
```

---

## Project Initialization

### 1. Create Project Directory

**Using Command Prompt:**
```cmd
:: Create project directory
mkdir C:\Users\YourName\projects\image-classifier
cd C:\Users\YourName\projects\image-classifier

:: Activate virtual environment
C:\Users\YourName\projects\dl_env\Scripts\activate
```

**Using PowerShell:**
```powershell
# Create project directory
New-Item -Path "C:\Users\YourName\projects\image-classifier" -ItemType Directory
cd C:\Users\YourName\projects\image-classifier

# Activate virtual environment
C:\Users\YourName\projects\dl_env\Scripts\Activate.ps1
```

### 2. Initialize Git Repository

**Using Git Bash (Recommended):**
```bash
# Initialize Git repo
git init -b main

# Configure identity (if not already done globally)
git config user.name "Your Name"
git config user.email "your.email@example.com"

# Verify
git config --list | grep user
```

**Using PowerShell:**
```powershell
# Initialize Git repo
git init -b main

# Configure identity
git config user.name "Your Name"
git config user.email "your.email@example.com"

# Verify
git config --list
```

### 3. Create Initial Project Structure

**Using PowerShell:**
```powershell
# Create directory structure
New-Item -Path "data\raw","data\processed","data\external" -ItemType Directory -Force
New-Item -Path "models\checkpoints","models\saved_models" -ItemType Directory -Force
New-Item -Path "notebooks" -ItemType Directory -Force
New-Item -Path "src\data","src\models","src\training","src\utils" -ItemType Directory -Force
New-Item -Path "configs","logs" -ItemType Directory -Force
New-Item -Path "results\figures","results\predictions" -ItemType Directory -Force

# Create __init__.py files for Python packages
New-Item -Path "src\__init__.py","src\data\__init__.py","src\models\__init__.py" -ItemType File
New-Item -Path "src\training\__init__.py","src\utils\__init__.py" -ItemType File

# Create README
New-Item -Path "README.md" -ItemType File
```

**Using Command Prompt:**
```cmd
:: Create directory structure
mkdir data\raw data\processed data\external
mkdir models\checkpoints models\saved_models
mkdir notebooks
mkdir src\data src\models src\training src\utils
mkdir configs logs
mkdir results\figures results\predictions

:: Create __init__.py files
type nul > src\__init__.py
type nul > src\data\__init__.py
type nul > src\models\__init__.py
type nul > src\training\__init__.py
type nul > src\utils\__init__.py

:: Create README
type nul > README.md
```

**Directory Structure:**
```
image-classifier\
├── data\                   # Data files (excluded from Git)
│   ├── raw\               # Original, immutable data
│   ├── processed\         # Cleaned, transformed data
│   └── external\          # External datasets
├── models\                # Model files (excluded from Git)
│   ├── checkpoints\       # Training checkpoints
│   └── saved_models\      # Final trained models
├── notebooks\             # Jupyter notebooks for exploration
├── src\                   # Source code
│   ├── data\             # Data loading and processing
│   ├── models\           # Model architectures
│   ├── training\         # Training scripts
│   └── utils\            # Utility functions
├── configs\              # Configuration files (YAML, JSON)
├── logs\                 # Training logs (excluded from Git)
├── results\              # Results and visualizations
│   ├── figures\          # Plots and graphs
│   └── predictions\      # Model predictions
├── requirements.txt      # Python dependencies
├── .gitignore           # Git ignore rules
└── README.md            # Project documentation
```

---

## Git Configuration for DL

### 1. Create .gitignore

**Using Git Bash or Command Prompt:**
```bash
# Create .gitignore file
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

# Windows OS
Thumbs.db
desktop.ini
$RECYCLE.BIN/
*.lnk

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

**Using PowerShell:**
```powershell
@"
# Python
__pycache__/
*.py[cod]
*`$py.class
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

# Deep Learning Specific
data/raw/*
data/processed/*
data/external/*
!data/raw/.gitkeep
!data/processed/.gitkeep
!data/external/.gitkeep

models/checkpoints/*
models/saved_models/*.pth
models/saved_models/*.h5
!models/checkpoints/.gitkeep
!models/saved_models/.gitkeep

# Logs
logs/*
*.log
!logs/.gitkeep
wandb/
runs/

# Windows OS
Thumbs.db
desktop.ini
`$RECYCLE.BIN/
*.lnk

# IDE
.vscode/
.idea/
"@ | Out-File -FilePath .gitignore -Encoding UTF8
```

### 2. Create .gitkeep Files

**Why .gitkeep?**
Git doesn't track empty directories. We use `.gitkeep` to preserve directory structure.

**PowerShell:**
```powershell
# Create .gitkeep files
New-Item -Path "data\raw\.gitkeep" -ItemType File -Force
New-Item -Path "data\processed\.gitkeep" -ItemType File -Force
New-Item -Path "data\external\.gitkeep" -ItemType File -Force
New-Item -Path "models\checkpoints\.gitkeep" -ItemType File -Force
New-Item -Path "models\saved_models\.gitkeep" -ItemType File -Force
New-Item -Path "logs\.gitkeep" -ItemType File -Force
New-Item -Path "results\predictions\.gitkeep" -ItemType File -Force
```

**Command Prompt:**
```cmd
type nul > data\raw\.gitkeep
type nul > data\processed\.gitkeep
type nul > data\external\.gitkeep
type nul > models\checkpoints\.gitkeep
type nul > models\saved_models\.gitkeep
type nul > logs\.gitkeep
type nul > results\predictions\.gitkeep
```

### 3. Create requirements.txt

```powershell
# Generate requirements.txt
pip freeze > requirements.txt
```

**Or create manually:**
```powershell
@"
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
"@ | Out-File -FilePath requirements.txt -Encoding UTF8
```

---

## Daily Workflow

### 1. Starting a New Work Session

**Using Git Bash:**
```bash
# Navigate to project
cd /c/Users/YourName/projects/image-classifier

# Activate virtual environment
source /c/Users/YourName/projects/dl_env/Scripts/activate

# Update from remote (if collaborating)
git pull origin main

# Check current status
git status
```

**Using PowerShell:**
```powershell
# Navigate to project
cd C:\Users\YourName\projects\image-classifier

# Activate virtual environment
C:\Users\YourName\projects\dl_env\Scripts\Activate.ps1

# Update from remote
git pull origin main

# Check current status
git status
```

### 2. Making Changes

**Using VS Code (Recommended for Windows):**
```powershell
# Open project in VS Code
code .

# Make changes in VS Code editor
# Use built-in Git GUI for staging and committing
```

**Using Command Line:**
```bash
# Edit file (use your preferred editor)
notepad src\data\preprocessing.py

# Check what changed
git status
git diff src/data/preprocessing.py

# Stage changes
git add src/data/preprocessing.py

# Commit
git commit -m "feat(data): add image augmentation function"
```

### 3. Working with Jupyter Notebooks

**Start Jupyter:**
```powershell
# Activate environment
dl_env\Scripts\Activate.ps1

# Start Jupyter
jupyter notebook

# Or use JupyterLab
jupyter lab
```

**Commit Notebook:**
```bash
# Add notebook
git add notebooks\experiment_001_baseline.ipynb

# Commit
git commit -m "exp: baseline CNN architecture exploration"
```

---

## Managing Experiments

### Experiment Branch Workflow

### 1. Creating Experiment Branch

**Git Bash:**
```bash
# Create and switch to experiment branch
git checkout -b experiment/resnet-transfer-learning

# Verify
git branch --show-current
```

### 2. Training Models

**PowerShell:**
```powershell
# Run training
python src\training\train.py --config configs\experiment_002_resnet.yaml

# Training will save checkpoints to models\checkpoints\
```

### 3. Tracking Results

```bash
# Create results file
echo "Experiment 002: ResNet Transfer Learning" > results\experiment_002_results.txt
echo "----------------------------------------" >> results\experiment_002_results.txt
echo "Best Validation Accuracy: 92.3%" >> results\experiment_002_results.txt

# Commit results
git add results\experiment_002_results.txt
git commit -m "exp: ResNet experiment results - 92.3% val acc"
```

### 4. Merging Successful Experiment

```bash
# Switch to main
git checkout main

# Merge experiment
git merge experiment/resnet-transfer-learning -m "Merge ResNet experiment"

# Push to remote
git push origin main
```

---

## Collaboration

### 1. Setting Up Remote Repository

**Using Git Bash:**
```bash
# Add remote (GitHub/GitLab)
git remote add origin git@github.com:username/image-classifier.git

# OR using HTTPS
git remote add origin https://github.com/username/image-classifier.git

# Verify
git remote -v

# Push main branch
git push -u origin main
```

### 2. Cloning Repository

**PowerShell:**
```powershell
# Clone repository
git clone https://github.com/username/image-classifier.git
cd image-classifier

# Setup environment
python -m venv dl_env
dl_env\Scripts\Activate.ps1
pip install -r requirements.txt
```

### 3. Pull Request Workflow

**Create Feature Branch:**
```bash
git checkout -b feature/data-augmentation-pipeline

# Make changes
# Commit changes
git add .
git commit -m "feat(data): implement advanced augmentation"

# Push
git push -u origin feature/data-augmentation-pipeline
```

**Using GitHub CLI (gh):**
```powershell
# Install GitHub CLI
winget install --id GitHub.cli

# Login
gh auth login

# Create PR
gh pr create --title "Add data augmentation" --body "Implements mixup and cutmix"
```

---

## Best Practices

### 1. Path Considerations for Windows

**Use forward slashes or raw strings in Python:**
```python
# Good - forward slashes work on all platforms
data_path = "data/raw/images"

# Good - pathlib (recommended)
from pathlib import Path
data_path = Path("data") / "raw" / "images"

# Avoid - backslashes need escaping
data_path = "data\\raw\\images"
```

### 2. Line Endings

**Already configured during Git installation, but verify:**
```bash
# Check current setting
git config core.autocrlf

# Should be 'true' on Windows
# This converts LF to CRLF on checkout, CRLF to LF on commit
```

### 3. Using Git Bash vs PowerShell vs CMD

**Recommendations:**
- **Git Bash**: Best for Git operations (Unix-like commands work)
- **PowerShell**: Good for Python/Windows operations
- **CMD**: Basic, works but limited

**Create shortcuts:**
```powershell
# PowerShell profile (similar to .bashrc)
notepad $PROFILE

# Add aliases:
function gs { git status }
function ga { git add $args }
function gc { git commit -m $args }
function gp { git push }
function activate-dl { C:\Users\YourName\projects\dl_env\Scripts\Activate.ps1 }
```

### 4. GPU Setup (NVIDIA)

**Install CUDA Toolkit:**
1. Download from: https://developer.nvidia.com/cuda-downloads
2. Install CUDA Toolkit (e.g., 11.8)
3. Install cuDNN

**Verify:**
```powershell
python -c "import torch; print(torch.cuda.is_available())"
python -c "import torch; print(torch.cuda.get_device_name(0))"
```

### 5. Windows Defender Exclusions

**Add project folder to exclusions for better performance:**
1. Open Windows Security
2. Virus & threat protection → Manage settings
3. Exclusions → Add or remove exclusions
4. Add folder: `C:\Users\YourName\projects`

---

## Troubleshooting

### Issue 1: PowerShell Execution Policy

**Error:**
```
File cannot be loaded because running scripts is disabled on this system
```

**Solution:**
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### Issue 2: Long Path Names

**Error:**
```
Filename too long
```

**Solution:**
```bash
# Enable long paths in Git
git config --system core.longpaths true
```

**Or enable in Windows:**
1. Run gpedit.msc
2. Computer Configuration → Administrative Templates → System → Filesystem
3. Enable "Enable Win32 long paths"

### Issue 3: CUDA Not Available

**Check:**
```powershell
# Check NVIDIA driver
nvidia-smi

# Check CUDA version
nvcc --version

# Reinstall PyTorch with correct CUDA version
pip uninstall torch torchvision torchaudio
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

### Issue 4: Git Line Ending Warnings

**Warning:**
```
LF will be replaced by CRLF
```

**This is normal on Windows. Ensure:**
```bash
git config core.autocrlf true
```

---

## Quick Reference

### Essential Windows Commands

**File Operations:**
```powershell
# PowerShell
New-Item -Path "file.py" -ItemType File        # Create file
Remove-Item "file.py"                           # Delete file
Copy-Item "src\*" "backup\"                    # Copy files
Move-Item "old.py" "new.py"                    # Rename/move

# Command Prompt
type nul > file.py                             # Create file
del file.py                                     # Delete file
copy src\* backup\                             # Copy files
move old.py new.py                             # Rename/move
```

**Git Commands (same as Linux):**
```bash
git status                                      # Check status
git add .                                       # Stage all
git commit -m "message"                        # Commit
git push origin main                           # Push
git pull origin main                           # Pull
git checkout -b feature-branch                 # New branch
```

### Useful Windows Tools

- **VS Code**: Best IDE for Python/DL on Windows
- **Windows Terminal**: Modern terminal app
- **Git Bash**: Unix-like shell for Git
- **GitHub Desktop**: GUI for Git
- **NVIDIA NSight**: GPU profiling tool

---

## Next Steps

1. ✅ Set up Git and Python environment
2. ✅ Create project structure
3. ✅ Configure .gitignore for DL
4. ✅ Start baseline experiment
5. ✅ Use branches for experiments
6. 📚 Learn [DVC](https://dvc.org) for data versioning
7. 📚 Explore [MLflow](https://mlflow.org) for tracking
8. 📚 Try [Weights & Biases](https://wandb.ai)

---

**Happy Coding on Windows! 🚀**
