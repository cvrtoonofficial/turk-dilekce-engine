---
description: Mevcut bir dilekçeyi tüm ajanlardan geçir. Kırmızı takım debate + Sherlock + Şeytanın Avukatı + Magesh.
argument-hint: "[dilekce-yolu]"
---

# /stres-testi — Mevcut Dilekçeyi Test Et

Elinde bir dilekçe taslağı var ve onu plugin'in tüm ajanlarından geçirmek istiyorsun. Bu komut `/dilekce-yaz`'ın **sadece test aşamalarını** çalıştırır; üretim aşamalarını atlar.

## Kullanım

```
/stres-testi <dilekce-yolu>
```

veya boş bırakırsan, plugin son üretilen dilekçeyi sorar.

## Akış

### 1. Dilekçeyi Yükle
Yüklenen .docx veya .md dilekçeyi oku. matter.md varsa onu da yükle, yoksa kısa bir dosya keşfi yap.

### 2. Sherlock Çıkarımı
`sherlock-ai` ajanını çağır. Dilekçedeki olguların **arka planındaki gizli örüntüleri** ortaya çıkar. Bu dilekçenin sahibi çoktan yazdığı için Sherlock'un işi yenidir; dilekçenin gözünden kaçırdıklarına bakar.

### 3. Kırmızı Takım (Paralel)
Şu üç ajan **paralel** çağrılır:
- `hakim-ai` — en olası ret kararını yazar
- `karsi-vekili-ai` — en güçlü cevap dilekçesini hazırlar  
- `edebi-elestirmen-ai` — üslup itirazlarını çıkarır

### 4. Koordinatör Sentezi
`koordinator-ai` çağrılır. Üç ajan + Sherlock çıktısı çapraz analiz edilir. Aksiyon listesi çıkar.

### 5. Şeytanın Avukatı (Bağımsız)
`seytanin-avukati-ai` çağrılır. Kontaminasyon önlemiyle yalnızca dilekçenin son hâlini ve matter.md'yi okur. Diğer ajanların raporlarına bakmaz.

### 6. Magesh Anti-Halüsinasyon
`magesh-anti-halusinasyon` skill'i çağrılır. Her atıf çapraz teyit edilir.

### 7. Final Rapor
Tüm bulgular bir `stres-test-raporu.md` dosyasında toplanır:
- 🔴 Kritik (zorunlu düzeltme)
- 🟡 Orta (önerilen düzeltme)
- 🟢 Küçük (bağlam bilgisi)
- ⚠ Halüsinasyon riski (zorunlu düzeltme)

## Çıktı

`pac-state/<matter>/stres-test-raporu-<YYYY-MM-DD>.md`

Bu rapor, dilekçeyi göndermeye hazır mı sorusuna **gerekçeli** bir cevap verir.

## Önemli

- Bu komut **dilekçenin kendisini değiştirmez**. Yalnızca bulguları raporlar.
- Düzeltmeleri uygulamak için kullanıcı kendisi karar verir.
- Eğer bulguları otomatik uygulamak istiyorsanız, `/dilekce-yaz` kullanın (akış içinde revizyon vardır).
