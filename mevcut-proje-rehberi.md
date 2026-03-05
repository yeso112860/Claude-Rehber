# Mevcut Projede Claude Code Kullanimi (Best Practices)

## ADIM 1: Projeyi Tanit

Mevcut proje dizinine gir ve Claude'u baslat:

```bash
cd mevcut-proje
claude
```

Ilk yapilacak is Claude'un projeyi anlamasini saglamak:

```
Bu projeyi analiz et. Dosya yapisini, kullanilan teknolojileri,
mevcut pattern'lari ve convention'lari ozetle.
```

> **Neden?** Claude projeyi tanimadan yapacagi degisiklikler
> mevcut pattern'larla uyumsuz olabilir.

---

## ADIM 2: CLAUDE.md Olustur veya Guncelle

Eger yoksa:

```
/init
```

Eger varsa mevcut CLAUDE.md'yi incele ve guncelle:

```
/memory
```

**Mevcut projeye ozel CLAUDE.md icerigi:**

```markdown
# Proje Adi

Mevcut bir e-ticaret uygulamasi. 2 yildir gelistiriliyor.

## Teknoloji

- Frontend: React 18 + TypeScript + Tailwind CSS
- Backend: Node.js + Express + Prisma ORM
- DB: PostgreSQL
- Test: Jest + React Testing Library
- CI: GitHub Actions

## Komutlar

- `pnpm dev` - gelistirme (frontend + backend ayni anda)
- `pnpm build` - uretim derlemesi
- `pnpm test` - tum testler
- `pnpm test:unit` - birim testler
- `pnpm test:e2e` - uctan uca testler
- `pnpm lint` - ESLint + Prettier
- `pnpm db:migrate` - veritabani migration
- `pnpm db:seed` - test verileri yukle

## Mevcut Pattern'lar (BUNLARA UY)

- API route'lari: src/routes/<kaynak>.routes.ts
- Controller'lar: src/controllers/<kaynak>.controller.ts
- Service'ler: src/services/<kaynak>.service.ts
- Prisma modelleri: prisma/schema.prisma
- React component'ler: src/components/<KomponentAdi>/index.tsx
- Hook'lar: src/hooks/use<HookAdi>.ts
- Test dosyalari: ayni dizinde <dosya>.test.ts

## Onemli Notlar

- Legacy kod var: src/legacy/ dizinine dokunma
- Env degiskenleri: .env.example dosyasini referans al
- PR acmadan once `pnpm lint && pnpm test` calistir
```

> **Best Practice:** Mevcut projelerde en onemli sey mevcut pattern'lari
> belgelemek. Claude yeni kod yazarken bunlara uyacaktir.

---

## ADIM 3: Projeyi Plan Modunda Kesfet

Degisiklik yapmadan once Plan Mode'da kesfet:

```
Shift+Tab  (Plan Mode'a gec)
```

```
src/services/ dizinindeki mevcut servislerin yapisini analiz et.
Ortak pattern'lari ve convention'lari bul.
```

```
Bu projedeki hata yonetimi nasil yapilmis? Ortak bir pattern var mi?
```

```
Mevcut testlerin yapisini incele. Hangi test framework'u kullaniliyor,
mock'lar nasil yapiliyor, test dosyalari nerede?
```

> **Neden Plan Mode?** Mevcut projede once oku, anla, sonra degistir.
> Plan Mode'da Claude hicbir dosyayi degistirmez.

---

## ADIM 4: Mevcut Kodu Anla

Claude'a spesifik sorular sor:

```
# Mimariyi anla
Bu projenin mimarisini acikla. Katmanlar nasil birbirleriyle iletisim kuruyor?

# Belirli bir ozelligi anla
Kullanici kimlik dogrulama akisi nasil calisiyor? Basindan sonuna takip et.

# Bagimliliklari anla
Bu fonksiyon baska nerelerde kullaniliyor? Degistirirsem ne etkilenir?

# Sorunlu alanlari bul
Bu dosyadaki potansiyel hatalari ve iyilestirme alanlarini bul.
```

---

## ADIM 5: Degisiklik Yapmadan Once

### 5a. Mevcut Testleri Calistir

```
Mevcut testleri calistir ve sonuclari raporla
```

Boylece baseline'iniz olur. Degisiklikten sonra kirilanlari gorursunuz.

### 5b. Git Durumunu Kontrol Et

```
git status ve son commitleri goster
```

### 5c. Branch Olustur

```
Bu ozellik icin yeni branch olustur: feature/yeni-ozellik
```

> **Best Practice:** Ana branch'te dogrudan calisma.
> Her ozellik/duzeltme icin ayri branch ac.

---

## ADIM 6: Degisiklik Stratejileri

### Bug Fix (Hata Duzeltme)

```
1. Hatanin ne oldugunu tanimla
2. "Bu hatayi incele ve kok nedenini bul" (Plan Mode)
3. "Duzeltmeyi uygula" (Normal Mode)
4. "Bu duzeltme icin test yaz"
5. "Mevcut testleri calistir, bir sey kirildi mi?"
6. Commit at
```

### Yeni Ozellik Ekleme

```
1. Ozelligi tanimla
2. Plan Mode'da analiz yaptir
3. "Bu ozelligi mevcut pattern'lara uygun sekilde implement et"
4. Kucuk parcalar halinde ilerle
5. Her parcadan sonra test
6. Review yaptir: "Yazdigimiz kodu incele"
7. Commit at
```

### Refactoring

```
1. "Bu modulu analiz et, iyilestirme alanlari neler?" (Plan Mode)
2. Oncelik belirle
3. Her refactoring adimini ayri commit olarak at
4. Her adimdan sonra testleri calistir
5. Davranisi degistirme, sadece yapiyi iyilestir
```

### Guvenlik Incelemesi

```
/security-review
```

Veya spesifik:

```
Bu projedeki guvenlik acikliklari icin tarama yap.
SQL injection, XSS, CSRF, hassas veri ifsa risklerini kontrol et.
```

---

## ADIM 7: Kod Review

### Kendi Kodunu Review Et

```
Son yaptigimiz degisiklikleri incele. Sorunlar:
- Mevcut pattern'lara uyuyor mu?
- Edge case'ler kapsamis mi?
- Performans sorunu var mi?
- Guvenlik acigi var mi?
```

### PR Review

```
/review
```

Veya spesifik PR icin:

```
/pr-comments 42
```

---

## ADIM 8: Buyuk Projelerde Baglam Yonetimi

### Odakli Calisma

Buyuk projelerde Claude'a tum projeyi degil, calistiginiz bolumu odakla:

```
Sadece src/services/payment/ dizinindeki kodu incele.
Baska dosyalara bakmana gerek yok su an.
```

### Baglami Temizle

Uzun calisma oturumlarinda:

```
/compact Su an odek konumuz payment servisi refactoring'i
```

### Ek Dizinler

Monorepo veya multi-dizin projelerde:

```
/add-dir ../shared-lib
```

### Subagent ile Paralel Arastirma

```
Bu projedeki tum API endpoint'lerini bul ve listele.
Ayni zamanda her birinin test coverage'ini kontrol et.
```

Claude bunu subagent'lara delege ederek paralel yapar.

---

## ADIM 9: CI/CD Entegrasyonu

### GitHub Actions ile Claude

Mevcut CI pipeline'ina Claude review ekle:

```yaml
# .github/workflows/claude-review.yml
name: Claude Code Review
on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: anthropics/claude-code-action@v1
        with:
          prompt: |
            Bu PR'i incele:
            - Kod kalitesi
            - Mevcut pattern'lara uyum
            - Test coverage
            - Guvenlik riskleri
```

---

## ADIM 10: Takim Calismasi

### Paylasilan Ayarlar (Version Control'e Ekle)

```
.claude/
├── CLAUDE.md           # Takim kurallari (git'e ekle)
├── settings.json       # Paylasilan izinler (git'e ekle)
├── agents/             # Paylasilan subagent'lar (git'e ekle)
├── skills/             # Paylasilan skill'ler (git'e ekle)
└── rules/              # Paylasilan kurallar (git'e ekle)

# GITIGNORE'A EKLE:
.claude/settings.local.json
CLAUDE.local.md
```

### Kisisel Ayarlar (Version Control Disinda)

```
CLAUDE.local.md         # Kisisel proje notlari
.claude/settings.local.json  # Kisisel izin ayarlari
~/.claude/CLAUDE.md     # Global kisisel tercihler
```

---

## KONTROL LISTESI: Mevcut Projeye Baslarken

- [ ] Projeyi Claude ile analiz ettim
- [ ] CLAUDE.md'ye mevcut pattern'lari belgeledim
- [ ] Mevcut testleri calistirip baseline aldim
- [ ] Plan Mode'da projeyi kesfettim
- [ ] Degisiklik icin branch actim
- [ ] Izin kurallarini ayarladim
- [ ] Hook'lari proje ihtiyacina gore yapilandirdim (lint, format)
- [ ] `.claude/` dizinini `.gitignore` ile ayarladim

---

## SIK YAPILAN HATALAR

### 1. Mevcut Pattern'lari Goz Ardi Etmek

```
# YANLIS:
"User servisi olustur"

# DOGRU:
"src/services/ dizinindeki mevcut servislerin yapisina uygun
 sekilde UserService olustur. Ayni error handling ve logging
 pattern'ini kullan."
```

### 2. Cok Genis Kapsamli Istekler

```
# YANLIS:
"Tum projeyi refactor et"

# DOGRU:
"src/services/payment.service.ts dosyasindaki processPayment
 fonksiyonunu kucuk, test edilebilir fonksiyonlara ayir"
```

### 3. Test Etmeden Ilerlemek

```
# YANLIS:
Ozellik yaz → Commit → Sonraki ozellik

# DOGRU:
Ozellik yaz → Test yaz → Testleri calistir → Review → Commit
```

### 4. Branch Kullanmamak

```
# YANLIS:
main branch'te dogrudan degisiklik yap

# DOGRU:
git checkout -b feature/ozellik-adi
# calis, test et, commit et
# PR ac, review et, merge et
```

### 5. Baglami Yonetmemek

```
# YANLIS:
Saatlerce ayni oturumda calis, baglam dolsun

# DOGRU:
Her buyuk gorev sonunda /compact calistir
Farkli gorevler icin farkli oturumlar veya worktree kullan
```

---

## HIZLI REFERANS: Gunluk Komutlar

```bash
# Sabah ise baslarken
claude -c                    # Son oturumu devam ettir

# Yeni gorev baslatirken
claude -w feature-adi        # Worktree ile izole calis

# Kod yazarken
Shift+Tab                    # Plan/Normal mod gecisi
/compact                     # Baglami temizle
Option+T                     # Extended thinking ac/kapat

# Review ve commit
/review                      # Degisiklikleri incele
/diff                        # Diff gor
"commit et"                  # Claude commit atsin

# Sorun cozme
/doctor                      # Kurulum sorunlari
claude --debug "api"         # API sorunlari
/hooks                       # Hook sorunlari
```
