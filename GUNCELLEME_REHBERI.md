# Veri dosyası güncelleme rehberi

Uygulama (Nema Hesaplama) yalnızca bu depodaki herkese açık dosyaları **okur**;
kullanıcıdan hiçbir veri gönderilmez.

## 1. Depo

GitHub'da herkese açık bir depo aç, örn. `nema-veri`. İçine:

- `veri.json` — uygulamanın güncelleme dosyası (aşağıda)
- `gizlilik.html` — gizlilik politikası (ekte)

**Settings → Pages → Branch: main / root** ile GitHub Pages'i aç. Gizlilik adresi:
`https://BatMer1613.github.io/nema-veri/gizlilik.html` (Play Console'a girilecek).

Uygulamaya girilecek veri adresi (raw):
`https://raw.githubusercontent.com/BatMer1613/nema-veri/main/veri.json`
(Uygulamada tanımlı.)

## 2. veri.json biçimi

Tüm alanlar isteğe bağlı. Yalnız **uygulamaya gömülü olandan sonra** açıklananları
yazman yeterli (gömülü veriler: memur katsayısı Temmuz 2026'ya, aylık TÜFE Eylül
2026'ya, nema oranı 2025'e kadar). Eski satırları silme; yeni satır ekle.

```json
{
  "surum": 1,
  "memurKatsayilari": [[2027, 1, 1.712345]],
  "nemaOranlari": {"2026": 40.5},
  "aylikTufe": {"2026": [4.84, 2.96, 1.94, 4.18, 1.71, 0.99, 1.78, 1.84, 1.84, 2.10]},
  "asgariUcret": [[2027, 1, 35000.00, 41176.47]],
  "emekliZam": [[2027, 1, 12.50]],
  "memurToplu": [[2028, 1, 6.0]]
}
```

| Alan | Biçim | Kaynak |
|---|---|---|
| memurKatsayilari | [yıl, ay (1 veya 7), aylık katsayı] | HMB maaş katsayıları genelgesi |
| nemaOranlari | "yıl": yüzde | OYAK Genel Kurulu (her yıl Mayıs) |
| aylikTufe | "yıl": [Ocak, Şubat, …] aylık % (yılın **tamamı**, açıklanan aya kadar) | TÜİK, her ayın 3'ü |
| asgariUcret | [yıl, ay, net, brüt] | Resmî Gazete |
| emekliZam | [yıl, 1=Ocak / 2=Temmuz, %] | SGK |
| memurToplu | [yıl, 1/2, %] toplu sözleşme oranı | Kamu Görevlileri Hakem Kurulu |

**Bildirimler bu dosyaya bağlıdır:** telefonlar günde bir kez dosyayı okur; yeni bir
ay/dönem/yıl eklendiyse kullanıcıya "Ekim 2026 enflasyonu %2,10" gibi bildirim gider.
Bu yüzden yanlış girilen satır herkese bildirim olarak gider — kaydetmeden önce
JSON'u bir doğrulayıcıdan (ör. jsonlint.com) geçir. Hatalı JSON yok sayılır, uygulama
son geçerli veriyle çalışmaya devam eder.

## 3. Takvim

- Her ayın 3'ü: `aylikTufe` içine yeni ayı ekle.
- Ocak ve Temmuz başı: `memurKatsayilari` (ve gerekirse `emekliZam`, `asgariUcret`).
- Mayıs: `nemaOranlari` (Genel Kurul sonrası).
