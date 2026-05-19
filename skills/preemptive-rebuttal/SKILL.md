---
name: turk-dilekce-engine:preemptive-rebuttal
version: 0.1.0
description: >
  Karşı tarafın yazmamış cevap dilekçesinin saldırı vektörlerini Walton critical questions yöntemiyle önceden tespit eder; her vektöre dilekçenin uygun paragraflarında zımni çürütme yerleştirir. Hâkimin olası ret refleksini yargi_mcp ret kararlarından çıkarıp önceden kapatır. Tetikleyiciler: "preemptive rebuttal", "karşı argüman", "ret refleksi", "önleyici çürütme", "saldırı vektörü", "zayıflık doldur". arguman-dokumasi'nın hemen ardından otomatik tetiklenir.
allowed-tools:
  - Read
  - Write
  - Edit
  - mcp__yargi_mcp__*
  - mcp__bafdf342-e57f-4eff-9a76-cef9f0201e9f__*
  - mcp__mevzuat_mcp__*
---

# Önleyici Çürütme (Preemptive Rebuttal)

## Amaç
Bryan A. Garner ve Antonin Scalia'nın Making Your Case (2008) ilkesini Türk hukukuna uyarlamak: "the best advocates do not pretend the other side doesn't exist; they show the court why it loses anyway." Karşı argümanı yoksaymak değil, önceden kapatmak.

## Üç Aşamalı Yöntem

### 1. Hâkim Ret Refleksi Tarama
Aynı tip davada `mcp__yargi_mcp__search_bedesten_unified` ile **ret kararlarını** çek. Sorgu örüntüsü:

```
phrase: dava türü anahtar kelimesi + "+red" veya "+bozma"
court_types: ["YARGITAYKARARI"]
```

Çekilen ret kararlarından **gerekçe örüntülerini** çıkar. Tipik örüntüler:
- HMK m.119 unsur eksikliği
- Görev/yetki itirazı
- Zamanaşımı / hak düşürücü süre
- İspat yükü hatası (HMK m.190)
- Senetle ispat zorunluluğu (HMK m.200)
- MK m.2 hakkın kötüye kullanımı
- Pasif husumet ehliyeti
- Husumet yöneltilmeyen üçüncü kişi
- Bilirkişi raporu çelişkisi

### 2. Karşı Vekil Saldırı Vektörü Tarama
Walton & Gordon'un (2013) "Critical Questions in Computational Models of Legal Argument" yöntemiyle, bizim her ana iddiamız için karşı tarafın açabileceği eleştirel soruları çıkar:

- **Uzman görüşünden argüman**: uzman gerçekten o alanda uzman mı? görüşü olayla bağlantılı mı?
- **İçtihatten argüman**: emsal kararın olgu örüntüsü mevcut davayla aynı mı? distinguishability var mı?
- **Belgeden argüman**: belge gerçek mi? bağlamı doğru mu? imza geçerli mi?
- **Tanıktan argüman**: tanığın görme/duyma koşulu uygun mu? menfaat ilişkisi var mı?

Her critical question, karşı tarafın olası saldırı vektörünü tanımlar.

### 3. Zımni Çürütme Yerleştirmesi

İki teknik var:

**(a) Açık çürütme** — "Karşı taraf X argümanını ileri sürebilirse de, Y nedeniyle bu argüman somut zemin bulamaz."

Açık çürütme gücünü kullanır ama metnin akışını biraz kırar. Yalnızca **kritik** karşı argümanlar için kullan; her küçük endişe için kullanma.

**(b) Zımni çürütme** — Karşı argümanı çürütecek olgu cümleyi metnin **doğal yerine** yerleştirme. Karşı argüman zaten kapanmış olur, açıkça anılmasına gerek kalmaz.

Örnek: Karşı tarafın "zımni anlaşma" argümanını çürütmek için:
- Açık yöntem: "Davalının zımni anlaşma argümanı, müvekkilimin teslim sonrası noter ihtarnamesiyle yaptığı itiraz karşısında somut zemin bulamaz."
- Zımni yöntem: "Müvekkilim, fesih bildirimini takip eden 48 saat içinde İstanbul 23. Noterliği'nin 12345 yevmiye sayılı ihtarnamesiyle teslim almayı reddettiğini bildirmiştir." (Bu cümle olgu kısmına yerleştirildiğinde, "zımni anlaşma" argümanı doğmadan ölür.)

## İlke (Komut Değil)

İyi bir önleyici çürütme görünmez. Hâkim metni okurken karşı argümanın çürütüldüğünü fark etmez, yalnızca metnin sağlam olduğunu hisseder. Eğer çürütme görünür hale geliyorsa, fazla açık yapılmıştır.

**Kural-of-thumb:** Bir bölümde en fazla bir açık preemptive rebuttal. Geri kalan saldırı vektörleri zımni biçimde olgu kısmına serpiştirilir.

## Çıktı

`pac-state/<matter>/preemptive-rebuttal.md`

```markdown
## Hâkim Ret Refleksi (yargi_mcp taraması)

| Ret Gerekçesi | Frekans | Bizim Durumumuz | Çürütme |
|---|---|---|---|
| HMK m.190 ispat yükü | yüksek | risk: orta | "Delil 7 (bilirkişi raporu) + Delil 4 (e-posta) ispat zincirini sağlar" |
| MK m.2 kötüniyet | düşük | risk: düşük | yerleştirme gerekmez |

## Karşı Vekil Saldırı Vektörleri (Walton CQ)

| Vektör | Olası argüman | Çürütme tipi | Konum |
|---|---|---|---|
| Belgeden | "E-posta gerçek değil" | zımni | OLGU §3 — "elektronik imzalı PDF formatında..." |
| İçtihat distinguishability | "Yargıtay 6.HD emsali farklı olgu örüntüsü" | açık | HUKUKİ DOKU §2 |

## Yerleştirme Notları
- OLGU §3'e ek cümle: "elektronik imzalı PDF formatında ve domain doğrulamalı..."
- HUKUKİ DOKU §2: açık paragraf eklendi
```

## Gold-Standard Örnek

Yargıtay'ın 2024'te kabul ettiği bir trafik kazası tazminat davasındaki dilekçenin önleyici çürütme örneği:

> "Davalı sürücünün, kazadan sonra olay yerinden uzaklaşmayıp polis tutanağı düzenleninceye kadar bekleyerek ifade vermesi (Trafik Tutanağı, 23.05.2024) bu davranışın bilinçli olmadığı yönündeki muhtemel savunmayla bağdaşmaz; bilakis davalı kendi tutanak ifadesinde 'yağmurun başlamasıyla yol şeridini ayırt edemediğini' beyan etmiştir."

Bu pasajda neyi gözlemle: davalının olası savunmaları (bilinçsizlik, görüş zayıflığı) iki adımda önceden çürütülüyor — bir kez olgu (polis tutanağına bekleyerek katılma), bir kez davalının kendi sözünün geri dönüşü. Karşı argümanın kendisi açıkça anılmıyor; yalnızca onunla "bağdaşmadığı" söyleniyor.
