---
name: turk-dilekce-engine:dosya-kesif
version: 0.1.0
description: >
  Dilekçe yazımı seansının giriş kapısı. Dosyanın taraflarını, vakıalarını, hukuki sebep adaylarını ve talep adaylarını çıkarıp matter.md dosyasını oluşturur. Tetikleyiciler: "yeni dilekçe", "dosya keşfi", "matter intake", "dilekçeye başlayalım", "müvekkil anlattı ki". Kullanıcı bir dava hakkında konuşmaya başladığında veya bir dosya/belgeyle geldiğinde otomatik tetiklenir.
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - mcp__mevzuat_mcp__*
  - mcp__yargi_mcp__*
  - mcp__hukuk-rag__*
---

# Dosya Keşfi

## Amaç
Her dilekçe seansının omurgasını oluşturan **matter.md** dosyasını üretir. Bu dosya, sonraki tüm skill'lerin ve ajanların okuduğu ortak gerçeklik kaynağıdır.

## Disiplin: "Sorgulama, Şablon Doldurma Değildir"
Dosya keşfi bir form doldurma egzersizi değildir. Müvekkilin anlatımının, yüklenen belgelerin ve kullanıcının cümleler arasında düşürdüklerinin **dikkatli bir okumasıdır**. Bilgi eksikse, eksiği boşluk olarak bırak ve işaretle — uydurma.

## Akış

### 1. Pasif Dinleme (varsa belge/yazışma)
Kullanıcı yüklü belgeleri Read ile oku. Eğer hukuk_rag koleksiyonu varsa `mcp__hukuk-rag__hukuk_rag_ara` ile dosyaya özgü pasajları çek.

### 2. Aktif Sorgulama (gerekiyorsa)
Eksik bilgi için kullanıcıya **odaklı** sorular sor — şablon değil. "Tarafların kimliği" tipinde toplu soru sorma; "Davalının ticari sicil numarası dosyada belirtilmiş mi?" tipinde tek tek.

### 3. Hukuki Sebep Ön-Tespiti
Olgu örüntüsünden olası hukuki sebep adaylarını çıkar. `mcp__mevzuat_mcp__*` ile ilgili madde tam metnini doğrula. **Atıfı kesinleştirmeden listeleme.**

### 4. matter.md Üretimi

```yaml
---
matter_id: <isim>-<tarih>
dilekce_turu: dava | cevap | replik | duplik | istinaf | temyiz | aym_bireysel | aihm | ihtarname
gorevli_mahkeme: <Asliye Hukuk | İş Mahkemesi | İdare Mahkemesi | ...>
yetkili_yer: <il/ilçe>
---

# Matter — <Müvekkil> v. <Karşı Taraf>

## TARAFLAR
- Davacı: ... (TC: ..., adres: ...)
- Davalı: ... (Vergi/MERSIS: ...)
- (varsa) Müdahil, ihbar olunan, üçüncü kişi

## TALEP ADAYLARI
1. (Petitum) ...
2. (Faiz) ...
3. (Vekalet ücreti, yargılama gideri)

## OLGU OMURGASI (kronolojik, ham)
- YYYY-MM-DD: ... (kaynak: ...)
- YYYY-MM-DD: ... (kaynak: ...)

## HUKUKİ SEBEP ADAYLARI
- TBK m.<X> — <ad> — durum eşleşmesi: tam | kısmi | araştırılacak
- HMK m.<Y> — usul

## DELİL ENVANTERİ
- D1: sözleşme (yüklendi: <path>)
- D2: noter ihtarnamesi (<noter>, <yevmiye>)
- D3: e-posta yazışması
- ...

## KRİTİK SÜRELER
- Zamanaşımı / hak düşürücü
- Tebliğ tarihi → cevap için 2 hafta (HMK m.127)
- İstinaf 2 hafta (HMK m.345)

## AÇIK NOKTALAR (eksiklik / belirsizlik)
- ...

## STRATEJİK NOTLAR (yorum, plugin'in sonraki skill'leri için)
- ...
```

## İlke (Komut Değil)
Iyi dosya keşfi, müvekkilin söylemediği ama söylediklerinin imasında duranı yakalar. Eğer müvekkil "anlaştık ama sonra fikrini değiştirdi" diyorsa, soruşturulması gereken **zımni anlaşma argümanı**, **uygun şart**, **culpa in contrahendo** olasılıklarıdır. Bu olasılıkları kendiliğinden açıklığa kavuştur; ama doğrulamadan matter.md'ye yazma.

## Bağlam Hatırlatmaları (gold-standard'tan)
Faruk Erem'in Bir Ceza Avukatının Anıları'nda gözlemlenen örüntü: müvekkilin ilk anlatımı her zaman noksan; üçüncü görüşmeye kadar dosyanın gerçek omurgası ortaya çıkar. Plugin de aynı sabırla işler — ilk geçişte kapatmaya çalışma.
