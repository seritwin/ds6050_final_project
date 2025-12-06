# Notebooks

This directory contains Jupyter notebooks for the histopathology metastasis detection pipeline.

## Notebooks Overview

### 1. `ImageSplittingScript.ipynb`

**Purpose**: Tile extraction and organization from whole slide images.

**Key Functions**:
- Reads `.ndpi` whole slide image files using OpenSlide
- Extracts non-overlapping tiles of configurable size (default: 1024×1024 or 2048×2048)
- Organizes extracted tiles by stain type (HE, BCL2, BCL3, MITF, PIR, PBP, ANX)
- Supports batch processing of multiple slides

**Inputs**:
- Whole slide images in `.ndpi` format

**Outputs**:
- PNG tile images organized in stain-specific directories

---

### 2. `image_slide_processor.ipynb`

**Purpose**: Quality control and filtering of extracted tiles.

**Key Functions**:
- `is_informative(tile, threshold)`: Determines if a tile contains meaningful tissue content based on white pixel ratio
- `clean_uninformative_slides(slide_dir, threshold)`: Batch processes a directory, removing tiles that are predominantly background

**Parameters**:
- `threshold`: White pixel ratio threshold (default: 0.91). Tiles with >91% white pixels are considered uninformative and removed.

**Inputs**:
- Directory of extracted tile images

**Outputs**:
- Filtered directory with only informative tiles retained

---

### 3. `ResNet_DenseNet_Executable.ipynb`

**Purpose**: Deep learning model for metastasis classification.

**Key Components**:

**Data Loading**:
- `MetastasisDataset`: Custom PyTorch Dataset class that matches image tiles to patient IDs and metastasis labels from a CSV file
- Handles normalization of patient codes for robust matching

**Model Architecture**:
- ResNet-based convolutional neural network (transfer learning)
- Binary classification: metastasis (1) vs. no metastasis (0)
- Input size: 256×256 pixels

**Training Pipeline**:
- Data augmentation (random horizontal flips)
- Train/validation/test split
- CUDA/GPU acceleration support

**Evaluation**:
- Patch-level and patient-level accuracy metrics
- `aggregate_by_patient_robust()`: Aggregates tile-level predictions to patient-level using mean probability pooling
- Confusion matrix and classification reports
- ROC-AUC and average precision scores

**Inputs**:
- Filtered tile images
- CSV file with patient IDs and metastasis labels

**Outputs**:
- Trained model weights
- Evaluation metrics and visualizations

---

## Recommended Execution Order

1. **`ImageSplittingScript.ipynb`** — Extract tiles from whole slide images
2. **`image_slide_processor.ipynb`** — Filter out uninformative tiles
3. **`ResNet_DenseNet_Executable.ipynb`** — Train and evaluate the classification model

## Directory Structure Expected

```
Project_Materials/
├── Images/
│   ├── 2048Tiles/          # Raw extracted tiles
│   ├── HE/                  # H&E stained tiles
│   ├── BCL2/                # BCL2 stained tiles
│   ├── BCL3/                # BCL3 stained tiles
│   ├── MITF/                # MITF stained tiles
│   ├── PIR/                 # PIR stained tiles
│   ├── PBP/                 # PBP stained tiles
│   └── ANX/                 # ANX stained tiles
├── CSV/
│   └── Image_Data.csv       # Patient IDs and metastasis labels
└── notebooks/
    ├── ImageSplittingScript.ipynb
    ├── image_slide_processor.ipynb
    └── ResNet_DenseNet_Executable.ipynb
```

## Notes

- The code is provided as `.ipynb` Jupyter notebooks
- GPU acceleration is recommended for model training
- Adjust tile size and threshold parameters based on your specific dataset characteristics
