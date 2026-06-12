# Topology-Preserving Image Segmentation with Spatial-Aware Persistent Feature Matching

## Overview

This repository presents a topology-aware deep learning framework for image segmentation that combines pixel-level learning with topological supervision through Persistent Homology and Spatial-Aware Persistent Feature Matching (SATLoss).

The framework is designed for topology-sensitive structures where preserving connectivity, branches, and holes is as important as pixel-wise accuracy.

Applications include:

* Road Network Extraction
* Crack Segmentation
* Retinal Vessel Segmentation
* Biomedical Microscopy Image Segmentation

The proposed framework combines:

* U-Net
* Binary Cross-Entropy (BCE)
* Dice Loss
* clDice Loss
* Spatial-Aware Topological Loss (SATLoss)

to improve both segmentation accuracy and structural consistency.

---

## Motivation

Traditional segmentation models are usually optimized using pixel-wise losses such as Binary Cross-Entropy and Dice Loss.

Although these losses often produce high pixel accuracy, they may generate:

* Broken vessels
* Disconnected roads
* Fragmented cracks
* Missing branches
* Artificial holes

These structural errors can significantly affect downstream analysis despite good pixel-level performance.

To address this limitation, this work incorporates Persistent Homology and Spatial-Aware Persistent Feature Matching into the training process, encouraging predictions that preserve the topology of the target structures.

---

## Methodology

### Segmentation Architecture

The segmentation backbone is a U-Net encoder-decoder network.

```text
Input Image
     │
     ▼
 Encoder
     │
     ▼
 Bottleneck
     │
     ▼
 Decoder
     │
     ▼
Probability Map
```

Skip connections preserve fine spatial details while deeper layers capture contextual information.

---

### Hybrid Loss Function

The total training objective is

L_total = λ₁L_BCE + λ₂L_Dice + λ₃L_clDice + λ₄L_SATLoss

where

* BCE improves pixel classification
* Dice improves region overlap
* clDice preserves connectivity
* SATLoss preserves topology

---

### Spatial-Aware Persistent Feature Matching

Persistent Homology is used to extract:

* Connected Components (β₀)
* Holes (β₁)

from both predicted masks and ground-truth masks.

Unlike ordinary persistence matching, SATLoss additionally incorporates spatial information through creator and destroyer coordinates.

The matching cost is

CSA(p,q) = d_topology(p,q) + λ d_spatial(p,q)

allowing features to be matched based on both topology and image location.

---

## Datasets

### Massachusetts Road Dataset

Road extraction from satellite imagery.

Characteristics:

* Thin connected structures
* Road intersections
* Branching patterns

---

### Crack Dataset

Crack detection from pavement images.

Characteristics:

* Irregular crack geometry
* Thin elongated structures
* Connectivity-sensitive segmentation

---

### DRIVE Retinal Vessel Dataset

Retinal vessel extraction from fundus images.

Characteristics:

* Vessel trees
* Small branches
* Complex topology

---

### C. elegans Microscopy Dataset

Biomedical microscopy segmentation.

Characteristics:

* Worm structures
* Thin biological objects
* Transfer learning evaluation

---

## Experimental Results

### Road Segmentation

| Model            | Dice   | clDice | β₀ Error | β₁ Error |
| ---------------- | ------ | ------ | -------- | -------- |
| BCE Only         | 0.7204 | 0.7310 | 0.7322   | 0.1150   |
| Hybrid           | 0.7207 | 0.7471 | 0.4688   | 0.1146   |
| Hybrid + SATLoss | 0.7209 | 0.7455 | 0.4916   | 0.0998   |

---

### Crack Segmentation

| Model            | Dice   | clDice | β₀ Error | β₁ Error |
| ---------------- | ------ | ------ | -------- | -------- |
| BCE Only         | 0.6308 | 0.6002 | 1.7445   | 0.1461   |
| Hybrid           | 0.7127 | 0.7410 | 0.3251   | 0.0498   |
| Hybrid + SATLoss | 0.6752 | 0.7012 | 0.5664   | 0.0573   |

---

### Transfer Learning Results

| Dataset    | Dice Before | Dice After |
| ---------- | ----------- | ---------- |
| C. elegans | 0.1604      | 0.7449     |
| DRIVE      | 0.0000      | 0.7976     |

These results demonstrate that topology-aware features learned from road segmentation can successfully transfer to biomedical imaging tasks.

---

## Evaluation Metrics

The framework evaluates both pixel-level and topological performance.

### Pixel Metrics

* Accuracy
* Dice Score
* clDice

### Topology Metrics

* β₀ Error (Connected Components)
* β₁ Error (Holes)

Lower β₀ and β₁ values indicate better topology preservation.

---

## Repository Structure

```text
topology-preserving-segmentation-satloss/

├── README.md
├── requirements.txt
├── LICENSE
│
├── notebooks/
│   ├── 01_road_segmentation_satloss.ipynb
│   ├── 02_crack_segmentation_cldice_satloss.ipynb
│   ├── 03_celegans_transfer_learning.ipynb
│   └── 04_drive_retinal_vessel_transfer_learning.ipynb
│
├── src/
│   ├── models/
│   ├── losses/
│   ├── metrics/
│   └── utils/
│
├── results/
│   ├── figures/
│   └── tables/
│
└── docs/
    └── thesis_summary.md
```

---

## Installation

Clone the repository:

```bash
git clone https://github.com/your_username/topology-preserving-segmentation-satloss.git

cd topology-preserving-segmentation-satloss
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

## Technologies Used

* Python
* PyTorch
* NumPy
* OpenCV
* Scikit-Image
* Gudhi
* Matplotlib
* Persistent Homology
* Topological Data Analysis

---

## Research Contributions

This project contributes:

* A topology-aware segmentation framework
* Integration of SATLoss with U-Net
* Learnable topology weighting strategy
* Transfer learning from roads to biomedical datasets
* Evaluation using both segmentation and topological metrics

---

## Future Work

Future extensions include:

* Attention U-Net
* UNet++
* Transformer-based segmentation
* 3D biomedical image segmentation
* Multi-class topology-aware learning
* Adaptive topology-loss weighting

---

## Citation

If you use this work, please cite:

Mutie, J. S. (2026).

Topology-Preserving Image Segmentation with Spatial-Aware Persistent Feature Matching.

Master of Science in Mathematical Sciences,

African Institute for Mathematical Sciences (AIMS), Rwanda.

---

## Author

Josia Suku Mutie

MSc Mathematical Sciences

African Institute for Mathematical Sciences (AIMS), Rwanda

Email: [josiah.mutie@aims.ac.rw](mailto:josiah.mutie@aims.ac.rw)

GitHub: https://github.com/mutie886

LinkedIn: https://linkedin.com/in/josia-mutie-350a19210
