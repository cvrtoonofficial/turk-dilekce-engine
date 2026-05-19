---
name: turk-dilekce-engine:gold-standard-korpus
version: 0.1.0
description: >
  Yargıtay ve AYM kararlarındaki başarılı taraf dilekçesi pasajlarını ve onların retorik ritmini bağlama yükler. Aynı zamanda ret kararlarındaki zayıf dilekçelerin örüntülerini anti-örnek olarak sunar. Tetikleyiciler: "gold-standard", "örnek pasaj", "benzer dava nasıl yazılmış", "retorik ritim", "anti-örnek". Diğer skill'ler çağırdığında otomatik tetiklenir.
allowed-tools:
  - Read
  - Write
  - Glob
  - Grep
  - mcp__yargi_mcp__*
  - mcp__bafdf342-e57f-4eff-9a76-cef9f0201e9f__*
  - mcp__hukuk-rag__*
---

# Gold-Standard Korpus

## Amaç
Yapay zekânın yazdığı her cümlenin altında, aynı tip davanın başarıyla yazılmış bir versiyonunun retorik ritmi bulunsun. Bu skill, bağlama göre seçilmiş gold-standard pasajları (kazanılmış davaların dilekçeleri) ve anti-örnekleri (reddedilmiş davaların zayıf pasajları) yükler.

## Çekirdek Korpus (Skill İçinde Gömülü)

Bu skill 5 sınıfta çekirdek örnek pasaj taşır. Geri kalan korpus runtime'da yargi_mcp ile çekilir.

### Sınıf A — Anlatı Açılışı (gold)

> "31 Ocak 2024 sabahı saat 09:14'te, müvekkilim Atatürk Caddesi 47 numaralı işyerinin önünde sözleşmenin imzalandığı yere, sözleşmede taahhüt edilen mali kaynaklara ulaşmak için bekliyordu. Davalı yetkilisinin verdiği randevu yerine gelmemesi, bu sözleşmenin ilk fiili ihlali değil, müvekkilim için son olduğu zincirin son halkasıydı."

Bu pasajda neyi gözlemle: kıvılcım anı önce (saat 09:14, somut yer), bağlamı kuran cümle ikinci, tematik bağlanma (zincirin son halkası) üçüncü.

### Sınıf A — Anlatı Açılışı (anti-örnek)

> "Müvekkilim ile davalı arasında bir sözleşme bulunmaktadır. Davalı bu sözleşmeyi ihlal etmiş, müvekkilim mağdur olmuştur. İşbu nedenle dava açma zarureti hasıl olmuştur."

Üç cümle, hiçbir somut anı yok, "ihlal", "mağdur", "zaruret" soyut. Hâkim metnin ikinci paragrafından sonra gözünü kaldırır. Anti-örnek.

### Sınıf B — Hukuki Sebep Zincirleme (gold)

> "Türk Borçlar Kanunu'nun 49. maddesi haksız fiil sorumluluğunun dört unsurunu (kusur, hukuka aykırılık, zarar, illiyet bağı) bir sıraya bağlamıştır. Davalının kasıtlı davranışı kusur unsurunu (TBK m.49/2 ağırlaştırıcı koşul), randevu yerine gelmeme eylemi sözleşmesel yükümlülüğün dışına çıkma niteliğiyle hukuka aykırılığı, müvekkilimin 14 saat boyunca yapmak zorunda kaldığı işlemlerin maliyeti zararı ve davalının bu maliyeti öngörmesi gereken koşulda davranması illiyet bağını sağlamaktadır."

Tek cümle dört unsuru tek tek olguya bağlıyor; her unsur kendi parantezinde norma referans veriyor. Cümle uzun ama mantık zinciri okunaklı.

### Sınıf B — Hukuki Sebep Zincirleme (anti-örnek)

> "Davalı TBK m.49 anlamında kusurludur. TBK m.50 uyarınca zarar vardır. TBK m.51 uyarınca illiyet bağı mevcuttur. Bu sebeple davalı TBK m.49 uyarınca sorumludur."

Madde sıralama; olgu yok, zincirleme yok, tekrar. Hâkim hangi olgunun hangi madde unsurunu karşıladığını **kendisi** bulmak zorunda — bu, kararı yazmasını zorlaştırır ve ret olasılığını artırır.

### Sınıf C — Karşı Argüman Çürütme (gold)

> "Davalının zımni anlaşma argümanına başvurabileceği akla gelse de, müvekkilimin fesih bildirimini takip eden 48 saat içinde gönderdiği ve İstanbul 23. Noterliği'nin 12345 yevmiye sayılı ihtarnamesiyle resmî biçim kazandırdığı itirazı, bu argümanın olgusal zeminini ortadan kaldırmaktadır."

Karşı argüman kendi adıyla anılmış, hemen olgusal zemini çekilmiş. "Davalı şunu diyebilir; ama yanlıştır" tipinde değil; daha incelikli, daha geri çekilmiş.

### Sınıf D — Talep Sonucu Kapanışı (gold)

> "Yüksek mahkemenizden, sözleşmenin 6. maddesindeki yazılı ihbar süresine uyulmaksızın yapılan feshin geçersizliğinin tespiti ile davalının müvekkilime 50.000 TL maddi ve 25.000 TL manevi tazminat ile dava tarihinden işleyecek yasal faizi ödemesine, yargılama gideri ve vekâlet ücretinin davalı üzerinde bırakılmasına karar verilmesini arz ve talep ederim."

Tek cümle. Her unsur (tespit, maddi, manevi, faiz, gider, vekalet) sıralı. Mahkemenin hüküm kısmına doğrudan kopyalanabilir.

### Sınıf E — Anayasal Hak Açılışı (gold — AYM bireysel başvuru)

> "Adil yargılanma hakkının çekirdek unsuru olan gerekçeli karar hakkı (Anayasa m.36, m.141), bir tarafın esaslı ve normatif olarak temellendirilmiş iddiasının derece mahkemesi tarafından tartışılması zorunluluğunu içerir. Mevcut yargılamada müvekkilimin Toparlak ve diğerleri başvurusu (B.No: 2023/59775) ile pekiştirilen bu hakkın ihlaline maruz kaldığı somut delillerle ortaya konulmuştur."

Bu sınıfta felsefi/yüksek soyut dil meşru — anayasal hak açılışı. Ancak hemen sonra somut delillere geçiş yapılıyor, dolayısıyla "felsefi cümle" eylemden kopmamış.

## Runtime Korpus Çekimi

Yukarıdaki çekirdek pasajların ötesinde, dosyaya özgü pasajlar `mcp__yargi_mcp__search_bedesten_unified` ile çekilir. Sorgu örüntüleri:

```
phrase: "[davacı vekili]" + dava türü anahtar kelimesi
court_types: ["YARGITAYKARARI"]
+ kabul kararlarına filtre (eğer karar metninden çekiliyorsa)
```

Çekilen pasajların **etiketlemesi** zorunludur:
- `kalite: gold | anti`
- `tip: dilekce_turu`
- `retorik_amac: anlati_acilisi | hukuki_sebep | karsi_arguman | talep_kapanisi`
- `eksen: logos | ethos | pathos`

## İlke (Komut Değil)

Korpus, modelin yazdığı her cümlenin altında işiteceği bir referans ritimdir. Model, korpusu **kopyalamaz**; korpusun ritmini, sıklığını, cümle uzunluğu örüntüsünü içselleştirir. Sonra ürettiği metinde aynı ritmin doğal eşi belirir.

Bu skill başlangıç sayıları az tutulmuştur (her sınıfta 1-2 örnek); kullanıcı plugin'i kullandıkça `/korpus-ekle` komutu ile büroya özgü pasajlar eklenir ve korpus zenginleşir.
