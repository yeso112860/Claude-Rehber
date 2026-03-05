---
paths:
  - "backend/src/**/product*"
  - "frontend/src/**/product*"
  - "frontend/src/**/Product*"
---

# Product Modulu Kurallari

## Fiyat Validasyonu ZORUNLU
- `price` alani ZORUNLU ve > 0 olmali
- Backend'de kontrol: `if (data.price <= 0) throw new Error("Fiyat sifirdan buyuk olmali")`
- Frontend form'da: min=0.01, step=0.01, type="number"
- Fiyat formatlamasi: TL cinsinden, 2 ondalik (toFixed(2))

## Stok Validasyonu ZORUNLU
- `stock` alani ZORUNLU ve >= 0 olmali (sifir olabilir = tukendi)
- Negatif stok ASLA kabul etme
- Backend'de kontrol: `if (data.stock < 0) throw new Error("Stok negatif olamaz")`

## Kategori
- Her urunun `category` alani ZORUNLU
- Gecerli kategoriler: 'elektronik' | 'giyim' | 'gida' | 'mobilya' | 'diger'
- Frontend'de Select component ile sectirilsin (serbest metin YASAK)

## SearchService Ek Filtreleri
- Fiyat araligi: `?minPrice=10&maxPrice=100`
- Kategoriye gore: `?category=elektronik`
- Stok durumu: `?inStock=true` (stock > 0 olanlari getir)

## Frontend
- Fiyat kolonu sag hizali, TL sembollu gosterilsin
- Stok 0 olan urunler kirmizi "Tukendi" badge gostersin
- Stok < 5 olan urunler sari "Az Kaldi" badge gostersin
