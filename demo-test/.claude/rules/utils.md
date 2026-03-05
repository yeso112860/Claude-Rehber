---
paths:
  - "backend/src/utils/**"
  - "frontend/src/utils/**"
---

# Utility Dosyasi Kurallari

- Default export YASAK, sadece named export kullan
- Her fonksiyon icin JSDoc aciklama ZORUNLU
- Pure function olsun (side effect yok, dis state degistirme)
- Parametre ve donus tipleri acikca yazilmali
- `any` tipi YASAK
- Console.log YASAK
- Hata mesajlari Turkce
- Her util dosyasi icin test dosyasi ZORUNLU: `<ad>.test.ts`
