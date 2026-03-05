# Yapay Zeka ile Geliştirme Süreçleri ve Cursor IDE

## 1. Giriş ve Motivasyon

Yazılım geliştirme süreçlerinde en büyük mücadele çoğu zaman teknik zorluklar değildir.
Asıl mücadele; zamanla yarışmak, artan iş yükünü yönetmek ve aynı kaliteyi koruyarak daha hızlı üretebilmektir.

Her sprintte benzer sorular tekrar eder:
Daha hızlı olabilir miyiz?
Tekrar eden işleri azaltabilir miyiz?
Geliştiricinin enerjisini gerçekten değer üreten kısımlara nasıl odaklayabiliriz?

Yapay zekâ destekli geliştirme araçları tam bu noktada bir fırsat sunuyor.
Ama mesele sadece yeni bir araç kullanmak değil.
Mesele, geliştirme alışkanlıklarını yeniden düşünmek.

Eğer bir işi daha kısa sürede, daha az mental yorgunlukla ve daha yüksek doğrulukla yapma imkânı varsa,
bunu görmezden gelmek bir seçenek olmamalı.

Bu yaklaşımın arkasındaki motivasyon çok basit:

Daha çok çalışmak değil,
daha akıllı çalışmak.

Bunun sonucunda ise eskiden 1-2 haftada (bir sprint'te) yapılan işler artık 2-4 güne indirilebiliyor.

---

## 2. Tarihsel Arka Plan: Buraya Nasıl Geldik?

### 2.1. Attention is All You Need (2017)

https://arxiv.org/pdf/1706.03762

Google'ın 2017'de yayımladığı bu makale iki temel kavramı tanıttı:

- **Attention Mekanizması:** Contexte göre kelimelere anlam verme yeteneği. Örneğin "yüz" kelimesi; "insan yüzü", "yüzmek" veya "sayı olarak 100" anlamına gelebilir. Önceki kelimelere bakarak doğru anlamı çıkarma becerisi.
- **Transformer Mimarisi:** Encoder (sol taraf) ve Decoder (sağ taraf) olmak üzere iki bölümden oluşur. Bugün kullandığımız GPT modelleri sadece decoder kısmını kullanır.

Bu makalenin yazarları arasında OpenAI'ın kurucularından **Ilya Sutskever** de yer alıyordu.

### 2.2. GPT-1 (2018)

OpenAI, Transformer makalesini uygulamaya geçirdi. 7.000 farklı alandan kitap toplayarak **117 milyon parametreli** bir dil modeli eğitti. Attention mekanizması sayesinde model artık contextten haberdar şekilde cevap üretebiliyordu. Ancak veri yetersizliği nedeniyle kapsamlı sorulara mantıklı yanıt veremiyordu.

### 2.3. Veri Toplama Süreci (Hugging Face - FineWeb Yazısı)


Hugging Face: Yapay zekanın githubı

Büyük şirketler model eğitimi için nasıl veri topluyor? Hugging Face bunu deneysel olarak açıkladı:

1. **Common Crawl:** Tüm internetin anlık görüntüsünü (snapshot) alan kar amacı gütmeyen kuruluş. Yaklaşık 3 milyar web sitesini indeksleyip AWS S3'e yüklüyor.
https://huggingface.co/spaces/HuggingFaceFW/blogpost-fineweb-v1

2. **URL Filtering:** Zararlı, gizli veri içeren siteler blok listesiyle çıkarılıyor.
3. **Text Extraction:** HTML, CSS, JavaScript gibi gereksiz kodlar temizleniyor; sadece metin içeriği kalıyor.
4. **Language Filtering:** Her dil için bir skor belirleniyor (örneğin İngilizce ≥ 0.65). Eşiği geçen siteler modele dahil ediliyor.
5. **PII Removal:** TC kimlik numarası, e-posta, telefon gibi kişisel veriler siliniyor.

Bu aşamalardan geçen rafine veri, LLM eğitiminde kullanılan ana veri kaynağını oluşturuyor.

### 2.4. GPT-2
https://huggingface.co/openai-community/gpt2/tree/main

OpenAI, veri kalitesini artırarak **1,5 milyar parametreli** GPT-2'yi eğitti. Open source olarak Hugging Face üzerinden erişilebilir (SafeTensor dosyaları, ~548 MB). Attention mekanizması ve sağlıklı veri birleşince performansta büyük artış sağlandı. Ancak yeni bir sorun ortaya çıktı: uzun metinlerde model konteksten kopuyor ve anlamsız cevaplar üretmeye başlıyordu.

### 2.5. GPT-3 ve ChatGPT'nin Doğuşu

GPT-3 ile uzun ve tutarlı cevap üretme problemi aşıldı. OpenAI, 2022'de ChatGPT'yi (GPT-3 tabanlı instruct model) **ChatGPT.com** üzerinden herkese açtı.

**Rekor:** 5 günde 1 milyon kullanıcıya ulaştı. Karşılaştırma olarak Instagram 2,5 ayda, Facebook 9 ayda, Netflix 3,5 yılda aynı sayıya ulaşmıştı.  

---

## 3. Yapay Zeka ile Geliştirmenin Evrimi

### 3.1. İlk Dönem: Copy-Paste Çağı

ChatGPT açıldıktan sonra geliştiriciler şöyle çalışıyordu:

- ChatGPT ekranına gidip "Abstract Factory Pattern'de Spring Boot implementasyonu yaz" gibi istekler yazılıyordu.
- Üretilen kod kopyalanıp projeye yapıştırılıyordu.
- Bug'lı kod da aynı şekilde ChatGPT'ye gönderilip düzeltme isteniyordu.

**Sorunlar:**
- **Zaman kaybı:** Sürekli sekme/pencere değiştirme.
- **Context eksikliği:** ChatGPT projenin konvansiyonlarından, dosya yapısından habersiz. Ürettiği kod projeye uymuyor, yapay zeka tarafından yazıldığı belli oluyordu.

### 3.2. İkinci Dönem: Extension'lar

VS Code'a **GitHub Copilot**, **Continue Dev**, **Cline** gibi eklentiler kurularak, IDE'den çıkmadan ve contextden haberdar şekilde geliştirme yapılabilir hale geldi.

### 3.3. Üçüncü Dönem: AI-Native IDE'ler

Tamamen yapay zeka odaklı yeni IDE'ler ortaya çıktı: **Cursor**, **Windsurf (Codeium)**, **Anysphere** vb. Bu araçlar sıfırdan AI destekli geliştirme için tasarlandı.

---

## 4. Cursor IDE

### 4.1. Nedir?

Cursor, **MIT'den üç öğrencinin** Visual Studio Code'u fork'layıp yapay zeka yetenekleriyle donattığı bir IDE. VS Code'a çok benzer bir arayüze sahip, ancak AI entegrasyonu native olarak gömülü. Şu an OpenAI ile rekabet eden, çok hızlı büyüyen ve kârlı bir şirket.

### 4.2. İlk Kurulum

İndirdiğinizde iki mod seçeneği sunuluyor:

- **Privacy Mode:** Cursor, kodlarınızı hiçbir şekilde saklamadığını, response döndükten sonra sildiğini iddia ediyor. Ancak yeni bazı özellikler önce sadece Public Mode'da kullanılabiliyor.
- **Public Mode:** Özellikle open source projelerde gönül rahatlığıyla kullanılabilir.

### 4.3. Temel Özellikler

#### Özellik 1: Auto-Completion (Otomatik Tamamlama)

Kod yazarken **T anında** ne yapıyorsanız, **T+1 anında** ne yapmak istediğinizi tahmin edip öneri sunar. Bir comment satırı yazıp Enter'a bastığınızda, Cursor bütün kodu otomatik önerir. Tab ile kabul edip ilerleyebilirsiniz.

Örnek: `// start api with express` yazdığınızda, Express sunucusu kodunu otomatik üretir. Hatta başka bir dosyada (package.json) Express dependency'sini bile otomatik ekler — siz o dosyada hiç Express'ten bahsetmemiş olsanız bile.

#### Özellik 2: Inline Edits (Satır İçi Düzenleme)

Kodun sadece belirli bir bölümünü seçip düzenleme talimatı verebilirsiniz. Seçili alan LLM'e kontekst olarak gönderilir.

- **Edit Selection:** Seçili kodu değiştirmesini istersiniz.
- **Quick Question:** Seçili alanın ne yaptığını sormak için kullanılır.

Token optimizasyonu açısından sadece gerekli kısmı seçmek önemlidir.

#### Özellik 3: Chat Paneli (eski adıyla Composer)

Sağ tarafta açılan, en güçlü özellik. ChatGPT'ye gidip copy-paste yapmak yerine, dosya referansı vererek doğrudan analiz ve düzenleme yaptırabilirsiniz.

Kullanım: `@server.js` yazarak dosyayı kontekst olarak verip "Bu dosyada ne yapılıyor?" diye sorabilirsiniz.

**Paralel çalışma:** Birden fazla chat sekmesi açıp farklı işleri eş zamanlı yürütebilirsiniz.

---

### 4.4. Modlar

Mod değiştirdiğinizde aslında arka plandaki **System Prompt** değişiyor:

| Mod | Açıklama |
|-----|----------|
| **Ask** | Sadece soru-cevap. Dosya düzenleme, oluşturma veya silme yetkisi yok. |
| **Agent** | Tam yetki. Dosya açar, siler, düzenler, terminal komutu çalıştırır. Geliştirme sürecinde genellikle bu mod kullanılır. |
| **Plan** | Büyük görevleri parçalara ayırıp to-do listesi oluşturur, adım adım ilerler. |
| **Custom** | Kendi modlarınızı tanımlayabilirsiniz (ör. Refactor, Bug Fix, Security Audit). Her moda özel system prompt ve model atayabilirsiniz. |

Custom mod örneği: "Refactor" modu tanımlayıp system prompt'a "SOLID ve OOP prensiplerine uygun şekilde refactor et" yazabilirsiniz. **Playbooks.com**'dan hazır modlar da kopyalanabilir.

---

### 4.5. Model Seçimi

Cursor, sizi LLM API süreçlerinden tamamen soyutlar. Built-in modeller:

- Claude Sonnet, Claude Opus
- GPT-4.5, GPT o1
- Gemini

Her modelin kendine özgü **trade-off**'ları vardır. Sonnet kodlamada hızlı ve başarılı; o1 matematiksel problemlerde çok güçlü ama yavaş (uluslararası matematik olimpiyatında %86 başarı — GPT 4.5 aynı testte %16). **Auto** modu açarsanız, Cursor işe en uygun modeli otomatik seçer.

**Harici model ekleme:** AWS Bedrock, Azure OpenAI, Google AI, Anthropic API gibi third-party provider'lar bağlanarak DeepSeek vb. modeller de kullanılabilir.

---

### 4.6. Context Window ve Tokenization

Yazdığınız her şey arka planda **token**'lara dönüştürülür (tokenization). Her modelin kabul ettiği maksimum token sayısına **Context Window** denir:

- Claude Sonnet: 200K token (API üzerinden)
- Gemini: 200K (API) / 1M (web arayüzü)

Cursor arka planda **optimizasyon** yapar: Verdiğiniz dosyaların tamamını değil, sadece en ilgili kısımları LLM'e gönderir. Bu özelliği devre dışı bırakmak isterseniz **Full Path** seçeneğini açabilirsiniz.

---

### 4.7. Max Mode

Normal modda her istekte ~25 tool call sınırı varken, Max Mode'da bu sınır ~200'e çıkar. Uzun süreçli görevler için idealdir ama daha fazla token tükettiğinden maliyetli olabilir.

---

### 4.8. Use Multiple Models

Aynı görevi paralelde birden fazla modelle çalıştırıp sonuçları karşılaştırabilirsiniz. Hangi modelin daha iyi çıktı ürettiğini görüp o sonucu kabul edebilirsiniz. (Git bağlantısı gerekli.)

---

### 4.9. Context Seçenekleri

Chat panelinde kontekst olarak verebileceğiniz kaynaklar:

| Kaynak | Açıklama |
|--------|----------|
| **Dosya** | `@server.js` şeklinde tek dosya referansı |
| **Klasör** | Tüm klasörü kontekst olarak verme |
| **Dokümentasyon** | .NET, AWS S3 vb. resmi dokümanların URL'si veya kendi dokümanlarınız |
| **Terminal** | Log çıktıları veya hata mesajları |
| **Git** | Branch'ler arası fark analizi, son commit incelemesi |
| **Resim** | Figma'dan SS alıp yapıştırarak HTML/CSS yazdırma |
| **Web** | Cursor panelinden web araması |
| **Speech** | Sesli giriş (az kullanılıyor) |

---

### 4.10. Proje Konfigürasyon Dosyaları

#### `.cursorignore`
Git'teki `.gitignore` mantığında: API key'ler, credentials gibi hassas bilgiler içeren dosyaları Cursor'ın okumasını engeller.

#### `.cursorindexignore`
Cursor projeyi açtığında tüm dosyaları indeksler (vektör veritabanında saklar, RAG ile hızlı/akıllı cevap üretmek için). Bu dosyaya eklediğiniz dosyalar indekslenmez.

#### `.cursor/` Klasörü

**Commands:** Sık kullanılan işlemleri tanımlayabilirsiniz. Örneğin `/create-pr` komutuyla PR açma, `/security-audit` komutuyla güvenlik taraması yapma.

**Hooks:** Her tool call öncesinde veya sonrasında otomatik çalışacak komutlar tanımlayabilirsiniz. Örneğin "her dosya edit'inden sonra formatla" gibi. `before_file_edit`, `after_file_edit` gibi hook noktaları mevcut.

**Rules:** Projeye özel kurallar tanımlanır. Örneğin:
- "Her yeni controller şu klasöre oluşturulsun"
- "Error handling şu pattern'e uygun olsun"
- "Endpoint naming convention şu şekilde olsun"

Rules'ların ne zaman uygulanacağı da ayarlanabilir:

| Seçenek | Açıklama |
|---------|----------|
| **Always Apply** | Her istekte çalışır (genelde gereksiz) |
| **Apply Intelligently** | Cursor ilgili olduğuna karar verdiğinde otomatik uygular |
| **Specific Files** | Belirli klasör/dosya değiştiğinde çalışır |
| **Manual** | Sadece siz tetiklediğinizde çalışır |

Monorepo kullanıyorsanız rules'ları frontend/backend olarak modüler şekilde bölebilirsiniz. Rules proje bazında veya global bazda tanımlanabilir.

---

## 5. MCP — Model Context Protocol

### 5.1. Problem

LLM'lerin iki temel eksikliği vardı:
- **Sadece read-only:** "Dubai'deki otelleri listele" olur ama "rezervasyon yap" olmaz.
- **Güncel değil:** Eğitim verisinin tarihinden sonraki olayları bilmez. Yeniden eğitmek çok maliyetli (Facebook, LLaMA 2 için 6.000 GPU x 12 gün = 2-3 milyon dolar harcadı).

### 5.2. Çözüm: MCP

**Anthropic** tarafından geliştirilen, **LSP (Language Server Protocol)**'den esinlenen açık kaynak bir protokol. LLM'lerin dış dünya ile standart bir şekilde etkileşime girmesini sağlar.

Protokol, tıpkı HTTP'nin RFC dokümanında tanımlandığı gibi, request/response formatlarını, parametreleri ve kuralları belirler. GitHub'da `modelcontextprotocol/specification` reposundan erişilebilir, versiyonlanır.

### 5.3. Cursor'da MCP Kullanımı

`.cursor/mcp.json` dosyasına MCP server tanımlamaları eklenir. Örnek senaryo:

> "Jira'daki in-progress task'ımı al → Kodu yaz → GitHub'da PR aç → Slack'tan takıma bildirim gönder"

Tek bir prompt ile tüm bu adımlar otomatik gerçekleşir. Cursor sırasıyla Atlassian MCP → GitHub MCP → Slack MCP'yi çağırır.

**Mevcut MCP'ler:** Figma, Jira/Atlassian, GitHub, Slack, Replicate ve daha birçoğu. Şirketler kendi MCP'lerini de Anthropic'in Python/TypeScript SDK'sıyla üretebilir (örn. Türk Hava Yolları kendi MCP'sini yayımladı).

---

## 6. Vibe Coding

Andrej Karpathy'nin (eski Tesla AI Direktörü) Twitter'da popülerleştirdiği terim. Hiç kod yazmadan, sadece doğal dil (natural language) kullanarak kod ürettirme işlemi.

### Prompt Yazma İpuçları

- **Semantic Compression:** En az kelimeyle en çok anlam ifade etme üzerine akademik çalışmalar var.
- **"What do you think?" yazmayın:** Personality vektörü devreye girip sonucu saptırabilir.
- **"Use code" yazın:** Matematiksel işlemlerde LLM'in doğrudan hesaplama yerine Python kodu yazıp çalıştırmasını sağlayın. Böylece halüsinasyon riski sıfırlanır.

---

## 7. Diğer Özellikler

- **Bug Bot:** GitHub entegrasyonu ile PR açıldığında otomatik bug analizi yapıp PR'a yorum olarak ekler.
- **Background Agents:** Uzun süren işlemleri bulutta arka planda çalıştırır, siz diğer işlerinize devam edersiniz.
- **Memory:** Sürekli tekrarladığınız talimatları hafızasında tutar, bir sonraki seferde siz söylemeden uygular.
- **Dosya dönüştürme:** MP4→MP3, PNG→SVG gibi işlemler de yapılabilir.
- **Web scraping:** "google.com'a git, ekran görüntüsü al, src klasörüne kaydet" gibi komutlar verilebilir.

---

## 8. Lokal LLM Kullanımı (Bonus)

Cursor dışında, tamamen ücretsiz ve internet gerektirmeyen bir yöntem de var:

1. **Ollama** ile open source modeli (LLaMA, DeepSeek vb.) lokale indirin.
2. **Continue.dev** eklentisini VS Code'a kurun.
3. Continue'ü Ollama'daki modele bağlayın.

Avantaj: Kod dışarı sızmaz, tamamen lokal çalışır, ücretsizdir. Dezavantaj: Sonnet gibi kapalı kaynak modelleri kullanamazsınız; model kalitesi donanımınıza ve seçtiğiniz modele bağlıdır.

---

## 9. Güncel Kalma Kaynakları

- **arXiv (arxiv.org):** Cornell Üniversitesi'nin akademik makale platformu. CS.AI filtresiyle en güncel yapay zeka makalelerine ulaşılabilir.
- **Hacker News (news.ycombinator.com):** Her gün en popüler 30-60 yazıyı okumak teknoloji gündemini takip etmek için çok değerli.
- **Andrej Karpathy YouTube:** "Intro to Large Language Models" (1 saatlik, avam diliyle LLM anlatımı), "Deep Dive into LLMs", "How I Use LLMs" gibi videolar şiddetle tavsiye edilir.
- **Twitter/X:** Yapay zeka alanında paylaşım yapan hesapları takip etmek.
