# Image Forgery Detection using Deep Learning  

## Overview
Digital image forgery techniques such as **copy–move** and **splicing** are increasingly difficult to detect using traditional handcrafted methods, especially after compression, resizing, or post-processing.

This project implements a **deep learning–based image tampering detection pipeline** using:
- CNN backbones (ResNet18, MobileNetV3, EfficientNet-B0)
- Attention mechanisms (CBAM)
- Knowledge Distillation for real-time deployment

The goal is to achieve **high forensic accuracy** while maintaining **low inference cost**, enabling practical deployment on resource-constrained systems.


## Key Contributions
- Implemented a **binary image forgery classifier** using the CASIA 2.0 dataset
- Enhanced ResNet18 with **Convolutional Block Attention Module (CBAM)** to focus on subtle tampering artifacts
- Trained multiple baselines and performed **fair model comparison**
- Applied **Knowledge Distillation** to transfer forensic knowledge from a heavy teacher (ResNet18+CBAM) to a lightweight student (MobileNetV3-Small)
- Evaluated **accuracy, F1-score, AUC-ROC, parameter count, checkpoint size, and inference latency**


## Dataset
- **CASIA v2.0 Image Tampering Dataset**
- Classes:
  - `Au` – Authentic images
  - `Tp` – Tampered images (copy-move, splicing, compositing)
- Images are indexed and split using **stratified sampling** to preserve class balance.

Dataset handling is fully reproducible via code (Kaggle API).


## Project Pipeline

1. **Dataset Preparation**
   - Download CASIA from Kaggle
   - Normalize folder structure (`Au/`, `Tp/`)
   - Create CSV-based train/validation splits

2. **Data Loading & Augmentation**
   - ImageNet normalization
   - Spatial and color augmentations for robustness
   - PyTorch `Dataset` + `DataLoader` pipeline

3. **Baseline Training**
   - ResNet18
   - MobileNetV3-Small
   - EfficientNet-B0

4. **Attention-Enhanced Model**
   - ResNet18 augmented with **CBAM**
   - Channel + Spatial attention applied after residual blocks

5. **Knowledge Distillation**
   - Teacher: ResNet18 + CBAM
   - Student: MobileNetV3-Small
   - Loss:
     - Hard loss (cross-entropy with label smoothing)
     - Soft loss (KL divergence on temperature-scaled logits)

6. **Evaluation**
   - Accuracy, F1-Score, AUC-ROC
   - Confusion Matrix, ROC Curve, Precision-Recall Curve
   - Model size and inference latency comparison


## Model Architectures

### Baselines
- **ResNet18** – strong representation, moderate compute
- **EfficientNet-B0** – accuracy-optimized
- **MobileNetV3-Small** – lightweight and fast

### Attention Model
- **ResNet18 + CBAM**
  - Improves focus on tampering boundaries and compression artifacts
  - Better generalization on subtle forgeries

### Distilled Model
- **KD-MobileNetV3-Small**
  - Learns forensic cues from the CBAM teacher
  - Retains low latency while improving accuracy


## Efficiency Comparison
- **Parameters**
- **Checkpoint Size (MB)**
- **Latency (ms/image, batch=32)**

Key observation:
- Knowledge distillation does **not** reduce parameters; it transfers decision boundaries to a smaller network, improving accuracy at the **same compute budget**.

## Repository
- contains the main file `Project_Binary_Classification_Image_tampering.py` with entire code
