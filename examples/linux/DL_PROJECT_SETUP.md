# Deep Learning Project Setup with Git - Linux Guide

A complete guide for setting up and managing a Deep Learning project with Git on Linux systems.

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

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install git

# Fedora/RHEL
sudo dnf install git

# Arch Linux
sudo pacman -S git

# Verify installation
git --version
```

### 2. Install Python and Deep Learning Tools

```bash
# Install Python 3 and pip
sudo apt install python3 python3-pip python3-venv

# Verify installation
python3 --version
pip3 --version
```

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

### 4. Install Deep Learning Frameworks

```bash
# Activate environment first
source ~/projects/dl_env/bin/activate

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
```bash
python3 -c "import torch; print(f'PyTorch: {torch.__version__}'); print(f'CUDA Available: {torch.cuda.is_available()}')"
```

---

## Project Initialization

### 1. Create Project Directory

```bash
# Create project directory
mkdir ~/projects/image-classifier
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

**Directory Structure Explanation:**
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

# OS
.DS_Store
Thumbs.db
*.bak

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

**Why .gitkeep?**
Git doesn't track empty directories. We use `.gitkeep` to preserve directory structure.

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

### 4. Create README.md

```bash
cat > README.md << 'EOF'
# Image Classifier Project

A deep learning project for image classification using PyTorch.

## Setup

### 1. Clone the repository
```bash
git clone <repository-url>
cd image-classifier
```

### 2. Create virtual environment
```bash
python3 -m venv dl_env
source dl_env/bin/activate
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

## Project Structure
```
image-classifier/
├── data/              # Data files (not tracked)
├── models/            # Model checkpoints and saved models
├── notebooks/         # Jupyter notebooks
├── src/               # Source code
├── configs/           # Configuration files
├── logs/              # Training logs
└── results/           # Results and visualizations
```

## Usage

### Training
```bash
python src/training/train.py --config configs/experiment_001.yaml
```

### Evaluation
```bash
python src/training/evaluate.py --model models/saved_models/best_model.pth
```

## Experiments

See `notebooks/experiments.md` for experiment tracking.

## Contributors

- Your Name <your.email@example.com>
EOF
```

---

## Project Structure

### 1. Create Sample Source Files

**Create data loader:**
```bash
cat > src/data/dataloader.py << 'EOF'
import torch
from torch.utils.data import Dataset, DataLoader
from torchvision import transforms
from PIL import Image
import os


class ImageDataset(Dataset):
    """Custom dataset for image classification."""

    def __init__(self, data_dir, transform=None):
        self.data_dir = data_dir
        self.transform = transform
        self.images = []
        self.labels = []
        self._load_data()

    def _load_data(self):
        """Load image paths and labels."""
        # Implementation here
        pass

    def __len__(self):
        return len(self.images)

    def __getitem__(self, idx):
        img_path = self.images[idx]
        label = self.labels[idx]

        image = Image.open(img_path).convert('RGB')

        if self.transform:
            image = self.transform(image)

        return image, label


def get_transforms(mode='train'):
    """Get data transforms for training or validation."""
    if mode == 'train':
        return transforms.Compose([
            transforms.RandomResizedCrop(224),
            transforms.RandomHorizontalFlip(),
            transforms.ToTensor(),
            transforms.Normalize(mean=[0.485, 0.456, 0.406],
                              std=[0.229, 0.224, 0.225])
        ])
    else:
        return transforms.Compose([
            transforms.Resize(256),
            transforms.CenterCrop(224),
            transforms.ToTensor(),
            transforms.Normalize(mean=[0.485, 0.456, 0.406],
                              std=[0.229, 0.224, 0.225])
        ])
EOF
```

**Create model architecture:**
```bash
cat > src/models/cnn_model.py << 'EOF'
import torch
import torch.nn as nn
import torch.nn.functional as F


class SimpleCNN(nn.Module):
    """Simple CNN for image classification."""

    def __init__(self, num_classes=10):
        super(SimpleCNN, self).__init__()
        self.conv1 = nn.Conv2d(3, 32, kernel_size=3, padding=1)
        self.conv2 = nn.Conv2d(32, 64, kernel_size=3, padding=1)
        self.conv3 = nn.Conv2d(64, 128, kernel_size=3, padding=1)

        self.pool = nn.MaxPool2d(2, 2)
        self.dropout = nn.Dropout(0.5)

        self.fc1 = nn.Linear(128 * 28 * 28, 512)
        self.fc2 = nn.Linear(512, num_classes)

    def forward(self, x):
        x = self.pool(F.relu(self.conv1(x)))
        x = self.pool(F.relu(self.conv2(x)))
        x = self.pool(F.relu(self.conv3(x)))

        x = x.view(x.size(0), -1)
        x = self.dropout(F.relu(self.fc1(x)))
        x = self.fc2(x)

        return x
EOF
```

**Create training script:**
```bash
cat > src/training/train.py << 'EOF'
import torch
import torch.nn as nn
import torch.optim as optim
from torch.utils.data import DataLoader
import argparse
from tqdm import tqdm
import yaml
import os


def train_epoch(model, dataloader, criterion, optimizer, device):
    """Train for one epoch."""
    model.train()
    running_loss = 0.0
    correct = 0
    total = 0

    pbar = tqdm(dataloader, desc='Training')
    for inputs, labels in pbar:
        inputs, labels = inputs.to(device), labels.to(device)

        optimizer.zero_grad()
        outputs = model(inputs)
        loss = criterion(outputs, labels)
        loss.backward()
        optimizer.step()

        running_loss += loss.item()
        _, predicted = outputs.max(1)
        total += labels.size(0)
        correct += predicted.eq(labels).sum().item()

        pbar.set_postfix({'loss': running_loss/len(dataloader),
                         'acc': 100.*correct/total})

    return running_loss / len(dataloader), 100. * correct / total


def validate(model, dataloader, criterion, device):
    """Validate the model."""
    model.eval()
    running_loss = 0.0
    correct = 0
    total = 0

    with torch.no_grad():
        for inputs, labels in dataloader:
            inputs, labels = inputs.to(device), labels.to(device)
            outputs = model(inputs)
            loss = criterion(outputs, labels)

            running_loss += loss.item()
            _, predicted = outputs.max(1)
            total += labels.size(0)
            correct += predicted.eq(labels).sum().item()

    return running_loss / len(dataloader), 100. * correct / total


def main(args):
    # Load config
    with open(args.config, 'r') as f:
        config = yaml.safe_load(f)

    # Setup device
    device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
    print(f'Using device: {device}')

    # TODO: Load data, create model, train
    # This is a template - implement based on your needs
    pass


if __name__ == '__main__':
    parser = argparse.ArgumentParser(description='Train image classifier')
    parser.add_argument('--config', type=str, default='configs/experiment_001.yaml',
                       help='Path to config file')
    args = parser.parse_args()
    main(args)
EOF
```

**Create config file:**
```bash
cat > configs/experiment_001.yaml << 'EOF'
# Experiment Configuration

experiment:
  name: "baseline_cnn"
  description: "Baseline CNN model with standard augmentation"
  seed: 42

data:
  train_dir: "data/processed/train"
  val_dir: "data/processed/val"
  test_dir: "data/processed/test"
  batch_size: 32
  num_workers: 4

model:
  name: "SimpleCNN"
  num_classes: 10
  pretrained: false

training:
  epochs: 50
  learning_rate: 0.001
  optimizer: "Adam"
  weight_decay: 0.0001
  scheduler: "ReduceLROnPlateau"
  early_stopping_patience: 10

logging:
  log_dir: "logs"
  checkpoint_dir: "models/checkpoints"
  save_best_only: true
  tensorboard: true
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

# Update from remote (if collaborating)
git pull origin main

# Check current status
git status

# Check current branch
git branch --show-current
```

### 2. Making Changes

**Scenario: Adding a new data preprocessing function**

```bash
# Edit the file
nano src/data/preprocessing.py

# Add new function
cat >> src/data/preprocessing.py << 'EOF'
def augment_image(image, augmentation_config):
    """Apply data augmentation to image."""
    # Implementation
    pass
EOF

# Check what changed
git status
git diff src/data/preprocessing.py
```

### 3. Committing Changes

```bash
# Stage specific file
git add src/data/preprocessing.py

# Check staged changes
git status

# Commit with descriptive message
git commit -m "feat(data): add image augmentation function

- Implement augment_image() with configurable transforms
- Support rotation, flip, and color jitter
- Add tests for augmentation pipeline"

# View commit
git log -1
```

### 4. Managing Notebooks

```bash
# Create new experiment notebook
cd notebooks
jupyter notebook experiment_001_baseline.ipynb

# After working in notebook
cd ..
git add notebooks/experiment_001_baseline.ipynb
git commit -m "exp: baseline CNN architecture exploration

- Tested different layer configurations
- Best validation accuracy: 85.2%
- Model saved to models/checkpoints/baseline_v1.pth"
```

---

## Managing Experiments

### Experiment Branch Workflow

**Why Use Branches for Experiments?**
- Isolate experimental code from main codebase
- Easy to discard failed experiments
- Compare different approaches side by side
- Clean history of what worked and what didn't

### 1. Creating Experiment Branch

```bash
# Create and switch to experiment branch
git checkout -b experiment/resnet-transfer-learning

# Verify you're on the new branch
git branch --show-current
```

### 2. Working on Experiment

```bash
# Make changes
nano src/models/resnet_model.py

# Add new model
cat > src/models/resnet_model.py << 'EOF'
import torch
import torch.nn as nn
from torchvision import models


class ResNetClassifier(nn.Module):
    """ResNet-based classifier with transfer learning."""

    def __init__(self, num_classes=10, pretrained=True):
        super(ResNetClassifier, self).__init__()
        self.resnet = models.resnet50(pretrained=pretrained)

        # Freeze early layers
        for param in list(self.resnet.parameters())[:-10]:
            param.requires_grad = False

        # Replace final layer
        num_features = self.resnet.fc.in_features
        self.resnet.fc = nn.Linear(num_features, num_classes)

    def forward(self, x):
        return self.resnet(x)
EOF

# Create new config for this experiment
cp configs/experiment_001.yaml configs/experiment_002_resnet.yaml

# Edit config
nano configs/experiment_002_resnet.yaml

# Commit experiment setup
git add src/models/resnet_model.py configs/experiment_002_resnet.yaml
git commit -m "exp: setup ResNet transfer learning experiment

- Add ResNet50 model with pretrained weights
- Freeze first 40 layers
- Create config for experiment 002"
```

### 3. Training and Tracking Results

```bash
# Run training
python src/training/train.py --config configs/experiment_002_resnet.yaml

# Save results
cat > results/experiment_002_results.txt << 'EOF'
Experiment 002: ResNet Transfer Learning
----------------------------------------
Date: 2025-01-15
Config: configs/experiment_002_resnet.yaml
Model: ResNet50 (pretrained)

Results:
- Best Validation Accuracy: 92.3%
- Final Test Accuracy: 91.8%
- Training Time: 2.5 hours
- Model Size: 97.8 MB

Notes:
- Transfer learning significantly improved accuracy
- Converged faster than baseline CNN
- Consider fine-tuning more layers
EOF

# Commit results (but not model weights!)
git add results/experiment_002_results.txt
git commit -m "exp: ResNet experiment results

Best val acc: 92.3% (vs baseline 85.2%)
Model saved to models/checkpoints/resnet_v1.pth
See results/experiment_002_results.txt for details"
```

### 4. Experiment Outcome - Success!

```bash
# Switch back to main
git checkout main

# Merge successful experiment
git merge experiment/resnet-transfer-learning -m "Merge ResNet transfer learning experiment

- 7.1% accuracy improvement over baseline
- Model: ResNet50 with transfer learning
- Config: experiment_002_resnet.yaml
- Results in results/experiment_002_results.txt"

# Delete experiment branch (optional)
git branch -d experiment/resnet-transfer-learning

# Push to remote
git push origin main
```

### 5. Experiment Outcome - Failed

```bash
# Experiment didn't work out
git checkout main

# Delete failed experiment branch
git branch -D experiment/complex-architecture

# Or keep it for reference
git push origin experiment/complex-architecture
```

---

## Collaboration

### 1. Setting Up Remote Repository

```bash
# Add remote (GitHub/GitLab)
git remote add origin git@github.com:username/image-classifier.git

# Verify
git remote -v

# Push main branch
git push -u origin main
```

### 2. Collaborator Workflow

**Person A (You) - Pushing Changes:**
```bash
# Make changes
git add src/models/new_architecture.py
git commit -m "feat(models): add EfficientNet architecture"

# Push to remote
git push origin main
```

**Person B (Teammate) - Getting Changes:**
```bash
# Clone repository (first time)
git clone git@github.com:username/image-classifier.git
cd image-classifier

# Setup environment
python3 -m venv dl_env
source dl_env/bin/activate
pip install -r requirements.txt

# Later, get updates
git pull origin main
```

### 3. Feature Branch Workflow

**Start new feature:**
```bash
# Create feature branch
git checkout -b feature/data-augmentation-pipeline

# Make changes
nano src/data/augmentation.py
git add src/data/augmentation.py
git commit -m "feat(data): implement advanced augmentation pipeline"

# Push feature branch
git push -u origin feature/data-augmentation-pipeline
```

**Create Pull Request:**
```bash
# Using GitHub CLI
gh pr create --title "Add advanced data augmentation pipeline" \
             --body "Implements mixup, cutmix, and random erasing"

# Or push and create PR on GitHub web interface
```

**After PR is merged:**
```bash
# Switch back to main
git checkout main

# Pull merged changes
git pull origin main

# Delete local feature branch
git branch -d feature/data-augmentation-pipeline
```

---

## Best Practices

### 1. Commit Messages for DL Projects

**Good commit messages:**
```bash
# Feature additions
git commit -m "feat(models): add Vision Transformer architecture"

# Bug fixes
git commit -m "fix(training): correct learning rate scheduler step timing"

# Experiments
git commit -m "exp: test dropout rates [0.3, 0.5, 0.7]

Results: 0.5 dropout achieved best validation accuracy (89.2%)"

# Data changes
git commit -m "data: add CIFAR-10 dataset preprocessing scripts"

# Documentation
git commit -m "docs: add model architecture diagrams"

# Configuration
git commit -m "config: update hyperparameters for final training run"
```

### 2. What to Track in Git

**DO track:**
- ✅ Source code (.py files)
- ✅ Configuration files (.yaml, .json)
- ✅ Requirements.txt
- ✅ Notebooks (with cleared outputs)
- ✅ Small sample datasets (< 1MB)
- ✅ Documentation
- ✅ Scripts
- ✅ Experiment logs (text summaries)

**DON'T track:**
- ❌ Large datasets
- ❌ Model checkpoints (.pth, .h5 files > 100MB)
- ❌ Virtual environments
- ❌ __pycache__ directories
- ❌ Training logs (TensorBoard files)
- ❌ Large result files

### 3. Managing Large Files

**Option 1: Git LFS (Large File Storage)**
```bash
# Install Git LFS
sudo apt install git-lfs
git lfs install

# Track large files
git lfs track "*.pth"
git lfs track "*.h5"

# Add .gitattributes
git add .gitattributes
git commit -m "chore: setup Git LFS for model files"

# Now large files are handled by LFS
git add models/saved_models/best_model.pth
git commit -m "model: add best trained model"
```

**Option 2: DVC (Data Version Control)**
```bash
# Install DVC
pip install dvc

# Initialize DVC
dvc init

# Track data directory
dvc add data/raw/images.zip

# Commit DVC files (not the actual data)
git add data/raw/images.zip.dvc .dvc/config
git commit -m "data: track images dataset with DVC"

# Setup remote storage (S3, GCS, etc.)
dvc remote add -d storage s3://mybucket/dvc-storage
dvc push
```

### 4. Clear Notebook Outputs Before Committing

**Manual method:**
```bash
# Clear outputs in Jupyter
# Cell -> All Output -> Clear

# Or use nbconvert
jupyter nbconvert --clear-output --inplace notebooks/*.ipynb

# Commit
git add notebooks/
git commit -m "notebooks: clear outputs before commit"
```

**Automated with pre-commit hook:**
```bash
# Install nbstripout
pip install nbstripout

# Setup filter
nbstripout --install

# Now notebooks are automatically cleaned on commit
```

### 5. Experiment Tracking

**Create experiment log:**
```bash
cat > notebooks/EXPERIMENTS.md << 'EOF'
# Experiment Log

## Experiment 001: Baseline CNN
- **Date**: 2025-01-10
- **Branch**: `experiment/baseline-cnn`
- **Config**: `configs/experiment_001.yaml`
- **Results**: Val Acc: 85.2%, Test Acc: 84.8%
- **Notes**: Good baseline, but could be improved
- **Status**: ✅ Completed

## Experiment 002: ResNet Transfer Learning
- **Date**: 2025-01-15
- **Branch**: `experiment/resnet-transfer-learning`
- **Config**: `configs/experiment_002_resnet.yaml`
- **Results**: Val Acc: 92.3%, Test Acc: 91.8%
- **Notes**: Significant improvement, merged to main
- **Status**: ✅ Completed, Merged

## Experiment 003: Data Augmentation Study
- **Date**: 2025-01-18
- **Branch**: `experiment/augmentation-study`
- **Config**: `configs/experiment_003_augmentation.yaml`
- **Results**: In progress
- **Status**: 🔄 Running
EOF

git add notebooks/EXPERIMENTS.md
git commit -m "docs: create experiment tracking log"
```

### 6. Regular Maintenance

**Weekly cleanup:**
```bash
# Check repository status
git status

# View recent activity
git log --oneline -10

# Check branch list
git branch -a

# Delete merged branches
git branch --merged | grep -v "\*" | grep -v "main" | xargs -n 1 git branch -d

# Fetch and prune
git fetch --prune

# Clean up large files not in .gitignore
git clean -xdn  # Dry run
git clean -xdf  # Actually clean
```

---

## Quick Reference

### Common Commands for DL Projects

```bash
# Daily workflow
git status                                    # Check status
git add src/models/new_model.py              # Stage changes
git commit -m "feat: add new model"          # Commit
git push origin main                          # Push to remote

# Experiment workflow
git checkout -b experiment/new-idea           # Create experiment branch
# ... do work ...
git add .
git commit -m "exp: testing new architecture"
git checkout main                             # Back to main
git merge experiment/new-idea                 # Merge if successful

# Collaboration
git pull origin main                          # Get updates
git push -u origin feature/new-feature       # Push feature branch

# Viewing changes
git diff                                      # See uncommitted changes
git log --oneline -10                        # View recent commits
git show HEAD                                 # Show last commit

# Undoing mistakes
git restore src/models/model.py              # Discard changes
git reset --soft HEAD~1                      # Undo last commit
git stash                                     # Save work temporarily
```

### Virtual Environment Management

```bash
# Activate
source ~/projects/dl_env/bin/activate

# Deactivate
deactivate

# Update requirements
pip freeze > requirements.txt

# Install from requirements
pip install -r requirements.txt
```

### Useful Aliases

Add to `~/.bashrc`:
```bash
# Git aliases
alias gs='git status'
alias ga='git add'
alias gc='git commit -m'
alias gp='git push'
alias gl='git log --oneline -10'

# Project aliases
alias dlenv='source ~/projects/dl_env/bin/activate'
alias cdproj='cd ~/projects/image-classifier'
alias nb='jupyter notebook'
```

Reload: `source ~/.bashrc`

---

## Troubleshooting

### Issue 1: Large Files Rejected by Git

**Error:**
```
remote: error: File models/model.pth is 150MB; this exceeds GitHub's file size limit of 100MB
```

**Solution:**
```bash
# Remove file from staging
git rm --cached models/model.pth

# Add to .gitignore
echo "models/*.pth" >> .gitignore

# Commit
git add .gitignore
git commit -m "fix: exclude large model files from Git"
```

### Issue 2: Merge Conflicts in Notebooks

**Solution:**
```bash
# Accept theirs (teammate's version)
git checkout --theirs notebooks/experiment.ipynb
git add notebooks/experiment.ipynb

# Or accept ours (your version)
git checkout --ours notebooks/experiment.ipynb
git add notebooks/experiment.ipynb

# Continue merge
git commit
```

### Issue 3: Accidentally Committed Large Files

**Solution:**
```bash
# Remove from last commit
git rm --cached models/large_model.pth
git commit --amend --no-edit

# Force push if already pushed (careful!)
git push --force origin main
```

---

## Next Steps

1. ✅ Set up environment and Git
2. ✅ Create project structure
3. ✅ Start with baseline experiment
4. ✅ Track experiments with branches
5. ✅ Collaborate with team
6. 📚 Learn about [DVC](https://dvc.org) for data versioning
7. 📚 Explore [MLflow](https://mlflow.org) for experiment tracking
8. 📚 Check out [Weights & Biases](https://wandb.ai) for visualization

---

**Happy Coding! 🚀**
