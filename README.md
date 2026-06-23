# Multi-Style Sliding CAPTCHA Gap Detection Dataset

This repository contains the dataset accompanying the paper
"Adaptive Difference Convolution: When Depthwise Separability Matters."

## Overview

The dataset is designed for evaluating geometric boundary localization
under controlled rendering-style variations. It consists of sliding
CAPTCHA images where the task is to detect rectangular gap targets
that undergo four distinct rendering perturbations while preserving
identical geometric structure.

## Dataset Structure

```
├── dataset/              # Training set
│   ├── backgrounds/      # Background images
│   └── labels.csv        # Annotations (filename, x, y, width, height, class)
├── testset_dark/         # Dark-style test set (1,000 images)
│   ├── backgrounds/
│   └── labels.csv
├── testset_light/        # Light-style test set (1,000 images)
│   ├── backgrounds/
│   └── labels.csv
├── testset_outline/      # Outline-style test set (1,000 images)
│   ├── backgrounds/
│   └── labels.csv
├── testset_white/        # White-style test set (1,000 images)
│   ├── backgrounds/
│   └── labels.csv
└── README.md
```

## Rendering Styles

| Style | Description | Implementation |
|---|---|---|
| White | Intensity truncation simulating erasure attacks | High-value clipping |
| Dark | Shadow masking reducing local contrast | Multiplicative decay + shadow overlay |
| Light | Overexposure causing detail loss | Non-linear brightness enhancement |
| Outline | Textureless geometric sketch | Canny edge extraction, texture suppression |

Each image contains two randomly positioned gap targets.

## Annotation Format

Annotations follow the standard bounding box format in `labels.csv`:

```
filename,x,y,width,height,class
```

- `filename`: image file name (relative to `backgrounds/`)
- `x, y`: top-left corner coordinates (pixels)
- `width, height`: bounding box dimensions (pixels)
- `class`: target class label

## Data Splitting

Splits are partitioned by **background template ID** to ensure that
training, validation, and test sets use entirely disjoint background
templates. This prevents the model from overfitting to specific
background textures and ensures that generalization must rely on
geometric structure rather than background memorization.

## Statistics

| Split | Rendering Style | Image Count |
|---|---|---|
| Train + Val | Mixed (White/Dark/Light/Outline) | 6,000 |
| Test | White | 1,000 |
| Test | Dark | 1,000 |
| Test | Light | 1,000 |
| Test | Outline | 1,000 |
| **Total** | | **10,000** |

Each image contains two annotated gap targets.

## Usage

The dataset is compatible with standard object detection pipelines
(e.g., Faster R-CNN). A custom `Dataset` class should:

1. Load images from `backgrounds/`
2. Parse `labels.csv` for bounding box annotations
3. Apply appropriate transforms for training or evaluation

## License and Citation

This dataset is released for research purposes. If you use it,
please cite the accompanying paper upon publication.

```
[Citation placeholder — will be updated upon acceptance]
```
