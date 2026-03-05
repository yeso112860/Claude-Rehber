# Kullanici Yonetim Sistemi

Node.js (Express + TypeScript) + React 18 monorepo.

## Teknoloji Stack

- **Backend:** Node.js 20, Express 4, TypeScript (strict), better-sqlite3
- **Frontend:** React 18, TypeScript (strict), RTK Query, MUI 5
- **Veritabani:** SQLite (demo icin)
- **Paket Yoneticisi:** npm

## Proje Yapisi

```
backend/
  src/
    routes/
      search/         → GET endpoint'leri (listeleme, filtreleme)
      operational/    → POST, PUT, DELETE endpoint'leri
    services/
      search/         → SearchService (sadece okuma islemleri)
      operational/    → OperationalService (sadece CUD islemleri)
    models/           → TypeScript interface'ler (entity + request/response)
    middleware/
      errorMiddleware.ts  → Merkezi hata yakalama (Express error handler)
    index.ts          → Express app, route registration (search once, operational sonra)
frontend/
  src/
    api/
      baseApi.ts      → RTK Query createApi tanimi
      <entity>Api.ts  → Her entity icin injectEndpoints
    features/
      <entity>/
        <Entity>ListPage.tsx  → MUI DataGrid liste sayfasi
        <Entity>Form.tsx      → MUI Dialog form (create/update)
    components/       → Paylasilan UI component'lari
    store/            → Redux store yapilandirmasi
    App.tsx           → Route tanimlari (ROUTES dizisi, asagiya bak)
```

## Mimari Kurallar

### 1. Search / Operational Ayirimi
- GET / filtreleme → SearchService + search route
- Create / Update / Delete → OperationalService + operational route
- Ayni service'te **karistirma yasak**

### 2. TypeScript
- Her iki tarafta da **strict mode zorunlu**
- `any` tipi **kesinlikle yasak** — backend dahil (models, services, routes)
- Entity interface'leri `backend/src/models/<entity>.ts` altinda tanimla

### 3. Response Formati (tutarli olmasi zorunlu)
```ts
// Basari
{ success: true,  data: T,    message?: string }
// Hata
{ success: false, data?: never, message: string }
```
- Search GET: 200
- Operational Create: **201**
- Operational Update/Delete: 200
- Hata: uygun HTTP status kodu (400, 404, 500)

### 4. Hata Yonetimi — Backend
- Her route'da try-catch kullan
- Hatalar `next(error)` ile `errorMiddleware.ts`'e ilet
- `errorMiddleware.ts` imzasi:
```ts
(err: Error, req: Request, res: Response, next: NextFunction) => void
```

### 5. Route Registration — index.ts
- Route'lar dogrudan `index.ts`'de import edilir, aggregator dosyasi yok
- Siralama: search route'lari once, operational route'lar sonra
- Ornek:
```ts
app.use('/api/users', userSearchRoutes);
app.use('/api/users', userOperationalRoutes);
```

### 6. Frontend — RTK Query
- API cagrisi **sadece RTK Query hook'lari** ile (fetch / axios yasak)
- Her feature icin ayri `<entity>Api.ts` dosyasi, `baseApi`'yi `injectEndpoints` ile genislet
- Tag-based cache invalidation **zorunlu** (providesTags + invalidatesTags)
- Endpoint isimlendirme: `get{Entity}s`, `get{Entity}ById`, `create{Entity}`, `update{Entity}`, `delete{Entity}`
- `transformResponse` ile backend response'unu unwrap et:
```ts
transformResponse: (res: ApiResponse<T>) => res.data
```

### 7. Frontend — Navigation (App.tsx)
- Route'lar `App.tsx` icindeki `ROUTES` dizisinden uretilir:
```ts
const ROUTES = [
  { path: '/users', element: <UserListPage /> },
  // yeni feature buraya eklenir
];
```
- Yeni feature eklendiginde ROUTES dizisine ekleme yap, baska yere dokunma

### 8. MUI Kullanim Kurallari
- Liste: `MUI DataGrid`
- Form: `MUI TextField`, `Select`, `Dialog`
- Loading: `MUI CircularProgress` veya `Skeleton`
- Error: `MUI Alert`
- Class component **yasak**, sadece function component

## Calistirma

```bash
cd backend && npm install && npm run dev    # Port 3001
cd frontend && npm install && npm run dev   # Port 3000 (proxy → 3001)
```

## TypeScript Kontrol (her degisiklik sonrasi)

```bash
cd backend && npx tsc --noEmit
cd frontend && npx tsc --noEmit
```