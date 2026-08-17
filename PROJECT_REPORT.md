# DroneWatch: YOLO11 Tabanlı İHA Tespit ve Takip Sistemi
### Proje Raporu

---

## 1. Proje Özeti

| Alan | Bilgi |
|---|---|
| **Proje Adı** | DroneWatch |
| **Amaç** | Video akışlarında drone/İHA tespiti, çoklu nesne takibi ve yoğunluk analizi |
| **Model** | YOLO11 (Ultralytics) |
| **Takip Algoritması** | ByteTrack |
| **Geliştirme Ortamı** | Google Colab (GPU) |
| **Dil / Kütüphaneler** | Python, Ultralytics, OpenCV, Roboflow SDK |
| **Lisans** | MIT |

**Kısa açıklama (repo description için):**
> Real-time drone/UAV detection and multi-object tracking system built with YOLO11 and ByteTrack, including detection density heatmap visualization.

---

## 2. GitHub Repository Ayarları

### 2.1 Repo İsmi
Önerilen isim seçenekleri (tire ile, küçük harf, açıklayıcı):
- `dronewatch-yolo11`
- `uav-detection-tracking`
- `drone-detect-track`

> **Not:** İsim seçerken zaten var olan tanınmış projelerle (ör. `Drone-Detection-YOLOv11x`) birebir aynı olmamasına dikkat edin — hem GitHub arama sonuçlarında karışıklık olmaz hem de "kopya" izlenimi oluşmaz.

### 2.2 Repo Açıklaması (Description alanı)
```
Real-time drone/UAV detection and tracking using YOLO11 and ByteTrack, with detection density heatmap visualization.
```

### 2.3 Topics (Etiketler)
Repo ayarlarında **"Add topics"** kısmına şunları ekleyin (arama/keşfedilebilirlik için):
```
object-detection, yolo11, drone-detection, computer-vision,
uav-detection, bytetrack, pytorch, ultralytics
```

### 2.4 Görünürlük
- **Public** — CV/portfolyo amaçlı ise mutlaka public olmalı.

### 2.5 Lisans
Repo oluştururken **"Add a license"** → **MIT License** seçin. Böylece hem kodu kullanan başkalarının hakları netleşir hem de sizin adınız telif sahibi olarak görünür.

### 2.6 .gitignore
Python şablonunu seçin (`Add .gitignore` → `Python`). Ayrıca aşağıdakileri manuel ekleyin:
```
# Model ağırlıkları (büyük dosyalar, repoya değil Releases/HF'e koyun)
*.pt
runs/
drone_watch_runs/

# Veri seti (genelde büyük, repoya koymayın)
dataset/
*.zip

# Colab/Jupyter
.ipynb_checkpoints/
```

---

## 3. Klasör / Dosya Yapısı

```
dronewatch-yolo11/
├── drone_watch.ipynb        # Ana Colab notebook (eğitim + inference)
├── README.md                 # Proje tanıtımı (kısa, görsel ağırlıklı)
├── PROJECT_REPORT.md         # Bu rapor (detaylı teknik dokümantasyon)
├── requirements.txt          # Bağımlılıklar
├── LICENSE                   # MIT
├── .gitignore
└── assets/                   # Ekran görüntüleri, sonuç grafikleri, gif'ler
    ├── sample_detection.png
    ├── results.png
    └── heatmap_output.png
```

---

## 4. Adım Adım Süreç

### Adım 1 — Ortam Kurulumu
```bash
pip install ultralytics roboflow opencv-python
```

### Adım 2 — Veri Setinin Hazırlanması
- Kaynak: [Drones Yolo11 A – Roboflow Universe](https://universe.roboflow.com/drone-a7lpy/drones-yolo11-a)
- 9.900 görüntü, 5 sınıf (`drone`, `bird`, `airplane`, `object`)
- Format: YOLO11 (`.txt` annotation + `data.yaml`)
- Roboflow API anahtarı ile `.download("yolov11")` komutuyla otomatik indirme

### Adım 3 — Model Eğitimi

**Kullanılan hiperparametreler:**

| Parametre | Değer | Açıklama |
|---|---|---|
| `model` | `yolo11n.pt` | Nano — hızlı baseline (istersen `s`/`m` dene) |
| `epochs` | 50 | Eğitim döngüsü sayısı |
| `imgsz` | 640 | Giriş görüntü boyutu |
| `batch` | 16 | Batch boyutu |
| `optimizer` | AdamW | Optimizasyon algoritması |
| `lr0` | 0.001 | Başlangıç öğrenme oranı |
| `patience` | 10 | Erken durdurma sabrı |

### Adım 4 — Değerlendirme
Doğrulama seti üzerinde şu metrikler raporlanır:
- **Precision**
- **Recall**
- **mAP@50**
- **mAP@50-95**

> Eğitim tamamlandıktan sonra bu tabloyu gerçek sonuçlarınızla doldurun:

| Metrik | Değer |
|---|---|
| Precision | *(doldurulacak)* |
| Recall | *(doldurulacak)* |
| mAP@50 | *(doldurulacak)* |
| mAP@50-95 | *(doldurulacak)* |
| Inference hızı | *(doldurulacak)* ms/görüntü |

### Adım 5 — Tespit + Takip (Inference)
```python
trained_model.track(
    source=INPUT_VIDEO,
    conf=0.35,
    iou=0.5,
    tracker="bytetrack.yaml",
    save=True
)
```
- `conf=0.35`: Güven eşiği (düşük eşik = daha fazla tespit ama daha çok yanlış pozitif)
- `iou=0.5`: NMS için IoU eşiği
- `tracker=bytetrack.yaml`: Kare bazlı kimlik takibi

### Adım 6 — Yoğunluk Haritası (Opsiyonel)
Tespitlerin video boyunca hangi bölgelerde yoğunlaştığını gösteren ısı haritası üretimi (`build_density_map` fonksiyonu, `PROJECT_README`'de kod mevcut).

### Adım 7 — Belgeleme ve Yayınlama
1. `README.md`'yi güncelleyin — örnek görseller, GIF, sonuç tablosu ekleyin
2. `assets/` klasörüne örnek çıktı görsellerini koyun
3. İlk commit'i atın (bkz. Bölüm 5)
4. Repo'yu public yapıp LinkedIn/CV'nize ekleyin

---

## 5. Commit / Branch İsimlendirme Standartları

Profesyonel görünüm için tutarlı bir commit mesajı formatı kullanın:

```
feat: add training pipeline
fix: correct data.yaml path
docs: update README with results
chore: add .gitignore
refactor: rename tracking function
```

**Branch isimlendirme (ileride geliştirme yaparsanız):**
```
main                    # stabil, çalışan kod
feature/webcam-support  # yeni özellik
fix/heatmap-bug         # hata düzeltme
```

---

## 6. requirements.txt

```
ultralytics>=8.3.0
roboflow>=1.1.0
opencv-python>=4.9.0
numpy>=1.26.0
```

---

## 7. Sonraki Geliştirme Adımları (Roadmap)

- [ ] Daha büyük/çeşitli veri seti ile yeniden eğitim
- [ ] `yolo11s` / `yolo11m` ile karşılaştırmalı benchmark
- [ ] Gerçek zamanlı webcam/RTSP stream desteği
- [ ] Streamlit tabanlı basit web arayüzü
- [ ] Docker containerization
- [ ] GitHub Actions ile otomatik test (CI)

---

## 8. Referanslar

- Ultralytics YOLO11 Documentation — docs.ultralytics.com/models/yolo11
- ByteTrack: Multi-Object Tracking by Associating Every Detection Box (Zhang et al., 2022)
- Roboflow Universe — Drones Yolo11 A Dataset
