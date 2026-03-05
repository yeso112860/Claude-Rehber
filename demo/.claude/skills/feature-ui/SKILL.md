---
name: feature-ui
description: Sadece frontend tarafi icin RTK Query API + MUI sayfa olusturur. Backend endpoint'leri hazir oldugunda kullan. Kullan: /feature-ui Dashboard, /feature-ui Settings
argument-hint: FeatureName
---

Verilen feature adi ($ARGUMENTS) icin sadece frontend dosyalarini olustur.
Feature adi PascalCase kabul edilir. Dosya isimlerinde: PascalCase = `$ARGUMENTS`, path = kucuk harf.

## Dosyalar:

1. `frontend/src/api/$ARGUMENTSApi.ts`
   - `baseApi.injectEndpoints` ile genislet, yeni `createApi` YASAK
   - Endpoint isimleri: `get$ARGUMENTSs`, `get$ARGUMENTSById`, `create$ARGUMENTS`, `update$ARGUMENTS`, `delete$ARGUMENTS`
   - Her endpoint'te `transformResponse: (res: ApiResponse<T>) => res.data` ZORUNLU
   - providesTags + invalidatesTags ZORUNLU
   - Response tipi icin interface tanimla, `any` YASAK

2. `frontend/src/features/$ARGUMENTS/$ARGUMENTSListPage.tsx`
   - MUI DataGrid liste sayfasi
   - isLoading → `Skeleton`, isError → `MUI Alert` (backend message'i goster)
   - Hata pattern'i: `{isError && <Alert severity="error">{error?.data?.message ?? 'Bir hata olustu'}</Alert>}`

3. `frontend/src/features/$ARGUMENTS/$ARGUMENTSForm.tsx`
   - MUI Dialog + TextField + Select
   - isLoading → `CircularProgress`
   - Props icin interface tanimla

4. `frontend/src/App.tsx` guncelle:
   - Sadece ROUTES dizisine ekle:
   ```ts
   { path: '/<feature>s', element: <$ARGUMENTSListPage /> }
   ```
   - Baska hicbir yere dokunma

## Kurallar:

- API cagrisi SADECE RTK Query hook'lari ile (fetch / axios YASAK)
- TypeScript strict, `any` YASAK
- MUI component'lari kullan (ham HTML input YASAK)
- Manuel `useState` ile loading/error yonetimi YASAK — RTK Query flag'leri kullan

## Kontrol:

```bash
cd frontend && npx tsc --noEmit
```

Hata varsa duzelt, sonra bittigini bildir.