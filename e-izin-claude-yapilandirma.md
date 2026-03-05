# e-Izin Projesi - Claude Code Yapilandirma Rehberi

Bu dosya, UseCase.md'deki 81 use case'i kod yazmadan
Claude Code CLAUDE.md + rules + agents + skills + hooks sistemiyle
nasil yonetilebilecegini gosterir.

---

## DIZIN YAPISI

```
e-izin/
├── CLAUDE.md                              # Ana proje rehberi
├── backend/
│   ├── CLAUDE.md                          # Backend rehberi
│   ├── common/                            # DTO, Enum, Constants
│   └── server/                            # Entity, Service, Controller
├── frontend/
│   └── CLAUDE.md                          # Frontend rehberi
├── .claude/
│   ├── settings.json                      # Izin ve hook ayarlari
│   ├── agents/
│   │   ├── usecase-analyzer.md            # Use case analiz agenti
│   │   ├── spring-reviewer.md             # Backend review agenti
│   │   ├── react-reviewer.md              # Frontend review agenti
│   │   ├── spring-test-writer.md          # Backend test yazici
│   │   ├── react-test-writer.md           # Frontend test yazici
│   │   ├── migration-writer.md            # Flyway migration agenti
│   │   └── security-auditor.md            # Guvenlik denetim agenti
│   ├──          # Entegrasyon kurallari
│   │   ├── modul-bld.md          rules/
│   │   ├── modul-sys.md                   # Sistem Yonetimi kurallari
│   │   ├── modul-kul.md                   # Kullanici/Yetki kurallari
│   │   ├── modul-cg.md                    # Cevre Gorevlisi kurallari
│   │   ├── modul-bsv.md                   # Basvuru Surecleri kurallari
│   │   ├── modul-deg.md                   # Degerlendirme kurallari
│   │   ├── modul-yen.md                   # Yenileme kurallari
│   │   ├── modul-mua.md                   # Muafiyet kurallari
│   │   ├── modul-ipt.md                   # Iptal Surecleri kurallari
│   │   ├── modul-ozl.md                   # Gorus/Ozel Islemler kurallari
│   │   ├── modul-blg.md                   # Belge Yonetimi kurallari
│   │   ├── modul-rpr.md                   # Raporlama kurallari
│   │   ├── modul-ent.md                   # Bildirim kurallari
│   │   ├── search-service.md              # SearchService pattern
│   │   ├── operational-service.md         # OperationalService pattern
│   │   ├── common-dto.md                  # DTO kurallari
│   │   ├── react-rtk-query.md             # RTK Query kurallari
│   │   ├── react-component.md             # Component kurallari
│   │   └── progress-tracking.md           # Gorev takip kurali
│   └── skills/
│       ├── usecase.md                     # Use case'den endpoint uret
│       ├── modul.md                       # Tum modul dosyalarini uret
│       ├── endpoint.md                    # Tekil endpoint seti uret
│       ├── feature-ui.md                  # React feature uret
│       ├── migration.md                   # Flyway migration uret
│       ├── feature.md                     # Git branch workflow
│       └── dep.md                         # Gradle dependency ekle
└── .mcp.json                              # MCP sunucu yapilandirmasi
```

---

## 1. CLAUDE.md DOSYALARI

### Ana CLAUDE.md (Proje Koku)

Dosya: `CLAUDE.md`

```markdown
# e-Izin Sistemi

Cevre ve Sehircilik Bakanligi e-Izin/Lisans basvuru ve degerlendirme sistemi.
Full-stack monorepo: Spring Boot API (multi-module) + React SPA.

## Proje Yapisi

- `backend/common/` - DTO, Enum, Constants (Spring bagimsiz)
- `backend/server/` - Entity, Repository, Service, Controller, Config
- `frontend/` - React 18.3 + TypeScript + RTK Query + MUI + Golge UI

## Hizli Baslatma

- Backend: `cd backend && ./gradlew bootRun`
- Frontend: `cd frontend && npm run dev`
- Full stack: `docker compose up`

## Ortam

- Backend: http://localhost:8080
- Frontend: http://localhost:5173
- PostgreSQL: localhost:5432
- Swagger: http://localhost:8080/swagger-ui.html

## Modul Kodlari

| Kod | Modul | UC Sayisi |
|-----|-------|-----------|
| SYS | Sistem Yonetimi ve Konfigurasyon | 8 |
| KUL | Kullanici ve Yetki Yonetimi | 6 |
| CG  | Cevre Gorevlisi Islemleri | 5 |
| BSV | Basvuru Surecleri | 8 |
| DEG | Degerlendirme Surecleri | 9 |
| YEN | Yenileme ve Guncelleme | 8 |
| MUA | Muafiyet Islemleri | 4 |
| IPT | Iptal Surecleri | 7 |
| OZL | Gorus ve Ozel Islemler | 5 |
| BLG | Belge Yonetimi | 5 |
| RPR | Raporlama ve Analitik | 6 |
| ENT | Entegrasyon Yonetimi | 6 |
| BLD | Bildirim Sistemi | 4 |

## Aktorler

### Basvuru Yapanlar
- Cevre Gorevlisi (CG) - Tesisler adina basvuru yapar
- Cevre Danismanlik Firmasi (CDF) - CG'leri istihdam eder
- Cevre Yonetim Birimi (CYB) - Isletme buynesindeki birim
- Firma Yetkilisi - Isletme sahibi/yetkili temsilci

### Degerlendirme Yapanlar
- Il Mudurlugu Personeli - EK-2 basvuru degerlendirme
- Bakanlik Personeli - EK-1 basvuru degerlendirme
- Sube Muduru - Onay/red karari

### Sistem
- Sistem Yoneticisi - Parametre, sablon, organizasyon yonetimi

### Dis Sistemler
- e-Yeterlik Sistemi - CG yetki kontrolu
- Belgenet - Elektronik belge yonetimi
- TOBB/TESK - Kapasite raporu
- e-CED Sistemi - CED karar sorgulama
- Doner Sermaye - Odeme sistemi

## Ana Surec Akisi

Uygunluk Basvurusu → GFB Basvurusu → Izin/Lisans Basvurusu
Her asamada: Degerlendirme → Uygun/Red/Iade

## Service Ayirimi (KRITIK!)

- **SearchService/SearchServiceImpl**: GET, filtreleme, listeleme, sorgulama
- **OperationalService/OperationalServiceImpl**: Create, Update, Delete, durum degistirme
- Bu ayirim hem Controller hem Service katmaninda gecerli

## Is Kurallari Referansi

- Il Md. Uygunluk yazisi: 1 yil gecerli
- GFB: 1 yil gecerli, degerlendirme 30 takvim gunu
- Izin/Lisans: 5 yil gecerli, degerlendirme 60 takvim gunu
- GFB'den sonra 180 gun icinde Izin/Lisans basvurusu yapilmali
- EK-1: Bakanlik degerlendirmesi, EK-2: Il Mudurlugu degerlendirmesi
- Parametre degisiklikleri anlik devreye girer
- Audit loglari 10 yil saklanir

@backend/CLAUDE.md
@frontend/CLAUDE.md
```

### Backend CLAUDE.md

Dosya: `backend/CLAUDE.md`

```markdown
# e-Izin Backend - Spring Boot

## Teknoloji

- Java 21, Spring Boot 3.3, Gradle 8.x (Kotlin DSL)
- Shadow JAR (com.github.johnrengelman.shadow)
- Spring Data JPA + Hibernate + PostgreSQL
- Flyway migration
- Spring Security + JWT (e-Devlet / Kurumsal giris)
- MapStruct, Lombok, springdoc-openapi

## Multi-Module

- `common/` - DTO, Enum, Constants (Spring bagimsiz, JPA annotation YOK)
- `server/` - Entity, Repository, Service, Controller

## Komutlar

- `./gradlew bootRun` - gelistirme
- `./gradlew test` - tum testler
- `./gradlew :server:test --tests "*.XxxTest"` - belirli test
- `./gradlew build` - derleme
- `./gradlew shadowJar` - fat JAR
- `./gradlew flywayMigrate` - migration

## Package Yapisi

### common modulu
```
com.golge.eizin.common
├── dto
│   ├── sys/          # SYS modul DTO'lari
│   ├── kul/          # KUL modul DTO'lari
│   ├── cg/           # CG modul DTO'lari
│   ├── bsv/          # BSV modul DTO'lari
│   ├── deg/          # DEG modul DTO'lari
│   ├── yen/          # YEN modul DTO'lari
│   ├── mua/          # MUA modul DTO'lari
│   ├── ipt/          # IPT modul DTO'lari
│   ├── ozl/          # OZL modul DTO'lari
│   ├── blg/          # BLG modul DTO'lari
│   ├── rpr/          # RPR modul DTO'lari
│   ├── ent/          # ENT modul DTO'lari
│   ├── bld/          # BLD modul DTO'lari
│   └── common/       # Ortak DTO'lar (ApiResponse, PageResponse)
├── enums
│   ├── BasvuruDurumu.java
│   ├── BelgeDurumu.java
│   ├── EkKapsami.java        # EK_1, EK_2
│   ├── IzinKonusu.java       # HAVA_EMISYONU, GURULTU, ATIKSU, DERIN_DENIZ
│   ├── LisansKonusu.java
│   ├── DegerlendirmeSonucu.java  # UYGUN, RED, IADE, EKSIKLIK
│   ├── KullaniciRolu.java
│   ├── BildirimTipi.java
│   └── VekaletDurumu.java
├── constants
│   ├── ApiConstants.java      # API path sabitleri
│   ├── SureConstants.java     # Belge sureleri (GFB_SURE=365, IZIN_SURE=1825)
│   └── MesajConstants.java    # Hata/bilgi mesajlari
├── exception
│   ├── ResourceNotFoundException.java
│   ├── BusinessRuleException.java
│   ├── SureAsimException.java
│   └── YetkisizErisimException.java
└── util
    ├── DateUtils.java
    └── BelgeNumarasiGenerator.java
```

### server modulu
```
com.golge.eizin.server
├── entity
│   ├── sys/          # Parametre, IzinKonuTanim, EkListesi, Organizasyon, OnayAkisi, BelgeSablonu, AtikKodu, Duyuru
│   ├── kul/          # Kullanici, Rol, KullaniciRol, Vekalet, AuditLog, Oturum
│   ├── cg/           # SorumluTesis, BasvuruTaslak
│   ├── bsv/          # Basvuru, BasvuruBelge, IsAkimSemasi, BasvuruOnay, OdemeBilgi
│   ├── deg/          # Degerlendirme, EksiklikBildirimi, DegerlendirmeKarari, PersonelYonlendirme
│   ├── yen/          # YenilemeBasvuru, FaaliyetDegisiklik, UnvanDegisiklik, AtikKoduEkleme
│   ├── mua/          # MuafiyetBasvuru, GeciciIsletme
│   ├── ipt/          # IptalKarari, YenidenAktiflestirme, FaaliyetSonlandirma
│   ├── ozl/          # GorusTalebi, OfbKayit
│   ├── blg/          # Belge, BelgeVersiyon, BelgeArsiv
│   ├── rpr/          # Rapor, RaporSablon
│   ├── ent/          # EntegrasyonLog, EntegrasyonSaglik
│   ├── bld/          # Bildirim, BildirimTercih
│   └── base/         # BaseEntity (id, createdAt, updatedAt, createdBy, isActive)
├── repository
│   ├── sys/, kul/, cg/, bsv/, deg/, yen/, mua/, ipt/, ozl/, blg/, rpr/, ent/, bld/
├── service
│   ├── search/       # GET/filtreleme servisleri
│   │   ├── sys/, kul/, cg/, bsv/, deg/, yen/, mua/, ipt/, ozl/, blg/, rpr/, ent/, bld/
│   │   └── impl/
│   └── operational/  # CUD servisleri
│       ├── sys/, kul/, cg/, bsv/, deg/, yen/, mua/, ipt/, ozl/, blg/, rpr/, ent/, bld/
│       └── impl/
├── controller
│   ├── search/       # GET endpoint'leri
│   │   └── sys/, kul/, cg/, bsv/, deg/, yen/, mua/, ipt/, ozl/, blg/, rpr/, ent/, bld/
│   └── operational/  # POST/PUT/DELETE endpoint'leri
│       └── sys/, kul/, cg/, bsv/, deg/, yen/, mua/, ipt/, ozl/, blg/, rpr/, ent/, bld/
├── mapper
├── config
│   ├── SecurityConfig.java
│   ├── CorsConfig.java
│   ├── SwaggerConfig.java
│   └── SchedulerConfig.java   # Zamanlayici isler (sure kontrol, senkronizasyon)
├── scheduler
│   ├── BelgeGecerlilikJob.java     # UC-YEN-001: Belge suresi uyari
│   ├── GfbOtomatikIptalJob.java    # UC-IPT-001: GFB otomatik iptal
│   ├── SenkronizasyonJob.java      # UC-KUL-004: Hesap Modulu sync
│   ├── SureUyariJob.java           # UC-DEG-006: Degerlendirme suresi
│   └── BildirimJob.java            # UC-BLD-002: Sure uyari bildirimleri
└── integration
    ├── EYeterlikClient.java        # e-Yeterlik sistemi
    ├── BelgenetClient.java         # Belgenet entegrasyonu
    ├── TobbTeskClient.java         # TOBB/TESK sorgulama
    ├── ECedClient.java             # e-CED sistemi
    └── DonerSermayeClient.java     # Odeme sistemi
```

## Kurallar

- Constructor injection (field injection YASAK)
- application.yml (properties DEGIL)
- @Slf4j loglama
- ResponseEntity<ApiResponse<T>> response format
- @Valid + Jakarta Validation
- @ControllerAdvice global exception handler
- Lombok: @Data, @Builder, @AllArgsConstructor
- DTO SADECE common'da, Entity SADECE server'da
- Search: @Transactional(readOnly=true)
- Operational: @Transactional
```

### Frontend CLAUDE.md

Dosya: `frontend/CLAUDE.md`

```markdown
# e-Izin Frontend - React

## Teknoloji

- React 18.3 + TypeScript 5 (strict mode)
- Vite, React Router v6
- RTK Query (API state), Redux Toolkit (client state)
- MUI (Material UI) + Golge UI
- React Hook Form + Zod
- Vitest + React Testing Library

## Komutlar

- `npm run dev` - gelistirme
- `npm run build` - derleme (tsc && vite build)
- `npm run test` - vitest
- `npm run lint` - ESLint
- `npm run type-check` - TypeScript kontrol

## Dosya Yapisi

```
src/
├── features/                    # Modul bazli
│   ├── sys/                     # Sistem Yonetimi
│   │   ├── api.ts               # RTK Query endpoints
│   │   ├── types.ts             # TypeScript tipleri
│   │   ├── components/
│   │   │   ├── ParametreYonetimi.tsx
│   │   │   ├── IzinKonuTanim.tsx
│   │   │   ├── EkListeYonetimi.tsx
│   │   │   ├── OrganizasyonSemasi.tsx
│   │   │   ├── OnayAkisiYapilandirma.tsx
│   │   │   ├── BelgeSablonYonetimi.tsx
│   │   │   ├── AtikKoduYonetimi.tsx
│   │   │   └── DuyuruYonetimi.tsx
│   │   └── __tests__/
│   ├── kul/                     # Kullanici Yonetimi
│   ├── cg/                      # Cevre Gorevlisi
│   ├── bsv/                     # Basvuru Surecleri
│   │   ├── api.ts
│   │   ├── types.ts
│   │   ├── components/
│   │   │   ├── UygunlukBasvuruForm.tsx       # UC-BSV-001
│   │   │   ├── GfbBasvuruForm.tsx            # UC-BSV-002
│   │   │   ├── IzinLisansBasvuruForm.tsx     # UC-BSV-003
│   │   │   ├── BelgeHavuzu.tsx               # UC-BSV-004
│   │   │   ├── IsAkimSemasi.tsx              # UC-BSV-005
│   │   │   ├── CokluOnay.tsx                 # UC-BSV-006
│   │   │   ├── EntegrasyonVeriCek.tsx        # UC-BSV-007
│   │   │   └── OdemeFormu.tsx                # UC-BSV-008
│   │   └── __tests__/
│   ├── deg/                     # Degerlendirme
│   ├── yen/                     # Yenileme
│   ├── mua/                     # Muafiyet
│   ├── ipt/                     # Iptal
│   ├── ozl/                     # Gorus/Ozel
│   ├── blg/                     # Belge
│   ├── rpr/                     # Raporlama
│   ├── ent/                     # Entegrasyon
│   └── bld/                     # Bildirim
├── pages/                       # Sayfa componentleri
├── components/                  # Ortak UI componentleri
│   ├── BasvuruStepper.tsx       # Cok adimli basvuru wizard
│   ├── DurumGostergesi.tsx      # Yesil/Sari/Kirmizi durum badge
│   ├── BelgeYukleme.tsx         # Drag & drop belge yukleme
│   ├── EImzaDialog.tsx          # e-Imza onay dialog
│   ├── OnayAkisiTimeline.tsx    # Coklu onay goruntuleme
│   ├── TesisKart.tsx            # Tesis ozet karti
│   └── FilterPanel.tsx          # Dinamik filtre paneli
├── hooks/
│   ├── useAuth.ts               # Kimlik dogrulama
│   ├── useRol.ts                # Rol bazli erisim kontrolu
│   ├── useBildirim.ts           # Bildirim hook
│   └── useTesis.ts              # Tesis secim hook
├── services/
│   └── baseApi.ts               # RTK Query base config
├── store/
│   └── index.ts                 # Redux store
├── types/
│   ├── api.ts                   # ApiResponse, PageResponse
│   ├── auth.ts                  # Kullanici, Rol, Oturum
│   └── enums.ts                 # Frontend enum'lari
├── theme/
│   └── index.ts                 # MUI + Golge UI tema
└── utils/
```

## Kurallar

- TypeScript strict, any YASAK
- Named export (default export YASAK)
- API: SADECE RTK Query (axios/fetch dogrudan YASAK)
- UI: Golge UI > MUI > custom
- Form: React Hook Form + Zod ZORUNLU
- Her sayfada Loading/Error/Empty state
- 2 space indent, trailing comma, single quote
```

---

## 2. RULES (MODUL BAZLI KURALLAR)

### Ornek: Basvuru Surecleri Kurali

Dosya: `.claude/rules/modul-bsv.md`

```markdown
---
paths:
  - "backend/**/bsv/**"
  - "frontend/src/features/bsv/**"
---

# BSV - Basvuru Surecleri Kurallari

## Use Case'ler
- UC-BSV-001: Il Md. Uygunluk Basvurusu (EK-3A formati)
- UC-BSV-002: GFB Basvurusu (GFB suresi 1 yil, degerlendirme 30 gun)
- UC-BSV-003: Cevre Izni/Lisansi Basvurusu (suresi 5 yil, degerlendirme 60 gun)
- UC-BSV-004: Belge Havuzundan Belge Secme
- UC-BSV-005: Is Akim Semasi ve Proses Ozeti
- UC-BSV-006: Basvuruyu Coklu Onaya Sunma
- UC-BSV-007: Entegre Sistemlerden Veri Cekme (e-CED, TOBB/TESK)
- UC-BSV-008: Basvuru Bedelini Odeme (Doner Sermaye)

## Is Kurallari

- BR-BSV-001: EK-1 → Bakanlik, EK-2 → Il Mudurlugu
- BR-BSV-002: Coklu onay tanimli ise tum onaylar gerekli
- BR-BSV-020: GFB konulari uygunluk yazisindan gelir, degistirilemez
- BR-BSV-021: GFB 1 yil gecerli
- BR-BSV-030: Izin/Lisans konulari GFB'den gelir
- BR-BSV-031: Izin/Lisans 5 yil gecerli

## Basvuru Akisi

Uygunluk → GFB → Izin/Lisans (sirali, atlanamaz)

## DTO Isimlendirme (common)

- CreateUygunlukBasvuruRequest
- CreateGfbBasvuruRequest
- CreateIzinLisansBasvuruRequest
- BasvuruResponse
- BasvuruFilterRequest
- BasvuruDurumResponse

## Entity (server)

- Basvuru: id, tesisId, ekKapsami, basvuruTipi, durum, basvuruTarihi, degerlendirenId
- BasvuruBelge: id, basvuruId, belgeTipi, dosyaYolu, yuklemeTarihi
- BasvuruOnay: id, basvuruId, onaylayanId, sira, durum, onayTarihi

## Entegrasyon

- e-CED: CED karari sorgula (ZORUNLU alan)
- TOBB/TESK: Sicil gazetesi, kapasite raporu
- Sifir Atik: Sifir atik belgesi
- Doner Sermaye: Odeme kontrolu

## Frontend

- BasvuruStepper: cok adimli wizard form
- EK kapsami secimi → dinamik alt kategori
- Belge listesi EK-3B/3C'den otomatik
- e-Imza dialog entegrasyonu
- Coklu onay timeline gorunumu
```

### Ornek: Degerlendirme Surecleri Kurali

Dosya: `.claude/rules/modul-deg.md`

```markdown
---
paths:
  - "backend/**/deg/**"
  - "frontend/src/features/deg/**"
---

# DEG - Degerlendirme Surecleri Kurallari

## Use Case'ler
- UC-DEG-001: Il Md. Uygunluk degerlendirme
- UC-DEG-002: GFB degerlendirme (30 gun)
- UC-DEG-003: Izin/Lisans degerlendirme (60 gun)
- UC-DEG-004: Eksiklik bildirimi
- UC-DEG-005: Red/Iade karari
- UC-DEG-006: Degerlendirme suresi takip (SCHEDULER JOB)
- UC-DEG-007: Basvuruyu personele yonlendirme
- UC-DEG-008: Belge olusturma ve imzaya gonderme
- UC-DEG-009: Degerlendirme gecmisini goruntuleme

## Is Kurallari

- GFB degerlendirme: max 30 takvim gunu
- Izin/Lisans degerlendirme: max 60 takvim gunu
- Eksiklik bildirimi suresi durdurur
- Red kararinda gerekce zorunlu
- Iade: basvuran duzeltip tekrar gonderebilir

## Scheduler Job

DegerlendirmeSuresiJob:
- Yaklasan sureler icin uyari bildirimi (7 gun, 3 gun, 1 gun)
- Sure asan basvurular icin yonetici bildirimi

## Degerlendirme Durumu Enum

ATANDI, DEGERLENDIRILIYOR, EKSIKLIK_BILDIRILDI, IADE_EDILDI, RED, UYGUN, IMZAYA_GONDERILDI, TAMAMLANDI
```

### Ornek: Entegrasyon Kurali

Dosya: `.claude/rules/modul-ent.md`

```markdown
---
paths:
  - "backend/**/ent/**"
  - "backend/**/integration/**"
  - "frontend/src/features/ent/**"
---

# ENT - Entegrasyon Yonetimi Kurallari

## Dis Sistemler

| Sistem | Kullanim | Client Sinifi |
|--------|----------|---------------|
| e-Yeterlik | CG yetki kontrolu, tesis listesi | EYeterlikClient |
| Belgenet | Belge arsivleme, resmi yazisma | BelgenetClient |
| TOBB/TESK | Kapasite raporu, sicil gazetesi | TobbTeskClient |
| e-CED | CED karar sorgulama | ECedClient |
| Doner Sermaye | Odeme islemleri, harc tahsilat | DonerSermayeClient |
| Hesap Modulu | Kullanici/firma/tesis senkronizasyon | HesapModuluClient |

## Entegrasyon Kurallari

- Her dis sistem icin ayri Client sinifi (integration/ paketi)
- Retry mekanizmasi: 3 deneme, exponential backoff
- Circuit breaker: 5 ardisik hata → devre keser
- Timeout: 30 saniye
- Hata durumunda: loglama + bildirim + fallback (onbellek/manuel giris)
- Her cagri EntegrasyonLog'a kaydedilir
- Saglik kontrolu: her 5 dakika (UC-ENT-003)

## Hata Yonetimi

- Entegrasyon hatasi basvuruyu ENGELLEMEZ
- Hata durumunda kullaniciya manuel giris secenegi sun
- "Veriler guncel olmayabilir" uyarisi goster (onbellek kullanilirsa)
```

---

## 3. AGENTS (UZMAN AJANLAR)

### Use Case Analizcisi

Dosya: `.claude/agents/usecase-analyzer.md`

```markdown
---
name: usecase-analyzer
description: UseCase.md dosyasindan use case analiz eder, backend ve frontend icin gerekli dosya listesi cikarir
tools: Read, Glob, Grep
model: sonnet
permissionMode: plan
---

UseCase.md dosyasindan belirli bir use case'i analiz et.

## Cikti Formati

Her use case icin su bilgileri cikar:

1. **Modul ve UC Kodu** (ornek: BSV, UC-BSV-001)
2. **Aktorler** ve rolleri
3. **On Kosullar** - hangi kontroller gerekli
4. **Temel Akis Adimlari** - is mantigi
5. **Alternatif Akislar** - branch'ler
6. **Istisnalar** - hata durumlari
7. **Is Kurallari** - BR kodlari ve kurallari

8. **Backend icin gerekli dosyalar:**
   - common/dto: Hangi DTO siniflar
   - common/enums: Hangi enum'lar
   - server/entity: Hangi entity'ler
   - server/repository: Hangi repository'ler
   - server/service/search: GET islemleri
   - server/service/operational: CUD islemleri
   - server/controller: Endpoint'ler
   - server/scheduler: Zamanlayici is gerekiyorsa
   - server/integration: Dis sistem entegrasyonu gerekiyorsa

9. **Frontend icin gerekli dosyalar:**
   - features/<modul>/api.ts: RTK Query endpoint'leri
   - features/<modul>/types.ts: TypeScript tipleri
   - features/<modul>/components/: UI componentleri
   - pages/: Sayfa componentleri
   - components/: Ortak componentler (gerekiyorsa)

10. **Migration**: SQL sema degisiklikleri
11. **Bagimliliklar**: Baska hangi use case'lere bagimli
```

### Guvenlik Denetcisi

Dosya: `.claude/agents/security-auditor.md`

```markdown
---
name: security-auditor
description: e-Izin sisteminin guvenlik gereksinimlerini denetler (KVKK, yetkilendirme, audit log)
tools: Read, Glob, Grep
model: sonnet
permissionMode: plan
---

e-Izin sistemi guvenlik kontrolu yap:

## Kontrol Listesi

1. **Yetkilendirme**: @PreAuthorize tum OperationalController'larda var mi?
2. **Rol Kontrolu**: Aktor-endpoint eslesmesi dogru mu?
   - CG: sadece kendi tesisleri
   - Il Md. Personeli: sadece EK-2
   - Bakanlik Personeli: sadece EK-1
3. **Veri Erisim**: Kullanici sadece yetkili oldugu verileri goruyor mu?
4. **Audit Log**: Tum CUD islemleri loglanmis mi? (BR-KUL-006: 10 yil saklama)
5. **KVKK**: Hassas veri (TC Kimlik, adres) logda maskelenmis mi?
6. **e-Imza**: Imza dogrulama tum onay akislarinda var mi?
7. **Oturum**: 30 dakika inaktivite sonrasi otomatik cikis
8. **Brute Force**: 3 basarisiz giris → 30 dakika kilitleme
9. **Entegrasyon**: Dis sistem token'lari kodda mi yoksa config'de mi?
10. **Belge Guvenligi**: Yuklenen dosyalarda virus taramas ve boyut kontrolu
```

---

## 4. SKILLS (HIZLI URETIM KOMUTLARI)

### Use Case'den Endpoint Uretme

Dosya: `.claude/skills/usecase.md`

```markdown
---
name: usecase
description: UseCase.md'den belirli bir UC kodunu okur ve tum backend + frontend dosyalarini uretir
tools: Read, Write, Edit, Glob, Grep, Bash
---

Verilen use case kodunu (ornek: UC-BSV-001) isle:

1. `/Users/karademir/Downloads/UseCase.md` dosyasindan ilgili UC bolumunu oku
2. progress.md dosyasi olustur (gorev takibi icin)
3. Analiz et: aktorler, on kosullar, temel akis, is kurallari
4. Backend dosyalarini olustur:
   - common/dto: Request, Response, Filter DTO'lari
   - common/enums: Gerekli enum'lar (yoksa)
   - server/entity: JPA entity
   - server/repository: Spring Data repository
   - server/mapper: MapStruct mapper
   - server/service/search: SearchService + Impl (GET islemleri)
   - server/service/operational: OperationalService + Impl (CUD islemleri)
   - server/controller/search: SearchController (GET endpoint'leri)
   - server/controller/operational: OperationalController (POST/PUT/DELETE)
5. Frontend dosyalarini olustur:
   - features/<modul>/types.ts
   - features/<modul>/api.ts (RTK Query)
   - features/<modul>/components/<Component>.tsx
6. Migration dosyasi olustur (gerekiyorsa)
7. Test dosyalarini olustur
8. progress.md'yi guncelle ve tamamlaninca sil

Mevcut projedeki pattern'lara ve .claude/rules/ kurallarına uy.
```

Kullanim:
```
/usecase UC-BSV-001
/usecase UC-DEG-003
/usecase UC-KUL-005
```

### Modul Uretici

Dosya: `.claude/skills/modul.md`

```markdown
---
name: modul
description: Bir moduldeki tum use case'leri sirayla isler ve tum dosyalari uretir
tools: Read, Write, Edit, Glob, Grep, Bash
---

Verilen modul kodunu (ornek: BSV) isle:

1. UseCase.md'den o moduldeki TUM use case'leri bul
2. progress.md olustur (tum UC'ler listelenir)
3. Her UC icin sirayla /usecase skill'ini uygula:
   - Oncelik: entity → DTO → repository → service → controller → frontend → test
   - Her UC tamamlaninca progress.md guncelle
4. Modul tamamlaninca:
   - Tum testleri calistir
   - progress.md'yi sil
   - Ozet rapor ver

Moduldeki UC'ler arasi bagimliliklara dikkat et.
Ortak entity ve DTO'lari tekrar olusturma, mevcut olani kullan.
```

Kullanim:
```
/modul SYS
/modul BSV
/modul DEG
```

### Endpoint ve Feature-UI (mevcut ornekler.md'den)

```
/endpoint Basvuru          # Backend endpoint seti
/feature-ui bsv            # Frontend feature modulu
/migration orders tablosu  # Migration dosyasi
/feature siparis-takip     # Git branch workflow
/dep Redis cache           # Gradle dependency
```

---

## 5. HOOKS

Dosya: `.claude/settings.json`

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "FILE=$(jq -r '.tool_input.file_path // empty'); if echo \"$FILE\" | grep -qE '\\.(tsx?|jsx?|css)$'; then cd frontend && npx prettier --write \"$FILE\" && npx eslint --fix \"$FILE\" 2>/dev/null || true; fi"
          }
        ]
      },
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "FILE=$(jq -r '.tool_input.file_path // empty'); if echo \"$FILE\" | grep -qE '\\.java$'; then cd backend && ./gradlew checkstyleMain --quiet 2>/dev/null || true; fi"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "FILE=$(jq -r '.tool_input.file_path // empty'); if echo \"$FILE\" | grep -q 'db/migration/V'; then echo 'Mevcut migration duzenlenmemeli! Yeni migration olustur.' >&2; exit 2; fi"
          }
        ]
      },
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "CMD=$(jq -r '.tool_input.command'); if echo \"$CMD\" | grep -qiE 'drop table|drop database|truncate|flywayClean|git push.*--force|rm -rf /'; then echo 'Tehlikeli komut engellendi' >&2; exit 2; fi"
          }
        ]
      }
    ],
    "SessionStart": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "echo '=== e-Izin Ortam Kontrolu ==='; java -version 2>&1 | head -1; node -v 2>/dev/null; echo \"Branch: $(git branch --show-current 2>/dev/null)\"; test -f progress.md && echo '--- Devam eden gorev ---' && head -20 progress.md || echo 'Aktif gorev yok'"
          }
        ]
      }
    ]
  }
}
```

> **SessionStart hook**: Oturum basinda progress.md varsa son kalinan yeri gosterir.
> Boylece context kaybi olsa bile nerede kaldigini bilirsin.

---

## 6. MCP YAPILANDIRMASI

Dosya: `.mcp.json`

```json
{
  "mcpServers": {
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": { "GITHUB_TOKEN": "${GITHUB_TOKEN}" }
    },
    "postgres": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": { "DATABASE_URL": "${DATABASE_URL}" }
    }
  }
}
```

---

## 7. IZIN KURALLARI

Dosya: `.claude/settings.json` (permissions bolumu)

```json
{
  "permissions": {
    "allow": [
      "Read", "Glob", "Grep",
      "Bash(cd backend && ./gradlew *)",
      "Bash(cd frontend && npm run *)",
      "Bash(./gradlew *)",
      "Bash(git status)", "Bash(git log *)", "Bash(git diff *)",
      "Bash(git add *)", "Bash(git checkout -b *)",
      "Edit(backend/common/src/**)", "Edit(backend/server/src/**)",
      "Edit(frontend/src/**)",
      "Write(backend/common/src/**)", "Write(backend/server/src/**)",
      "Write(frontend/src/**)",
      "Write(backend/server/src/main/resources/db/migration/*.sql)"
    ],
    "deny": [
      "Bash(rm -rf *)", "Bash(git push --force *)",
      "Bash(./gradlew flywayClean*)",
      "Edit(.env*)", "Write(.env*)",
      "Edit(**/*secret*)", "Edit(**/*credential*)"
    ]
  }
}
```

---

## 8. ORNEK IS AKISI

### Senaryo: BSV modulu sifirdan

```bash
# 1. Branch olustur
/feature basvuru-surecleri

# 2. Tum modulu uret
/modul BSV

# Claude su adimlari takip eder:
# → progress.md olusturur (8 UC listelenir)
# → UC-BSV-001: Uygunluk Basvurusu entity + DTO + service + controller + frontend
# → UC-BSV-002: GFB Basvurusu ...
# → UC-BSV-003: Izin/Lisans Basvurusu ...
# → ... (8 UC tamamlanana kadar)
# → Testleri calistirir
# → progress.md siler
# → Ozet rapor verir

# 3. Commit
"BSV modulu basvuru surecleri tamamlandi, commit et"
```

### Senaryo: Tekil use case

```bash
# Sadece belirli bir UC
/usecase UC-DEG-004

# Claude:
# → UseCase.md'den UC-DEG-004 (Eksiklik Bildirimi) okur
# → progress.md olusturur
# → Backend: EksiklikBildirimi entity, DTO, SearchService, OperationalService, Controller
# → Frontend: EksiklikBildirimiForm.tsx, api.ts'e endpoint ekler
# → Test yazar ve calistirir
# → progress.md siler
```

### Senaryo: Sadece analiz

```bash
# Plan modunda analiz (kod yazmadan)
Shift+Tab  # Plan Mode

"UC-BSV-001 use case'ini analiz et.
 Hangi entity, DTO, service, controller dosyalari gerekli?
 Hangi dis sistem entegrasyonlari lazim?
 Hangi is kurallari uygulanmali?"
```

---

## 9. MODUL URETIM ONCELIK SIRASI

Moduller arasi bagimliliklar nedeniyle su sirada uretilmeli:

```
1. SYS  - Sistem Yonetimi (temel parametreler, tanimlar)
2. KUL  - Kullanici Yonetimi (auth, rol, yetki)
3. ENT  - Entegrasyon (dis sistem client'lari)
4. BLD  - Bildirim (bildirim altyapisi)
5. BLG  - Belge Yonetimi (belge CRUD)
6. CG   - Cevre Gorevlisi (tesis yonetimi)
7. BSV  - Basvuru Surecleri (ana is akisi)
8. DEG  - Degerlendirme (basvuru degerlendirme)
9. YEN  - Yenileme (belge yenileme)
10. MUA - Muafiyet (muafiyet islemleri)
11. IPT - Iptal (iptal surecleri)
12. OZL - Gorus/Ozel (ek islemler)
13. RPR - Raporlama (son: tum veriler hazir oldugunda)
```

---

## 10. PROGRESS.MD ORNEGI

`/modul BSV` calistirildiginda olusan progress.md:

```markdown
# Gorev: BSV - Basvuru Surecleri Modulu

## Yapilacaklar
- [ ] UC-BSV-001: Il Md. Uygunluk Basvurusu (Entity, DTO, Service, Controller, Frontend)
- [ ] UC-BSV-002: GFB Basvurusu
- [ ] UC-BSV-003: Cevre Izni/Lisansi Basvurusu
- [ ] UC-BSV-004: Belge Havuzundan Belge Secme
- [ ] UC-BSV-005: Is Akim Semasi ve Proses Ozeti
- [ ] UC-BSV-006: Coklu Onay
- [ ] UC-BSV-007: Entegre Sistemlerden Veri Cekme
- [ ] UC-BSV-008: Basvuru Bedeli Odeme
- [ ] Tum testleri calistir ve dogrula

## Tamamlananlar
(henuz yok)

## Notlar
- Bagimliliklar: SYS (parametreler), KUL (auth), ENT (dis sistemler), BLG (belgeler)
- e-CED, TOBB/TESK entegrasyonlari ENT modulunde hazir olmali
```
