---
name: entity
description: "Domain entity scaffold eder: Entity + EF Config + Repository. Kullanım: /entity Basvuru bsv"
user_invocable: true
---

Verilen entity adını ve modül kodunu işle: `/entity {EntityAdi} {modülkodu}`

## Oluşturulacak Dosyalar

### 1. Domain Entity
`eIzin.Domain/Entities/{ModulKodu}/{EntityAdi}.cs`
- `BaseEntity<Guid>` türevi
- private set property'ler
- Statik factory method `Olustur(...)`
- Domain behavior metodları
- private constructor

### 2. EF Konfigürasyonu
`eIzin.Infrastructure/Persistence/Configurations/{EntityAdi}Configuration.cs`
- `ToTable("{CogulAdi}", "{modülkodu}")` - schema modül kodu
- Property konfigürasyonları
- Index tanımları
- İlişki tanımları

### 3. Repository Interface
`eIzin.Application/Interfaces/I{EntityAdi}Repository.cs`
- `GetByIdAsync`, `ListAsync`, `AddAsync`, `Update`, `Delete`

### 4. Repository Implementation
`eIzin.Infrastructure/Persistence/Repositories/{EntityAdi}Repository.cs`
- Interface implementasyonu
- EF Core LINQ sorguları

### 5. DbContext Kaydı
`eIzin.Infrastructure/Persistence/EIzinDbContext.cs`'e `DbSet<{EntityAdi}>` ekle

## Naming

```
Entity          → PascalCase, tekil: Basvuru
Table           → çoğul: Basvurular
Schema          → küçük modül kodu: bsv
Interface       → I{EntityAdi}Repository
Implementation  → {EntityAdi}Repository
Configuration   → {EntityAdi}Configuration
```
