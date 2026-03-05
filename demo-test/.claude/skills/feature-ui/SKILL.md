---
name: feature-ui
description: Sadece frontend tarafi icin RTK Query API + MUI sayfa olusturur. Kullan: /feature-ui Dashboard, /feature-ui Settings
argument-hint: FeatureName
---

Verilen feature adi icin frontend dosyalarini olustur.

## Dosyalar:

1. `frontend/src/api/<feature>Api.ts` - RTK Query API (injectEndpoints + tag invalidation)
2. `frontend/src/features/<feature>/<Feature>ListPage.tsx` - MUI DataGrid liste
3. `frontend/src/features/<feature>/<Feature>Form.tsx` - MUI Dialog form

## Kurallar:

- RTK Query hook'lari kullan (fetch/axios YASAK)
- TypeScript strict, `any` YASAK
- MUI component'lari kullan
- baseApi'den injectEndpoints ile turet
- providesTags + invalidatesTags ZORUNLU
