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

İnsansız hava araçları (İHA / UAV), güvenlikten teslimata ve tarıma kadar birçok alanda yaygın şekilde kullanılmaktadır. Bu yaygınlaşma, hava sahası güvenliği ve gizlilik kaygılarını da beraberinde getirmektedir.

Bu proje; YOLO (You Only Look Once) nesne tespit ailesinin en güncel sürümlerinden biri olan **YOLOv11x** mimarisini kullanarak gerçek zamanlı drone tespiti ve takibi sunar. Mimaride yer alan **C3k2 blokları**, **SPPF** ve **C2PSA spatial attention** özellikleri sayesinde karmaşık ve dinamik arka planlarda küçük, hızlı hareket eden hedefler yüksek hassasiyetle tespit edilir.

---

## 2. Dataset and Preprocessing

Eğitim ve doğrulama süreçlerinde özel olarak etiketlenmiş İHA veri seti kullanılmıştır.

* **Veri Yapısı:**
  * Toplam 1,000+ eğitim görseli ve 347 doğrulama görseli (369 nesne örneği).
  * YOLO formatında tek sınıf (`drone`) olarak etiketlenmiştir.
* **Ön İşleme & Artırma (Augmentation):**
  * Görseller **640x640** boyutuna getirilmiş ve en-boy oranını korumak için *letterboxing* uygulanmıştır.
  * Genelleştirme yeteneğini artırmak için **Mosaic augmentation**, **Random scaling** ve **HSV color jitter** teknikleri kullanılmıştır.

```yaml
# data.yaml
train: ../drone_dataset/train
val: ../drone_dataset/valid
nc: 1
names: ['drone']

```

---

## 3. Training and Model Performance

Eğitim süreci **Google Colab Pro+** üzerinde **NVIDIA L4 GPU** ivmelendirmesi ile yürütülmüştür.

* **Parametre Sayısı:** ~56.8M
* **Görüntü Boyutu:** 640x640
* **Batch Size / Epoch:** 16 / 32
* **Optimizer:** AdamW (Initial LR: 0.001)

### Doğrulama Seti Sonuçları:

| Metrik | Değer |
| --- | --- |
| **Precision** | 0.922 |
| **Recall** | 0.831 |
| **mAP@50** | 0.905 |
| **mAP@50-95** | 0.546 |

### İşlem Süreleri (Speed / Latency):

* **Preprocessing:** 0.1 ms / görsel
* **Inference:** 8.9 ms / görsel
* **Postprocessing:** 1.0 ms / görsel

---

## 4. Requirements

Projenin çalıştırılması için gerekli temel kütüphaneler ve bağımlılıklar:

* Python 3.8+
* PyTorch 2.6.0+cu124
* Ultralytics YOLOv11
* OpenCV
* CUDA-compatible GPU

---

## 5. Inference and Tracking

Model, video akışlarında tespit ile birlikte çoklu nesne takibi (MOT - **ByteTrack** / **DeepSORT**) algoritmalarını entegre ederek objelerin kimliklerini (ID) korur.

```python
from ultralytics import YOLO

# Eğitilmiş modeli yükle
model = YOLO("best.pt")

# Video üzerinde tespit ve takip çalıştır
results = model.track(
    source="video.mp4",
    conf=0.3,
    iou=0.5,
    show=True
)

```

---

## 6. Heatmap Visualization

Drone hareketlerinin uzamsal ve zamansal yoğunluğunu analiz etmek için sisteme ısı haritası görselleştirmesi eklenmiştir. Bu yöntem, uçuş koridorlarını ve sık kullanılan bölgeleri tespit etmeyi kolaylaştırır.

```python
import cv2
from ultralytics import YOLO

cap = cv2.VideoCapture("input_video.mp4")
model = YOLO("best.pt")

# Kareler işlenerek tespitlerin kumulatif yoğunluğu ısı haritası olarak katmanlanır.

```

---

## 7. Limitations and Recommendations

* **Sınırlılıklar:**
* Veri seti büyüklüğü sınırlı olduğundan farklı çevre koşullarında genelleştirme desteğine ihtiyaç duyabilir.
* Tek sınıflı tespit (`drone`) yapar; farklı drone modelleri arasında ayrım yapmaz.
* Yoğun ve gürültülü arka planlarda veya aşırı olumsuz hava koşullarında kaçırılan tespitler olabilir.


* **Öneriler:**
* Farklı ortamlarda canlı dağıtım öncesi alan bazlı fine-tuning önerilir.
* Senaryoya göre güven ve IoU eşik değerleri optimize edilmelidir.



---

## 8. License & Related Work

* **License:** MIT License
* **Diğer YOLO Projeleri:**
* [Drone Detection using YOLOv8x](https://github.com/doguilmak)
* [Drone Detection using YOLOv7](https://github.com/doguilmak)



```

```
