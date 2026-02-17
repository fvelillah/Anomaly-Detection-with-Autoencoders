# Unsupervised Anomaly Detection and Localization in Industrial Images

This repository contains the implementation of unsupervised anomaly detection and localization using **Convolutional Autoencoders (CAEs)**. The project focuses on learning the "normal" manifold of industrial samples from the **MVTec AD** dataset (specifically the "capsule" category) to detect and localize defects without prior exposure to anomalous data.

<img width="1600" height="814" alt="csm_dataset_overview_large_6f330dede4" src="https://github.com/user-attachments/assets/9bd4da07-32e4-4252-a2ae-e7f5012d0c4f" />

## Project Overview

Detecting anomalous regions in images is critical for computer vision tasks such as industrial quality control. This project evaluates two neural architectures trained exclusively on normal images to identify defects through reconstruction error and pixel-wise heatmaps. For a detailed explanation of the methodology and results, please refer to the [full report](report_Hamre_Hurtado_Abassi.pdf).

## Architectures

### Model I: Residual Autoencoder (file "ad_model1.ipynb")

A 2D autoencoder architecture inspired by medical imaging, designed to capture essential features through a symmetrical encoder-decoder structure.

* **Encoder:** Utilizes residual downsampling blocks to maintain gradient flow while reducing spatial dimensions.


* **Decoder:** A mirrored structure that reconstructs the input image from the bottleneck representation.

<img width="8846" height="4276" alt="architecture" src="https://github.com/user-attachments/assets/3ff3affc-d5e1-489f-aadc-fce65f7d0fef" />

### Model II: Self-Supervised SDC-CCM (file "ad_model2.ipynb")

A more advanced framework based on the paper "Self-supervised training with autoencoders for visual anomaly detection" (from Bauer, et al.) that integrates state-of-the-art modules to capture high-order spatial dependencies and enhance feature reconstruction. 

<img width="531" height="625" alt="model2_arch" src="https://github.com/user-attachments/assets/e6e067d0-3927-4fea-9036-cba0a53a0815" />

* **Stacked Dilated Convolutions (SDC):** The bottleneck consists of SDC blocks with varying dilation rates (1, 2, 4, 8, 16, 32) to capture multi-scale spatial context.

* **Convex Combination Modules (CCM):** Acts as an attention-based skip connection mechanism. It dynamically combines encoder features and decoder features using a learned coefficient matrix:

<img width="661" height="457" alt="model2_arch(1)" src="https://github.com/user-attachments/assets/95149d1e-b419-49f3-8958-01ab33ef6fb3" />

* **Self-Supervised Training:** Implements an artificial corruption strategy using elastic deformations and external textures (from the DTD dataset) to simulate defects during the training phase.
  
<img width="950" height="672" alt="Captura de pantalla 2026-02-17 135243" src="https://github.com/user-attachments/assets/58251df7-ae52-4921-8b69-f32c771e1d64" />

## Results

### Performance Summary

The models were evaluated on both classification (detecting if an image is anomalous) and localization (segmenting the defect).

| Metric | Model I | Model II |
| --- | --- | --- |
| **Image ROC-AUC** | 0.65 | **0.70** |
| **Pixel ROC-AUC** | 0.74 | **0.84** |

### Visualizations

#### Anomaly Localization Heatmaps

Model II demonstrated strong localization capabilities, particularly for large defects, by generating Gaussian-filtered reconstruction error maps.

<img width="2070" height="1257" alt="heatmap_confige2" src="https://github.com/user-attachments/assets/1a7bf97a-bafe-4f51-a025-11cd7e8157b0" />

## Installation & Usage

1. Clone the repository:
```bash
git clone https://github.com/your-username/your-repo.git

```


2. Install dependencies:
```bash
pip install -r requirements.txt

```


3. Open the `notebooks/anomaly_detection.ipynb` in Google Colab or a local Jupyter environment to train or evaluate the models.


## References

Guoting Luo, Wei Xie, Ronghui Gao, Tao Zheng, Lei Chen, and Huaiqiang Sun. Unsupervised anomaly detection in brain mri: Learning abstract distribution from massive healthy brains. Computers in Biology and Medicine, 154:106610, 2023. doi:10.1016/j.compbiomed.2023.106610.

Alexander Bauer, Shinichi Nakajima, and Klaus-Robert Muller. Self-supervised training with autoencoders for visual anomaly detection, 2024.


## Credits

Developed at **Politecnico di Torino** by:

* Elisabeth Lykken Hamre 
* Fernando Velilla Hurtado 
* Moiz Ahmad Abassi 
