---
paths:
  - "frontend/src/api/**"
---

# RTK Query Kurallari

## Yapi
- `baseApi.ts` icinde `createApi` ile tek bir base tanimla
- Her entity icin ayri `<entity>Api.ts` dosyasi olustur
- `baseApi`'yi `injectEndpoints` ile genislet, yeni `createApi` **YASAK**

## Endpoint Isimlendirme
```ts
get{Entity}s        // GET /api/<entity>s
get{Entity}ById     // GET /api/<entity>s/:id
create{Entity}      // POST /api/<entity>s
update{Entity}      // PUT /api/<entity>s/:id
delete{Entity}      // DELETE /api/<entity>s/:id
```

## transformResponse — ZORUNLU
Backend `{ success, data, message }` donduruyor — her endpoint'te unwrap et:
```ts
transformResponse: (res: ApiResponse<T>) => res.data
```
`ApiResponse<T>` interface'i `models/` altinda tanimli olmali.

## Cache Invalidation — ZORUNLU
```ts
// Okuma endpoint'leri
providesTags: (result, error, id) => [{ type: 'Entity', id }]

// Yazma endpoint'leri
invalidatesTags: [{ type: 'Entity', id: 'LIST' }]
```
Tag tanimsiz endpoint **kabul edilmez**.

## Yasaklar
- Dogrudan `fetch` / `axios` cagrisi **YASAK**
- `any` tipi **YASAK**
- Response tipini `transformResponse` olmadan kullanma