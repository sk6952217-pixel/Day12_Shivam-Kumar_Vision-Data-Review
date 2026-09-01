# Day 12 – Dataset V1 and Approved Preprocessing

##  Objective

The objective of Day 12 was to finalize the dataset preparation process based on the previous EDA and data-quality analysis.

The main tasks were:

- Present EDA findings
- Verify input-target image pairing
- Define preprocessing steps
- Decide augmentation strategy
- Define train/validation/test split logic
- Record preprocessing configuration
- Prepare Dataset V1 for model development

##  Dataset Summary

The underwater image dataset contains:

- Input images: 1078
- Target images: 1078
- Generated images: 1078
- Total images: 3235
- Image format: PNG
- Image dimensions: 256 × 256 pixels

## 3. EDA Findings

The input images were analyzed for brightness and contrast.

### Brightness Statistics

- Number of input images: 1078
- Average brightness: 70.16
- Minimum brightness: 1.03
- Maximum brightness: 246.69

### Contrast Statistics

- Average contrast: 26.68
- Minimum contrast: 2.19
- Maximum contrast: 73.32

### Observations

- Some input images are very dark.
- Some input images have very low contrast.
- The dataset contains a wide range of brightness levels.
- The dataset contains different contrast conditions.
- Target images appeared clearer than input images.
- Generated images appeared closer to the target images.
- No major color difference was observed in the inspected samples.
- No noticeable blur or noise was observed.
- Important image details appeared to be preserved.

##  Pairing and Data Quality

Input and target images were paired using their common image number.

Example:

img 74_input.png
img 74_target.png

One duplicate file was identified:

epoch_0110/img 74_input - Copy.png

The file was compared with the original input image and confirmed to be an exact duplicate.

The duplicate will not be treated as a separate data sample.

### Pairing Result

- Valid input-target pairs: 312
- Training pairs: 218
- Validation pairs: 47
- Testing pairs: 47

The remaining unmatched images will be checked separately before any final dataset expansion.

##  Preprocessing Strategy

### RGB Conversion

Images are converted to RGB format to preserve the three color channels required for underwater image enhancement.

### Image Size

Images are maintained at:

256 × 256 pixels

The original images are already 256 × 256, so unnecessary resizing is avoided.

### Normalization

Pixel values are converted from:

0–255

to:

0–1

Normalization provides a consistent numerical range for model training.

### Tensor Format

Images are converted from:

Height × Width × Channels

to:

Channels × Height × Width

Example:

256 × 256 × 3

to:

3 × 256 × 256

##  Reason for Preprocessing Choices

### 256 × 256 Image Size

The dataset images are already 256 × 256 pixels. Keeping the original size avoids unnecessary resizing and possible loss of details.

### RGB Format

RGB is used because color information is important for underwater image enhancement.

### 0–1 Normalization

Normalization provides a consistent pixel range and makes the images suitable for neural-network training.

### Paired Processing

Input and target images are kept together so that every degraded image corresponds to its correct target image.

##  Augmentation Strategy

No augmentation is used in Dataset V1.

This is an image-to-image enhancement task, so input and target images must remain spatially aligned.

Unnecessary transformations could change the relationship between the input and target images.

Augmentation may be considered later if required.

##  Dataset Split Strategy

The valid input-target pairs are divided into:

| Dataset | Pairs | Percentage |
|---|---:|---:|
| Training | 218 | 70% |
| Validation | 47 | 15% |
| Testing | 47 | 15% |
| Total | 312 | 100% |

A random seed of 42 is used so that the split can be reproduced.

##  Leakage Prevention

The following steps are used to reduce data leakage:

- Input and target images are paired before splitting.
- Duplicate images are checked before splitting.
- The same input-target pair cannot appear in multiple splits.
- Train, validation, and test sets are created after pairing.
- The original dataset is kept separate from the prepared dataset.

##  Dataset Loader

A PyTorch Dataset and DataLoader were implemented.

The loader successfully produced:

Train: 218
Validation: 47
Test: 47

Input batch shape:
torch.Size([8, 3, 256, 256])

Target batch shape:
torch.Size([8, 3, 256, 256])

This confirms that the prepared images can be loaded correctly for model training.

##  Dataset V1 Structure


```text
prepared_dataset_v1/
│
├── train/
│   ├── input/
│   └── target/
│
├── val/
│   ├── input/
│   └── target/
│
├── test/
│   ├── input/
│   └── target/
│
└── preprocessing_config_v1.json

##  Preprocessing Configuration

Version: V1
Image Size: 256 × 256
Image Format: PNG
Channels: 3
Color Format: RGB
Normalization: 0–1
Batch Size: 8
Random Seed: 42
Augmentation: None
Train Split: 70%
Validation Split: 15%
Test Split: 15%

##  Data Preparation Workflow

Raw Dataset
     ↓
Quality Check
     ↓
Duplicate Check
     ↓
Input-Target Pairing
     ↓
RGB Conversion
     ↓
256 × 256 Image Size
     ↓
Normalize 0–1
     ↓
Train / Validation / Test Split
     ↓
Dataset Loader
     ↓
Model Training
     ↓
Evaluation

## . Data Storage

The original dataset will be kept unchanged.

The prepared Dataset V1 is stored separately to prevent accidental modification or loss of the original dataset.

##  Dataset V1 Status

Dataset V1 has been prepared with:

- Valid input-target pairs
- Duplicate checking
- RGB conversion
- 256 × 256 image size
- 0–1 normalization
- PyTorch Dataset and DataLoader
- 70/15/15 train-validation-test split
- No augmentation

The current prepared dataset contains:

312 valid input-target pairs

with:

218 training pairs
47 validation pairs
47 testing pairs
