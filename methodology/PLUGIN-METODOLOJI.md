# turk-dilekce-engine — Metodoloji Özeti

Bu plugin'in altında yatan akademik temel ve tasarım kararlarının kısa özeti. Detaylı akademik araştırma için `dilekce-plugin-on-arastirma.docx` belgesine bakın.

## Tasarım Felsefesi: "Talimat Değil, Tanıklık"

Tek tasarım ilkesi: **modele ne yapması gerektiğini söylemek yerine, ustaların ne yaptığını göster.** Açık komutlar (örn. "felsefi dil kullan") aşırı-uygulamaya yol açar; örnek-temelli konumlandırma (exemplar-dense priming) bağlam-duyarlı uygulamayı doğal kılar.

Akademik temel:
- Anthropic Constitutional AI (Bai vd., 2022)
- Few-shot exemplar learning (Brown vd., 2020 — GPT-3 paper)
- Anthropic resmi skill design ilkeleri (2025-2026 dönemi rehberleri)
- Wei vd. (2022) — Chain-of-Thought Prompting

## Üç Katmanlı Mimari

### Katman 1: Skill'ler (Sezgisel Rehberlik)
8 skill, in-context yüklenir, bağlama göre tetiklenir. Her skill: kısa ilke ifadesi + gold-standard örnekler + anti-örnekler.

### Katman 2: Ajanlar (Kontaminasyon Önleyici Roller)
6 ajan, izole bağlamda çalışır. Hâkim, karşı vekil, edebî eleştirmen, Sherlock, şeytanın avukatı, koordinatör — her biri ayrı bir perspektifi, ana modeli kirletmeden taşır.

### Katman 3: Komutlar ve Hook'lar (Kullanıcı Etkileşim Sınırı)
4 slash command (`/dilekce-yaz`, `/stres-testi`, `/uslup-kontrol`, `/korpus-ekle`) + 2 hook (PostToolUse, PreCompact). Kullanıcının doğrudan tetikleyebileceği veya otomatik çalışan kapılar.

## Akademik Kaynaklar (Kısa)

### Türk Hukuk Geleneği
- Ali Haydar Özkent, *Avukatın Kitabı* (1940)
- Faruk Erem, *Bir Ceza Avukatının Anıları*
- Sami Selçuk, *Ana Dili Bilinci, Türkçe ve Hukuk Dilimiz*
- Ertuğrul Uzun, *Hukuk Metodolojisinin Sorunları* (2016)
- Ahmet Kerem Çakın, *Avukatlar İçin Dilekçe Yazım Teknikleri* (2024)
- Ayça Akkayan-Yıldırım, "Retorik Üçgen" çalışması
- Fahrettin Kayhan, "Türk Savunma Ekolü Retorik Teorisi"

### Klasik Retorik ve Argümantasyon
- Aristoteles, *Retorik* (logos / ethos / pathos)
- Stephen Toulmin, *The Uses of Argument* (1958) — altı bileşen
- Chaïm Perelman & Lucie Olbrechts-Tyteca, *The New Rhetoric* (1958) — universal audience
- Eveline T. Feteris, *Fundamentals of Legal Argumentation* (1999) — Türkçe çevirisi (Ertuğrul Uzun, 2010)
- Douglas Walton & Thomas F. Gordon (2013) — critical questions

### Hukuki Anlatı
- Anthony G. Amsterdam & Jerome Bruner, *Minding the Law* (Harvard UP, 2000)
- Philip N. Meyer, *Storytelling for Lawyers* (Oxford UP, 2014)

### Modern Legal Writing
- Bryan A. Garner & Antonin Scalia, *Making Your Case* (2008)
- Bryan A. Garner, *The Winning Brief*, *Legal Writing in Plain English*
- Ross Guberman, *Point Made* (Oxford UP, 2014)

### Bilişsel Yük ve Okunaklılık
- Shaun Spencer & Adam Feldman, "Words Count" — Legal Writing Journal
- Kristen Konrad Robbins-Tiscione (2002) — federal hâkim anketi
- Kathryn M. Stanchi, "The Power of Priming in Legal Persuasion"
- John Sweller (1988+) — cognitive load theory

### Sherlock / Detective Yöntem
- Carlo Ginzburg, "Clues: Roots of a Scientific Paradigm" (1979)
- Charles Sanders Peirce — abductive reasoning

### Anti-Halüsinasyon
- Varun Magesh, Faiz Surani, Matthew Dahl, Mirac Suzgun, Christopher D. Manning, Daniel E. Ho, "Hallucination-Free? Assessing the Reliability of Leading AI Legal Research Tools" — *Journal of Empirical Legal Studies*, 2025 (Stanford RegLab)

### Realist Hukuk Teorisi (Hâkim-AI temeli)
- Karl Llewellyn, *The Common Law Tradition* (1960) — situation sense
- Joseph Hutcheson, "The Judgment Intuitive" (1929) — judicial hunch

## Akış Şeması

```
1. Müvekkil/dosya
   ↓
2. /dilekce-yaz çağrılır
   ↓
3. dosya-kesif (matter.md)
   ↓
4. sherlock-ai (aksaklıklar)
   ↓
5. anlati-omurga (m.194 testi)
   ↓
6. arguman-dokumasi (Toulmin)
   ↓
7. gold-standard-korpus (örnek pasajlar yüklenir)
   ↓
8. preemptive-rebuttal (hâkim refleksi + Walton CQ)
   ↓
9. İLK TASLAK (dilekce-v1.md)
   ↓
10. retorik-ucgen-kalibrator (logos/ethos/pathos)
    ↓
11. uslup-akicilik-katmani (Özkent/Erem/Selçuk damarı)
    ↓
12. KIRMIZI TAKIM DEBATE (paralel):
    - hakim-ai
    - karsi-vekili-ai
    - edebi-elestirmen-ai
    ↓
13. koordinator-ai (sentez + aksiyon listesi)
    ↓
14. REVİZYON (dilekce-v2.md)
    ↓
15. seytanin-avukati-ai (bağımsız final test)
    ↓
16. magesh-anti-halusinasyon (ZORUNLU GATE)
    ↓
17. FINAL .docx
```

## Kritik İlkeler

1. **Şablon yasağı** — Plugin asla şablon doldurmaz.
2. **HMK m.297 eşlemesi** — Her dilekçe, hâkimin kararını yazmasını kolaylaştırır.
3. **m.194 somutlaştırma** — Her vakıa cümlesi 3+ disiplinden geçer.
4. **Anti-halüsinasyon zorunluluğu** — Hiçbir çıktı Magesh skill'inden geçmeden teslim edilmez.
5. **"Az ve sezilmez" edebî prensibi** — Hâkim "ne güzel yazılmış" demediği oranda akıcılık doğru.
6. **Gizlilik** — Müvekkil bilgileri yerel kalır.
7. **Profesyonel sorumluluk avukatındadır** — Plugin yardımcı, karar verici değil.
