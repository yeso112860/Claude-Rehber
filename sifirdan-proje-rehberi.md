# Sifirdan Proje Baslatma Rehberi (Best Practices)

## ADIM 1: Proje Dizinini Olustur ve Claude'u Baslat

```bash
mkdir yeni-proje && cd yeni-proje
git init
claude
```

Ilk is olarak `/init` calistir. Bu otomatik olarak `CLAUDE.md` dosyasi olusturur.

```
/init
```

> **Neden onemli?** CLAUDE.md olmadan Claude her seferinde projeyi sifirdan anlamaya
> calisir. Bu dosya projenin "beyni" gibi calisir.

---

## ADIM 2: CLAUDE.md Dosyasini Yapilandir

`/init` sonrasi olusan CLAUDE.md dosyasini duzenle. Asagidaki yapiya uy:

```markdown
# Proje Adi

Kisa proje aciklamasi (1-2 cumle).

## Teknoloji

- Runtime: Node.js 20 / Python 3.12 / vb.
- Framework: Next.js / FastAPI / vb.
- Veritabani: PostgreSQL / MongoDB / vb.
- Paket yoneticisi: pnpm / bun / pip

## Komutlar

- `pnpm dev` - gelistirme sunucusu
- `pnpm build` - uretim derlemesi
- `pnpm test` - testleri calistir
- `pnpm lint` - lint kontrolu

## Kod Kurallari

- 2 space indentation
- Fonksiyon isimleri camelCase
- Dosya isimleri kebab-case
- Her fonksiyona birim test yaz
- Turkce commit mesajlari

## Dosya Yapisi

- `src/` - kaynak kod
- `src/components/` - UI bilesenleri
- `src/lib/` - yardimci fonksiyonlar
- `tests/` - test dosyalari
```

> **Kural:** CLAUDE.md 200 satiri gecmesin. Kisa, net, spesifik talimatlar verin.
> "Kodu guzel yaz" yerine "2-space indent, camelCase fonksiyonlar" gibi somut kurallar.

---

## ADIM 3: Izin Modunu Ayarla

Projenin basinda guvenli calis, sonra rahatla:

```
/permissions
```

**Onerilen baslangic ayarlari:**

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Bash(npm run *)",
      "Bash(pnpm *)",
      "Bash(git status)",
      "Bash(git log *)",
      "Bash(git diff *)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force *)"
    ]
  }
}
```

> **Best Practice:** Baslangicta `default` modda calis. Projeyi tanidiktan sonra
> `acceptEdits` moduna gec. `bypassPermissions` sadece CI/CD ortamlarinda kullan.

---

## ADIM 4: Proje Iskelesini Claude ile Olustur

Claude'a projeyi tarif et ve iskeleyi olusturmasini iste:

```
Bu proje bir REST API. Node.js + Express + TypeScript + PostgreSQL kullanacagiz.
Proje iskelesini olustur:
- src/routes/ - API endpointleri
- src/models/ - veritabani modelleri
- src/middleware/ - ara katmanlar
- src/utils/ - yardimci fonksiyonlar
- tests/ - test dosyalari
- package.json, tsconfig.json, .env.example dosyalarini olustur
```

> **Dikkat:** Tek seferde cok fazla dosya olusturmak yerine katman katman ilerle.
> Once iskelet, sonra bir endpoint, sonra test, sonra bir sonraki endpoint.

---

## ADIM 5: Git Yapisini Kur

```
/terminal-setup
```

Claude ile commit atarken dikkat edilecekler:

```
# Her anlamli degisiklikte commit at
# Claude'a commit attir:
Bu degisiklikleri commit et

# Veya dogrudan:
git add src/routes/auth.ts src/models/user.ts
git commit -m "feat: kullanici kimlik dogrulama endpointi ekle"
```

> **Best Practice:**
> - Kucuk, anlamli commitler at (tek ozellik = tek commit)
> - `.env`, credentials gibi dosyalari asla commit etme
> - `.gitignore` dosyasini ilk basta dogru ayarla

---

## ADIM 6: Kurallari Konuya Gore Ayir

Proje buyudukce `.claude/rules/` dizinini kullan:

```
.claude/
├── CLAUDE.md              # Genel proje kurallari
├── settings.json          # Izin ve hook ayarlari
└── rules/
    ├── code-style.md      # Kod stili kurallari
    ├── testing.md         # Test yazma kurallari
    ├── api-design.md      # API tasarim kurallari
    └── database.md        # Veritabani kurallari
```

Path-specific kural ornegi (`.claude/rules/api-design.md`):

```markdown
---
paths:
  - "src/routes/**/*.ts"
  - "src/controllers/**/*.ts"
---

# API Tasarim Kurallari

- Her endpoint try-catch ile sarmalanmali
- HTTP durum kodlarini dogru kullan (201 create, 404 not found)
- Response formati: { success: boolean, data?: T, error?: string }
- Rate limiting middleware kullan
```

---

## ADIM 7: Hook'lari Ayarla

Basit ama etkili hook'lar:

```
/hooks
```

Veya `.claude/settings.json` icinde:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write $(jq -r '.tool_input.file_path')"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "if echo $(jq -r '.tool_input.command') | grep -qE 'rm -rf|drop database|truncate'; then echo 'Tehlikeli komut engellendi' >&2; exit 2; fi"
          }
        ]
      }
    ]
  }
}
```

---

## ADIM 8: Subagent Olustur (Opsiyonel)

Tekrar eden gorevler icin ozellestirilmis agent olustur:

```
mkdir -p .claude/agents
```

`.claude/agents/test-writer.md`:

```markdown
---
name: test-writer
description: Yeni yazilan kod icin birim testler olusturur
tools: Read, Glob, Grep, Write
model: sonnet
---

Verilen kod dosyasi icin kapsamli birim testleri yaz.
Test framework olarak projedeki mevcut framework'u kullan.
Edge case'leri ve hata senaryolarini da kapsayacak testler ekle.
```

---

## ADIM 9: Gelistirme Dongusu

Projeyi gelistirirken su donguyu takip et:

```
1. Ozellik tanimla     →  Claude'a ne istedigini acikla
2. Plan modu           →  Shift+Tab ile Plan Mode'a gec, Claude analiz etsin
3. Plani onayla        →  Shift+Tab ile Normal Mode'a don
4. Uygulama           →  Claude kodu yazsin
5. Test                →  "Bu degisiklik icin test yaz"
6. Lint/Format         →  Hook otomatik yapar veya "lint calistir"
7. Review              →  "Yazdigimiz kodu incele, sorun var mi?"
8. Commit              →  "Bu degisikligi commit et"
9. Tekrarla
```

> **Best Practice:**
> - Her ozellik icin once Plan Mode'da analiz yaptir
> - Tek seferde birden fazla ozellik isteme, sirayla ilerle
> - Claude'un yazdigi kodu oku ve anla
> - Karmasik islemlerde `/compact` ile baglami temizle

---

## ADIM 10: Proje Buyudugunde

### Baglam Yonetimi

```
/compact               # Konusmayi sıkıstır
/context               # Baglam kullanimini gor
```

Claude'un baglami dolunca otomatik sikistirma yapar ama buyuk gorevlerde
elle `/compact` calistirmak daha iyi sonuc verir.

### Paralel Calisma (Worktree)

Birden fazla ozellik uzerinde ayni anda calismak icin:

```bash
# Terminal 1: Auth ozelligi
claude -w auth-feature

# Terminal 2: Dashboard ozelligi
claude -w dashboard-feature
```

### Auto Memory

Claude zamanla projeyi ogrenecek. Bunu hizlandirmak icin:

```
Bu projedeki onemli pattern'lari ve convention'lari hatirla
```

Veya dogrudan `/memory` ile bellek dosyalarini duzenle.

---

## KONTROL LISTESI: Proje Baslangici

- [ ] `git init` yapildi
- [ ] `/init` ile CLAUDE.md olusturuldu
- [ ] CLAUDE.md'ye teknoloji, komutlar, kurallar eklendi
- [ ] `.gitignore` ayarlandi (.env, node_modules, vb.)
- [ ] Izin kurallari ayarlandi (`/permissions`)
- [ ] Proje iskelesi olusturuldu
- [ ] Ilk commit atildi
- [ ] Hook'lar ayarlandi (format, lint)
- [ ] Test altyapisi kuruldu
- [ ] Kural dosyalari olusturuldu (ihtiyaca gore)

---

## YAPILMAMASI GEREKENLER

1. **CLAUDE.md'yi cok uzun yapma** - 200 satiri gecme, kisa ve net tut
2. **Tek seferde her seyi isteme** - Adim adim ilerle
3. **Izinleri tamamen kapatma** - `bypassPermissions` yerel gelistirmede bile riskli
4. **`.env` dosyalarini commit etme** - Claude'a bunu hatırlat
5. **Test yazmadan ilerleme** - Her ozellikten sonra test iste
6. **Baglami temizlemeden uzun calisma** - `/compact` kullan
7. **Claude'un ciktisini okumadan onaylama** - Her zaman incele
8. **Cok fazla hook ekleme** - Sadece gercekten gerekli olanlari ekle
9. **Karmasik CLAUDE.md kurallari** - Basit, somut, olculebilir kurallar yaz
10. **Model secimini goz ardi etme** - Basit isler icin `/fast`, karmasik icin Opus
