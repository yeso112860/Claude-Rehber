---
name: endpoint
description: "Var olan entity için CQRS endpoint seti oluşturur. Kullanım: /endpoint Basvuru bsv"
user_invocable: true
---

Verilen entity ve modül için CQRS endpoint seti oluştur:

## Application Katmanı

**Queries:**
- `List{EntityAdi}Query + Handler` → GET /api/{modul}/{kaynak} (sayfalı)
- `Get{EntityAdi}ByIdQuery + Handler` → GET /api/{modul}/{kaynak}/{id}

**Commands:**
- `Create{EntityAdi}Command + Handler` → POST /api/{modul}/{kaynak}
- `Update{EntityAdi}Command + Handler` → PUT /api/{modul}/{kaynak}/{id}
- `Delete{EntityAdi}Command + Handler` → DELETE /api/{modul}/{kaynak}/{id}

**Validators:**
- `Create{EntityAdi}CommandValidator`
- `Update{EntityAdi}CommandValidator`

**DTO:**
- `{EntityAdi}Dto` (record) - dönüş tipi
- `Create{EntityAdi}Request` - oluşturma input
- `Update{EntityAdi}Request` - güncelleme input

## API Katmanı

Controller action'ları api.md kurallarına uy:
- `[Authorize(Roles = "...")]` aktörlere göre
- `CancellationToken` zorunlu
- `ApiResponse<T>` response format

## Angular Katmanı

Service metodları frontend.md kurallarına uy:
- `getAll(params?)`, `getById(id)`, `create(request)`, `update(id, request)`, `delete(id)`
- inject(), HttpClient, Observable dönüş
