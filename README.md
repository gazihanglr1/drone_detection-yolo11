<div align="center">

# DroneWatch

### Real-Time UAV Detection & Multi-Object Tracking with YOLO11

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/)
[![YOLO11](https://img.shields.io/badge/Model-YOLO11-00FFFF.svg)](https://docs.ultralytics.com/models/yolo11/)
[![Tracker](https://img.shields.io/badge/Tracker-ByteTrack-orange.svg)](https://arxiv.org/abs/2110.06864)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Colab](https://img.shields.io/badge/Run%20on-Google%20Colab-F9AB00.svg)](https://colab.research.google.com/)

<br>

<img src="assets/sample_detection.png" alt="Sample Detection" width="720"/>

</div>

---

## Overview

DroneWatch, video akışlarında drone/İHA tespiti ve çoklu nesne takibi yapan uçtan uca bir bilgisayarlı görü sistemidir. YOLO11 mimarisi üzerine özel eğitilmiş bir model ile ByteTrack algoritmasını birleştirerek, hızlı ve küçük hedefleri kareler arasında tutarlı bir şekilde takip eder. Sistem ayrıca zaman içindeki tespit yoğunluğunu görselleştiren bir ısı haritası (heatmap) modülü içerir.

Temel yetenekler:
- Özel eğitilmiş YOLO11 tabanlı drone tespiti
- ByteTrack ile karesel kimlik takibi (ID tutarlılığı)
- Tespit yoğunluğu ısı haritası
- Gerçek zamanlı çıkarım (inference) performansı
- Kapsamlı değerlendirme metrikleri (precision, recall, mAP)

---

## Demo

<div align="center">

| Tespit + Takip | Yoğunluk Haritası |
|:---:|:---:|
| <img src="assets/tracking_demo.gif" width="380"/> | <img src="assets/heatmap_output.png" width="380"/> |

</div>

---

## Performance

Model, doğrulama setinde aşağıdaki metriklerle değerlendirilmiştir:

| Metrik | Değer |
|---|---|
| Precision | 0.90 |
| Recall | 0.91 |
| mAP@50 | 0.90 |
| mAP@50-95 | 0.71 |
| Inference Hızı | 14 ms/görüntü |
| Model Boyutu | 45 MB |

<div align="center">
<img src="assets/results.png" alt="Training Results" width="700"/>
</div>

---

## Dataset

| Özellik | Detay |
|---|---|
| Kaynak | [Drones Yolo11 A — Roboflow Universe](https://universe.roboflow.com/drone-a7lpy/drones-yolo11-a) |
| Görüntü Sayısı | 9,900 |
| Sınıflar | drone, bird, airplane, object |
| Format | YOLO11 (TXT annotations + data.yaml) |
| Train/Val/Test Split | %70 Train / %20 Val / %10 Test |

---

## Model Configuration

```yaml
model: yolo11s.pt
epochs: 50
imgsz: 640
batch: 16
optimizer: AdamW
lr0: 0.001
patience: 10
tracker: bytetrack.yaml
conf_threshold: 0.35
iou_threshold: 0.5
