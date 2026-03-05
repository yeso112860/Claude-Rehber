---
name: endpoint
description: Verilen entity icin backend (Express route + service + model) ve frontend (RTK Query API + MUI sayfa) olusturur. Kullan: /endpoint User, /endpoint Product
argument-hint: EntityName
---

Verilen entity adi ($ARGUMENTS) icin tum katmanlari olustur.
Entity adi PascalCase kabul edilir (User, Product, Order vb.).
Dosya isimlerinde kullanilan format: PascalCase = `$ARGUMENTS`, camelCase = ilk harf kucuk.

## Backend (Node.js + Express + TypeScript):

1. `backend/src/models/$ARGUMENTS.ts`
   - Entity interface (id + alanlar)
   - Request/response interface'leri
   - `ApiResponse<T>` kullaniliyorsa buraya import et

2. `backend/src/services/search/$ARGUMENTSSearchService.ts`
   - Sadece: `findAll`, `findById`, `findByFilter`
   - Yazma metodu ekleme — YASAK

3. `backend/src/services/operational/$ARGUMENTSOperationalService.ts`
   - Sadece: `create`, `update`, `delete`
   - Okuma metodu ekleme — YASAK

4. `backend/src/routes/search/$ARGUMENTSSearchRoutes.ts`
   - Sadece GET endpoint'leri
   - Her route'da try-catch + `next(error)`

5. `backend/src/routes/operational/$ARGUMENTSOperationalRoutes.ts`
   - POST (201), PUT (200), DELETE (200)
   - Her route'da try-catch + `next(error)`

6. `backend/src/index.ts` guncelle:
   - Search route'u once, operational route sonra ekle:
   ```ts
   app.use('/api/<entity>s', $ARGUMENTSSearchRoutes);
   app.use('/api/<entity>s', $ARGUMENTSOperationalRoutes);
   ```

## Frontend (React 18 + RTK Query + MUI):

7. `frontend/src/api/$ARGUMENTSApi.ts`
   - `baseApi.injectEndpoints` ile genislet
   - Endpoint isimleri: `get$ARGUMENTS s`, `get$ARGUMENTSById`, `create$ARGUMENTS`, `update$ARGUMENTS`, `delete$ARGUMENTS`
   - Her endpoint'te `transformResponse: (res) => res.data`
   - providesTags + invalidatesTags zorunlu

8. `frontend/src/features/$ARGUMENTS/$ARGUMENTSListPage.tsx`
   - MUI DataGrid
   - isLoading → Skeleton, isError → Alert

9. `frontend/src/features/$ARGUMENTS/$ARGUMENTSForm.tsx`
   - MUI Dialog + TextField + Select
   - isLoading → CircularProgress

10. `frontend/src/App.tsx` guncelle:
    - ROUTES dizisine ekle:
    ```ts
    { path: '/<entity>s', element: <$ARGUMENTSListPage /> }
    ```
    - Sadece ROUTES dizisine dokun, baska degisiklik yapma

## Kontrol (tum dosyalar tamamlandiktan sonra):

```bash
cd backend && npx tsc --noEmit
cd frontend && npx tsc --noEmit
```

TypeScript hatasi varsa duzelt, sonra bittigini bildir.