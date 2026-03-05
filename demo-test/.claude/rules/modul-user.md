---
paths:
  - "backend/src/**/user*"
  - "frontend/src/**/user*"
  - "frontend/src/**/User*"
---

# User Modulu Kurallari

## Soft Delete ZORUNLU
- Kullaniciyi veritabanindan SILME (DELETE FROM YASAK)
- Bunun yerine `active = false` yap
- OperationalService.delete() metodu aslinda soft delete yapmali:
  ```typescript
  // YANLIS:
  db.prepare('DELETE FROM users WHERE id = ?').run(id);
  // DOGRU:
  db.prepare('UPDATE users SET active = false, updated_at = datetime("now") WHERE id = ?').run(id);
  ```
- DELETE endpoint response: `{ success: true, message: "Kullanici deaktif edildi" }`

## SearchService
- findAll: varsayilan olarak SADECE aktif kullanicilari getir (WHERE active = true)
- Tum kullanicilari gormek icin `?includeInactive=true` parametresi destekle

## Roller
- Gecerli roller: 'admin' | 'user' | 'editor'
- Rol degisikligi sadece admin yapabilir (yorum olarak belirt)

## Frontend
- Pasif kullanicilar listede gri renkte gosterilsin
- Sil butonu yerine "Deaktif Et" yazsin
- Onay dialog'u: "Bu kullaniciyi deaktif etmek istediginize emin misiniz?"
