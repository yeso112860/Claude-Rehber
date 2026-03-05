---
paths:
  - "backend/src/**/order*"
  - "frontend/src/**/order*"
  - "frontend/src/**/Order*"
---

# Order Modulu Kurallari

## SILME YASAK
- Order ASLA silinemez (DELETE endpoint YOK)
- OperationalService'te delete metodu OLMAYACAK
- Bunun yerine "iptal" islemi var: status → CANCELLED

## Durum Makinesi (State Machine) ZORUNLU

Gecerli gecisler:
```
DRAFT → CONFIRMED      (onayla)
DRAFT → CANCELLED      (iptal)
CONFIRMED → SHIPPED    (kargoya ver)
CONFIRMED → CANCELLED  (iptal)
SHIPPED → DELIVERED     (teslim edildi)
```

Gecersiz gecisler (HATA firlatmali):
- DELIVERED → herhangi bir sey (teslim edilen degismez)
- CANCELLED → herhangi bir sey (iptal edilen degismez)
- SHIPPED → CANCELLED (kargodayken iptal edilemez)
- DRAFT → SHIPPED (onaylanmadan kargoya verilmez)
- DRAFT → DELIVERED (onaylanmadan teslim edilmez)

## Backend
- OperationalService'te SADECE `create` ve `updateStatus` metodlari olsun
- `updateStatus(id, newStatus)` metodu gecerli gecisi kontrol etsin
- Gecersiz geciste hata: `"Gecersiz durum gecisi: ${current} → ${newStatus}"`
- Create'te varsayilan status: DRAFT

## Enum Tanimi
```typescript
type OrderStatus = 'DRAFT' | 'CONFIRMED' | 'SHIPPED' | 'DELIVERED' | 'CANCELLED';
```

## Frontend
- Durum gecisleri icin butonlar goster (sadece gecerli gecisler)
- DRAFT: "Onayla" ve "Iptal Et" butonlari
- CONFIRMED: "Kargoya Ver" ve "Iptal Et" butonlari
- SHIPPED: "Teslim Edildi" butonu
- DELIVERED/CANCELLED: buton yok (son durum)
- Her durum icin farkli renk Chip:
  - DRAFT: default (gri)
  - CONFIRMED: info (mavi)
  - SHIPPED: warning (sari)
  - DELIVERED: success (yesil)
  - CANCELLED: error (kirmizi)
