---
paths:
  - "backend/src/routes/operational/**"
  - "backend/src/services/operational/**"
---

# Operational Katmani Kurallari

- SADECE CUD islemleri: create, update, delete
- Okuma islemi (findAll, findById, filter) YASAK - SearchService'e ait
- Route'larda sadece POST, PUT, DELETE method kullan
- GET method YASAK
- Create response: 201 status + { success: true, data: T }
- Update/Delete response: { success: true, data/message }
