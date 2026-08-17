<div align="center">

# DroneWatch

### Real-Time UAV Detection & Multi-Object Tracking with YOLO11x

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/)
[![YOLO11](https://img.shields.io/badge/Model-YOLO11x-00FFFF.svg)](https://docs.ultralytics.com/models/yolo11/)
[![Tracker](https://img.shields.io/badge/Tracker-ByteTrack-orange.svg)](https://arxiv.org/abs/2110.06864)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Colab](https://img.shields.io/badge/Run%20on-Google%20Colab-F9AB00.svg)](https://colab.research.google.com/)

<br>

<img src="assets/sample_detection.png" alt="Sample Detection" width="720"/>

</div>

---

## 1. Project Overview

Unmanned aerial vehicles (UAVs / drones) are increasingly utilized across various domains, ranging from surveillance and security to logistics and agriculture. However, this rapid integration also introduces critical airspace security and privacy concerns.

This repository features a robust framework for real-time drone detection and multi-object tracking powered by **YOLOv11x**, one of the state-of-the-art vision architectures in the YOLO lineage. Leveraging structural enhancements such as **C3k2 blocks**, **SPPF**, and **C2PSA spatial attention**, the system reliably identifies small and fast-moving aerial targets across complex and dynamic operational environments.

---

## 2. Dataset and Preprocessing

The model was trained and evaluated using a custom annotated dataset tailored for UAV detection.

* **Data Structure:**
  * Comprises over 1,000 training images alongside 347 validation images (containing 369 annotated target instances).
  * Annotated in standard YOLO format under a single target class (`drone`).
* **Preprocessing & Augmentation:**
  * Input frames are standardized to **640x640** dimensions using *letterboxing* to maintain intrinsic aspect ratios.
  * To enhance model robustness and generalization across unseen environments, pipelines incorporate **Mosaic augmentation**, **Random scaling**, and **HSV color jitter**.

```yaml
# data.yaml
train: ../drone_dataset/train
val: ../drone_dataset/valid
nc: 1
names: ['drone']
