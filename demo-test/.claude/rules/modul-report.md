---
paths:
  - "backend/src/**/report*"
  - "frontend/src/**/report*"
  - "frontend/src/**/Report*"
---

# Report Modulu Kurallari

## SADECE OKUMA - CUD ISLEMLERI YASAK
- OperationalService OLMAYACAK (dosya olusturma)
- OperationalRoutes OLMAYACAK (dosya olusturma)
- POST, PUT, DELETE endpoint'leri YASAK
- Sadece SearchService ve SearchRoutes olacak
- Frontend'de "Yeni Ekle", "Duzenle", "Sil" butonlari OLMAYACAK

## Backend Endpoint'leri (sadece GET)

1. `GET /api/reports/summary` - Genel ozet
   - Toplam kullanici sayisi
   - Toplam urun sayisi
   - Toplam siparis sayisi
   - Durum bazli siparis dagilimi

2. `GET /api/reports/orders-by-status` - Siparis durum dagilimi
   - Her OrderStatus icin adet

3. `GET /api/reports/products-by-category` - Urun kategori dagilimi
   - Her kategori icin adet ve toplam stok

4. `GET /api/reports/low-stock` - Dusuk stoklu urunler
   - stock < 5 olan urunler listesi

## Frontend
- MUI Card'lar ile istatistik gosterimi (DataGrid DEGIL)
- Sayi kartlari: toplam kullanici, urun, siparis
- Basit bar/pie chart yapisi (MUI Box + inline style ile basit gosterim yeterli)
- Tablo sadece low-stock listesi icin

## RTK Query
- Sadece query endpoint'leri (mutation YOK)
- providesTags: ['Report']
- Otomatik yenileme: pollingInterval 30 saniye (refetchOnFocus: true)
