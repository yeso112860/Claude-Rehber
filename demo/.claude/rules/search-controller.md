---
paths:
  - "backend/src/routes/search/**"
  - "backend/src/services/search/**"
---

# Search Katmani Kurallari

- SADECE okuma islemleri: findAll, findById, findByFilter
- Yazma islemi (create, update, delete) YASAK
- Route'larda sadece GET method kullan
- Response: { success: true, data: T }
- Hata durumunda: { success: false, message: "..." }
