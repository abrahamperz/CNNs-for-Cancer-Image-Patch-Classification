# 🔬 CNNs for Cancer Image Patch Classification

![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-Keras-FF6F00?logo=tensorflow&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![Best AUC](https://img.shields.io/badge/Best%20validation%20AUC-0.947-brightgreen)

A study of convolutional neural network architectures for **binary
classification of histopathology image patches** — distinguishing 96×96 px
patches that contain metastatic tumor tissue from those that do not.

The work is based on the
[Histopathologic Cancer Detection](https://www.kaggle.com/competitions/histopathologic-cancer-detection)
competition on Kaggle, whose data comes from the
[PatchCamelyon (PCam)](https://github.com/basveeling/pcam) benchmark, derived in
turn from the [Camelyon16](https://camelyon16.grand-challenge.org/) challenge.

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
cropped from a larger digital pathology scan, predict whether the center
32×32 px region contains metastatic tumor tissue. Automating this kind of
screening can help pathologists triage slides faster and more consistently.

The full workflow — EDA, preprocessing, model building, training, and
evaluation — lives in a single, self-documenting notebook.

## Dataset

| Property | Value |
|---|---|
| Source | [PatchCamelyon (PCam)](https://github.com/basveeling/pcam), via the [Kaggle Histopathologic Cancer Detection](https://www.kaggle.com/competitions/histopathologic-cancer-detection) dataset |
| Labeled examples | 220,025 |
| Image format | 96×96 px, 3-channel RGB |
| Positive class share | ~40% |
| Evaluation metric | Area under the ROC curve (AUC) |

> The dataset is **not** included in this repository. Download it from the
> source (see [Getting Started](#getting-started)).

## Approach

1. **EDA** — inspect class balance, view sample patches, and check per-channel
   RGB statistics and pixel-intensity distributions.
2. **Preprocessing** — train on a **balanced subset** (equal positive/negative
   examples) to fit within memory limits. Normalization (`Rescaling`) and
   center-cropping are baked into the model as its first layers.
3. **Modeling** — five models of increasing sophistication are trained and
   compared, each isolating the effect of one design choice, all using the Adam
   optimizer, binary cross-entropy loss, and early stopping.
4. **Evaluation** — training/validation curves plus validation AUC on a held-out
   20% split.

| Model | Technique it introduces | Parameters |
|---|---|---:|
| **Model #1** | Baseline — 3 conv blocks + 1 dense head | 159,041 |
| **Model #2** | 5 conv blocks + batch normalization | 1,702,145 |
| **Model #3** | Deeper dense head (more capacity) | 2,610,945 |
| **Model #4** | Model #2 + data augmentation | 1,702,145 |
| **Model #5** | Transfer learning — MobileNetV2 + fine-tuning | ~2.3M |

## Results

All models are compared on the same held-out **20% validation split** using
**AUC**. Best result: the **fine-tuned MobileNetV2** (Model #5) at **0.947
validation AUC**.

| Model | Approach | Validation AUC |
|:------|:---------|:--------------:|
| Model #1 | 3-block CNN (baseline) | 0.856 |
| Model #2 | 5-block CNN + batch norm | 0.925 |
| Model #3 | 5-block CNN + deep head | 0.930\* |
| Model #4 | Model #2 + augmentation | 0.914 |
| Model #5 | MobileNetV2, frozen | 0.914 |
| **Model #5** | **MobileNetV2, fine-tuned** | **0.947** 🏆 |

\* Model #3 reaches a high peak, but its validation loss diverges, so the score
is not trustworthy — more capacity without more regularization simply overfits.

**Key findings**
- **Capacity has diminishing returns** — the mid-sized Model #2 beat the heavier
  Model #3.
- **Augmentation improved generalization** more than peak score: Model #4's
  validation loss stopped diverging, giving the cleanest from-scratch curves.
- **Transfer learning was the highest-leverage move** — a pre-trained MobileNetV2,
  then fine-tuned, reached the best AUC with far less training than the custom
  CNNs.
- **More data was not a guaranteed win** — doubling the balanced training set did
  not improve results here.

## Repository Structure

```
CNNs-for-Cancer-Image-Patch-Classification/
├── cnn-medical-image-classification.ipynb   # Full study: EDA → modeling → results
└── README.md
```

## Getting Started

1. **Clone the repository**
   ```bash
   git clone https://github.com/abrahamperz/CNNs-for-Cancer-Image-Patch-Classification.git
   cd CNNs-for-Cancer-Image-Patch-Classification
   ```
2. **Get the data.** Either run the notebook directly on Kaggle (the dataset is
   attached automatically), or download it locally via the
   [Kaggle CLI](https://github.com/Kaggle/kaggle-api):
   ```bash
   kaggle competitions download -c histopathologic-cancer-detection
   ```
   The notebook auto-locates the data folder that contains `train_labels.csv`;
   adjust the paths if you run it elsewhere.
3. **Install dependencies**
   ```bash
   pip install tensorflow keras numpy pandas matplotlib opencv-python tifffile
   ```
4. **Run the notebook**
   ```bash
   jupyter notebook cnn-medical-image-classification.ipynb
   ```

## Future Work

- Use **streaming input pipelines** (`tf.data`) to train on the full dataset
  without memory limits.
- Try **stronger backbones** (EfficientNet, ResNet) and systematically tune how
  many layers to unfreeze during fine-tuning.
- Add **learning-rate schedules** and other non-default hyperparameters to tame
  the validation-metric swings seen in the from-scratch models.
- Explore **test-time augmentation** and model ensembling for a further
  robustness gain.
