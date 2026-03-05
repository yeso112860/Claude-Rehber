# Claude Code CLI - Kapsamli Rehber

## 1. SLASH KOMUTLARI

| Komut | Aciklama |
|-------|----------|
| `/help` | Yardim ve mevcut komutlari goster |
| `/clear` | Konusma gecmisini temizle (takma adlar: `/reset`, `/new`) |
| `/compact [talimat]` | Konusmayi ozetleyerek baglami kucult |
| `/config` | Ayarlar arayuzunu ac (takma ad: `/settings`) |
| `/context` | Gecerli baglam kullanimini gorsellestir |
| `/copy` | Son yaniti panoya kopyala |
| `/cost` | Token kullanim istatistiklerini goster |
| `/desktop` | Oturumu Claude Code Desktop uygulamasinda devam ettir |
| `/diff` | Etkilesimli diff gostericiyi ac (uncommitted degisiklikler) |
| `/doctor` | Claude Code kurulumunu ve ayarlarini tani ve onar |
| `/exit` | CLI'den cik (takma ad: `/quit`) |
| `/export [dosya]` | Konusmayi duz metin olarak disa aktar |
| `/extra-usage` | Rate limit'e ulasildiginda calismayi surdurmek icin ayar yap |
| `/fast [on\|off]` | Fast mode ac/kapat (ayni model, daha hizli cikti) |
| `/feedback` | Claude Code hakkinda geri bildirim gonder (takma ad: `/bug`) |
| `/fork [ad]` | Konusmanin bir kopyasini olustur |
| `/hooks` | Hook yapilandirmasini yonet |
| `/ide` | IDE entegrasyonlarini yonet |
| `/init` | Projeyi `CLAUDE.md` rehberi ile baslat |
| `/keybindings` | Klavye kisayollari yapilandirma dosyasini ac |
| `/login` | Anthropic hesabina giris yap |
| `/logout` | Anthropic hesabindan cik |
| `/mcp` | MCP sunucu baglantilarin yonet |
| `/memory` | `CLAUDE.md` dosyalarini duzenle, auto memory ac/kapat |
| `/model [model]` | AI modelini sec veya degistir |
| `/output-style [stil]` | Cikti stilini degistir (Default, Explanatory, Learning) |
| `/permissions` | Izinleri goruntule veya guncelle (takma ad: `/allowed-tools`) |
| `/plan` | Plan moduna gir |
| `/plugin` | Eklentileri yonet |
| `/pr-comments [PR]` | GitHub PR yorumlarini al ve goster |
| `/release-notes` | Degisim gunlugunu goruntule |
| `/rename [ad]` | Oturumu yeniden adlandir |
| `/resume [oturum]` | Onceki konusmayi devam ettir (takma ad: `/continue`) |
| `/review` | PR'i kod kalitesi, dogruluk, guvenlik icin incele |
| `/rewind` | Konusmayi/kodu onceki noktaya geri sar |
| `/sandbox` | Sandbox modunu ac/kapat |
| `/security-review` | Degisiklikleri guvenlik aciklari icin analiz et |
| `/skills` | Mevcut skill'leri listele |
| `/stats` | Gunluk kullanim ve oturum gecmisini gorsellestir |
| `/status` | Ayarlar arayuzunu ac (Durum sekmesi) |
| `/tasks` | Arka plan gorevlerini listele ve yonet |
| `/terminal-setup` | Terminal keybinding'lerini yapilandir (Shift+Enter vb.) |
| `/theme` | Renk temasini degistir |
| `/usage` | Plan kullanim sinirlarini goster |
| `/vim` | Vim duzenleme modunu ac/kapat |
| `/add-dir <yol>` | Oturuma yeni calisma dizini ekle |
| `/agents` | Subagent yapilandirmasini yonet |

---

## 2. KLAVYE KISAYOLLARI

### Genel

| Kisayol | Aciklama |
|---------|----------|
| `Ctrl+C` | Gecerli girisi veya uretimi iptal et |
| `Ctrl+D` | Oturumdan cik |
| `Ctrl+L` | Terminal ekranini temizle |
| `Ctrl+O` | Ayrintili ciktiyi ac/kapat (arac kullanimi detaylari) |
| `Ctrl+R` | Komut gecmisinde ters arama |
| `Ctrl+B` | Calisan gorevi arka plana al |
| `Ctrl+T` | Gorev listesini ac/kapat |
| `Ctrl+G` | Metin duzenleyicide istegi ac |
| `Ctrl+V` | Panodan goruntu yapistir (macOS) |
| `Shift+Tab` | Izin modlari arasinda gecis yap |
| `Option+P` / `Alt+P` | Modeli degistir |
| `Option+T` / `Alt+T` | Extended thinking ac/kapat |
| `Esc + Esc` | Geri sar veya ozetle |
| `Up/Down` | Komut gecmisinde gezin |

### Cok Satirli Giris

| Yontem | Kisayol |
|--------|---------|
| Hizli kacis | `\` + `Enter` |
| macOS | `Option+Enter` |
| iTerm2/WezTerm/Ghostty/Kitty | `Shift+Enter` |
| Kontrol dizisi | `Ctrl+J` |

### Metin Duzenleme

| Kisayol | Aciklama |
|---------|----------|
| `Ctrl+K` | Satirin sonuna kadar sil |
| `Ctrl+U` | Tum satiri sil |
| `Ctrl+Y` | Silinen metni yapistir |
| `Alt+B` / `Alt+F` | Sozcuk bazinda geri/ileri git |

---

## 3. CLI BAYRAKLARI (Baslatma Secenekleri)

| Bayrak | Aciklama |
|--------|----------|
| `-p, --print <sorgu>` | Sorgula ve cik (headless/SDK modu) |
| `-c, --continue` | Son konusmayi devam ettir |
| `-r, --resume <oturum>` | Belirli oturumu devam ettir |
| `-w, --worktree [ad]` | Izole git worktree baslat |
| `--model <model>` | AI modelini belirt |
| `--permission-mode <mod>` | Izin modunu ayarla |
| `--system-prompt <metin>` | Sistem istemini degistir |
| `--append-system-prompt <metin>` | Sistem istemine ek metin ekle |
| `--output-format <format>` | Cikti formati (text, json, stream-json) |
| `--max-turns <sayi>` | Agentic donus siniri |
| `--max-budget-usd <tutar>` | Harcama siniri |
| `--mcp-config <dosya>` | MCP sunucularini JSON'dan yukle |
| `--tools <araclar>` | Kullanilabilir araclari kisitla |
| `--add-dir <yol>` | Ek calisma dizini ekle |
| `--debug [kategoriler]` | Debug modunu etkinlestir |
| `--verbose` | Ayrintili gunlugu ac |
| `--version, -v` | Surum numarasini goster |
| `--dangerously-skip-permissions` | Tum izin istemlerini atla (dikkat!) |
| `--agent <ad>` | Belirli subagent ile baslat |
| `--chrome` / `--no-chrome` | Chrome entegrasyonunu ac/kapat |

---

## 4. OZELLIKLER OZETI

### 4.1 CLAUDE.md (Proje Rehberi)

Claude'a kalici talimatlar veren dosyalar. Farkli kapsamlarda kullanilir:

| Konum | Kapsam |
|-------|--------|
| `./CLAUDE.md` veya `./.claude/CLAUDE.md` | Proje (takim paylasimi) |
| `~/.claude/CLAUDE.md` | Kullanici (tum projeler) |
| `./CLAUDE.local.md` | Yerel proje (gitignore'da tutulur) |
| `.claude/rules/*.md` | Konuya ozel kurallar |

`/init` ile otomatik olusturulur. `@dosya.md` ile baska dosyalari ice aktarabilir.

### 4.2 MCP (Model Context Protocol)

Dis araclar ve veri kaynaklari ile baglanti saglayan protokol.

```bash
# Sunucu ekle
claude mcp add --transport http <ad> <URL>
claude mcp add --transport stdio <ad> -- <komut>

# Listele / Kaldir
claude mcp list
claude mcp remove <ad>
```

Yapilandirma dosyalari: `~/.claude.json` (kisisel), `.mcp.json` (proje).

### 4.3 Hooks (Kancalar)

Claude Code yasam dongusunde belirli olaylarda shell komutlari calistirir.

**Olay turleri:**
- `SessionStart` - Oturum basinda
- `UserPromptSubmit` - Istem gonderildiginde
- `PreToolUse` - Arac calistirilmadan once
- `PostToolUse` - Arac basarili olduktan sonra
- `Stop` - Claude yanit vermeyi bitirdiginde
- `Notification` - Bildirim gonderildiginde
- `SubagentStart/Stop` - Subagent basinda/sonunda

**Cikis kodlari:** `0` = izin ver, `2` = engelle, diger = devam et.

Ayarlar: `~/.claude/settings.json` icerisindeki `hooks` bolumu.

### 4.4 Subagents (Ozellestirilmis Ajanlar)

Belirli gorevler icin optimize edilmis AI asistanlari.

**Yerlesik subagentlar:**
- **Explore** - Hizli codebase arama (Haiku model)
- **Plan** - Plan modunda arastirma
- **General-purpose** - Karmasik cok adimli gorevler

**Ozel subagent olusturma:** `.claude/agents/` altinda Markdown dosyasi:

```markdown
---
name: code-reviewer
description: Kod kalitesi ve en iyi uygulamalar icin inceleme yapar
tools: Read, Glob, Grep
model: sonnet
---

Kodu kalite, guvenlik ve performans icin incele.
```

### 4.5 Agent Teams (Takim Calismasi) - Deneysel

Birden fazla Claude oturumunun koordineli calismasini saglar.

- **Aktiflestime:** `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`
- **Team Lead**: Ana oturum, gorevleri dagitir ve koordine eder
- **Teammate'ler**: Bagimsiz Claude oturumlari, kendi context window'lari var
- **Paylasilan gorev listesi**: Task'lar `pending → in_progress → completed` akar
- **Mailbox**: Teammate'ler arasi dogrudan mesajlasma
- **Teammate modlari**:
  - `auto` - Arka planda otomatik (varsayilan)
  - `in-process` - Ayni terminalde, Shift+Down ile gecis
  - `tmux` - Ayri tmux panellerinde gorsel takip

**Subagent'dan farki:** Subagent gorev bitince rapor verir ve kapanir.
Agent Teams'de teammate'ler bagimsiz calisir, birbirleriyle mesajlasir,
paylasilan gorev listesi uzerinden koordine olur.

### 4.6 Plan Mode (Plan Modu)

Claude dosyalari okur ve analiz eder ama degistirmez. Guvenli arastirma modu.

- `Shift+Tab` ile gecis yap
- `claude --permission-mode plan` ile baslat
- `/plan` komutu ile gir

### 4.7 Izin Modlari

| Mod | Davranis |
|-----|----------|
| `default` | Her arac kullaniminda izin ister |
| `acceptEdits` | Dosya duzenlemelerini otomatik kabul eder |
| `plan` | Salt oku - degisiklik veya komut yok |
| `dontAsk` | Otomatik reddet (on onayli araclar haric) |
| `bypassPermissions` | Tum izin kontrollerini atla |

Kural sozdizimi: `settings.json` icerisinde `allow`, `ask`, `deny` listeleri.

### 4.8 Auto Memory (Otomatik Bellek)

Claude'un oturumlar arasi ogrendiklerini kaydettigi sistem.

- Konum: `~/.claude/projects/<proje>/memory/`
- `/memory` ile yonet
- `MEMORY.md` her konusmaya yuklenir (max 200 satir)
- Konu dosyalari (`patterns.md`, `debugging.md`) olusturulabilir

### 4.9 Vim Mode

`/vim` komutu ile Vim tarzinda duzenleme modu aktif edilir.
- `Esc` - NORMAL moda gec
- `i/a/o` - INSERT moda gec
- `h/j/k/l` - NORMAL modda navigasyon

### 4.10 IDE Entegrasyonlari

- **VS Code**: Extension marketplace'den "Claude Code" yukle
- **JetBrains**: Plugin olarak yukle
- `/ide` komutu ile durumu kontrol et

### 4.11 Git Entegrasyonu

- Worktree destegi (`-w` bayragi)
- `/diff` ile degisiklikleri gor
- `/review` ile PR incele
- `/pr-comments` ile PR yorumlarini al
- GitHub Actions entegrasyonu

### 4.12 Skills (Beceriler)

Yeniden kullanilabilir ozel komutlar. `.claude/skills/` altinda Markdown dosyasi olarak tanimlanir.

### 4.13 Extended Thinking

Opus 4.6 modeli icin uyarlanabilir derin dusunme ozelligi.
- `Option+T` / `Alt+T` ile ac/kapat
- `/model` ile effort level ayarla (low/medium/high)

---

## 5. ONEMLI DOSYA KONUMLARI

| Amac | Konum |
|------|-------|
| Kullanici ayarlari | `~/.claude/settings.json` |
| Proje ayarlari | `.claude/settings.json` |
| Yerel ayarlar | `.claude/settings.local.json` |
| Klavye kisayollari | `~/.claude/keybindings.json` |
| Skill'ler | `~/.claude/skills/` veya `.claude/skills/` |
| Subagent'lar | `~/.claude/agents/` veya `.claude/agents/` |
| Kurallar | `.claude/rules/` |
| Bellek | `~/.claude/projects/<proje>/memory/` |
| MCP config (kisisel) | `~/.claude.json` |
| MCP config (proje) | `.mcp.json` |

---

## 6. HEADLESS / SDK KULLANIMI

```bash
# Basit sorgu
claude -p "bu fonksiyonu acikla"

# Stdin'den oku
cat code.js | claude -p "bu kodda hata bul"

# JSON cikti
claude -p "JSON formatla" --output-format json

# Donus siniri
claude -p "sorgu" --max-turns 3

# Butce siniri
claude -p "sorgu" --max-budget-usd 5.00
```

---

## 7. HATA AYIKLAMA

```bash
claude --debug "api,mcp"   # Debug modu
claude --verbose            # Ayrintili gunluk
/doctor                     # Kurulum tanilama
/hooks                      # Hook yapilandirmasini kontrol et
/config                     # Ayarlari dogrula
/compact                    # Baglami kucult
```

---

## 8. ORTAM DEGISKENLERI

| Degisken | Aciklama |
|----------|----------|
| `CLAUDE_CODE_DISABLE_AUTO_MEMORY=1` | Auto memory'i devre disi birak |
| `MAX_THINKING_TOKENS=10000` | Extended thinking token siniri |
| `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=50` | Otomatik sikistirma yuzdesi |
| `MCP_TIMEOUT=10000` | MCP API timeout |

---

## 9. PLUGINS (EKLENTILER)

Claude Code eklenti sistemi ile 3. parti ozellikler eklenebilir.

### Plugin Yonetimi

```bash
/plugin                     # Eklenti yonetim arayuzunu ac
/plugin browse               # Mevcut eklentileri kesfet
/plugin install <ad>         # Eklenti yukle
/plugin list                 # Yuklenen eklentileri listele
/plugin remove <ad>          # Eklenti kaldir
```

### Plugin Yapisi

Eklentiler Claude Code'a yeni skill'ler, hook'lar ve MCP sunuculari ekleyebilir.
Bir plugin su yapilari icerebilir:

- **Skills**: Yeni slash komutlari (ornek: `/review-pr`, `/commit`)
- **Hooks**: Otomatik tetiklenen islemler
- **MCP Servers**: Yeni dis arac entegrasyonlari
- **Rules**: Ek kural setleri

### Plugin Dizini

```
~/.claude/plugins/<plugin-adi>/
├── manifest.json          # Plugin tanimlamasi
├── skills/                # Plugin'in skill'leri
├── hooks/                 # Plugin'in hook'lari
└── rules/                 # Plugin'in kurallari
```

### Plugin Yuklemesi

```bash
# Marketplace'den
/plugin browse

# Yerel dizinden
claude --plugin-dir ./my-plugins

# Oturum basinda belirli plugin ile
claude --plugin-dir /Users/ben/custom-plugins
```

---

## 10. MCP (Model Context Protocol) - DETAYLI REHBER

### MCP Nedir?

MCP, Claude Code'un dis araclar ve veri kaynaklariyla iletisim kurmasini saglayan
acik kaynakli protokoldur. Veritabani, GitHub, Notion, dosya sistemi gibi
kaynaklara dogrudan erisim saglar.

### Transport Turleri

| Tur | Aciklama | Kullanim |
|-----|----------|---------|
| `stdio` | Yerel isleme baglantili (stdin/stdout) | Yerel araclar, CLI tabanli sunucular |
| `http` | HTTP uzerinden uzak baglanti | Bulut servisleri, uzak API'ler |

### MCP Sunucu Ekleme Ornekleri

```bash
# === STDIO TRANSPORT (Yerel Islem) ===

# GitHub - Issue, PR, Repository yonetimi
claude mcp add --transport stdio github \
  --env GITHUB_TOKEN=ghp_xxxx \
  -- npx -y @anthropic-ai/mcp-server-github

# PostgreSQL - Veritabani sorgulama
claude mcp add --transport stdio postgres \
  --env DATABASE_URL=postgresql://user:pass@localhost:5432/mydb \
  -- npx -y @anthropic-ai/mcp-server-postgres

# Dosya Sistemi - Belirli dizinlere erisim
claude mcp add --transport stdio filesystem \
  -- npx -y @anthropic-ai/mcp-server-filesystem /path/to/dir1 /path/to/dir2

# Puppeteer - Web sayfasi otomasyonu
claude mcp add --transport stdio puppeteer \
  -- npx -y @anthropic-ai/mcp-server-puppeteer

# SQLite - Yerel veritabani
claude mcp add --transport stdio sqlite \
  -- npx -y @anthropic-ai/mcp-server-sqlite --db-path /path/to/db.sqlite

# Memory - Kalici bilgi deposu
claude mcp add --transport stdio memory \
  -- npx -y @anthropic-ai/mcp-server-memory

# === HTTP TRANSPORT (Uzak Servis) ===

# Notion
claude mcp add --transport http notion https://mcp.notion.com/mcp

# Ozel API (Bearer token ile)
claude mcp add --transport http my-api https://api.example.com/mcp \
  --header "Authorization: Bearer YOUR_TOKEN"
```

### Proje Bazli MCP (.mcp.json)

Dosya: `.mcp.json` (proje koku, git'e eklenebilir)

```json
{
  "mcpServers": {
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "postgres": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "${DATABASE_URL}"
      }
    }
  }
}
```

> **Guvenlk:** Token'lari `${ENV_VAR}` ile referans et, dogrudan yazma.

### MCP Kapsamlari

| Kapsam | Dosya | Paylasim |
|--------|-------|----------|
| Kisisel | `~/.claude.json` | Hayir - sadece sizin icin |
| Proje | `.mcp.json` | Evet - takim ile paylas (git) |

### MCP Yonetim Komutlari

```bash
claude mcp list              # Tum sunuculari listele
claude mcp get <ad>          # Sunucu detaylarini gor
claude mcp remove <ad>       # Sunucuyu kaldir
/mcp                         # Oturum icinde baglanti durumu
```

### MCP Izinleri

```json
{
  "permissions": {
    "allow": [
      "mcp__github__get_issue",
      "mcp__github__list_issues",
      "mcp__postgres__query"
    ],
    "deny": [
      "mcp__github__delete_*",
      "mcp__postgres__execute"
    ]
  }
}
```

---

## 11. REMOTE / TELEPORT / DESKTOP

### Remote Mode

Claude Code oturumunu claude.ai web arayuzunden yonet:

```bash
# Yeni remote oturum olustur
claude --remote "Bu projedeki hatalari bul"

# Mevcut oturumu remote'a ac
/remote-control
```

### Teleport

Web'deki oturumu yerel terminale tasi:

```bash
claude --teleport
```

### Desktop App

Oturumu masaustu uygulamasinda devam ettir:

```
/desktop
```

### Mobil

```
/mobile              # QR kod ile mobil uygulamaya baglan
```

---

## 12. CONTEXT7 ve DIS DOKUMANTASYON

Claude Code, Context7 MCP plugin'i ile guncel kutuphane dokumantasyonlarina
erisebilir. Bu sayede eski bilgiler yerine en guncel API dokumantasyonlarini kullanir.

Kullanim:
```
"React Router v7 dokumantasyonundan nested routes ornegi goster"
"Spring Boot 3.3 @Cacheable orneklerini Context7'den getir"
```

---

## 13. OUTPUT STYLES (CIKTI STILLERI)

```bash
/output-style Default       # Standart davranis
/output-style Explanatory   # Egitimsel aciklamalar
/output-style Learning      # Sizden kod yazmanizi ister, dogrudan yazmaz
```

Ozel stil olusturulabilir. Stiller Claude'un cevap verme bicimini degistirir.

---

## 14. SANDBOX MODE

Desteklenen platformlarda (Linux, macOS sandbox) Claude Code'u izole ortamda calistir:

```
/sandbox                    # Sandbox modunu ac/kapat
```

Sandbox acikken Claude Code dosya sistemi ve ag erisimi sinirlandirilir.

---

## 15. GELISMIS KULLANIM IPUCLARI

### Pipe ile Kullanim

```bash
# Dosya icerigini Claude'a gonder
cat src/UserService.java | claude -p "Bu servisteki hatalari bul"

# Git diff'i analiz ettir
git diff | claude -p "Bu degisiklikleri ozetle ve sorunlari belirt"

# Birden fazla dosya
find src -name "*.java" | head -5 | xargs cat | claude -p "Bu dosyalardaki ortak pattern nedir?"

# Log analizi
tail -100 app.log | claude -p "Bu loglardaki hatalari analiz et"
```

### Zincirleme Kullanim

```bash
# Analiz + Duzeltme
claude -p "Bu dosyadaki hatalari JSON listesi olarak don" --output-format json \
  | claude -p "Bu hatalari duzelt"

# CI/CD icinde
claude -p "Bu PR'daki degisiklikleri incele" --max-turns 3 --max-budget-usd 1.00
```

### Coklu Dizin Calismasi

```bash
# Monorepo'da birden fazla dizinle calis
claude --add-dir ../shared-lib --add-dir ../common-types

# Oturum icinde
/add-dir ../baska-proje
```

### Oturum Yonetimi

```bash
# Oturumu adlandir
/rename auth-refactoring

# Onceki oturumu devam ettir
claude --resume auth-refactoring

# Son oturumu devam ettir
claude -c

# Oturumu catalla (dallandir)
/fork experimental-approach
```

---

*Bu dosya Claude Code CLI'nin temel ozelliklerinin ozet rehberidir.
Detayli ornekler icin ornekler.md dosyasina bakin.*
