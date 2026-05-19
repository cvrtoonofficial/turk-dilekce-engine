---
name: turk-dilekce-engine:arguman-dokumasi
version: 0.1.0
description: >
  Her ana iddiayı Toulmin'in altı bileşeniyle iç kontrolden geçirir. Claim → Data → Warrant → Backing → Qualifier → Rebuttal eşlemesini Türk hukukuna uyarlar. Garanti zımniyse görünür kılar veya zımni kalmasına izin verir. Tetikleyiciler: "argüman dokuması", "iddiayı sağlamlaştır", "Toulmin testi", "hukuki sebep zincirleme", "argümantasyon kontrol". anlati-omurga tamamlandıktan sonra otomatik tetiklenir.
allowed-tools:
  - Read
  - Write
  - Edit
  - mcp__mevzuat_mcp__*
  - mcp__yargi_mcp__*
  - mcp__hukuk-rag__*
---

# Argüman Dokuması

## Amaç
Olgu omurgasından türeyen her ana iddiayı Stephen Toulmin'in (The Uses of Argument, Cambridge UP 1958) altı bileşenli modelinde iç kontrolden geçirmek. Eveline T. Feteris'in (Fundamentals of Legal Argumentation, Springer 1999) hukuki uyarlamasını izleyen sistemin sonucu: zincirin koparılamayacağı bir argüman dokusudur.

## Toulmin Altılısının Türk Hukuk Karşılığı

| Toulmin | Türk Hukuku | Örnek |
|---------|-------------|-------|
| Claim | Talep sonucu (petitum) | "Davalının davacıya 50.000 TL tazminat ödemesine karar verilmesi" |
| Data / Grounds | Vakıalar (causa petendi) | "Sözleşmenin 6. maddesindeki ihbar süresi, 15:42'deki e-posta..." |
| Warrant | Garanti — olguyu sonuca bağlayan norm | "TBK m.347-353: süresinden önce fesih bildirimi geçersizdir" |
| Backing | Destek — garantiyi pekiştiren kaynak | "Yargıtay 6. HD <E./K.> kararı bu yönde içtihadı pekiştirir" |
| Qualifier | Niteleyici — koşullu sınır | "Ancak fesih bildirimi kabul edilmiş ve mecur teslim alınmışsa..." |
| Rebuttal | Çürütme koşulu | "Mevcut olayda teslim kabul edilmemiş, noter ihtarnamesiyle itiraz edilmiştir" |

## İlke: Her Altısının Görünür Olması Gerekmez
Usta yazımda garanti sıklıkla **zımni**; niteleyici ve çürütme koşulu **dokuma içinde** yer alır, tek tek sayılmaz. Bu skill modelin her iddiada hangi bileşenin görünür hangisinin zımni kalacağına bağlama göre karar vermesini sağlar.

Açık komut değil, sezgi: "Garantinin metinde görünmesi, hâkimin onu tartışacağı oranda gereklidir." Eğer Yargıtay'ın aynı tip davada bu normu çoğunlukla atıfsız uyguladığı görülüyorsa, garanti zımni kalabilir; eğer normun uygulanması tartışmalı veya yeni içtihatla şekilleniyorsa, garanti görünür olmalı ve Backing ile pekiştirilmelidir.

## Adımlar

### 1. Her Claim'i Sırala
Anlatı omurgasından doğan her talep sonucu adayını ayrı bir iddia satırı olarak yaz.

### 2. Data Eşlemesi
Her Claim'e onu destekleyen vakıaları (matter.md → olgu omurgası) eşle. Eksik veri varsa **kullanıcıya işaretle**, doldurma.

### 3. Warrant Tespiti
Her Claim için olguyu sonuca bağlayan normatif kuralı `mcp__mevzuat_mcp__*` ile çek. **Maddenin tam metnini al**, parafrazla yetinme. Madde tam metni yoksa o iddia askıdadır.

### 4. Backing Toplama
`mcp__yargi_mcp__*` ile aynı normu uygulayan üst yargı kararlarından **en az bir**, tercihen iki emsal çek. Yargıtay HGK > Daire > BAM hiyerarşisini koru.

### 5. Qualifier Sezgisi
Hangi koşulda garanti çalışmaz? Walton critical questions yöntemiyle düşün (bu skill preemptive-rebuttal skill'iyle çapraz çalışır):
- Bu norm hangi istisnaları taşır?
- Hangi olgu varlığında karşı tarafa avantaj doğar?

### 6. Rebuttal Yerleştirmesi
Qualifier'da tespit edilen koşulun bizim olayımızda **gerçekleşmediğini** gösteren olguyu metne yerleştir. Bu yerleştirme genellikle **olgu omurgasında** olur, "rebuttal paragrafı" olarak ayrı değil.

## Çıktı

`pac-state/<matter>/arguman-matrisi.yaml`

```yaml
claims:
  - id: C1
    petitum: "Davalının davacıya 50.000 TL maddi tazminat ödemesine"
    data:
      - "Sözleşme m.6 — 30 günlük yazılı ihbar süresi"
      - "18.02.2025 — davalının e-posta ile fesih bildirimi"
      - "Aynı gün fiili tahliye"
    warrant: "TBK m.347, m.353"
    backing:
      - source: "Yargıtay 6. HD"
        decision: "E.2023/5421 K.2023/8765 T.15.05.2023"
        verified: true  # magesh ile teyit edildi
    qualifier: "Fesih bildirimi kabul edilmiş ve mecur teslim alınmışsa zımni anlaşma değerlendirmesi"
    rebuttal_fact:
      - "19.02.2025 — noter ihtarnamesi (Yev: 12345) ile itiraz"
    visibility:
      warrant: implicit  # zımni
      backing: visible   # görünür
      rebuttal: woven    # olgu kısmına yerleştirildi
```

## Gold-Standard Örnek

Aşağıda Yargıtay'ın olumlu sonuçlandırdığı bir kira davasındaki dilekçenin argüman dokuması örneği:

> "[OLGU] Davalı, 18.02.2025 tarihinde elektronik posta ile fesih bildirmiş, aynı gün mecuru tahliye etmiştir. Müvekkilim ertesi gün noterden ihtarname ile bu fesih bildirimine itiraz ederek mecurun teslimini kabul etmediğini açıkça beyan etmiştir.
>
> [HUKUKİ DOKU] Türk Borçlar Kanunu'nun 347 ve 353. maddeleri gereği, kira sözleşmesinin süresinden önce feshinde tarafların sözleşmede öngördüğü ihbar süresine uyulması zorunludur. Yargıtay 6. Hukuk Dairesi'nin yerleşik içtihadı (E. 2023/5421 K. 2023/8765) bu kuralı pekiştirmektedir.
>
> Davalının ileri sürebileceği zımni anlaşma argümanı, müvekkilimin teslimi açıkça reddeden noter ihtarnamesi karşısında somut zemin bulamaz."

Üç paragraf — olgu, garanti (görünür) + destek, preemptive rebuttal (zımni qualifier'a açık cevap). Toulmin altısı görünür biçimde mevcut ama paragraflar "Toulmin testi" gibi değil, akan hukuk metni gibi okunuyor.

## Anti-Halüsinasyon Uyarısı
`magesh-anti-halusinasyon` skill'i bu skill'in çıktısını **mecburi olarak** denetler. Backing kısmında listelenen her Yargıtay/AYM kararının E./K./tarih bilgisi yargi_mcp ile teyit edilir; teyit edilmemiş atıf metinden çıkarılır.
