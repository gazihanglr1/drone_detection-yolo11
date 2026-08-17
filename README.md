<div align="center">

# 🛸 DroneWatch

### Real-Time UAV Detection & Multi-Object Tracking with YOLO11

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/)
[![YOLO11](https://img.shields.io/badge/Model-YOLO11-00FFFF.svg)](https://docs.ultralytics.com/models/yolo11/)
[![Tracker](https://img.shields.io/badge/Tracker-ByteTrack-orange.svg)](https://arxiv.org/abs/2110.06864)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Colab](https://img.shields.io/badge/Run%20on-Google%20Colab-F9AB00.svg)](https://colab.research.google.com/)

<br>

<img src="assets/sample_detection.png" alt="Sample Detection" width="720"/>

*<sub>PLACEHOLDER — eğitim sonrası örnek tespit ekran görüntüsü buraya gelecek</sub>*

</div>

---

## 📖 Overview

**DroneWatch**, video akışlarında drone/İHA tespiti ve çoklu nesne takibi yapan uçtan uca bir bilgisayarlı görü sistemidir. **YOLO11** mimarisi üzerine özel eğitilmiş bir model ile **ByteTrack** algoritmasını birleştirerek, hızlı ve küçük hedefleri kareler arasında tutarlı bir şekilde takip eder. Sistem ayrıca zaman içindeki tespit yoğunluğunu görselleştiren bir ısı haritası (heatmap) modülü içerir.

**Temel yetenekler:**
- 🎯 Özel eğitilmiş YOLO11 tabanlı drone tespiti
- 🔄 ByteTrack ile karesel kimlik takibi (ID tutarlılığı)
- 🔥 Tespit yoğunluğu ısı haritası
- ⚡ Gerçek zamanlı çıkarım (inference) performansı
- 📊 Kapsamlı değerlendirme metrikleri (precision, recall, mAP)

---

## 🎬 Demo

<div align="center">

| Tespit + Takip | Yoğunluk Haritası |
|:---:|:---:|
| <img src="assets/tracking_demo.gif" width="380"/> | <img src="assets/heatmap_output.png" width="380"/> |
| *PLACEHOLDER — takip GIF'i* | *PLACEHOLDER — heatmap çıktısı* |

</div>

---

## 📊 Performance

Model, doğrulama setinde aşağıdaki metriklerle değerlendirilmiştir:

| Metrik | Değer |
|---|---|
| **Precision** | `PLACEHOLDER` |
| **Recall** | `PLACEHOLDER` |
| **mAP@50** | `PLACEHOLDER` |
| **mAP@50-95** | `PLACEHOLDER` |
| **Inference Hızı** | `PLACEHOLDER` ms/görüntü |
| **Model Boyutu** | `PLACEHOLDER` MB |

<div align="center">
<img src="assets/results.png" alt="Training Results" width="700"/>

*<sub>PLACEHOLDER — Ultralytics eğitim sonuç grafiği (loss/mAP eğrileri)</sub>*
</div>

---

## 🗂️ Dataset

| Özellik | Detay |
|---|---|
| **Kaynak** | [Drones Yolo11 A — Roboflow Universe](https://universe.roboflow.com/drone-a7lpy/drones-yolo11-a) |
| **Görüntü Sayısı** | 9,900 |
| **Sınıflar** | `drone`, `bird`, `airplane`, `object` |
| **Format** | YOLO11 (TXT annotations + `data.yaml`) |
| **Train/Val Split** | `PLACEHOLDER` |

---

## ⚙️ Model Configuration

```yaml
model: yolo11n.pt
epochs: 50
imgsz: 640
batch: 16
optimizer: AdamW
lr0: 0.001
patience: 10
tracker: bytetrack.yaml
conf_threshold: 0.35
iou_threshold: 0.5
```

---

## 🚀 Getting Started

### Prerequisites
```bash
Python 3.10+
CUDA-compatible GPU (opsiyonel, önerilir)
```

### Installation
```bash
git clone https://github.com/KULLANICI_ADINIZ/dronewatch-yolo11.git
cd dronewatch-yolo11
pip install -r requirements.txt
```

### Usage

**1. Google Colab üzerinden (önerilen):**
`drone_watch.ipynb` dosyasını [Google Colab](https://colab.research.google.com/)'da açın ve hücreleri sırayla çalıştırın.

**2. Yerel ortamda:**
```python
from ultralytics import YOLO

model = YOLO("PLACEHOLDER_best.pt")
results = model.track(
    source="your_video.mp4",
    conf=0.35,
    iou=0.5,
    tracker="bytetrack.yaml",
    save=True
)
```

---

## 📁 Project Structure

```
dronewatch-yolo11/
├── drone_watch.ipynb        # Ana notebook (eğitim + inference pipeline)
├── PROJECT_REPORT.md         # Detaylı teknik dokümantasyon
├── requirements.txt          # Bağımlılıklar
├── LICENSE                   # MIT
├── .gitignore
└── assets/                   # Görseller ve demo çıktıları
```

---

## 🛣️ Roadmap

- [ ] Daha geniş/çeşitli veri seti ile yeniden eğitim
- [ ] `yolo11s` / `yolo11m` model karşılaştırması
- [ ] Gerçek zamanlı webcam / RTSP stream desteği
- [ ] Streamlit tabanlı web arayüzü
- [ ] Docker containerization
- [ ] ONNX / TensorRT export ile hızlandırma

---

## 🧠 Technical Approach

<details>
<summary><b>Detayları göster</b></summary>

<br>

**Neden YOLO11?**
`PLACEHOLDER` — mimari seçim gerekçenizi buraya yazın (ör. C3k2 blokları, SPPF, C2PSA dikkat mekanizması küçük/hızlı hedeflerde avantaj sağlıyor).

**Neden ByteTrack?**
`PLACEHOLDER` — düşük güvenli tespitleri de değerlendirerek ID kaybını azaltması, hesaplama açısından hafif olması.

**Zorluklar ve çözümler:**
`PLACEHOLDER` — küçük nesne tespiti, hızlı hareket, aydınlatma koşulları gibi karşılaştığınız zorlukları ve çözüm yaklaşımınızı buraya ekleyin.

</details>

---

## 📚 References

- Khanam, R., & Hussain, M. *YOLOv11: An Overview of the Key Architectural Enhancements*, 2024.
- Zhang, Y. et al. *ByteTrack: Multi-Object Tracking by Associating Every Detection Box*, 2022.
- [Ultralytics YOLO11 Documentation](https://docs.ultralytics.com/models/yolo11/)
- [Roboflow Universe — Drones Yolo11 A Dataset](https://universe.roboflow.com/drone-a7lpy/drones-yolo11-a)

---

## 📄 License

Bu proje [MIT Lisansı](LICENSE) altında yayınlanmıştır.

---

<div align="center">

**`PLACEHOLDER — Adınız / GitHub / LinkedIn`**

⭐ Projeyi faydalı bulduysanız yıldız vermeyi unutmayın!

</div>
