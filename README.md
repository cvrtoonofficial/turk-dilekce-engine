# turk-dilekce-engine

> Hâkimin reddetmediği, karşı tarafın çürütemediği, müvekkilin kendi hikâyesini gördüğü bir dilekçe nasıl yazılır?

Türk hukukunda dilekçe (dava, cevap, replik, istinaf, temyiz, AYM bireysel başvuru, ihtarname) üretiminin niteliğini yükselten retorik motoru. **Şablon kullanmaz.** AI'ya "şu üslubu kullan, felsefi cümle kur" demek yerine, ustaların metinleriyle (Faruk Erem, Ali Haydar Özkent, Sami Selçuk + Yargıtay/AYM gold-standard pasajları) örnek-temelli davranış kazandırır. Model, ne zaman ne yoğunlukta hangi retorik aracı kullanacağına kendi sezgisiyle karar verir.

## Kurulum

```bash
# Marketplace üzerinden
/plugin install turk-dilekce-engine

# Veya doğrudan
cp -r turk-dilekce-engine ~/.claude/plugins/
```

## Mimari Özeti

**8 Skill** (sezgisel rehberler, in-context yüklenir):

| Skill | Görev |
|-------|-------|
| dosya-kesif | Tarafları, vakıaları, hukuki sebepleri, talep adaylarını çıkarır. matter.md oluşturur. |
| anlati-omurga | Olguları karakter + çatışma + kronoloji + tema dörtlüsünde organize eder. m.194 testi. |
| arguman-dokumasi | Her iddiayı Toulmin altılısı ile iç kontrole alır. Garanti zımniyse görünür kılar. |
| gold-standard-korpus | yargi_mcp'den onaylı dilekçe pasajları + ret pasajları (anti-örnek). |
| preemptive-rebuttal | Walton critical questions ile karşı saldırı vektörleri; zımni çürütme. |
| retorik-ucgen-kalibrator | Logos/ethos/pathos dengesini paragraf-bazında bağlamda kalibre eder. |
| uslup-akicilik-katmani | Mahkeme nutku geleneği + bilişsel yük literatürü. "Az ve sezilmez" bekçisi. |
| magesh-anti-halusinasyon | Her atıfı çapraz teyit. Misattribution / hierarchy / fabrication kontrolü. |

**6 Ajan** (kontaminasyon önleyici, izole bağlam, rol oynayan sub-AI'lar):

| Agent | Rol |
|-------|-----|
| hakim-ai | Görevli mahkemenin hâkimi; en olası ret kararını HMK m.297 standardında yazar. |
| karsi-vekili-ai | Karşı taraf vekili; bizim dilekçemize en güçlü cevap dilekçesini hazırlar. |
| edebi-elestirmen-ai | Üslup denetleyicisi; abartılı edebî unsur ve sahte yüksek dil bekçisi. |
| seytanin-avukati-ai | Final stres testi (Sherlock 10-adım-önde + kontaminasyon önleme). |
| sherlock-ai | Mikro-detaylardan büyük somut çıkarımlar. Bilirkişi raporundan, yazışmalardan, davalı imalarından dosyaya yeni delil çıkar. |
| koordinator-ai | Ajanlar arası debate'i yönetir; Walton dialektik üç-katmanı. |

**4 Slash Command:**

- `/dilekce-yaz` — Yeni dilekçe yazma seansı başlat (akış: 1→8)
- `/stres-testi` — Mevcut dilekçeyi tüm ajanlardan geçir
- `/uslup-kontrol` — Tek başına üslup-akıcılık değerlendirmesi
- `/korpus-ekle` — Gold-standard veya anti-örnek pasaj ekle

**1 Hook:**

- PostToolUse magesh otomatiği — Bir dilekçe `.docx` üretildiğinde anti-halüsinasyon kalkanı otomatik çalışır.

## Bağımlılıklar

Bu plugin **kendi MCP'sini getirmez**. Mevcut Türk hukuk MCP'lerine bağımlıdır:

- `yargi_mcp` — Yargıtay, Danıştay, AYM, AİHM, KVKK kararları
- `mevzuat_mcp` — Kanun, KHK, Tüzük, Yönetmelik içerik
- `yoktez_mcp` — YÖK Ulusal Tez Merkezi
- `literatur_mcp` — DergiPark akademik makale
- `hukuk_rag` (opsiyonel) — Yerel dava koleksiyonu semantik arama

## Felsefe: "Talimat Değil, Tanıklık"

Bu plugin'in tek tasarım ilkesi şudur: **modele ne yapması gerektiğini söylemek yerine, ustaların ne yaptığını göster.**

`uslup-akicilik-katmani` skill'ine "felsefi dil kullan" diye komut vermek aşırı-uygulamaya yol açar (her paragrafta yapay soyutlama). Bunun yerine skill, Erem-Özkent-Selçuk'tan örnek pasajlar ve abartılı anti-örnekler içerir; model, bağlamda hangi paragrafta hangi yoğunlukta edebî unsur kullanacağına kendi sezgisiyle karar verir.

Akademik dayanak: Anthropic Constitutional AI (Bai et al. 2022), few-shot exemplar-dense priming (Brown et al. 2020), Anthropic heuristic-driven skill design (2025-2026 resmi rehber).

## Uyarılar ve Sınırlar

- **Profesyonel sorumluluk avukatındadır.** Plugin yardımcıdır, karar verici değildir.
- **Müvekkil sırrı korunur** — çıktılar yerel kalmalı, dış API'lere müvekkil bilgisi göndermek için açıkça onay gerekir.
- **Anti-halüsinasyon zorunlu** — hiçbir çıktı magesh skill'inden geçmeden teslim edilmez.
- **Edebî unsur olgusal doğruluğu süslemek için değil, hâkimin okumayı bırakmamasına hizmet için kullanılır.** "Az ve sezilmez" — eğer hâkim "ne güzel yazılmış" diyorsa, abartı eşiği aşılmıştır.

## Akademik Temel

Bu plugin'in altında 30 sayfalık akademik ön araştırma raporu vardır (`dilekce-plugin-on-arastirma.docx`). Temel kaynaklar: Ertuğrul Uzun, Eveline T. Feteris, Ayça Akkayan-Yıldırım, Fahrettin Kayhan, Ahmet Kerem Çakın, Vedat Ahsen Coşar, Kemal Gözler, Stephen Toulmin, Chaïm Perelman, Douglas Walton, Anthony G. Amsterdam, Bryan A. Garner, Ross Guberman, Faruk Erem, Ali Haydar Özkent, Sami Selçuk, Shaun Spencer & Adam Feldman, Kathryn M. Stanchi, John Sweller, Varun Magesh ve diğerleri.

## Versiyon

`0.1.0` — İlk yayım. Üslup-akıcılık katmanı ve Sherlock-AI dahil 8 skill + 6 ajan.
