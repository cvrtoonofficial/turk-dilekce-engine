---
name: turk-dilekce-engine:anlati-omurga
version: 0.1.0
description: >
  Olgu kısmının iskeletini üretir. Olayları kronoloji + karakter + çatışma + tema dörtlüsünde organize eder. HMK m.194 somutlaştırma yükümlülüğünü önceden test eder. Tetikleyiciler: "olgu kısmı", "anlatı omurgası", "dilekçenin olguları", "olayları sıraya koy", "müvekkilin hikâyesi". dosya-kesif tamamlandıktan sonra otomatik tetiklenir.
allowed-tools:
  - Read
  - Write
  - Edit
  - mcp__hukuk-rag__*
---

# Anlatı Omurgası

## Amaç
Olgu kısmının "vaka kataloğu" olmaktan çıkıp **hâkimi metnin sonuna kadar taşıyan bir anlatı** olmasını sağlamak. Anthony G. Amsterdam ve Jerome Bruner'in (Minding the Law, Harvard UP 2000) belgelediği üzere, hukuki argümanın özünde "kılık değiştirmiş ve çevrilmiş hikâyeler" vardır; olgu kısmı bu hikâyenin omurgasıdır.

## Dörtlü Disiplin

### Karakter
- **Müvekkil:** kahraman değil; dürüst, tutarlı, meşru beklentilere sahip bir özne. Müvekkilin kişiliğini metne **eylemleri yoluyla** taşı, sıfatlarla değil. "İyi niyetli müvekkil" yazma; müvekkilin iyi niyetinin görüldüğü olguları sırala.
- **Karşı taraf:** kötü adam değil; bizim müvekkilin meşru beklentilerini ihlal etmiş bir özne. Karakter çamuratma yasaktır (etik ve stratejik). Faruk Erem'in deyimiyle: "Avukat karşı tarafa hakaret eden değil, hâkime hakaretle karşılaştırılacak iyi davranışı gösterendir."
- **Üçüncü kişiler:** tanıklar, bilirkişiler, üçüncü taraf tedarikçiler — anlatıdaki rolü kadar yer ver.

### Çatışma
Davanın çekirdeğindeki gerilim **tek bir cümle** ile ifade edilebilmelidir. Bu cümle metnin temasıdır. Örnek: "Davalı, sözleşmenin 6. maddesinde öngörülen ihbar süresine uymaksızın akdi feshetmiştir." Tüm metin bu cümleyi tahkim eder.

### Kronoloji
Olaylar **takvim sırasına göre değil, anlatı sırasına göre** sunulur. Anlatı sırası tipik olarak:
1. Kritik kıvılcım anı (dramatic inciting moment) — "Şu tarihte şu oldu."
2. Geri dönüş (flashback) — bağlamı kuran öncül olgular.
3. Kıvılcımdan günümüze lineer akış.
4. Dava açma anına bağlanma.

Bu yapı, hâkimin metnin önemli anına önce ulaşmasını sağlar (Stanchi primacy effect).

### Tema
Yukarıdaki çatışma cümlesinin metin boyunca yankılanması. Tematik konsantrasyon, dilekçenin entelektüel ağırlığını yaratır. Her büyük paragraf temaya bir referansla kapanmalıdır — açıkça değil, sezilerek.

## Somutlaştırma Testi (HMK m.194)

Üretilen her vakıa cümlesi şu dört filtreden geçer:
- **Kim?** Hangi kişi, hangi sıfatla?
- **Ne?** Hangi eylem, hangi davranış?
- **Ne zaman?** Kesin tarih veya tarih aralığı, kanıtlanabilir biçimde.
- **Nerede / nasıl?** Mekan veya yöntem, hangi ispat aracıyla?

En az üç filtreden geçmeyen cümle soyut iddia riski taşır; **kullanıcıya işaretle**, sessizce yumuşatma.

## Gold-Standard Örnek Pasaj

> "Müvekkilim, 15 Mart 2024 tarihinde davalı şirketin İstanbul Atatürk Mahallesi'ndeki merkezinde, davalı yetkilisi N. Y. ile imzaladığı 4 sayfalık sözleşmenin 6. maddesinde, fesih için 30 günlük yazılı ihbar süresi öngörülmüştü. 18 Şubat 2025 tarihinde, müvekkilim sözleşmenin gereklerini eksiksiz yerine getirirken, davalı 15:42'de elektronik posta ile feshi bildirmiş ve aynı gün 17:30'da mecuru fiilen tahliye etmiştir. (Delil 1: sözleşme, Delil 4: e-posta, Delil 7: kapıcı tanıklığı.)"

Bu pasajda dörtlü disiplin nasıl iç içe geçti, gözlemle:
- Kim: müvekkilim, davalı, davalı yetkilisi N.Y., kapıcı
- Ne: imzalama, ihbar süresi, fesih bildirimi, tahliye
- Ne zaman: 15 Mart 2024, 18 Şubat 2025, 15:42, 17:30
- Nerede/nasıl: İstanbul Atatürk Mahallesi, 4 sayfalık sözleşme, elektronik posta

## Anti-Örnek

> "Davalı sözleşmeyi haksız olarak feshetmiştir. Müvekkilim bu durumdan mağdur olmuştur."

İki cümle, beş soyutluk hatası: "haksız", "sözleşmeyi", "feshetmiştir", "bu durumdan", "mağdur". Hiçbir kim/ne/ne zaman/nerede yok. Bu cümle ret riski taşır.

## Çıktı

`pac-state/<matter>/anlati-omurga.md`

```markdown
## Tema (tek cümle)
...

## Kronoloji (anlatı sırasında)
1. [Kıvılcım] ...
2. [Geri dönüş — bağlam] ...
3. [Lineer akış] ...
4. [Bağlanma] ...

## Karakter Notları
- Müvekkil: ... (sıfat değil eylem)
- Davalı: ...
- Tanıklar: ...

## m.194 Test Sonucu
- 12 vakıa cümlesi → 11 tam somut, 1 işaretli (Delil 3 belirsiz)
```
