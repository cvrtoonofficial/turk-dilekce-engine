---
description: Gold-standard veya anti-örnek dilekçe pasajı ekle. Büro kendi korpusunu geliştirir.
---

# /korpus-ekle — Korpus Genişletme

Plugin'in gold-standard-korpus skill'inin çekirdek pasajlarına büroya özgü yeni pasajlar ekler. Her büronun zamanla **kendi korpusu** oluşur — bu da plugin'in büronun üslubunu öğrenmesini sağlar.

## Kullanım

```
/korpus-ekle
```

Sonrasında interaktif olarak şunları girer:

1. **Kaynak türü:** 
   - Yargıtay kararındaki başarılı taraf dilekçesi pasajı (gold)
   - Yargıtay kararındaki başarısız taraf dilekçesi pasajı (anti-örnek)
   - Büronun kendi başarılı dilekçelerinden bir pasaj
   - Büronun zayıf bulduğu eski dilekçelerden anti-örnek

2. **Pasaj metni:** (3-15 cümle aralığında, tek paragraf veya kısa bölüm)

3. **Etiketleme:**
   - Kalite: `gold` veya `anti`
   - Dilekçe türü: `dava` / `cevap` / `replik` / `istinaf` / `temyiz` / `aym_bireysel` / `aihm` / `ihtarname`
   - Retorik amaç: `anlati_acilisi` / `hukuki_sebep_zincirleme` / `karsi_arguman_curutme` / `talep_kapanisi` / `anayasal_hak_acilisi`
   - Eksen: `logos` / `ethos` / `pathos` / `dengeli`
   - Hukuk alanı: `borclar` / `ticaret` / `is` / `idare` / `vergi` / `ceza` / `fikri_haklar` / vb.

4. **Bağlam notu:** Bu pasaj **neden** gold/anti? Bir cümlede özetle.

5. **Kaynak referansı:** (Yargıtay kararı E./K. veya büro dosya numarası — anonimleştirilmiş)

## İşleyiş

Bilgiler alındıktan sonra plugin:

1. Pasajı `corpus/<kalite>/<dilekce_turu>/<etiket>.md` dosyasına yazar.
2. Korpus index dosyasını günceller (`corpus/_index.yaml`).
3. Bir sonraki dilekçe seansında bu pasaj otomatik olarak ilgili bağlama yüklenir.

## Çıktı

```markdown
# Eklendi: corpus/gold/dava/anlati-acilisi-trafikkazasi-2025-001.md

Pasaj kaydedildi. Plugin'in gold-standard korpusunda artık 47 gold + 23 anti-örnek var.

Bir sonraki dava dilekçesi seansında bu pasaj "anlati_acilisi" amacıyla retrieval bağlamına dahil edilecek.
```

## Önemli

- **Müvekkil bilgisi taşıyan pasajlar anonimleştirilmeli.** Plugin uyarı verir; kullanıcı onaylamadan pasajı kaydetmez.
- **Tek bir kaynaktan en fazla 3-5 pasaj ekle** — korpus dengeli olmalı.
- **Aynı türden 100+ pasaj birikirse**, kullanıcıya hatırlat: "Korpus belli bir türde fazla yoğunlaştı; çeşitlendirmek faydalı."
