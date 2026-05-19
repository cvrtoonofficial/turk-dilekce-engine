---
description: Tek başına üslup-akıcılık değerlendirmesi. Edebî unsur abartısı / sahte yüksek dil / dramatik çağrı / monoton ritim kontrolü.
argument-hint: "[dilekce-yolu veya metin]"
---

# /uslup-kontrol — Sadece Üslup Değerlendirmesi

Bir dilekçeyi (veya dilekçe parçasını) yalnızca üslup ve akıcılık eksenlerinde değerlendir. Argümantasyon, hukuki sebep, ispat eksenlerine girmez.

## Kullanım

```
/uslup-kontrol <dilekce-yolu veya doğrudan metin>
```

## Akış

### 1. `turk-dilekce-engine:uslup-akicilik-katmani` skill'i
Şu eksenlerde tarama:
- Cümle ritmi (kısa-uzun-kısa örüntüsü)
- Paralellik kullanımı (paragraf başına 1 maks)
- Metafor frekansı (bölüm başına 1 ideal)
- Kırmızı çizgi ihlalleri:
  - Tezi süsleme (zayıf tezi örtme)
  - Dramatik çağrı
  - Sahte yüksek dil
  - "Az ve sezilmez" prensibi ihlali

### 2. `edebi-elestirmen-ai` ajanı
Sami Selçuk çizgisinde tek tek paragraf eleştirisi. Düzeltme önerisi sunar ama dilekçeyi değiştirmez.

### 3. Birleşik Rapor
İki kaynağın bulgularını tek raporda birleştir.

## Çıktı

```markdown
# Üslup-Kontrol Raporu
Tarih: <YYYY-MM-DD>
İncelenen: <dilekce-yolu>

## CÜMLE RİTİM HARİTASI
| Paragraf | Sözcük dizilimi | Değerlendirme |
|---|---|---|
| OLGU §1 | 12-32-8 | ✓ ideal |
| OLGU §2 | 24-26-22 | ⚠ monoton, çeşitlendir |

## PARALELLİK
- 3 paralel yapı tespit; 1 paragrafta 2 tane (ihlal)
- Düzeltme: OLGU §5'teki ikinci paralel yapıyı düz cümleye çevir

## METAFOR
- 4 metafor tespit; 2'si aynı bölümde (ihlal)
- Düzeltme: HUKUKİ DOKU §3'teki "yağmurda kalmak" metaforunu kaldır

## KIRMIZI ÇİZGİ İHLALLERİ
- ✗ TALEP §1: "Sayın hâkim, kaderi ellerinize bırakıyoruz" — dramatik çağrı yasağı
- ⚠ HUKUKİ DOKU §2: "Pek muhterem haklarımız" — sahte yüksek dil

## ÖNERİLEN DÜZELTMELER (tek tek paragraf)
1. ...
2. ...
3. ...

## GENEL SONUÇ
"Az ve sezilmez" prensibi: %72 korunmuş, %28 ihlal.
Tahmini hâkim yorulma noktası: §5 (orta-arka).
```

## Çıktı Konumu

`pac-state/<matter>/uslup-kontrol-<YYYY-MM-DD>.md`

## Önemli

Bu komut hızlı (3-5 dakika). Dilekçenin **akıcılığını** test eder. Hukuki sağlamlığı test etmez — onun için `/stres-testi` kullanın.
