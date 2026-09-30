# 🔬 Histopathologic Cancer Detection with CNNs

![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-Keras-FF6F00?logo=tensorflow&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![Kaggle](https://img.shields.io/badge/Kaggle-Competition-20BEFF?logo=kaggle&logoColor=white)
![Best AUC](https://img.shields.io/badge/Best%20public%20AUC-0.9216-brightgreen)

Convolutional Neural Networks that classify histopathologic image patches as
**cancerous** or **non-cancerous**, built for the Kaggle
[Histopathologic Cancer Detection](https://www.kaggle.com/competitions/histopathologic-cancer-detection)
competition. Course project for **CSCA 5642 – Introduction to Deep Learning**
(University of Colorado Boulder).

---

## Table of Contents
- [Overview](#overview)
- [Dataset](#dataset)
- [Approach](#approach)
- [Results](#results)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
- [Future Work](#future-work)

---

## Overview

The task is a **binary image classification** problem: given a 96×96 px patch
extracted from a larger digital pathology scan, predict whether the center
32×32 px region contains metastatic tumor tissue. Automating this kind of
detection can help pathologists review slides faster and more consistently.

The full workflow — EDA, preprocessing, model building, training, and
evaluation — lives in a single, self-documenting notebook.

## Dataset

| Property | Value |
|---|---|
| Source | [Kaggle Histopathologic Cancer Detection](https://www.kaggle.com/competitions/histopathologic-cancer-detection) |
| Training examples | 220,025 |
| Image format | 96×96 px, 3-channel RGB, TIFF (`.tif`) |
| Positive class share | ~40% |
| Evaluation metric | Area under the ROC curve (AUC) |

> The dataset is **not** included in this repository. Download it from the
> competition page (see [Getting Started](#getting-started)).

## Approach

1. **EDA** — inspect class balance, view sample patches, and check per-channel
   RGB statistics and pixel-intensity distributions.
2. **Preprocessing** — train on a **balanced subset** (equal positive/negative
   examples) to fit within Kaggle's memory limits. Normalization
   (`Rescaling`) and center-cropping (`Cropping2D`) are baked into the model as
   its first layers.
3. **Modeling** — three CNN architectures of increasing capacity are trained
   and compared, all using the Adam optimizer, binary cross-entropy loss, and
   early stopping.
4. **Evaluation** — training/validation curves plus Kaggle public scores.

| Model | Architecture | Parameters |
|---|---|---:|
| **Model #1** | 3 conv blocks + 1 dense head (baseline) | 159,041 |
| **Model #2** | 5 conv blocks + batch norm | 1,702,145 |
| **Model #3** | 5 conv blocks + deeper dense head | 2,610,945 |

## Results

Best result: **Model #2** with a public AUC of **0.9216** on a balanced 40k
training set.

| Model | 40k set (AUC) | 100k set (AUC) |
|:------|:-------------:|:--------------:|
| Model #1 | 0.8557 | 0.8455 |
| **Model #2** | **0.9216** 🏆 | 0.9132 |
| Model #3 | 0.9063 | 0.9088 |

**Key findings**
- More parameters did not guarantee better performance — the mid-sized Model #2
  beat the heavier Model #3.
- More than doubling the training set (40k → 100k) did **not** improve scores,
  suggesting it pays to iterate on architecture first on a smaller set.

## Repository Structure

```
CSCA5642-CNN-Cancer-Detection-Kaggle/
├── csca-5642-week-3-cnn-cancer-detection.ipynb   # Full analysis: EDA → modeling → results
└── README.md
```

## Getting Started

1. **Clone the repository**
   ```bash
   git clone https://github.com/<your-username>/CSCA5642-CNN-Cancer-Detection-Kaggle.git
   cd CSCA5642-CNN-Cancer-Detection-Kaggle
   ```
2. **Get the data.** Either run the notebook directly on Kaggle (the dataset is
   attached automatically), or download it locally via the
   [Kaggle CLI](https://github.com/Kaggle/kaggle-api):
   ```bash
   kaggle competitions download -c histopathologic-cancer-detection
   ```
   The notebook expects the data under
   `/kaggle/input/histopathologic-cancer-detection/`; adjust the paths if you
   run it elsewhere.
3. **Install dependencies**
   ```bash
   pip install tensorflow keras numpy pandas matplotlib opencv-python tifffile
   ```
4. **Run the notebook**
   ```bash
   jupyter notebook csca-5642-week-3-cnn-cancer-detection.ipynb
   ```

## Future Work

- Add **data augmentation** (rotations, flips, skews) to enlarge and diversify
  the training set.
- Use **streaming input pipelines** (`tf.data`) to train on the full dataset
  without memory limits.
- Explore **transfer learning** with pre-trained backbones.
- Tune non-default hyperparameters (learning rate schedules, conv strides,
  batch-norm momentum).
