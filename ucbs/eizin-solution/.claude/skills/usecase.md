---
name: usecase
description: "UseCase.md'den belirli bir UC kodunu okur ve tam stack kod üretir. Kullanım: /usecase UC-BSV-001"
user_invocable: true
---

Verilen Use Case kodunu (örnek: UC-BSV-001) aşağıdaki adımlarla işle:

## ADIM 0: Başlangıç

`progress.md` dosyasını oluştur:
```
# Görev: {UC_KODU} - {UC_ADI}
Başlangıç: {TIMESTAMP}

## Yapılacaklar
- [ ] UseCase.md analizi
- [ ] Domain Entity
- [ ] Application Command/Query + Handler
- [ ] Application DTO + Validator
- [ ] Infrastructure Repository + EF Config
- [ ] API Controller
- [ ] EF Core Migration
- [ ] Angular Model + Service
- [ ] Angular Component (List + Form)
- [ ] progress.md temizle
```

## ADIM 1: UseCase.md Analizi

`/Users/karademir/ornek-calisma/ucbs/UseCase.md` dosyasından {UC_KODU} bölümünü oku ve çıkar:
- Aktörler ve rolleri
- Ön koşullar
- Temel akış adımları
- Alternatif ve istisna akışları
- İş kuralları (BR-XXX-NNN)
- Bağımlı use case'ler

Modül kodunu tespit et (UC-{MODUL}-{NNN}).

## ADIM 2: Domain Katmanı

**Entity**: `eIzin.Domain/Entities/{ModulKodu}/{EntityAdi}.cs`
- BaseEntity<Guid> türevi, private set, factory method `Olustur(...)`, domain behavior metodları

**Enum** (gerekiyorsa): `eIzin.Domain/Enums/{EnumAdi}.cs`

**Domain Exception** (gerekiyorsa): `eIzin.Domain/Exceptions/{ModulKodu}Errors.cs`

## ADIM 3: Application Katmanı

**DTO**: `eIzin.Application/DTOs/{ModulKodu}/{EntityAdi}Dto.cs`
- `{EntityAdi}Dto` (record), `Create{EntityAdi}Request`, `Update{EntityAdi}Request`

**Commands**: `eIzin.Application/Commands/{ModulKodu}/`
- `Create{EntityAdi}Command.cs` + `Create{EntityAdi}CommandHandler.cs`
- `Update{EntityAdi}Command.cs` + Handler (gerekiyorsa)
- `Delete{EntityAdi}Command.cs` + Handler (gerekiyorsa)

**Queries**: `eIzin.Application/Queries/{ModulKodu}/`
- `List{EntityAdi}Query.cs` + Handler (sayfalı, filtrelenebilir)
- `Get{EntityAdi}ByIdQuery.cs` + Handler

**Validators**: `eIzin.Application/Validators/{ModulKodu}/`
- UseCase.md iş kurallarına göre FluentValidation

**AutoMapper**: `eIzin.Application/Mappings/{ModulKodu}MappingProfile.cs`

**Repository Interface**: `eIzin.Application/Interfaces/I{EntityAdi}Repository.cs`

## ADIM 4: Infrastructure Katmanı

**Repository**: `eIzin.Infrastructure/Persistence/Repositories/{EntityAdi}Repository.cs`

**EF Config**: `eIzin.Infrastructure/Persistence/Configurations/{EntityAdi}Configuration.cs`
- `ToTable("{CogulAdi}", "{modülkodu}")` - schema modül kodu

**DbContext**: `eIzin.Infrastructure/Persistence/EIzinDbContext.cs`'e `DbSet<{EntityAdi}>` ekle

**Migration**: `dotnet ef migrations add Add_{ModulKodu}_{EntityAdi}Table --project eIzin.Infrastructure --startup-project eIzin.API`

## ADIM 5: API Katmanı

**Controller**: `eIzin.API/Controllers/{ModulKodu}Controller.cs`
- Var olan controller'a action ekle veya yeni oluştur
- UseCase aktörlerine göre `[Authorize(Roles = "...")]`
- MediatR pattern, CancellationToken zorunlu

## ADIM 6: Angular Katmanı

**Model**: `eIzin.Angular/src/app/features/{modülkodu}/models/{entity-adi}.model.ts`

**Service**: `eIzin.Angular/src/app/features/{modülkodu}/services/{entity-adi}.service.ts`
- inject(), HttpClient, Observable dönüş

**List Component**: `eIzin.Angular/src/app/features/{modülkodu}/components/{entity-adi}-list/`
- PrimeNG p-table, Signals, Loading/Error/Empty state

**Form Component**: `eIzin.Angular/src/app/features/{modülkodu}/components/{entity-adi}-form/`
- Reactive Form, PrimeNG, UseCase validation kuralları

**Route**: `eIzin.Angular/src/app/features/{modülkodu}/{modülkodu}.routes.ts`

## ADIM 7: DI Kayıt

`eIzin.API/Extensions/` altında: `services.AddScoped<I{EntityAdi}Repository, {EntityAdi}Repository>();`

## ADIM 8: Tamamlama

1. progress.md güncelle: tüm görevler tamamlandı
2. Eksik bağlantı/DI kaydı varsa belirt
3. progress.md sil
4. Özet: kaç dosya oluşturuldu, hangi endpoint'ler eklendi
