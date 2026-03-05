# Canli Demo Rehberi

Sunumda bu adimlari takip et. Tahmini sure: 12-18 dakika.
Oncesinde `npm` ve `node` kurulu olmali.

---

## HAZIRLIK (Sunum oncesi)

```bash
cd demo-test
git init
git add -A
git commit -m "ilk commit: Claude Code yapilandirmasi"
```

> Bu adim ZORUNLU. Claude Code git reposu ister.
> Sunumda "CLAUDE.md, rules, agents ve skills hazir. Simdi Claude'a 4 farkli modulu urettirecegiz" de.

---

## ADIM 1: Claude Code Baslat ve Tanitim (2 dk)

```bash
cd demo-test
claude
```

Soyle:
```
Bu projenin CLAUDE.md ve modul kurallarini oku ve ozetle
```

> Claude 4 modulu ve farkli kurallarini ozetleyecek:
> - User: soft delete
> - Product: fiyat/stok validasyonu
> - Order: durum makinesi, silme yasak
> - Report: sadece okuma
>
> "Her modul icin farkli kurallar tanimladik, Claude hepsini biliyor" de.

---

## ADIM 2: Proje Iskeleti Olustur (3 dk)

Soyle:
```
Bu projenin backend ve frontend iskeletini olustur.
Backend: Express + TypeScript + better-sqlite3 (port 3001)
Frontend: React 18 + TypeScript + RTK Query + MUI (port 3000, Vite)
package.json, tsconfig.json, temel dosyalar ve konfigurasyonu yap.
Frontend'de baseApi.ts, store/index.ts, App.tsx, main.tsx olustur.
Backend'de index.ts, database.ts (4 tablo: users, products, orders, reports icin), error middleware olustur.
```

> Claude tum dosyalari olusturacak. "Tek komutla proje iskeleti hazir" de.

---

## ADIM 3: /endpoint User - Soft Delete (3 dk)

Soyle:
```
/endpoint User
```

> Claude modul-user.md kuralini okuyacak ve:
> - delete metodu gercekten silmeyecek, active=false yapacak
> - findAll varsayilan olarak sadece aktif kullanicilari getirecek
> - Frontend'de "Sil" yerine "Deaktif Et" yazacak
>
> "Ayni /endpoint komutu ama User kuralina gore soft delete uyguladi" de.

---

## ADIM 4: /endpoint Product - Validasyon (2 dk)

Soyle:
```
/endpoint Product
```

> Claude modul-product.md kuralini okuyacak ve:
> - Fiyat > 0 ve stok >= 0 validasyonu ekleyecek
> - Kategori enum (elektronik, giyim, gida...) ile sinirlandiracak
> - Frontend'de fiyat TL formatli, stok badge'leri olacak
>
> "Ayni skill, farkli kural, farkli cikti. Validasyon otomatik" de.

---

## ADIM 5: /endpoint Order - Durum Makinesi (3 dk) ⭐ En etkileyici

Soyle:
```
/endpoint Order
```

> Claude modul-order.md kuralini okuyacak ve:
> - DELETE endpoint OLUSTURMAYACAK (silme yasak!)
> - Durum makinesi: DRAFT→CONFIRMED→SHIPPED→DELIVERED
> - Gecersiz gecis kontrolu (DELIVERED→CONFIRMED engellenir)
> - Frontend'de her durum icin farkli renk Chip ve sadece gecerli butonlar
>
> "Ayni /endpoint komutu 3. kez ama tamamen farkli davranis.
>  Order'da delete yok, durum makinesi var. Kurallar belirliyor" de.

---

## ADIM 6: /endpoint Report - Sadece Okuma (2 dk)

Soyle:
```
/endpoint Report
```

> Claude modul-report.md kuralini okuyacak ve:
> - OperationalService OLUSTURMAYACAK (sadece okuma!)
> - POST/PUT/DELETE endpoint yok
> - Frontend'de Yeni Ekle/Sil butonu yok, sadece Card'lar ile istatistik
>
> "4. modul: tamamen farkli. CUD yok, sadece raporlama" de.

---

## ADIM 7: Calistir ve Goster (2 dk)

Soyle:
```
Backend ve frontend'i kur ve calistir
```

> Tarayicida localhost:3000 → 4 modullu calisan uygulama!
> "10 dakikada, sifirdan, 4 modullu uygulama. Her modul kendi kuralina uygun" de.

---

## ADIM 8: Diger Skill'ler - Endpoint Disinda (3 dk) ⭐ Cesitlilik

Skill'ler sadece CRUD degil. Her tekrarlayan pattern icin skill tanimlanabilir:

### 8a. Middleware olustur
```
/middleware error-handler
```
> Global error handling middleware uretir. Stack trace gizleme, hata loglama, tutarli response formati.

### 8b. Paylasilan component olustur
```
/component ConfirmDialog
```
> Silme/iptal icin kullanilacak onay dialog'u uretir. Props interface, MUI Dialog, severity renkleri.

### 8c. Utility olustur
```
/util currency-formatter frontend
```
> TL formatlama fonksiyonlari + test dosyasi uretir. Named export, JSDoc, pure function.

### 8d. Validasyonlu form olustur
```
/form ProductForm
```
> Zod schema (Turkce hata mesajlari) + React Hook Form + MUI form component uretir.

> "Endpoint disinda 4 skill daha var. Middleware, component, util, form...
>  Her tekrarlayan pattern bir skill olabilir" de.

---

## ADIM 9: Code Review - Modul Farki (2 dk)

Soyle:
```
@express-reviewer tum backend kodunu incele
```

> Agent modul kurallarini okuyup her modulu kendi kuralina gore inceleyecek:
> - User'da soft delete dogru uygulanmis mi?
> - Product'ta validasyon var mi?
> - Order'da delete endpoint var mi? (OLMAMALI)
> - Report'ta OperationalService var mi? (OLMAMALI)
>
> "Reviewer da modul kurallarini biliyor, her modulu farkli denetliyor" de.

---

## ALTERNATIF DEMO ADIMLARI (sure kalirsa)

### Hook Gosterimi
```
rm -rf /
# Hook engelleyecek: "TEHLIKELI KOMUT ENGELLENDI"
```

### Plan Mode
```
# Shift+Tab → Plan Mode
Bu projede hangi guvenlik aciklari olabilir?
# Claude analiz eder ama degistirmez
```

### React Reviewer
```
@react-reviewer frontend kodunu incele
# Order'da mutation button'lari dogru mu, Report'ta mutation yok mu kontrol eder
```

---

## ANAHTAR MESAJLAR

1. **"Ayni skill, farkli kurallar, farkli sonuc"**
   /endpoint 4 kez yazdin. Her seferinde farkli modul kurali devreye girdi.

2. **"Her tekrarlayan pattern bir skill olabilir"**
   CRUD, middleware, form, util, component... hepsi skill ile otomatik.

3. **"Kurallar kod uzerine degil, dosya yoluna bagli"**
   `modul-order.md` sadece order* dosyalarina uygulanir.

4. **"Review da kurallari biliyor"**
   @express-reviewer her modulun ozel kuralini okuyup ona gore denetliyor.

5. **"0 satir kod yazdik, 4 modul + altyapi cikti"**
   Sadece CLAUDE.md + rules + skills tanimladik, Claude kodu uretti.

---

## DOSYA YAPISI

```
demo-test/
├── CLAUDE.md                              ← Proje kurallari + 4 modul tablosu
├── DEMO-REHBER.md                         ← Bu dosya
└── .claude/
    ├── settings.json                      ← Izinler + tehlikeli komut hook'u
    ├── agents/
    │   ├── code-reviewer.md               ← Genel inceleme
    │   ├── express-reviewer.md            ← Backend (modul kurallarini biliyor)
    │   ├── react-reviewer.md              ← Frontend (modul kurallarini biliyor)
    │   ├── test-writer.md                 ← Test yazici
    │   └── security-checker.md            ← Guvenlik tarayici
    ├── rules/
    │   ├── search-controller.md           ← Search: sadece GET
    │   ├── operational-controller.md      ← Operational: sadece CUD
    │   ├── react-component.md             ← React kurallari
    │   ├── rtk-query.md                   ← RTK Query kurallari
    │   ├── modul-user.md                  ← User: soft delete
    │   ├── modul-product.md               ← Product: fiyat/stok validasyonu
    │   ├── modul-order.md                 ← Order: durum makinesi, silme yasak
    │   ├── modul-report.md                ← Report: sadece okuma, CUD yok
    │   ├── utils.md                       ← Utility: pure function, named export, test
    │   ├── middleware.md                  ← Middleware: next(), try-catch, sira
    │   ├── shared-component.md            ← Component: interface, PascalCase, barrel
    │   └── form-schema.md                 ← Form: Zod Turkce, React Hook Form, MUI
    └── skills/
        ├── endpoint/SKILL.md              ← /endpoint User → full-stack CRUD (modul kurallarini okuyor)
        ├── feature-ui/SKILL.md            ← /feature-ui → sadece frontend sayfasi
        ├── util/SKILL.md                  ← /util currency-formatter frontend → utility + test
        ├── form/SKILL.md                  ← /form ProductForm → Zod + React Hook Form + MUI
        ├── middleware/SKILL.md            ← /middleware auth → Express middleware
        └── component/SKILL.md             ← /component ConfirmDialog → paylasilan MUI component
```

> Kaynak kod YOK. 4 modulun tum kodu sunumda Claude tarafindan uretilecek.
