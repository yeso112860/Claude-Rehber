---
description: ".NET Core backend kuralları - Clean Architecture, C# kodlama standartları"
paths:
  - "**/*.cs"
---

# .NET Core Backend Kuralları

## Clean Architecture - Katman Sorumlulukları

### eIzin.Domain
- `Entities/{ModulKodu}/` - BaseEntity<TId> türeyen iş entity'leri
- `ValueObjects/` - Immutable değer nesneleri (TcKimlik, IzinNo vb.)
- `Enums/` - BasvuruDurumu, EkKapsami, IzinKonusu, DegerlendirmeSonucu vb.
- `Exceptions/` - DomainException türevleri
- `Events/` - IDomainEvent implementasyonları
- `Interfaces/` - YALNIZCA domain interface'leri (IRepository<T> gibi generics)

Domain'de OLMAMASI gerekenler: EF Core referansı, dış servis bağımlılığı

### eIzin.Application
- `Commands/{ModulKodu}/` - CreateXxxCommand + Handler, UpdateXxxCommand + Handler, DeleteXxxCommand + Handler
- `Queries/{ModulKodu}/` - GetXxxQuery + Handler, ListXxxQuery + Handler
- `DTOs/{ModulKodu}/` - XxxDto, XxxRequest, XxxResponse
- `Validators/` - FluentValidation sınıfları
- `Interfaces/` - IXxxRepository, IXxxService
- `Mappings/` - AutoMapper profilleri
- `Common/` - Pagination<T>, ApiResponse<T>, Result<T>

Application'da OLMAMASI gerekenler: EF Core DbContext, HTTP Client, Controller kodu

### eIzin.Infrastructure
- `Persistence/EIzinDbContext.cs` - Ana DbContext
- `Persistence/Configurations/` - IEntityTypeConfiguration<T>
- `Persistence/Migrations/` - EF Core migration
- `Persistence/Repositories/` - Repository implementasyonları
- `ExternalServices/` - EYeterlikService, BelgenetService, TobbTeskService, ECedService, DonerSermayeService
- `Identity/` - JWT token yönetimi
- `Services/` - NotificationService, DocumentService vb.

### eIzin.API
- `Controllers/` - Modül bazlı (SysController, BsvController vb.)
- `Middleware/` - ExceptionHandlingMiddleware, AuditLoggingMiddleware
- `Filters/` - Authorization, Validation filter'ları
- `Extensions/` - DI registration

## Kodlama Standartları

- C# 12: primary constructors, collection expressions
- `async`/`await` tüm I/O işlemlerinde ZORUNLU
- Constructor injection (field injection YASAK)
- `var` kullan - tür açık olduğunda
- `sealed` sınıflar için sealed keyword
- `record` tipler DTO ve ValueObject için tercih edilir
- Nullable reference types aktif: `#nullable enable`

## Namespace Yapısı

```
EIzin.Domain.Entities.Sys
EIzin.Domain.Entities.Bsv
EIzin.Application.Commands.Bsv
EIzin.Application.Queries.Sys
EIzin.Application.DTOs.Bsv
EIzin.Infrastructure.Persistence.Repositories
EIzin.API.Controllers
```

## Entity Yapısı

```csharp
public sealed class Basvuru : BaseEntity<Guid>
{
    public Guid TesisId { get; private set; }
    public EkKapsami EkKapsami { get; private set; }
    public BasvuruDurumu Durum { get; private set; }

    private Basvuru() { }

    public static Basvuru Olustur(Guid tesisId, EkKapsami ekKapsami)
    {
        return new Basvuru { TesisId = tesisId, EkKapsami = ekKapsami, Durum = BasvuruDurumu.Taslak };
    }

    public void DegerlendirmeyeAl(Guid degerlendirenId) { /* domain logic */ }
}
```

## Command/Query Handler Yapısı

```csharp
public sealed class CreateBasvuruCommandHandler
    : IRequestHandler<CreateBasvuruCommand, Result<BasvuruDto>>
{
    private readonly IBasvuruRepository _repository;
    private readonly IUnitOfWork _unitOfWork;

    public async Task<Result<BasvuruDto>> Handle(
        CreateBasvuruCommand request, CancellationToken cancellationToken)
    {
        // Business rule → Entity factory → Repository.Add → UnitOfWork.SaveChanges → Result.Success
    }
}
```

## Result Pattern

```csharp
return Result<BasvuruDto>.Success(dto);
return Result<BasvuruDto>.Failure("Başvuru bulunamadı");
return Result<BasvuruDto>.Failure(DomainErrors.Basvuru.NotFound);
```

## FluentValidation

```csharp
public sealed class CreateBasvuruCommandValidator : AbstractValidator<CreateBasvuruCommand>
{
    public CreateBasvuruCommandValidator()
    {
        RuleFor(x => x.TesisId).NotEmpty();
        RuleFor(x => x.EkKapsami).IsInEnum();
    }
}
```

## Logging

- `ILogger<T>` constructor injection
- Structured logging: `_logger.LogInformation("Başvuru oluşturuldu. {BasvuruId}", id)`
- Hassas veri maskeleme (TC Kimlik): `{TcKimlik:mask}`
