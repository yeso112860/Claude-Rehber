# Siparis Yonetim Sistemi

Node.js (Express + TypeScript) + React 18 monorepo.
4 modullu demo proje.

## Teknoloji Stack

- **Backend:** Node.js 20, Express 4, TypeScript, better-sqlite3
- **Frontend:** React 18, TypeScript (strict), RTK Query, MUI 5
- **Veritabani:** SQLite (demo icin)
- **Paket Yoneticisi:** npm

## Moduller

| Modul | Aciklama | Ozel Kural |
|-------|----------|------------|
| **user** | Kullanici yonetimi | Soft delete (gercekten silme, active=false yap) |
| **product** | Urun katalogu | Fiyat > 0 ve stok >= 0 validasyonu ZORUNLU |
| **order** | Siparis yonetimi | Durum makinesi: DRAFT→CONFIRMED→SHIPPED→DELIVERED. Silme YASAK, sadece iptal (CANCELLED) |
| **report** | Raporlama | SADECE okuma. OperationalService YOK, create/update/delete YOK |

## Proje Yapisi

```
backend/
  src/
    routes/
      search/       → GET endpoint'leri (listeleme, filtreleme)
      operational/   → POST, PUT, DELETE endpoint'leri
    services/
      search/       → SearchService (sadece okuma islemleri)
      operational/  → OperationalService (sadece CUD islemleri)
    models/         → TypeScript interface'ler ve DB baglantisi
    middleware/     → Express middleware'leri
    utils/          → Yardimci fonksiyonlar
    index.ts        → Express app baslatma
frontend/
  src/
    api/            → RTK Query baseApi ve feature API dosyalari
    features/       → Feature-based sayfa ve component'lar
    components/     → Paylasilan UI component'lari
    store/          → Redux store yapilandirmasi
    utils/          → Frontend yardimci fonksiyonlar
```

## Mimari Kurallar

1. **Search/Operational Ayirimi:**
   - GET/filtreleme → SearchService + search route
   - Create/Update/Delete → OperationalService + operational route
   - Ayni service'te karistirma!

2. **Frontend:**
   - API cagrilari SADECE RTK Query ile (fetch/axios YASAK)
   - TypeScript strict mode, `any` tipi YASAK
   - MUI component'lari kullan
   - Tag-based cache invalidation ZORUNLU
   - Form validasyonu: Zod + React Hook Form (useState ile form YASAK)

3. **Backend:**
   - Her response: `{ success: boolean, data: T, message?: string }`
   - TypeScript interface'ler models/ altinda
   - Hata yonetimi: try-catch + error middleware
   - Utility fonksiyonlar: pure function, named export, JSDoc

4. **Paylasilan Component'lar:**
   - Props icin interface tanimla ve export et
   - components/index.ts'den barrel export
   - MUI sx prop kullan (inline style YASAK)

## Ilerleme Takibi (progress.md) - OTOMATIK

`progress.md` dosyasi projenin canli durum takip dosyasidir.
Her istek ve islem sonrasinda OTOMATIK guncellenir.

### Kurallar:

1. **Oturum basinda**: progress.md yoksa OLUSTUR, varsa OKU ve devam et
2. **Her istek sonrasi**: Yapilan islemi progress.md'ye ekle
3. **Her dosya olusturma/duzenleme sonrasi**: Hangi dosya degisti yaz
4. **Hata olustugunda**: Hatayi ve cozumu not et
5. **Gorev tamamlandiginda**: Gorevi [x] olarak isaretle
6. **200 SATIR SINIRI**: progress.md 200 satiri GECMEMELI. Yaklasinca eski tamamlanmis adimlari ozetle ve kisalt
7. **Oturum bittiginde veya tum gorevler bittiginde**: progress.md dosyasini SIL

### Format:

```markdown
# Proje Ilerleme Durumu
Son guncelleme: [tarih saat]

## Aktif Gorev: [gorev adi]
- [x] Tamamlanan adim
- [x] Tamamlanan adim
- [ ] Su an yapilan adim  ← BURADA
- [ ] Bekleyen adim

## Tamamlanan Gorevler
- ✅ Gorev 1 (kisa ozet)
- ✅ Gorev 2 (kisa ozet)

## Olusturulan/Degistirilen Dosyalar
- backend/src/models/user.ts (olusturuldu)
- frontend/src/api/userApi.ts (olusturuldu)
- backend/src/index.ts (guncellendi - user route eklendi)

## Notlar
- Onemli kararlar veya karsilasilan sorunlar
```

### Neden otomatik?
- Context window dolup /compact olursa → progress.md sayesinde nerede kaldigini bilirsin
- Oturum kopup /resume ile devam edersen → progress.md'den okuyup devam edersin
- Baska bir kisi projeye baktiginda → progress.md'den son durumu gorur

## Calistirma

```bash
cd backend && npm install && npm run dev    # Port 3001
cd frontend && npm install && npm run dev   # Port 3000 (proxy → 3001)
```
