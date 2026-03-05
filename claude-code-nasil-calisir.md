# Claude Code Nasil Calisir?

Claude Code, terminalinizde calisan bir AI asistanidir.
Kodunuzu okur, yazar, calistirir ve yonetir. Bu dosya teknik olarak
arka planda neler oldugunu aciklar.

---

## 1. TEMEL CALISMA PRENSIBI

```
┌─────────────────────────────────────────────────────────┐
│                    SIZ (Terminal)                        │
│                                                         │
│  "UserService'e findByEmail metodu ekle"                │
│                                                         │
└──────────────────────┬──────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────┐
│                  CLAUDE CODE CLI                        │
│                                                         │
│  1. Mesajinizi alir                                     │
│  2. Baglami toplar (CLAUDE.md, rules, memory, dosyalar) │
│  3. Anthropic API'ye gonderir                           │
│  4. Model "tool use" karari verir                       │
│  5. Araclari calistirir (Read, Edit, Bash, vb.)        │
│  6. Sonucu modele geri gonderir                        │
│  7. Model bitti mi karar verir (bitti → cevap, devam → 4) │
│  8. Size sonucu gosterir                               │
│                                                         │
└──────────────────────┬──────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────┐
│                ANTHROPIC API (Claude Model)             │
│                                                         │
│  - Sisteme mesajiniz + baglam gider                    │
│  - Model dusunur (extended thinking)                   │
│  - "Read dosya X" veya "Edit dosya Y" gibi             │
│    tool call karari verir                              │
│  - Sonuc gelince tekrar dusunur                        │
│  - Bitti → text cevap doner                            │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

Ozet: Siz yazarsiniz → Claude dusunur → arac calistirir → sonuca bakar →
gerekirse tekrar arac calistirir → tamamlaninca size cevap verir.

Bu donguse **agentic loop** denir. Claude tek seferde cevap vermez,
adim adim araclar kullanarak gorevi tamamlar.

---

## 2. AGENTIC LOOP (AJAN DONGUSU)

Claude Code'un temel farki budur. Klasik chatbot'lar tek mesaj alip
tek cevap verir. Claude Code ise:

```
Siz: "Auth modulu icin login endpoint'i yaz"

Claude (ic dusunce):
  1. Oncelikle projeyi anlamaliyim → Read CLAUDE.md
  2. Mevcut pattern'lari gorelim → Glob **/controller/**
  3. Bir ornek controller okuyayim → Read UserController.java
  4. Auth icin SecurityConfig var mi? → Grep "SecurityConfig"
  5. SecurityConfig'i okuyayim → Read SecurityConfig.java
  6. Simdi yazabilirim → Write AuthController.java
  7. Service da lazim → Write AuthService.java
  8. Test de yazayim → Write AuthServiceTest.java
  9. Derlensin mi kontrol edeyim → Bash ./gradlew compileJava
  10. Hata var, duzelteyim → Edit AuthService.java
  11. Tekrar deneyelim → Bash ./gradlew compileJava
  12. Basarili. Tamamlandi.

Claude: "Auth modulu olusturdum. AuthController, AuthService ve
         AuthServiceTest dosyalarini ekledim. ..."
```

Bu 12 adimin her biri bir **turn** (donus). Her turn'de:
1. Model dusunur
2. Bir veya birden fazla arac cagirmaya karar verir
3. Araclar calisir, sonuc modele doner
4. Model sonuca bakar, bir sonraki adima karar verir

### Turn Sayisi

- Varsayilan: sinir yok (gorev bitene kadar)
- `--max-turns 5` ile sinirlandirilabilir (headless modda)
- Her turn API cagrisi = token tuketimi

---

## 3. SYSTEM PROMPT (SISTEM ISTEMI)

Claude Code basladiginda modele gonderilen ilk mesaj sudur:

```
┌─────────────────────────────────────────────────┐
│              SYSTEM PROMPT                       │
│                                                  │
│  Claude Code varsayilan talimatlar               │
│  + CLAUDE.md (proje koku)                        │
│  + ~/.claude/CLAUDE.md (kullanici)               │
│  + .claude/rules/*.md (kural dosyalari)          │
│  + MEMORY.md (auto memory)                       │
│  + MCP sunucu talimatlari                        │
│  + Plugin talimatlari                            │
│  + Aktif skill talimatlari                       │
│                                                  │
│  Toplam: ~10-50K token (projeye gore degisir)    │
└─────────────────────────────────────────────────┘
```

Bu yuzden CLAUDE.md'yi kisa tutmak onemli - her mesajda
context'e yukleniyor ve token tuketiyor.

---

## 4. CONTEXT WINDOW (BAGLAM PENCERESI)

Claude modeli sabit boyutlu bir baglam penceresine sahiptir:

```
┌────────────────────────────────────────────────────────────────┐
│                    CONTEXT WINDOW (~200K token)                 │
│                                                                │
│  ┌──────────────────────┐                                      │
│  │ System Prompt        │ ~10-50K (CLAUDE.md, rules, memory)   │
│  │ (her zaman yuklu)    │                                      │
│  └──────────────────────┘                                      │
│                                                                │
│  ┌──────────────────────┐                                      │
│  │ Konusma Gecmisi      │ Buyuyor...                           │
│  │ (mesajlar + arac     │                                      │
│  │  sonuclari)          │                                      │
│  └──────────────────────┘                                      │
│                                                                │
│  ┌──────────────────────┐                                      │
│  │ Bos Alan             │ Azaliyor...                          │
│  │ (yeni mesajlar icin) │                                      │
│  └──────────────────────┘                                      │
│                                                                │
│  Doluluk %80'e ulasinca → OTOMATIK COMPACT (sikistirma)        │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

### Context Dolunca Ne Olur?

1. Claude Code otomatik olarak `/compact` calistirir
2. Onceki mesajlar ozetlenir (kisa bir ozet + onemli bilgiler)
3. Ozet yeni context'e eklenir, eski mesajlar silinir
4. Calisma devam eder

### /compact Nasil Calisir?

```
ONCESI (150K token):
  System prompt (20K)
  + 50 mesaj konusma (100K)
  + Bos alan (30K)

/compact SONRASI (~50K token):
  System prompt (20K)
  + Ozet mesaj (5K): "Su ana kadar sunlari yaptik: ..."
  + Bos alan (125K)  ← cok daha fazla yer
```

Bu yuzden uzun gorevlerde periyodik olarak `/compact` calistirmak
veya progress.md dosyasi tutmak onemli. Compact sonrasi
progress.md okuyarak kaldigi yerden devam edebilir.

---

## 5. TOOL USE (ARAC KULLANIMI)

Claude Code'un erisebilecegi araclar:

### Dosya Araclari

| Arac | Ne Yapar | Ornek |
|------|----------|-------|
| **Read** | Dosya oku | `Read("src/App.tsx")` |
| **Write** | Yeni dosya olustur | `Write("src/new.tsx", icerik)` |
| **Edit** | Var olan dosyayi duzenle | `Edit("src/App.tsx", eski → yeni)` |
| **Glob** | Dosya ara (pattern) | `Glob("**/*.java")` |
| **Grep** | Dosya icinde ara (regex) | `Grep("class UserService")` |

### Sistem Araclari

| Arac | Ne Yapar | Ornek |
|------|----------|-------|
| **Bash** | Terminal komutu calistir | `Bash("./gradlew test")` |
| **WebFetch** | Web sayfasi oku | `WebFetch("https://docs.spring.io/...")` |
| **WebSearch** | Web arama yap | `WebSearch("spring boot 3 jwt")` |

### Ozel Araclar

| Arac | Ne Yapar |
|------|----------|
| **Agent** | Alt ajan (subagent) calistir |
| **AskUserQuestion** | Size soru sor |
| **EnterPlanMode** | Plan moduna gec |
| **TaskCreate/Update** | Gorev listesi yonet |
| **MCP araclari** | Dis sistem araclari (mcp__github__*, mcp__postgres__*) |

### Arac Cagri Akisi

```
Model: "Read dosyasini cagirmak istiyorum"
        ↓
Claude Code: Izin kontrolu yapar
        ↓
  ┌─────────┐     ┌──────────┐     ┌─────────────┐
  │ allow?  │─Evet─▶ Calistir │────▶│ Sonuc modele │
  │         │     └──────────┘     │ geri doner   │
  │         │                      └─────────────┘
  │         │
  │         │─Hayir─▶ Size sorar: "Bu dosyayi okuyabilir miyim?"
  │         │              │
  │         │         Evet/Hayir
  └─────────┘
```

---

## 6. PERMISSION SYSTEM (IZIN SISTEMI)

Her arac cagrisi izin kontrolunden gecer:

```
┌─────────────────────────────────────────────────────┐
│                 IZIN KONTROL AKISI                    │
│                                                      │
│  Claude "Bash(./gradlew test)" cagirmak istiyor     │
│                                                      │
│  1. settings.json → "deny" listesi kontrol           │
│     → Eslesme var mi? EVET → ENGELLE                 │
│     → Eslesme yok → devam                            │
│                                                      │
│  2. settings.json → "allow" listesi kontrol          │
│     → Eslesme var mi? EVET → IZIN VER (sessizce)     │
│     → Eslesme yok → devam                            │
│                                                      │
│  3. Permission mode kontrol                          │
│     → "bypassPermissions" → IZIN VER                 │
│     → "acceptEdits" + Edit araci → IZIN VER          │
│     → "plan" + Edit/Write/Bash → ENGELLE             │
│     → "default" → KULLANICIYA SOR                    │
│                                                      │
│  4. Kullaniciya sorar:                               │
│     "Allow Bash(./gradlew test)?"                    │
│     [Yes] [Yes, always] [No]                         │
│                                                      │
│     "Yes, always" → allow listesine eklenir          │
│                                                      │
└─────────────────────────────────────────────────────┘
```

### Shift+Tab Gecisi

```
  ┌──────────┐        ┌──────────┐        ┌──────────┐
  │  Normal  │───────▶│   Plan   │───────▶│  Auto-   │
  │  Mode    │        │  Mode    │        │  Accept  │
  │          │◀───────│          │◀───────│          │
  └──────────┘        └──────────┘        └──────────┘
      Shift+Tab          Shift+Tab           Shift+Tab

  Normal: Her araca izin sorar
  Plan: Sadece okuma araclari, degisiklik yok
  Auto-Accept: Dosya duzenleme otomatik onay
```

---

## 7. HOOKS SISTEMI

Hook'lar Claude Code'un yasam dongusunde belirli noktalarda
otomatik calisan shell komutlaridir:

```
┌─────────────────────────────────────────────────────────────┐
│                    HOOK YASAM DONGUSU                        │
│                                                             │
│  ╔═══════════════╗                                          │
│  ║ SessionStart  ║──▶ Oturum basladiginda                   │
│  ╚═══════════════╝    (ortam kontrol, progress.md oku)      │
│         │                                                    │
│         ▼                                                    │
│  ╔═══════════════════╗                                      │
│  ║ UserPromptSubmit  ║──▶ Siz mesaj gonderdiginizde         │
│  ╚═══════════════════╝    (istem dogrulama, zenginlestirme) │
│         │                                                    │
│         ▼                                                    │
│  ╔═══════════════╗                                          │
│  ║ PreToolUse    ║──▶ Arac calistirilmadan ONCE             │
│  ╚═══════════════╝    (tehlikeli komut engelle)             │
│         │                                                    │
│         ├── exit 0 → IZIN VER                               │
│         ├── exit 2 → ENGELLE (stderr = hata mesaji)         │
│         ├── diger  → IZIN VER (stderr loglanir)             │
│         │                                                    │
│         ▼                                                    │
│  ╔═══════════════╗                                          │
│  ║  ARAC CALISIR ║                                          │
│  ╚═══════════════╝                                          │
│         │                                                    │
│         ▼                                                    │
│  ╔═══════════════╗                                          │
│  ║ PostToolUse   ║──▶ Arac basariyla calistiktan SONRA      │
│  ╚═══════════════╝    (prettier format, lint calistir)      │
│         │                                                    │
│         ▼                                                    │
│  ╔═══════════════╗                                          │
│  ║     Stop      ║──▶ Claude cevap vermeyi bitirdiginde     │
│  ╚═══════════════╝    (dogrulama, ozet)                     │
│         │                                                    │
│         ▼                                                    │
│  ╔═══════════════╗                                          │
│  ║  Notification ║──▶ Bildirim gonderildiginde              │
│  ╚═══════════════╝    (masaustu uyari)                      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Hook Veri Akisi

Hook'lar stdin'den JSON alir, stdout/stderr'e yazar:

```
Claude "Edit src/App.tsx" yapmak istiyor
        │
        ▼
PreToolUse hook calisir:
  STDIN (JSON):
  {
    "tool_name": "Edit",
    "tool_input": {
      "file_path": "src/App.tsx",
      "old_string": "...",
      "new_string": "..."
    }
  }

  Hook script:
    FILE=$(jq -r '.tool_input.file_path')
    echo "$FILE isleniyor..."    ← stdout: modele geri bildirim
    # exit 0 → izin ver
    # exit 2 → engelle

        │
        ▼ (exit 0 ile izin verildi)

Edit araci calisir
        │
        ▼

PostToolUse hook calisir:
  STDIN (JSON):
  {
    "tool_name": "Edit",
    "tool_input": { "file_path": "src/App.tsx", ... },
    "tool_output": "Dosya basariyla duzenlendi"
  }

  Hook script:
    FILE=$(jq -r '.tool_input.file_path')
    npx prettier --write "$FILE"    ← otomatik format
```

---

## 8. SUBAGENT SISTEMI

Claude Code ana islem icinden alt islemler (subagent) calistirabilir:

```
┌───────────────────────────────────────────────────────────┐
│                    ANA CLAUDE OTURUMU                      │
│                                                           │
│  Siz: "Tum test dosyalarini incele ve eksikleri raporla"  │
│                                                           │
│  Claude (ana):                                            │
│    "Bu is icin paralel subagent'lar kullanayim"            │
│                                                           │
│    ┌─────────────────┐  ┌─────────────────┐               │
│    │ Subagent 1      │  │ Subagent 2      │               │
│    │ (Explore tipi)  │  │ (Explore tipi)  │               │
│    │                 │  │                 │               │
│    │ backend/        │  │ frontend/       │               │
│    │ testlerini      │  │ testlerini      │               │
│    │ incele          │  │ incele          │               │
│    │                 │  │                 │               │
│    │ Model: haiku    │  │ Model: haiku    │               │
│    │ Araclar: Read,  │  │ Araclar: Read,  │               │
│    │ Glob, Grep      │  │ Glob, Grep      │               │
│    └────────┬────────┘  └────────┬────────┘               │
│             │                    │                         │
│             ▼                    ▼                         │
│    ┌─────────────────────────────────────┐                │
│    │ Sonuclar ana Claude'a doner         │                │
│    │ (her subagent tek mesaj doner)      │                │
│    └─────────────────────────────────────┘                │
│                                                           │
│  Claude (ana):                                            │
│    Sonuclari birlestirir ve size rapor sunar              │
│                                                           │
└───────────────────────────────────────────────────────────┘
```

### Subagent Turleri

```
┌──────────────────────────────────────────────────────────┐
│                                                          │
│  Explore (Haiku)          General-purpose                │
│  ├── Hizli, ucuz          ├── Tum araclar                │
│  ├── Sadece okuma         ├── Okuma + yazma              │
│  ├── Glob, Grep, Read     ├── Karmasik gorevler          │
│  └── Codebase arama       └── Cok adimli islemler        │
│                                                          │
│  Plan                     Ozel Agent (.claude/agents/)   │
│  ├── Plan modunda          ├── Markdown ile tanimlanir    │
│  ├── Sadece okuma          ├── Ozel system prompt         │
│  └── Arastirma            ├── Kisitli araclar            │
│                            ├── Belirli model              │
│                            └── Worktree izolasyonu        │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

### Neden Subagent?

1. **Paralel calisma**: 3 farkli dizini ayni anda ara
2. **Context koruma**: Buyuk sonuclar ana context'i doldurmaz
3. **Uzmanlik**: Her agent kendi konusunda optimize
4. **Izolasyon**: Worktree ile guvenli degisiklik

---

## 9. AGENT TEAMS (TAKIM CALISMASI)

Agent Teams, Subagent'lardan farkli olarak **birden fazla bagimsiz Claude
orneginin ortak bir gorev listesi uzerinden koordineli calismasini** saglar.
Deneysel bir ozelliktir.

```
SUBAGENT vs AGENT TEAMS KARSILASTIRMASI
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  SUBAGENT (Bolum 8)              AGENT TEAMS (Bu bolum)
  ─────────────────               ──────────────────────
  Ana agent tetikler              Team Lead koordine eder
  Rapor ana agent'a doner         Teammate'ler bagimsiz calisir
  Tek yonlu iletisim              Cift yonlu mesajlasma (mailbox)
  Ana context icinde              Her biri ayri context window
  Gorev bitince kapanir           Gorev listesi uzerinden surekli
  "Git su dosyayi ara"            "Sen backend yap, ben frontend"
  ─────────────────               ──────────────────────
```

### Nasil Aktiflestirilir?

```bash
# Environment variable ile
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
claude

# Veya dogrudan
CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1 claude
```

### Mimari Yapi

```
┌─────────────────────────────────────────────────────────────┐
│                     AGENT TEAMS                              │
│                                                              │
│  ┌──────────────────────┐                                    │
│  │     TEAM LEAD         │ ← Siz bununla konusursunuz       │
│  │  (Ana Claude oturumu) │                                    │
│  │                        │                                    │
│  │  Gorevleri:            │                                    │
│  │  ├── Teammate olustur  │                                    │
│  │  ├── Task ata          │                                    │
│  │  ├── Koordine et       │                                    │
│  │  └── Sonuclari topla   │                                    │
│  └──────────┬─────────────┘                                    │
│             │                                                  │
│    ┌────────┼────────┐                                        │
│    │        │        │                                        │
│    ▼        ▼        ▼                                        │
│  ┌──────┐ ┌──────┐ ┌──────┐                                  │
│  │ T1   │ │ T2   │ │ T3   │  ← Teammate'ler                  │
│  │      │ │      │ │      │                                    │
│  │Backend│ │Front │ │Test  │  Her biri bagimsiz               │
│  │ kodu │ │ end  │ │yazici│  Claude oturumu                   │
│  └──┬───┘ └──┬───┘ └──┬───┘                                  │
│     │        │        │                                        │
│     ▼        ▼        ▼                                        │
│  ┌──────────────────────────────────────┐                      │
│  │        PAYLASILAN GOREV LISTESI       │                      │
│  │                                        │                      │
│  │  Task 1: ✅ Entity olustur     [T1]   │                      │
│  │  Task 2: 🔄 Service yaz        [T1]   │                      │
│  │  Task 3: ⏳ Component olustur  [T2]   │                      │
│  │  Task 4: ⏳ Test yaz   [T3, T2 bekle] │                      │
│  │                                        │                      │
│  └──────────────────────────────────────┘                      │
│                                                                │
│  ┌──────────────────────────────────────┐                      │
│  │        MAILBOX (MESAJ KUTUSU)         │                      │
│  │                                        │                      │
│  │  T1 → T2: "Entity'ler hazir,          │                      │
│  │            DTO'lari kullanabilirsin"   │                      │
│  │  Lead → T3: "Oncelik backend test"    │                      │
│  │                                        │                      │
│  └──────────────────────────────────────┘                      │
│                                                                │
└─────────────────────────────────────────────────────────────┘
```

### Teammate Modlari

```
┌──────────────────────────────────────────────────────────────┐
│                    TEAMMATE MODLARI                            │
│                                                                │
│  1. AUTO (Varsayilan)                                          │
│     ├── Team lead otomatik teammate olusturur                  │
│     ├── Gorev atama lead'in kararina bagli                    │
│     └── Arka planda calisir                                    │
│                                                                │
│  2. IN-PROCESS                                                 │
│     ├── Ayni terminal penceresinde                             │
│     ├── Shift+Down ile teammate'ler arasi gecis               │
│     ├── Her teammate'in ciktisini gorebilirsin                │
│     └── Oturum resume destegi YOK                             │
│                                                                │
│  3. TMUX                                                       │
│     ├── tmux split pane'lerde                                  │
│     ├── Her teammate ayri panelde gorunur                     │
│     ├── Gorsel takip kolayligi                                │
│     └── tmux kurulu olmali                                    │
│                                                                │
└──────────────────────────────────────────────────────────────┘
```

### Gorev Yasam Dongusu

```
  pending ──────► in_progress ──────► completed
     │                │
     │                │ (hata/engel)
     │                ▼
     │           Yeni task olustur
     │           (engelleyici sorun icin)
     │
     └── blockedBy: [task_id]
         Bagimliliklari cozulunce
         otomatik acilir
```

**Task bagimliliklari:**
- `blockedBy`: Bu task baslamadan once tamamlanmasi gereken task'lar
- `blocks`: Bu task tamamlaninca acilacak task'lar
- Bir task tamamlaninca, onu bekleyen task'lar otomatik olarak `pending` olur

### Klavye Kisayollari

| Kisayol | Islem |
|---------|-------|
| `Shift+Down` | Teammate'ler arasi gecis (in-process modda) |
| `Ctrl+T` | Gorev listesini goster |
| `Enter` | Secili teammate oturumunu gor |
| `Escape` | Teammate'i durdur |

### Pratik Ornek: Paralel Feature Gelistirme

```
Team Lead'e soylediginiz:
  "User modulu icin backend ve frontend'i paralel gelistir"

Team Lead'in yaptigi:

  1. Teammate-Backend olusturur
     → Task: "UserEntity, UserSearchService, UserOperationalService,
              UserSearchController, UserOperationalController yaz"

  2. Teammate-Frontend olusturur
     → Task: "userApi.ts (RTK Query), UserPage.tsx, UserForm.tsx yaz"
     → blockedBy: Backend DTO'lari (common modulunde)

  3. Teammate-Test olusturur
     → Task: "Backend + frontend testleri yaz"
     → blockedBy: Backend task + Frontend task

  Akis:
  ┌──────────┐
  │ Backend  │──► DTO'lar hazir ──► Frontend baslar
  │ (T1)     │                       (T2)
  └──────────┘                       │
       │                             │
       └──────── Her ikisi de ───────┘
                 tamamlaninca
                     │
                     ▼
              ┌──────────┐
              │  Test    │
              │  (T3)   │
              └──────────┘
```

### Maliyet ve Sinirlamalar

- **Maliyet**: Takim boyutuyla ~dogrusal artar (her teammate ayri context)
- **Deneysel**: `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` gerekli
- **In-process**: Session resume destegi yok
- **Ic ice takim yok**: Teammate baska teammate olusturamaz
- **Ayni dosya riski**: Iki teammate ayni dosyayi duzenlerse catisma olabilir

---

## 10. MCP CALISMA MEKANIZMASI

```
┌────────────────────────────────────────────────────────────┐
│                    CLAUDE CODE                              │
│                                                            │
│  "users tablosundaki son 10 kaydi goster"                  │
│                                                            │
│  Model karar verir:                                        │
│    → mcp__postgres__query aracini cagir                    │
│                                                            │
└──────────┬─────────────────────────────────────────────────┘
           │
           ▼
┌──────────────────────────┐
│    MCP TRANSPORT KATMANI │
│                          │
│  ┌────────────────────┐  │
│  │ STDIO Transport    │  │     ┌─────────────────────────┐
│  │                    │──────▶│ npx mcp-server-postgres  │
│  │ stdin/stdout ile   │  │     │ (yerel islem)            │
│  │ JSON-RPC iletisim  │◀──────│                          │
│  └────────────────────┘  │     │ SQL calistirir           │
│                          │     │ Sonuc JSON doner         │
│  ┌────────────────────┐  │     └─────────────────────────┘
│  │ HTTP Transport     │  │
│  │                    │──────▶ https://mcp.notion.com/mcp
│  │ HTTP POST ile      │  │     (uzak sunucu)
│  │ JSON-RPC iletisim  │◀──────
│  └────────────────────┘  │
│                          │
└──────────────────────────┘
```

### MCP Arac Kesfetme

Claude Code basladiginda:

```
1. ~/.claude.json ve .mcp.json dosyalarini okur
2. Tanimli MCP sunucularina baglanir
3. Her sunucudan mevcut araclari sorgular (tools/list)
4. Araclar Claude modeline tanimlanir
5. Model artik bu araclari kullanabilir

Ornek:
  postgres sunucusu → mcp__postgres__query araci tanimlanir
  github sunucusu → mcp__github__get_issue, mcp__github__create_pr, ... tanimlanir
```

---

## 11. MEMORY (BELLEK) SISTEMI

```
┌──────────────────────────────────────────────────────────┐
│                   BELLEK KATMANLARI                       │
│                                                          │
│  ┌────────────────────────────────────────────┐          │
│  │ 1. CLAUDE.md (Proje)                       │          │
│  │    Her oturumda yuklenir                   │          │
│  │    Proje kurallari, komutlar, yapilar      │          │
│  │    Siz yazarsiniz, siz guncellersiniz      │          │
│  └────────────────────────────────────────────┘          │
│                                                          │
│  ┌────────────────────────────────────────────┐          │
│  │ 2. Auto Memory (MEMORY.md)                 │          │
│  │    Her oturumda yuklenir (max 200 satir)   │          │
│  │    Claude otomatik yazar ve gunceller      │          │
│  │    Ogrenilenler, pattern'lar, tercihler    │          │
│  │    ~/.claude/projects/<proje>/memory/      │          │
│  └────────────────────────────────────────────┘          │
│                                                          │
│  ┌────────────────────────────────────────────┐          │
│  │ 3. Konusma Gecmisi (Session)               │          │
│  │    Sadece aktif oturumda                   │          │
│  │    /compact ile ozetlenir                  │          │
│  │    /resume ile devam edilebilir            │          │
│  │    Diske kaydedilir (session persistence)  │          │
│  └────────────────────────────────────────────┘          │
│                                                          │
│  ┌────────────────────────────────────────────┐          │
│  │ 4. progress.md (Gecici)                    │          │
│  │    Gorev surerken mevcut                   │          │
│  │    Gorev bitince silinir                   │          │
│  │    Context kaybi durumunda referans         │          │
│  └────────────────────────────────────────────┘          │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

### Bilgi Onceligi

Claude karar verirken su onceligi kullanir:

```
1. Kullanicinin son mesaji (en yuksek oncelik)
2. Konusma gecmisi
3. CLAUDE.md + rules (proje talimatlari)
4. Auto Memory (ogrenilmis bilgiler)
5. Model'in kendi bilgisi (egitim verisi)
```

---

## 12. EXTENDED THINKING (DERIN DUSUNME)

Opus 4.6 modelinde uyarlanabilir dusunme:

```
┌─────────────────────────────────────────────────────┐
│               DUSUNME SURECI                         │
│                                                      │
│  Siz: "Bu kodda neden race condition olusuyor?"      │
│                                                      │
│  ┌──────────────────────────────────────────┐        │
│  │ THINKING (gizli, size gosterilmez)       │        │
│  │                                          │        │
│  │ Hmm, once kodu okuyayim...               │        │
│  │ Bu fonksiyon async ama mutex yok...      │        │
│  │ Iki thread ayni anda X'e erisebilir...   │        │
│  │ Cozum olarak RWLock kullanilabilir...    │        │
│  │ Ama AtomicReference daha performansli... │        │
│  │                                          │        │
│  │ (Bu kisim token tuketir ama gorulmez)    │        │
│  └──────────────────────────────────────────┘        │
│                                                      │
│  ┌──────────────────────────────────────────┐        │
│  │ RESPONSE (size gosterilir)               │        │
│  │                                          │        │
│  │ "Race condition su satirda olusuyor:     │        │
│  │  ... cunku ... cozum olarak ..."         │        │
│  └──────────────────────────────────────────┘        │
│                                                      │
│  Effort Level:                                       │
│    Low    → Az dusunme, hizli cevap                  │
│    Medium → Orta dusunme (varsayilan)                │
│    High   → Derin dusunme, karmasik problemler       │
│                                                      │
│  Option+T / Alt+T → Ac/kapat                         │
│  /model → Effort level ayarla                        │
│                                                      │
└─────────────────────────────────────────────────────┘
```

---

## 13. DOSYA DUZENLEME MEKANIZMASI

Claude Code dosya duzenlerken **tam dosya degil, sadece diff** gonderir:

```
┌─────────────────────────────────────────────────────┐
│                EDIT ARACI                             │
│                                                      │
│  Edit(                                               │
│    file_path: "src/UserService.java",                │
│    old_string: "public User findById(Long id) {\n"   │
│                "    return repo.findById(id)\n"       │
│                "        .orElse(null);\n"             │
│                "}",                                   │
│    new_string: "public User findById(Long id) {\n"   │
│                "    return repo.findById(id)\n"       │
│                "        .orElseThrow(() ->\n"         │
│                "            new NotFoundException("   │
│                "\"User\", id));\n"                    │
│                "}"                                    │
│  )                                                   │
│                                                      │
│  Kurallar:                                           │
│  - old_string dosyada BENZERSIZ olmali               │
│  - Eslesen birden fazla yer varsa HATA verir         │
│  - Bulunamazsa HATA verir                            │
│  - Girintileme (indent) bire bir esmeli              │
│                                                      │
│  Bu yuzden Claude once Read ile dosyayi okur,        │
│  sonra Edit ile degisiklik yapar.                    │
│                                                      │
└─────────────────────────────────────────────────────┘
```

### Write vs Edit

```
Write: Tum dosyayi yeniden yazar (yeni dosya veya tamamen degistirme)
Edit:  Sadece degisen kismi gonderir (mevcut dosyada kucuk degisiklik)

Best practice: Mevcut dosyalarda Edit tercih et (daha az token, daha guvenli)
```

---

## 14. PLAN MODE DETAY

```
┌───────────────────────────────────────────────────────┐
│                   PLAN MODE AKISI                      │
│                                                        │
│  Siz: Shift+Tab (Plan Mode'a gec)                      │
│                                                        │
│  ┌──────────────────────────────────────┐              │
│  │ KULLANILABILIR ARACLAR:              │              │
│  │  ✅ Read    - Dosya oku              │              │
│  │  ✅ Glob    - Dosya ara              │              │
│  │  ✅ Grep    - Icerik ara             │              │
│  │  ✅ Agent   - Subagent (okuma)       │              │
│  │  ✅ WebFetch - Web oku               │              │
│  │  ✅ WebSearch - Web ara              │              │
│  │                                      │              │
│  │ YASAKLI ARACLAR:                     │              │
│  │  ❌ Write   - Dosya olusturma        │              │
│  │  ❌ Edit    - Dosya duzenleme        │              │
│  │  ❌ Bash    - Komut calistirma       │              │
│  │  ❌ NotebookEdit                     │              │
│  └──────────────────────────────────────┘              │
│                                                        │
│  Claude: "Projeyi analiz ettim. Sunum:                 │
│    1. X dosyasini degistirmek gerekiyor                 │
│    2. Y entity'si olusturulmali                         │
│    3. Z testi yazilmali                                 │
│    Plan onaylanirsa uygulayabilirim."                   │
│                                                        │
│  Siz: Shift+Tab (Normal Mode'a gec)                    │
│  Siz: "Plani uygula"                                   │
│                                                        │
│  Claude: (artik Write, Edit, Bash kullanabilir)        │
│          Plani adim adim uygular                       │
│                                                        │
└───────────────────────────────────────────────────────┘
```

---

## 15. TOKEN EKONOMISI

Her islem token tuketir. Maliyetin nereden geldigini anlamak:

```
┌─────────────────────────────────────────────────────────┐
│                TOKEN TUKETIMI                            │
│                                                         │
│  INPUT TOKEN (siz + baglam → modele gider):             │
│    System prompt (CLAUDE.md, rules, memory)   ~20K      │
│    Konusma gecmisi                            ~50K      │
│    Arac sonuclari (okunan dosyalar, vb.)      ~30K      │
│    Sizin mesajiniz                             ~0.1K    │
│    ────────────────────────────────────────             │
│    Toplam input: ~100K token/turn                       │
│                                                         │
│  THINKING TOKEN (dusunme, gorulmez):                    │
│    Extended thinking                          ~5-50K    │
│                                                         │
│  OUTPUT TOKEN (model → size doner):                     │
│    Arac cagrilari (Read, Edit, Bash)          ~0.5K     │
│    Text cevap                                 ~0.5K     │
│    ────────────────────────────────────────             │
│    Toplam output: ~1K token/turn                        │
│                                                         │
│  ONEMLI: Input token en buyuk maliyet kalemidir.        │
│  Her turn'de TUM context tekrar gonderilir.             │
│  Bu yuzden:                                             │
│    - CLAUDE.md'yi kisa tut                              │
│    - /compact ile context temizle                       │
│    - Subagent kullan (buyuk sonuclari izole et)         │
│    - Gereksiz dosya okumalarindan kacin                 │
│                                                         │
│  /cost → Gecerli oturumdaki token kullanimini goster    │
│  /usage → Plan limitlerini goster                       │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## 16. WORKTREE MEKANIZMASI

```
┌───────────────────────────────────────────────────────────┐
│                 GIT WORKTREE YAPISI                        │
│                                                           │
│  Ana repo: /Users/ben/proje/                              │
│    └── .git/                                              │
│    └── .claude/worktrees/                                 │
│            ├── auth-feature/        ← worktree 1          │
│            │   └── (tam kopya, ayri branch)               │
│            └── dashboard-feature/   ← worktree 2          │
│                └── (tam kopya, ayri branch)               │
│                                                           │
│  Her worktree:                                            │
│  - Ayri branch uzerinde calisir                          │
│  - Ayri calisma dizini (dosya sistemi izolasyonu)        │
│  - Ayni .git/objects paylasir (disk tasarrufu)           │
│  - Birbirini etkilemez                                   │
│                                                           │
│  Kullanim:                                                │
│    Terminal 1: claude -w auth     → auth-feature branch   │
│    Terminal 2: claude -w dashboard → dashboard branch     │
│    Terminal 3: claude             → main branch           │
│                                                           │
│  Oturum bitince:                                         │
│    Degisiklik yoksa → worktree otomatik temizlenir       │
│    Degisiklik varsa → "Worktree'yi koruyalim mi?" sorar  │
│                                                           │
└───────────────────────────────────────────────────────────┘
```

---

## 17. SKILLS CALISMA MEKANIZMASI

```
┌───────────────────────────────────────────────────────┐
│                SKILL YUKLEME AKISI                      │
│                                                        │
│  Siz: "/endpoint Product"                              │
│                                                        │
│  1. Claude Code .claude/skills/ ve                     │
│     ~/.claude/skills/ dizinlerini tarar                 │
│                                                        │
│  2. endpoint.md dosyasini bulur                        │
│                                                        │
│  3. Frontmatter'i okur:                                │
│     ---                                                │
│     name: endpoint                                     │
│     description: REST endpoint seti olusturur          │
│     tools: Write, Read, Glob, Edit                     │
│     ---                                                │
│                                                        │
│  4. Skill icerigi system prompt'a eklenir              │
│                                                        │
│  5. "Product" argumani ile birlikte                     │
│     skill talimatlari modele gonderilir                 │
│                                                        │
│  6. Model skill talimatlarini takip ederek             │
│     dosyalari olusturur                                │
│                                                        │
│  Sonuc: Skill = ozel system prompt + kisitli araclar   │
│                                                        │
└───────────────────────────────────────────────────────┘
```

---

## 18. OTURUM YASAM DONGUSU

```
claude komutu calistirilir
        │
        ▼
┌─ BASLANGIC ────────────────────────────────────────┐
│  1. Settings yukle (user → project → local)        │
│  2. CLAUDE.md dosyalarini yukle                    │
│  3. Auto memory yukle (MEMORY.md)                  │
│  4. Rules dosyalarini yukle (.claude/rules/)       │
│  5. MCP sunucularina baglan                        │
│  6. Plugin'leri yukle                              │
│  7. SessionStart hook'larini calistir              │
│  8. --init varsa init hook'larini calistir         │
│  9. Kullaniciya prompt goster                      │
└────────────────────────────┬───────────────────────┘
                             │
                             ▼
┌─ KONUSMA DONGUSU ──────────────────────────────────┐
│                                                     │
│  Kullanici mesaj yazar                              │
│        │                                            │
│        ▼                                            │
│  UserPromptSubmit hook calisir                      │
│        │                                            │
│        ▼                                            │
│  Mesaj + context → Anthropic API                    │
│        │                                            │
│        ▼                                            │
│  ┌─ AGENTIC LOOP ─────────────────────┐            │
│  │                                     │            │
│  │  Model dusunur (thinking)           │            │
│  │        │                            │            │
│  │        ├── Tool call? ─── EVET ──┐  │            │
│  │        │                         │  │            │
│  │        │    PreToolUse hook      │  │            │
│  │        │    Izin kontrolu        │  │            │
│  │        │    Arac calistirilir    │  │            │
│  │        │    PostToolUse hook     │  │            │
│  │        │    Sonuc modele doner   │  │            │
│  │        │         │               │  │            │
│  │        │         └───────────────┘  │            │
│  │        │                            │            │
│  │        └── Bitti? ─── EVET ──┐      │            │
│  │                              │      │            │
│  │  Stop hook calisir          │      │            │
│  │  Text cevap gosterilir      │      │            │
│  │                              │      │            │
│  └──────────────────────────────┘      │            │
│                                        │            │
│  Context %80+ doldu?                   │            │
│    EVET → Otomatik /compact            │            │
│                                        │            │
│  Kullanici yeni mesaj yazar → tekrarla │            │
│                                        │            │
└────────────────────────────────────────┘            │
                                                      │
┌─ BITIS ────────────────────────────────────────────┐
│  /exit veya Ctrl+D                                  │
│  SessionEnd hook calisir                            │
│  Oturum diske kaydedilir (resume icin)             │
│  Worktree varsa: "Koruyalim mi?" sorar             │
│  Auto memory guncellenir (gerekiyorsa)             │
└─────────────────────────────────────────────────────┘
```

---

## OZET: BIR KOMUTUN YOLCULUGU

```
Siz: "UserService'e findByEmail ekle"

1. [CLI]       Mesajinizi alir
2. [Hook]      UserPromptSubmit hook calisir (varsa)
3. [Context]   CLAUDE.md + rules + memory + gecmis toplanir
4. [API]       Tum context + mesaj → Anthropic API
5. [Model]     Dusunur (thinking tokens)
6. [Model]     Karar: "Read UserService.java"
7. [Hook]      PreToolUse hook (izin kontrolu)
8. [Izin]      allow listesinde mi? → evet → calistir
9. [Arac]      Read araci dosyayi okur
10. [Hook]     PostToolUse hook (varsa)
11. [API]      Dosya icerigi + onceki context → API
12. [Model]    Dusunur: "Simdi Edit yapayim"
13. [Hook]     PreToolUse hook
14. [Izin]     allow? → evet
15. [Arac]     Edit araci dosyayi duzenler
16. [Hook]     PostToolUse hook → prettier calisir
17. [API]      Sonuc → API
18. [Model]    Dusunur: "Tamamlandi, cevap vereyim"
19. [Hook]     Stop hook (varsa)
20. [CLI]      Cevap size gosterilir
21. [Memory]   Gerekiyorsa auto memory guncellenir
```
