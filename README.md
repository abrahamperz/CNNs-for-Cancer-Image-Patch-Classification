# Cancer Detection Using Convolutional Neural Networks (CNN)

## Course Overview
This project is part of **CSCA 5642: Machine Learning**, a course offered by CU Boulder.
- Focuses on the application of deep learning techniques, particularly Convolutional Neural Networks (CNNs), for image classification tasks.
- Utilizes Python and Jupyter Notebook for model development and experimentation.

### Key Learning Outcomes:
- Understanding the architecture and training of CNNs for binary image classification.
- Implementing data preprocessing and augmentation techniques to enhance model performance.
- Evaluating model accuracy and loss over training epochs.
- Gaining practical experience in applying deep learning models to real-world datasets.

## Project Summary
**Objective:**
Develop a CNN model to classify histopathologic images of lymph node sections, determining the presence or absence of metastatic cancer.

**Significance:**
Accurate detection of metastatic cancer in histopathologic images is crucial for timely diagnosis and treatment planning. Automating this process with deep learning models can assist pathologists in making more efficient and accurate assessments.

## Project Components

### Introduction
The project involves training a CNN to perform binary classification on histopathologic images. The goal is to predict whether a given image patch contains metastatic tissue.

### Methodology
1. **Data Preparation:**
   - Loading and preprocessing histopathologic image data.
   - Normalizing image pixel values and resizing images to a consistent dimension.
   - Splitting the dataset into training and validation sets.

2. **Model Development:**
   - Constructing a CNN architecture with convolutional, pooling, and dense layers.
   - Compiling the model with appropriate loss functions and optimizers.
   - Training the model over several epochs while monitoring performance metrics.

3. **Evaluation Metrics:**
   - Tracking training and validation accuracy and loss to assess model performance.
   - Utilizing confusion matrices and ROC curves for a comprehensive evaluation.

### Conclusion
The CNN model achieved satisfactory performance in classifying histopathologic images for cancer detection. The results demonstrate the potential of deep learning models in assisting medical image analysis tasks.

## Model Used
- **Convolutional Neural Network (CNN):** A custom-built architecture designed for binary classification of histopathologic images, implemented using TensorFlow and Keras.

## Future Work
- Experiment with deeper and more complex CNN architectures to improve classification accuracy.
- Implement data augmentation techniques to increase the diversity of the training dataset.
- Explore transfer learning approaches using pre-trained models to enhance performance.

## Repository Contents
- **Notebook:** `csca-5642-week-3-cnn-cancer-detection.ipynb` – Contains the complete code for data preprocessing, model construction, training, and evaluation.
- **Data:** The dataset used for this project is sourced from the [Kaggle Histopathologic Cancer Detection competition](https://www.kaggle.com/competitions/histopathologic-cancer-detection).
