---
name: endpoint
description: Verilen entity icin backend (Express route + service + model) ve frontend (RTK Query API + MUI sayfa) olusturur. Kullan: /endpoint User, /endpoint Product
argument-hint: EntityName
---

Verilen entity adi ($ARGUMENTS) icin tum katmanlari olustur.

## Backend (Node.js + Express + TypeScript):

1. `backend/src/models/<ad>.ts` - TypeScript interface'leri (entity + request/response)
2. `backend/src/services/search/<ad>SearchService.ts` - SearchService (findAll, findById, findByFilter) - sadece okuma
3. `backend/src/services/operational/<ad>OperationalService.ts` - OperationalService (create, update, delete) - sadece CUD
4. `backend/src/routes/search/<ad>SearchRoutes.ts` - GET endpoint'leri (@GetMapping karsiligi)
5. `backend/src/routes/operational/<ad>OperationalRoutes.ts` - POST, PUT, DELETE endpoint'leri

## Frontend (React 18 + RTK Query + MUI):

6. `frontend/src/api/<ad>Api.ts` - RTK Query injectEndpoints (getAll, getById, create, update, delete + tag invalidation)
7. `frontend/src/features/<ad>/<Ad>ListPage.tsx` - MUI DataGrid liste sayfasi
8. `frontend/src/features/<ad>/<Ad>Form.tsx` - MUI Dialog form (create/update)

## Kurallar:

- backend/src/index.ts'e yeni route'lari import edip ekle
- frontend/src/App.tsx'e navigation ekle (gerekiyorsa)
- TypeScript strict, `any` YASAK
- RTK Query tag-based cache invalidation ZORUNLU
- Response formati: { success: boolean, data: T, message?: string }
- SearchService: sadece okuma (findAll, findById, findByFilter)
- OperationalService: sadece CUD (create, update, delete)
- SQLite (better-sqlite3) kullan

## Sonra:

- `cd backend && npx tsc --noEmit` ile TypeScript kontrolu yap
- `cd frontend && npx tsc --noEmit` ile TypeScript kontrolu yap
