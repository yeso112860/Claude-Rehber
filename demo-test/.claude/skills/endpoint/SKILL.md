---
name: endpoint
description: Verilen entity icin backend (Express route + service + model) ve frontend (RTK Query API + MUI sayfa) olusturur. Kullan: /endpoint User, /endpoint Product, /endpoint Order, /endpoint Report
argument-hint: EntityName
---

Verilen entity adi ($ARGUMENTS) icin tum katmanlari olustur.

ONEMLI: Once .claude/rules/ altinda bu entity icin modul kurali var mi kontrol et.
Varsa o kurallara KESINLIKLE uy. Yoksa standart pattern uygula.

## Backend (Node.js + Express + TypeScript):

1. `backend/src/models/<ad>.ts` - TypeScript interface'leri (entity + request/response + enum/tipler)
2. `backend/src/services/search/<ad>SearchService.ts` - SearchService (findAll, findById, findByFilter) - sadece okuma
3. `backend/src/services/operational/<ad>OperationalService.ts` - OperationalService (create, update, delete) - sadece CUD
4. `backend/src/routes/search/<ad>SearchRoutes.ts` - GET endpoint'leri
5. `backend/src/routes/operational/<ad>OperationalRoutes.ts` - POST, PUT, DELETE endpoint'leri

## Frontend (React 18 + RTK Query + MUI):

6. `frontend/src/api/<ad>Api.ts` - RTK Query injectEndpoints (query + mutation + tag invalidation)
7. `frontend/src/features/<ad>/<Ad>ListPage.tsx` - MUI DataGrid liste sayfasi
8. `frontend/src/features/<ad>/<Ad>Form.tsx` - MUI Dialog form (create/update)

## Modul Bazli Istisnalar:

- Modul kurali "SADECE OKUMA" diyorsa → OperationalService, OperationalRoutes OLUSTURMA, frontend'te mutation YOK
- Modul kurali "SILME YASAK" diyorsa → delete metodu ve DELETE endpoint OLUSTURMA
- Modul kurali "Soft Delete" diyorsa → delete yerine active=false yap
- Modul kurali "Durum Makinesi" tanimliyorsa → status gecis kontrolu ekle, gecerli/gecersiz gecisleri uygula
- Modul kurali validasyon tanimliyorsa → backend'te ve frontend form'da uygula

## Genel Kurallar:

- backend/src/index.ts'e yeni route'lari import edip ekle
- frontend/src/App.tsx'e navigation ekle (gerekiyorsa)
- TypeScript strict, `any` YASAK
- RTK Query tag-based cache invalidation ZORUNLU
- Response formati: { success: boolean, data: T, message?: string }
- SQLite (better-sqlite3) kullan
