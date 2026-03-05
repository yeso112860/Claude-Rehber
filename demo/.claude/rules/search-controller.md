---
paths:
  - "backend/src/routes/search/**"
  - "backend/src/services/search/**"
---

# Search Katmani Kurallari

## Kapsam
Sadece okuma islemleri: `findAll`, `findById`, `findByFilter`

## Yasaklar
- Yazma islemi (create, update, delete) **YASAK** — OperationalService'e ait
- POST, PUT, DELETE method **YASAK** — sadece GET kullan
- `any` tipi **YASAK** — TypeScript strict zorunlu

## Route Kurallari
- Sadece GET method kullan
- Route path'leri: `GET /api/<entity>s`, `GET /api/<entity>s/:id`, `GET /api/<entity>s/filter`
- Her route'da try-catch kullan, hatalar `next(error)` ile ilet

## Response Formati
```ts
// Basari
{ success: true, data: T, message?: string }

// Hata
{ success: false, message: string }
```
- HTTP status: basari 200, hata uygun kod (404, 500)