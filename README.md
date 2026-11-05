# Deep Learning-Based Brain Lesion Segmentation from Multimodal MRI

**Medical Image Analysis | Deep Learning | Computer Vision | Biomedical AI**

## Overview

Medical image segmentation is a fundamental task in computer-aided diagnosis, enabling the identification and delineation of pathological structures from medical imaging data. Accurate brain lesion segmentation from Magnetic Resonance Imaging (MRI) remains challenging due to variations in lesion appearance, heterogeneous image intensity distributions, anatomical complexity, and differences in imaging modalities.

This project presents a deep learning-based brain lesion segmentation pipeline developed using the **MOTUM v2.2 MRI dataset**. The primary objective is to investigate automated lesion segmentation through an end-to-end experimental workflow involving medical image preprocessing, dataset preparation, model development, training, validation, and quantitative performance evaluation.

The implementation uses a patient-level dataset splitting strategy and evaluates segmentation performance using multiple complementary metrics, including Dice Similarity Coefficient (DSC), Intersection over Union (IoU), Precision, Recall, F1-score, and 95th Percentile Hausdorff Distance (HD95).

Beyond overall segmentation performance, the project investigates modality-wise performance differences, providing an opportunity to examine how MRI modality characteristics influence segmentation accuracy and boundary delineation.

## Research Objectives

The main objectives of this research project are:

- Develop a deep learning-based pipeline for automated brain lesion segmentation from MRI.
- Implement systematic medical image preprocessing and slice preparation.
- Establish patient-level training, validation, and testing partitions to reduce patient-level data leakage.
- Investigate segmentation performance across available MRI modalities.
- Evaluate segmentation accuracy using overlap-based and boundary-based metrics.
- Examine the strengths and limitations of the trained model.
- Establish a reproducible experimental foundation for further research in medical image segmentation.

## Dataset Description

**Dataset:** MOTUM v2.2

The experiments use a dataset containing 132 image-mask pairs from 66 patients.

| Dataset partition | Patients |
|:---|---:|
| Training | 46 |
| Validation | 10 |
| Testing | 10 |
| **Total** | **66** |

### Slice Distribution

| Partition | Number of slices |
|:---|---:|
| Training | 1,177 |
| Validation | 237 |
| Testing | 237 |

Training slice distribution:

- Positive slices: 795
- Negative slices: 382

The dataset is partitioned at the patient level rather than randomly distributing individual slices. This is an important consideration in medical imaging experiments because slices from the same patient can contain highly correlated anatomical and pathological information.

The original dataset should be obtained from its authorized source. Patient images and segmentation masks are not redistributed in this repository.

## Methodology

The experimental pipeline is organized into the following stages.

### 1. Data Preparation and Preprocessing

The initial stage involves loading the MRI image-mask pairs, preparing the dataset, and generating suitable two-dimensional slices for model development.

The preprocessing workflow is designed to maintain correspondence between MRI images and their associated segmentation masks.

### 2. Patient-Level Dataset Partitioning

The dataset is divided into three independent partitions:

- Training set: 46 patients
- Validation set: 10 patients
- Testing set: 10 patients

The training partition is used for model optimization, the validation partition for model selection, and the testing partition for final performance assessment.

### 3. Deep Learning Model

A deep learning segmentation model with approximately **2,029,345 parameters** is implemented and trained for brain lesion segmentation.

The model learns spatial representations from MRI input images and produces segmentation predictions.

The training workflow includes:

- Model initialization and optimization.
- Training and validation loss monitoring.
- Validation-based Dice evaluation.
- Learning-rate configuration.
- Checkpoint saving and resumption.
- Selection of the best validation checkpoint.

### 4. Model Training and Checkpoint Selection

Training was conducted for 15 epochs in the reported experimental run, with checkpoint resumption from a previous training state.

The best validation Dice score was obtained at epoch 13.

| Training parameter | Value |
|:---|---:|
| Model parameters | 2,029,345 |
| Training epochs in resumed run | 15 |
| Best validation epoch | 13 |
| Best validation Dice | 0.6555 |
| Learning rate at reported epochs | 0.000250 |
| Training time | 44.23 minutes |

The best-performing validation checkpoint was subsequently loaded for testing.

## Experimental Results

The trained model was evaluated using the held-out test partition.

### Overall Test Performance

| Evaluation Metric | Result |
|:---|---:|
| Patient-pooled Dice | **82.75%** |
| Patient-pooled IoU | **71.31%** |
| Positive-slice Dice | 78.51% |
| Positive-slice IoU | 69.01% |
| Pixel Precision | 83.55% |
| Pixel Recall | 86.65% |
| Pixel F1-score | 85.07% |
| HD95 | 12.12 mm |

The patient-pooled Dice score of 82.75% indicates substantial spatial overlap between predicted and reference segmentation masks under the implemented evaluation procedure.

The pixel-level precision and recall values indicate that the model identifies a substantial proportion of target lesion pixels while maintaining a reasonable level of prediction precision.

The HD95 value provides complementary information about segmentation boundary discrepancies that cannot be fully characterized by overlap-based metrics alone.

### Modality-Wise Performance

The following results demonstrate differences in segmentation performance between the evaluated MRI modalities.

| Modality | Dice | IoU | HD95 (mm) |
|:---|---:|---:|---:|
| FLAIR | **84.02%** | **74.79%** | **9.48** |
| T1CE | 69.75% | 59.81% | 16.31 |

FLAIR achieved higher Dice and IoU scores and a lower HD95 value than T1CE in the reported modality-wise evaluation.

This difference motivates further investigation into modality-specific image characteristics, lesion contrast, preprocessing strategies, and their influence on segmentation performance.

**Evaluation note:** The reported patient-pooled Dice and IoU are associated with the FLAIR evaluation. The available evaluation output does not establish a combined patient-pooled score across both modalities. The aggregation and positive-slice selection procedures should be considered when comparing metrics.

## Technical Stack

The project uses the following technologies and research tools:

- **Python:** Experimental implementation.
- **PyTorch:** Deep learning model development and training.
- **NumPy:** Numerical computation and array manipulation.
- **Google Colab:** Cloud-based experimentation and GPU-assisted training.
- **Medical image processing libraries:** MRI data preparation and preprocessing.
- **Matplotlib:** Experimental visualization and result analysis.

## Repository Structure

```text
Deep-Learning-for-Multimodal-MRI-Segmentation/
│
├── README.md
│
├── notebooks/
│   └── Multimodal_MRI_Brain_Lesion_Segmentation.ipynb
│
├── results/
│   ├── training_curves.png
│   ├── modality_comparison.png
│   └── segmentation_examples.png
│
├── requirements.txt
│
├── LICENSE
│
└── .gitignore
```

## Reproducibility

The primary experimental implementation is provided as a Jupyter notebook.

To reproduce the experiments:

1. Obtain the MOTUM v2.2 dataset through its authorized distribution source.
2. Configure the dataset root directory.
3. Install the required Python dependencies.
4. Open the notebook in Google Colab or a compatible Jupyter environment.
5. Execute the preprocessing and dataset preparation stages.
6. Configure the training parameters.
7. Train the segmentation model.
8. Evaluate the selected checkpoint using the testing partition.

Dataset paths, hardware availability, library versions, and training configurations may need adjustment depending on the execution environment.

## Limitations and Future Research Directions

Although the experimental results demonstrate promising segmentation performance, several aspects require further investigation.

**1. Generalization and Statistical Reliability**

The held-out test partition contains 10 patients. Additional patient-level experiments and repeated runs with different random seeds are needed to assess the stability and generalization of the results.

**2. Modality-Specific Performance**

The difference between FLAIR and T1CE performance motivates further investigation into modality-specific preprocessing, image normalization, and feature representation.

**3. Boundary Accuracy**

The HD95 results indicate that segmentation boundary errors remain an important area for improvement.

**4. Comparative Evaluation**

Future experiments should compare the proposed implementation against established segmentation architectures, including U-Net, Attention U-Net, and nnU-Net, using consistent data partitions and evaluation procedures.

**5. Model Robustness**

Further work could investigate data augmentation, class imbalance, sensitivity to hyperparameters, and performance across different patient populations.

**6. Statistical Analysis**

Reporting patient-level mean, standard deviation, confidence intervals, and appropriate statistical comparisons would strengthen the reliability of the experimental findings.

## Conclusion

This project establishes an end-to-end deep learning workflow for brain lesion segmentation using MRI data.

The experimental results demonstrate a patient-pooled Dice score of 82.75% in the reported FLAIR evaluation, alongside quantitative modality-wise comparisons and boundary-based assessment.

More importantly, the project provides a foundation for investigating medical image segmentation challenges, including modality-dependent performance, patient-level generalization, and boundary accuracy.

The next research stage will focus on systematic baseline comparisons, improved experimental reproducibility, and more comprehensive patient-level evaluation.

## Research Interests

- Medical Image Segmentation
- Deep Learning for Healthcare
- Biomedical Image Analysis
- Multimodal MRI Analysis
- Computer Vision
- AI-Assisted Medical Diagnosis
- Robust and Generalizable Machine Learning

## Acknowledgements

The experiments use the MOTUM v2.2 dataset. Appropriate dataset attribution and citations should be included according to the original dataset documentation and licensing requirements.

---

**Research Project:** Brain Lesion Segmentation from MRI  
**Implementation:** Python / PyTorch  
**Environment:** Google Colab  
**Status:** Experimental implementation and ongoing research development
