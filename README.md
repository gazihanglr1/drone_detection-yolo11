# DroneWatch — YOLO11 ile İHA Tespit ve Takip

Video akışlarında drone/İHA tespiti ve çoklu nesne takibi yapan bir bilgisayarlı görü projesi.

## Özellikler
- YOLO11 tabanlı özel eğitilmiş nesne tespit modeli
- ByteTrack ile karesel takip (aynı drone'un kimliğini kareler arası koruma)
- Tespit yoğunluğu haritası (isteğe bağlı)

## Veri Seti
[Drones Yolo11 A — Roboflow Universe](https://universe.roboflow.com/drone-a7lpy/drones-yolo11-a) (9.900 görüntü, 5 sınıf: drone, kuş, uçak, obje)

## Kurulum
```bash
pip install ultralytics roboflow opencv-python
```

## Kullanım
1. `drone_watch.ipynb` dosyasını Google Colab'da açın
2. Kendi Roboflow API anahtarınızı girin
3. Hücreleri sırayla çalıştırın

## Sonuçlar




## Lisans
MIT

## Referanslar
- [Ultralytics YOLO11 Documentation](https://docs.ultralytics.com/models/yolo11/)
- [ByteTrack Paper](https://arxiv.org/abs/2110.06864)
