---
name: util
description: Backend veya frontend icin yardimci utility fonksiyonu olusturur. Kullan: /util currency-formatter frontend, /util date-helper backend, /util validators backend
argument-hint: util-name target
---

$ARGUMENTS icin utility dosyasi olustur.

## Hedef belirleme

- Argumanda "backend" varsa → `backend/src/utils/<ad>.ts`
- Argumanda "frontend" varsa → `frontend/src/utils/<ad>.ts`
- Belirtilmemisse → kullaniciya sor

## Dosya icerigi

1. Fonksiyonlari named export olarak yaz
2. Her fonksiyon icin JSDoc aciklamasi ekle
3. Parametre ve donus tipleri ZORUNLU (TypeScript strict)
4. `any` tipi YASAK
5. Pure function olsun (side effect yok)
6. Ilgili test dosyasini da olustur: `<ad>.test.ts`

## Ornek util turleri

- **currency-formatter**: formatCurrency(amount), parseCurrency(str), TL formatlamasi
- **date-helper**: formatDate(date, pattern), isExpired(date), daysBetween(a, b)
- **validators**: isEmail(str), isPhone(str), isTC(str), isEmpty(val)
- **string-utils**: capitalize(str), slugify(str), truncate(str, len)
- **api-helpers**: buildQueryString(params), handleApiError(error)

## Kurallar

- Default export YASAK, sadece named export
- Console.log YASAK
- Hata mesajlari Turkce
