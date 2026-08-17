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

Unmanned aerial vehicles (UAVs) are becoming increasingly widespread across a variety of applications, including security, delivery, agriculture, and surveillance. This rapid adoption has also increased the need for reliable aerial-space monitoring and raised concerns regarding airspace security and privacy.

This project provides real-time drone detection and tracking using **YOLO11x**, one of the latest models in the YOLO (You Only Look Once) object detection family. The architecture incorporates **C3k2 blocks**, **SPPF**, and **C2PSA spatial attention**, enabling the system to detect small and fast-moving targets more effectively in complex and dynamic backgrounds.

---

## 2. Dataset and Preprocessing

A custom, manually annotated UAV dataset was used for both the training and validation stages.

* **Dataset Structure:**

  * More than 1,000 training images and 347 validation images containing 369 annotated object instances.
  * The dataset uses the YOLO annotation format with a single class (`drone`).
* **Preprocessing & Augmentation:**

  * Images were resized to **640x640** while preserving their original aspect ratio through *letterboxing*.
  * To improve generalization and robustness, **Mosaic augmentation**, **Random scaling**, and **HSV color jitter** techniques were applied.

```yaml
# data.yaml
train: ../drone_dataset/train
val: ../drone_dataset/valid
nc: 1
names: ['drone']

```

---

## 3. Training and Model Performance

The training process was performed on **Google Colab Pro+** using an **NVIDIA L4 GPU** for accelerated model training.

* **Parameter Count:** ~56.8M
* **Image Size:** 640x640
* **Batch Size / Epoch:** 16 / 32
* **Optimizer:** AdamW (Initial LR: 0.001)

### Validation Set Results:

| Metric        | Value |
| ------------- | ----- |
| **Precision** | 0.922 |
| **Recall**    | 0.831 |
| **mAP@50**    | 0.905 |
| **mAP@50-95** | 0.546 |

### Processing Times (Speed / Latency):

* **Preprocessing:** 0.1 ms / image
* **Inference:** 8.9 ms / image
* **Postprocessing:** 1.0 ms / image

---

## 4. Requirements

The main libraries and dependencies required to run the project are:

* Python 3.8+
* PyTorch 2.6.0+cu124
* Ultralytics YOLOv11
* OpenCV
* CUDA-compatible GPU

---

## 5. Inference and Tracking

The model supports video-based detection together with multi-object tracking (MOT) using algorithms such as **ByteTrack** and **DeepSORT**. These tracking methods allow detected objects to maintain consistent identities across consecutive video frames, enabling continuous monitoring of individual drones.

```python
from ultralytics import YOLO

# Load the trained model
model = YOLO("best.pt")

# Run detection and tracking on the video
results = model.track(
    source="video.mp4",
    conf=0.3,
    iou=0.5,
    show=True
)

```

---

## 6. Heatmap Visualization

A heatmap visualization module is included to analyze the spatial and temporal distribution of detected drone movements. By accumulating detection locations over time, the system can highlight areas with higher drone activity and provide a clearer representation of frequently used flight paths and zones.

```python
import cv2
from ultralytics import YOLO

cap = cv2.VideoCapture("input_video.mp4")
model = YOLO("best.pt")

# Frames are processed and detections are accumulated
# to generate a cumulative spatial heatmap.

```

---

## 7. Limitations and Recommendations

* **Limitations:**

* Due to the relatively limited dataset size, additional validation and fine-tuning may be required to achieve robust generalization across significantly different environmental conditions.

* The current implementation uses a single detection class (`drone`) and therefore does not distinguish between different drone models or types.

* Detection performance may decrease in highly cluttered backgrounds, poor visibility, or challenging weather conditions.

* **Recommendations:**

* Area-specific fine-tuning with data collected from the target deployment environment is recommended before real-world deployment.

* Confidence and IoU thresholds should be optimized according to the specific operational scenario.

---

## 8. License & Related Work

* **License:** MIT License
* **Other YOLO Projects:**
* [Drone Detection using YOLOv8x](https://github.com/doguilmak)
* [Drone Detection using YOLOv7](https://github.com/doguilmak)
