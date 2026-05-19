---
name: turk-dilekce-engine:magesh-anti-halusinasyon
version: 0.1.0
description: >
  ZORUNLU GATE. Dilekçedeki her atıfı (Yargıtay, AYM, AİHM, Danıştay, KVKK Kurul, mevzuat, doktrin, büro emsali) yargi_mcp / mevzuat_mcp / literatur_mcp / yoktez_mcp ile çapraz teyit eder. Magesh ve diğerleri (2025) "Hallucination-Free?" çalışmasındaki üç hata kategorisini (misattributed authorship, mishandled hierarchy, fabricated citation) yapısal olarak işaretler. Tetikleyiciler: "atıfları doğrula", "halüsinasyon kontrolü", "Magesh", "anti-halüsinasyon", "kalkanı çalıştır". Bir dilekçe çıktısı üretildiğinde otomatik olarak hook ile tetiklenir.
allowed-tools:
  - Read
  - Write
  - Edit
  - Grep
  - mcp__yargi_mcp__*
  - mcp__mevzuat_mcp__*
  - mcp__bafdf342-e57f-4eff-9a76-cef9f0201e9f__*
  - mcp__96dc4a19-628d-4d78-b645-8306e9232dcc__*
  - mcp__417bcc1e-6b4e-414f-a232-c2e87fbb98d6__*
  - mcp__3f9cf426-181c-4035-93b7-39ad26a99dae__*
  - mcp__hukuk-rag__*
---

# Magesh Anti-Halüsinasyon Kalkanı

## Amaç
Varun Magesh, Faiz Surani, Matthew Dahl, Mirac Suzgun, Christopher D. Manning ve Daniel E. Ho'nun (Stanford RegLab) 2025 yılında Journal of Empirical Legal Studies'te yayımladıkları "Hallucination-Free? Assessing the Reliability of Leading AI Legal Research Tools" çalışmasında belgelenen üç temel halüsinasyon kategorisini yapısal olarak engellemek:

1. **Misattributed authorship** — içtihadın yanlış mahkemeye atfedilmesi (örn. Yargıtay 6. HD kararının AYM'ye atfı)
2. **Mishandled hierarchy** — alt mahkeme kararının üst hiyerarşide konumlandırılması (örn. ilk derece kararının "Yargıtay içtihadı" gibi sunulması)
3. **Fabricated citation** — varlığı doğrulanamayan / uydurma içtihat (örn. "Yargıtay 1. HD E.2024/9999 K.2024/8888" — sayılar gerçek mahkeme kararına denk düşmüyor)

Stanford araştırması, hatta uzmanlaşmış hukuki AI araçlarının (Lexis+ AI %17, Westlaw AI %33, GPT-4 %43) bu hatalardan tamamen muaf olmadığını göstermiştir. Plugin'in bu skill'i **zorunlu gate**'tir: hiçbir çıktı buradan geçmeden kullanıcıya teslim edilmez.

## Üç Aşamalı Tarama

### Aşama 1: Atıf Çıkarımı (regex + bağlam)
Üretilen dilekçeden tüm atıfları çıkar. Atıf örüntüleri:
- Yargıtay: `(?:Yargıtay\s+)?(\d+)\.\s*(Hukuk|Ceza)\s+Daires(?:i|inin).*?E[.,]?\s*(\d{4})/(\d+)\s+K[.,]?\s*(\d{4})/(\d+)`
- HGK / BGK / CGK / CGKK
- AYM bireysel başvuru: `B\.?\s*No[:\.]?\s*(\d{4})/(\d+)`
- AYM norm denetimi: `E\.\s*(\d{4})/(\d+),?\s*K\.\s*(\d{4})/(\d+)`
- AİHM: `(?:App\.?\s*No\.?|Başvuru\s*No)\s*[:\.]?\s*(\d+/\d+)`
- Mevzuat: kanun adı + madde no + (varsa) RG tarih/sayı
- Doktrin: yazar + eser + sayfa (varsa)

### Aşama 2: Çapraz Teyit

Her atıf için ilgili MCP'ye sorgu:
- Yargıtay/Danıştay → `mcp__yargi_mcp__search_bedesten_unified` (esas no + karar no ile arama)
- AYM bireysel → `mcp__yargi_mcp__search_anayasa_unified` decision_type=bireysel_basvuru
- AYM norm denetimi → `mcp__yargi_mcp__search_anayasa_unified` decision_type=norm_denetimi
- AİHM → `mcp__yargi_mcp__search_bedesten_unified` (uygun filtre) veya WebSearch
- KVKK → `mcp__yargi_mcp__search_kvkk_decisions`
- Mevzuat → `mcp__mevzuat_mcp__search_within_kanun` veya benzeri tip-uygun arama
- Doktrin → `mcp__417bcc1e-6b4e-414f-a232-c2e87fbb98d6__search_articles` (DergiPark) + `mcp__3f9cf426-181c-4035-93b7-39ad26a99dae__search_yok_tez_detailed`

**Doğrulama sonucu üç durumdan biri:**
- ✓ Doğrulandı (E./K./tarih + mahkeme + içerik özeti tutarlı)
- ⚠ Kısmî (numara doğru ama daire/birim eşleşmiyor)
- ✗ Bulunamadı / çelişkili

### Aşama 3: Üç Hata Kategorisinin Yapısal Kontrolü

**Misattributed authorship kontrolü:**
- Bir karar bir mahkemeye atfedildi. Doğrulamada gerçek mahkeme farklı çıktı → işaretle.
- Örnek hata: "AYM 6. HD" — AYM'de 6. HD yoktur, bu Yargıtay yapısıdır.

**Mishandled hierarchy kontrolü:**
- Bir karar "Yargıtay içtihadı" diye sunuldu ama gerçekte BAM (istinaf) veya yerel mahkeme kararı çıktı.
- Bir karar "yerleşik içtihat" diye sunuldu ama tek bir karar var ve aksi yönde son dönem kararı mevcut.

**Fabricated citation kontrolü:**
- Atıfı doğrulayan kaynak bulunamadı → fabrike şüphesi.
- Atıf doğru ama içerik özeti uydurma → kısmî fabrike.

## Çıktı

`pac-state/<matter>/magesh-dogrulama-raporu.md`

```markdown
# Anti-Halüsinasyon Doğrulama Raporu — <Matter ID>

Çıktı dosyası: dilekce-v3.docx
Tarama tarihi: <YYYY-MM-DD>
Toplam atıf: 27
Doğrulanan: 24 (%89)
Kısmî: 2 (%7)
Bulunamayan / şüpheli: 1 (%4)

## ✓ Doğrulanan Atıflar (24)
1. Yargıtay 6. HD E.2023/5421 K.2023/8765 T.15.05.2023 — bağ kuran içtihat olarak doğrulandı.
2. AYM Tahsin Toparlak ve diğerleri B.No: 2023/59775 T.24.07.2023 — gerekçeli karar hakkı ihlali olarak doğrulandı.
...

## ⚠ Kısmî Doğrulama (2)
- "Yargıtay HGK 2024/XYZ K." — esas yıl/no doğru, karar yıl/no eşleşmiyor. **DÜZELT** veya **KAYNAĞA GERİ DÖN.**
- "Danıştay 5. Daire 2025/12345" — daire numarası 13 olmalı. **DÜZELT.**

## ✗ ŞÜPHELİ (1)
- "Yargıtay 12. HD E.2019/8888 K.2020/9999" — yargi_mcp'de bulunamadı, içerik özeti uydurma şüphesi. **DİLEKÇEDEN ÇIKAR veya teyit edilebilir alternatif emsal getir.**

## SONUÇ
Dilekçe **şu anki haliyle KULLANILMAZ.** Yukarıdaki 1 ⚠ ve 1 ✗ kaynağın düzeltilmesi şarttır.
```

## İlke (Komut Değil)

Halüsinasyon kalkanı bir filtre değil, bir disiplindir. Skill, her atıfı sorgular; ama daha önemlisi, **doğrulama kararının net olmadığı her durumda kullanıcıya işaretler**. "Muhtemelen doğru" yetmez; ya doğrulandı ya doğrulanmadı.

Plugin'in temel sözleşmesi şudur: **doğrulanmamış atıfı taşıyan hiçbir çıktı teslim edilmez.** Bu sözleşme kullanıcıya geçici bir gecikme yaratabilir, ama Magesh ve diğerlerinin belgelediği halüsinasyon vakalarının baroyu, müvekkili ve mahkemeyi ne kadar zora soktuğunu hatırlayalım — gecikme, hatadan iyidir.

## Otomatik Tetikleme (Hook)

Bu skill `hooks/hooks.json` içinde bir PostToolUse hook ile bağlıdır. Bir dilekçe `.docx` üretildiğinde skill otomatik çalışır; kullanıcının ayrıca tetiklemesi gerekmez.
