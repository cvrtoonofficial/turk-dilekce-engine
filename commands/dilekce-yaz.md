---
description: Yeni bir dilekçe yazma seansı başlat. Sekiz skill + altı ajan sırayla işletilir.
---

# /dilekce-yaz — Tam Akış Dilekçe Üretimi

Türk hukukunda yeni bir dilekçe yazma seansı başlatır. Plugin'in tüm bileşenleri sırasıyla çalışır.

## Akış

### Aşama 1 — Dosya Keşfi
`turk-dilekce-engine:dosya-kesif` skill'i çağrılır. Müvekkil anlatımı / yüklü belgeler okunur, matter.md üretilir.

### Aşama 2 — Sherlock Ön Tarama
`sherlock-ai` ajanı çağrılır. Belgeler içindeki aksaklıklar, gizli olgular, yeni argüman olasılıkları çıkarılır.

### Aşama 3 — Anlatı Omurgası
`turk-dilekce-engine:anlati-omurga` skill'i çağrılır. Olgu kısmının iskeleti üretilir, m.194 somutlaştırma testi yapılır.

### Aşama 4 — Argüman Dokuması
`turk-dilekce-engine:arguman-dokumasi` skill'i çağrılır. Her ana iddia Toulmin altılısı ile iç kontrolden geçer.

### Aşama 5 — Gold-Standard Korpus Yüklemesi
`turk-dilekce-engine:gold-standard-korpus` skill'i çağrılır. Bağlama uygun başarılı dilekçe pasajları ve anti-örnekler model context'ine yüklenir.

### Aşama 6 — Preemptive Rebuttal
`turk-dilekce-engine:preemptive-rebuttal` skill'i çağrılır. Hâkim ret refleksi taranır, Walton critical questions ile karşı saldırı vektörleri çıkarılır, zımni çürütmeler yerleştirilir.

### Aşama 7 — Taslak Üretimi
Skill'ler + Sherlock çıkarımları + gold-standard ritmi birlikte değerlendirilerek **ilk taslak** üretilir. dilekce-v1.md (markdown) kaydedilir.

### Aşama 8 — Retorik Üçgen Kalibrasyonu
`turk-dilekce-engine:retorik-ucgen-kalibrator` skill'i çağrılır. Paragrafların logos-ethos-pathos dengesi kontrol edilir, felsefi dil konumu kalibre edilir.

### Aşama 9 — Üslup-Akıcılık Katmanı
`turk-dilekce-engine:uslup-akicilik-katmani` skill'i çağrılır. Cümle ritmi, paralellik, ölçülü imge taraması; abartı kırmızı çizgileri kontrol edilir.

### Aşama 10 — Kırmızı Takım Debate (Walton Üç-Katman)
Üç ajan paralel olarak çağrılır:
- `hakim-ai` — en olası ret kararını yazar
- `karsi-vekili-ai` — en güçlü cevap dilekçesini hazırlar
- `edebi-elestirmen-ai` — üslup itirazlarını çıkarır

Sonra `koordinator-ai` çağrılır; üç çıktıyı çapraz analiz eder, aksiyon listesi çıkarır.

### Aşama 11 — Revizyon
Koordinatör'ün aksiyon listesindeki kritik ve orta öncelikli düzeltmeler uygulanır. dilekce-v2.md üretilir.

### Aşama 12 — Final Stres Testi
`seytanin-avukati-ai` ajanı, **kontaminasyon önlemiyle** (yalnızca v2 dilekçeyi ve matter.md'yi okur), final saldırı turunu yapar. Kritik bulgular varsa dilekce-v3.md üretilir.

### Aşama 13 — Anti-Halüsinasyon Gateway
`turk-dilekce-engine:magesh-anti-halusinasyon` skill'i **zorunlu olarak** çalışır. Her atıf çapraz teyit edilir. Doğrulanmayan atıflar kullanıcıya işaretlenir.

### Aşama 14 — Final .docx Üretimi
Tüm aşamalar geçildiğinde UYAP standardına yakın .docx çıktısı üretilir (Times New Roman 12pt, 1.5 satır aralığı, 2.5cm marjlar, sayfa numarası).

## Kullanım

```
/dilekce-yaz
```

Sonrasında müvekkil bilgisi, dosya türü ve eldeki belgeler hakkında konuşmaya başlayın. Plugin akışı yönetir.

## Çıktı

`pac-state/<matter>/` altında:
- `matter.md` — dosya keşfi
- `anlati-omurga.md`
- `arguman-matrisi.yaml`
- `preemptive-rebuttal.md`
- `retorik-ucgen-raporu.md`
- `uslup-akicilik-raporu.md`
- `sherlock-cikarimlar.md`
- `hakim-ret-simulasyonu.md`
- `karsi-cevap-simulasyonu.md`
- `edebi-elestirmen-raporu.md`
- `koordinator-raporu-tur1.md`
- `seytanin-avukati-stres-test.md`
- `magesh-dogrulama-raporu.md`
- `dilekce-v1.md` ... `dilekce-final.docx`
