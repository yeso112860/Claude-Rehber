# Kullanici Yonetim Sistemi

Node.js (Express + TypeScript) + React 18 monorepo.

## Teknoloji Stack

- **Backend:** Node.js 20, Express 4, TypeScript, better-sqlite3
- **Frontend:** React 18, TypeScript (strict), RTK Query, MUI 5
- **Veritabani:** SQLite (demo icin)
- **Paket Yoneticisi:** npm

## Proje Yapisi

```
backend/
  src/
    routes/
      search/       → GET endpoint'leri (listeleme, filtreleme)
      operational/   → POST, PUT, DELETE endpoint'leri
    services/
      search/       → SearchService (sadece okuma islemleri)
      operational/  → OperationalService (sadece CUD islemleri)
    models/         → TypeScript interface'ler ve DB baglantisi
    middleware/     → Express middleware'leri
    index.ts        → Express app baslatma
frontend/
  src/
    api/            → RTK Query baseApi ve feature API dosyalari
    features/       → Feature-based sayfa ve component'lar
    components/     → Paylasilan UI component'lari
    store/          → Redux store yapilandirmasi
```

## Mimari Kurallar

1. **Search/Operational Ayirimi:**
   - GET/filtreleme → SearchService + search route
   - Create/Update/Delete → OperationalService + operational route
   - Ayni service'te karistirma!

2. **Frontend:**
   - API cagrilari SADECE RTK Query ile (fetch/axios YASAK)
   - TypeScript strict mode, `any` tipi YASAK
   - MUI component'lari kullan
   - Tag-based cache invalidation ZORUNLU

3. **Backend:**
   - Her response: `{ success: boolean, data: T, message?: string }`
   - TypeScript interface'ler models/ altinda
   - Hata yonetimi: try-catch + error middleware

## Ilerleme Takibi (progress.md)

Her istek ve islem sonrasi `progress.md` dosyasini otomatik guncelle:
- Oturum basinda: yoksa olustur, varsa oku ve devam et
- Her adim sonrasi: yapilani yaz, dosya degisikliklerini not et
- 200 satiri gecme, eski tamamlanan adimlari kisalt
- Tum gorevler bitince progress.md'yi sil

## Calistirma

```bash
cd backend && npm install && npm run dev    # Port 3001
cd frontend && npm install && npm run dev   # Port 3000 (proxy → 3001)
```
