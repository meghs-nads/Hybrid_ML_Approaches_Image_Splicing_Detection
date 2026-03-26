# Image Splicing Detection & Localization

A hybrid deep learning approach for detecting and localizing image splicing attacks using a combination of handcrafted forensic features and CNN-based deep learning.

## Overview

This project implements a state-of-the-art solution for **passive blind image splicing detection and localization** using:

- **Handcrafted Features**: Local Binary Patterns (LBP), Discrete Cosine Transform (DCT), Discrete Wavelet Transform (DWT), and Photo Response Non-Uniformity (PRNU)
- **Deep Learning**: ResNet-50 backbone with early fusion architecture
- **Multi-task Learning**: Simultaneous classification and segmentation for improved forensic analysis

The model is trained and evaluated on:
- **Columbia Image Splicing Detection Dataset**
- **DeFACTO Splicing Detection Dataset**

## Features

✨ **Hybrid Feature Fusion**
- Combines hand-engineered forensic features with deep neural networks
- Early fusion approach for better feature integration
- Handcrafted features capture compression artifacts and frequency domain anomalies

🎯 **Multi-task Learning**
- **Classification**: Detect whether an image is authentic or tampered
- **Segmentation**: Localize the exact region of splicing (when masks are available)
- Edge-aware loss for precise boundary detection

⚙️ **Robust Architecture**
- High-resolution input (512×512) to capture fine forensic details
- Data augmentation including JPEG compression simulation
- Stratified train/validation/test split
- Comprehensive metrics: Accuracy, Precision, Recall, F1-score, ROC-AUC

## Dataset

The project automatically downloads datasets from Kaggle:

1. **Columbia Uncompressed Mirror** - High-quality splicing examples with ground truth masks
2. **DeFACTO Dataset** (fallback) - Large-scale splicing detection dataset

### Data Split
- **Training**: 70%
- **Validation**: 10%
- **Testing**: 20%

(Automatically stratified by label to ensure balanced representation)

## Requirements

```
Python 3.7+
PyTorch >= 1.9
torchvision
torchmetrics >= 1.4.0
albumentations == 1.4.7
timm == 1.0.9
pywavelets == 1.6.0
scikit-image == 0.24.0
scikit-learn
OpenCV (cv2)
numpy
matplotlib
```

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/image-splicing-detection.git
cd image-splicing-detection
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Set up Kaggle API:
   - Download `kaggle.json` from [Kaggle Account Settings](https://www.kaggle.com/account)
   - Upload when prompted by the notebook

## Usage

### Running in Google Colab (Recommended)

The notebook is optimized for Colab with GPU support:

```python
# In Colab, simply upload the notebook and run all cells
# The notebook will automatically:
# 1. Install dependencies
# 2. Download the dataset
# 3. Train the model
# 4. Evaluate and generate metrics
```

### Local Execution

```python
# Configure Kaggle credentials first
# Then run the Jupyter notebook
jupyter notebook code.ipynb
```

## Model Architecture

### Backbone
- **ResNet-50** pretrained on ImageNet
- High-resolution input (512×512) for forensic detail preservation

### Feature Fusion
```
Input Image
    ↓
├─→ CNN Stream → ResNet-50 → Feature maps (2048-dim)
├─→ Handcrafted Stream → [LBP, DCT, DWT, PRNU] → Feature vector
    ↓
Early Fusion → Concatenate features
    ↓
├─→ Classification Head → Binary logits
└─→ Segmentation Head → Mask prediction
```

## Configuration

Key hyperparameters in the notebook:

```python
CFG = {
    "img_size": 512,              # Input resolution
    "batch_size": 16,             # Training batch size
    "epochs": 40,                 # Training epochs
    "lr": 1e-4,                   # Learning rate
    "fusion": "EARLY",            # Feature fusion strategy
    "use_localization": True,     # Enable mask prediction
    "cls_weight": 1.0,            # Classification loss weight
    "seg_weight": 0.5,            # Segmentation loss weight
    "edge_weight": 0.1,           # Edge loss weight
    "augment_jpeg": True,         # JPEG compression augmentation
}
```

## Results

The model outputs comprehensive metrics:

### Classification Metrics
- **Accuracy**: Overall correct predictions
- **Precision**: True positive rate among predictions
- **Recall**: Detection rate of tampered images
- **F1-Score**: Harmonic mean of precision and recall
- **ROC-AUC**: Area under the ROC curve

### Segmentation Metrics (if masks available)
- Pixel-level accuracy for localization
- Boundary precision for tampering region edges

## Handcrafted Features

### Local Binary Pattern (LBP)
Captures local texture patterns that differ between authentic and tampered regions.

### Discrete Cosine Transform (DCT)
Analyzes frequency domain anomalies from JPEG compression inconsistencies.

### Discrete Wavelet Transform (DWT)
Detects frequency and scale discontinuities at splicing boundaries.

### Photo Response Non-Uniformity (PRNU)
Exploits sensor-specific noise patterns that are disrupted in tampered regions.

## Training Process

1. **Data Loading**: Stratified split ensures balanced classes
2. **Augmentation**: Aggressive augmentation including JPEG compression
3. **Optimization**: Adam optimizer with learning rate scheduling
4. **Loss Functions**: 
   - Binary Cross-Entropy for classification
   - Dice Loss for segmentation
   - Custom edge loss for boundary precision
5. **Evaluation**: Per-batch metrics with detailed final reports

## Output Files

The model automatically saves:
- Training metrics (CSV format)
- Classification metrics and confusion matrix
- Localization visualizations (if applicable)
- Model checkpoints and weights

## Troubleshooting

### Dataset Download Issues
- Ensure Kaggle API is properly configured
- Check that you've accepted dataset terms on Kaggle
- Verify internet connection stability

### CUDA Out of Memory
- Reduce batch size in `CFG["batch_size"]`
- Reduce image size in `CFG["img_size"]` to 256

### Poor Performance
- Ensure dataset is properly indexed
- Check that images load correctly
- Verify augmentation isn't too aggressive

## Project Structure

```
code.ipynb
├── Environment & Dependencies
├── Kaggle Authentication
├── Dataset Download & Indexing
├── Data Splitting
├── Configuration
├── Handcrafted Feature Extractors
├── Dataset Class & Augmentation
├── Model Architecture
├── Training Loop
├── Evaluation & Metrics
└── Results Visualization
```

## Future Improvements

- [ ] Multi-dataset meta-learning for generalization
- [ ] Attention mechanisms for feature refinement
- [ ] Lightweight mobile-friendly model variants
- [ ] Real-time inference optimization
- [ ] Extended to other forgery types (copy-move, inpainting)

## Citation

If you use this project in your research, please cite:

```bibtex
@project{image-splicing-detection,
  title={Hybrid Handcrafted + Deep Forensics for Image Splicing Detection},
  author={Your Name},
  year={2024},
  url={https://github.com/yourusername/image-splicing-detection}
}
```

## License

MIT License - See LICENSE file for details

## Contact

For questions, issues, or suggestions, please open an issue on GitHub or contact the maintainer.

---

**Made with ❤️ for Image Forensics Research**
