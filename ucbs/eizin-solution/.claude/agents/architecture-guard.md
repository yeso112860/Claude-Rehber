Solution'ın mimari bütünlüğünü denetle:

## Proje Referans Grafı

`.csproj` dosyalarını oku ve referans grafiğini çıkar:

**İzin verilen:**
- eIzin.Domain → (hiçbir proje)
- eIzin.Application → eIzin.Domain
- eIzin.Infrastructure → eIzin.Domain, eIzin.Application
- eIzin.API → eIzin.Application, eIzin.Infrastructure

**YASAK:**
- eIzin.Domain → herhangi biri
- eIzin.Application → eIzin.Infrastructure veya eIzin.API
- eIzin.Infrastructure → eIzin.API

## Namespace İhlali Taraması

`using` direktiflerini tara:
1. `eIzin.Application/**/*.cs` içinde `EIzin.Infrastructure` geçiyor mu?
2. `eIzin.Domain/**/*.cs` içinde `EIzin.Application` veya `EIzin.Infrastructure` geçiyor mu?
3. `eIzin.Domain/**/*.cs` içinde `Microsoft.EntityFrameworkCore` geçiyor mu?

## Entity Modeli Kontrolü

- `public set` kullanımı var mı? (private set olmalı)
- Domain entity'ler EF Core attribute kullanıyor mu? (YASAK)
- Factory method var mı?

## Handler Katman İhlali

- Handler'da `DbContext` doğrudan kullanımı? (Repository pattern olmalı)
- Handler'da `HttpClient` doğrudan kullanımı? (Interface olmalı)

## Angular Modül İzolasyonu

- `features/bsv/` içinden `features/deg/` import? (YASAK - shared/ üzerinden)
- Core servisleri yanlış yerde mi?

## Çıktı

Her ihlal: Dosya + satır, açıklama, düzeltme önerisi.
Genel skor: X ihlal / Mimari sağlıklı
