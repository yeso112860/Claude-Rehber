---
paths:
  - "backend/src/routes/operational/**"
  - "backend/src/services/operational/**"
---

# Operational Katmani Kurallari

## Kapsam
Sadece CRUD islemleri: `create`, `update`, `delete`

## Yasaklar
- Okuma islemi (findAll, findById, findByFilter) **YASAK** — SearchService'e ait
- GET method **YASAK** — sadece POST, PUT, DELETE kullan
- `any` tipi **YASAK** — TypeScript strict zorunlu

## Route Kurallari
- POST → create (201 donus)
- PUT → update (200 donus)
- DELETE → delete (200 donus)
- Her route'da try-catch kullan, hatalar `next(error)` ile ilet

## Response Formati
```ts
// Create basari
{ success: true, data: T, message?: string }  // HTTP 201

// Update / Delete basari
{ success: true, data?: T, message: string }  // HTTP 200

// Hata (her islem)
{ success: false, message: string }           // HTTP 400 | 404 | 500
```