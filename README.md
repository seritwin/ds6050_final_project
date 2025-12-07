  # Histopathology Metastasis Detection

A deep learning pipeline for detecting metastasis in histopathology whole slide images (WSI) using convolutional neural networks.

# Metadata:
```yaml
  Authors: Serita Seright & Cameron Preasmyer
  Course: DS 6050
  Date: Dec 2025
  Project Name: Metastasis Prediction
```


## Project Overview

This project implements an end-to-end workflow for analyzing digitized pathology slides to classify tissue samples for metastasis. The pipeline includes:

- **Whole Slide Image Processing**: Extraction of tiles from high-resolution .ndpi pathology slides
- **Data Preprocessing**: Automated filtering of uninformative (background) tiles
- **Deep Learning Classification**: ResNet-based/DenseNet-based binary classification model for metastasis detection

## Repository Structure

```
├── README.md
├── Code/
│   ├── README.md
│   ├── ImageSplittingScript.ipynb 
│   ├── image_slide_processor.ipynb
│   └── ResNet_DenseNet_Executable.ipynb
```

## Data Availability

**Important Notice Regarding Data**

The imaging data and associated clinical information used in this project **cannot be made publicly available** due to patient privacy protections and compliance with healthcare data regulations (e.g., HIPAA). The datasets contain protected health information (PHI) derived from patient tissue samples.

If you are interested in accessing the data for research purposes, please contact the project maintainers to discuss potential data sharing agreements and institutional review board (IRB) requirements.

## Pipeline Overview

### 1. Tile Extraction (`ImageSplittingScript.ipynb`)
Extracts fixed-size tiles from whole slide images using OpenSlide. Supports configurable tile sizes and strides, and organizes outputs by stain type.

### 2. Tile Filtering (`image_slide_processor.ipynb`)
Removes uninformative tiles (predominantly white/background regions) based on pixel intensity thresholds, retaining only tiles with meaningful tissue content.

### 3. Classification Model (`ResNet_DenseNet_Executable.ipynb`)
Trains a ResNet-based convolutional neural network for binary classification (metastasis vs. no metastasis) on H&E stained tissue tiles. Includes patient-level aggregation of predictions.

## Requirements

- Python 3.8+
- PyTorch
- torchvision
- OpenSlide
- PIL/Pillow
- NumPy
- Pandas
- scikit-learn
- matplotlib
- seaborn
- tqdm

## Stain Types Supported

The pipeline handles multiple immunohistochemistry stains:
- **HE** (Hematoxylin & Eosin)
- **BCL2** / **BCL3**
- **MITF**
- **PIR**
- **PBP**
- **ANX**

   
