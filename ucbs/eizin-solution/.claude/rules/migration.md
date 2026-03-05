---
description: "EF Core migration kuralları - adlandırma, konfigürasyon, schema"
paths:
  - "**/Migrations/**"
  - "**/*DbContext*"
---

# EF Core Migration Kuralları

## TEMEL KURAL: Migration dosyaları ASLA elle düzenlenmez

## Migration Adlandırma

```
Add_{ModulKodu}_{EntityAdi}Table           # Yeni tablo
Add_{ModulKodu}_{SutunAdi}Column          # Yeni sütun
Remove_{ModulKodu}_{SutunAdi}Column       # Sütun kaldırma
Add_{ModulKodu}_{IndexAdi}Index           # Index ekleme
```

## EF Core Konfigürasyon

```csharp
public sealed class BasvuruConfiguration : IEntityTypeConfiguration<Basvuru>
{
    public void Configure(EntityTypeBuilder<Basvuru> builder)
    {
        builder.ToTable("Basvurular", "bsv");  // Schema = modül kodu
        builder.HasKey(x => x.Id);
        builder.Property(x => x.EkKapsami).HasConversion<string>().HasMaxLength(10).IsRequired();
        builder.Property(x => x.BasvuruNo).HasMaxLength(20).IsRequired();
        builder.HasIndex(x => x.TesisId).HasDatabaseName("IX_Basvurular_TesisId");
    }
}
```

## Schema Yapısı (PostgreSQL - modül bazlı)

```
sys  → SistemParametresi, IzinKonuTanim, EkListesi, Organizasyon, OnayAkisi, BelgeSablon
kul  → Kullanici, Rol, KullaniciRol, Vekalet, AuditLog
cg   → SorumluTesis, BasvuruTaslak
bsv  → Basvuru, BasvuruBelge, BasvuruOnay, OdemeBilgi
deg  → Degerlendirme, EksiklikBildirimi, DegerlendirmeKarari
yen  → YenilemeBasvuru, FaaliyetDegisiklik
mua  → MuafiyetBasvuru
ipt  → IptalKarari
ozl  → GorusTalebi
blg  → Belge, BelgeVersiyon
rpr  → Rapor
ent  → EntegrasyonLog
bld  → Bildirim, BildirimTercih
```

## BaseEntity

```csharp
public abstract class BaseEntity<TId>
{
    public TId Id { get; protected set; }
    public DateTime OlusturmaTarihi { get; private set; }
    public DateTime? GuncellemeTarihi { get; private set; }
    public Guid OlusturanKullaniciId { get; private set; }
    public Guid? GuncelleyenKullaniciId { get; private set; }
    public bool AktifMi { get; private set; } = true;
}
```

## Veri Tipi Standartları

```
Guid       → uuid
string     → text veya varchar(N)
DateTime   → timestamp with time zone (timestamptz)
decimal    → numeric(18, 4)
bool       → boolean
enum       → varchar(50) (string olarak sakla)
```

## Migration Sonrası Kontrol

1. Migration dosyasını incele - beklenmedik değişiklik var mı?
2. `dotnet ef migrations script` ile SQL çıktısına bak
3. Down metodu doğru mu kontrol et
4. `dotnet ef database update` ile uygula
