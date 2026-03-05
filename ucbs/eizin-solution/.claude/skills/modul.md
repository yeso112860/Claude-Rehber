---
name: modul
description: "Bir modüldeki tüm use case'leri sırayla işler ve tam stack kod üretir. Kullanım: /modul BSV"
user_invocable: true
---

Verilen modül kodunu (örnek: BSV) işle:

## ADIM 0: Modül Analizi

1. `/Users/karademir/ornek-calisma/ucbs/UseCase.md` dosyasından `{MODUL_KODU}` modülündeki TÜM UC'leri bul
2. UC listesini çıkar (UC-{MODUL}-001, UC-{MODUL}-002, ...)
3. `progress.md` oluştur:

```
# Görev: {MODUL_KODU} - {MODUL_ADI} Modülü
Başlangıç: {TIMESTAMP}

## Bağımlılıklar
{Bağımlı modüller - hazır olduğunu kontrol et}

## Use Case Listesi
- [ ] UC-{MODUL}-001: {UC_ADI}
- [ ] UC-{MODUL}-002: {UC_ADI}
...
```

## ADIM 1: Bağımlılık Kontrolü

Bağımlılık matrisi:
```
SYS  → bağımlılık yok
KUL  → SYS
ENT  → SYS, KUL
BLD  → SYS, KUL
BLG  → SYS, KUL, BLD
CG   → SYS, KUL, BLG
BSV  → SYS, KUL, CG, BLG, ENT, BLD
DEG  → BSV, KUL, BLG, BLD
YEN  → BSV, DEG, BLG, ENT
MUA  → BSV, SYS
IPT  → BSV, DEG, YEN
OZL  → BSV, DEG, BLG
RPR  → tüm modüller
```

Kontrol: `eIzin.Domain/Entities/{bagimliModul}/` dizini var mı? Eksikse DUR ve kullanıcıyı bilgilendir.

## ADIM 2: Her UC İçin Üretim

Her UC için sırayla /usecase skill mantığını uygula:
1. Tüm adımları çalıştır
2. progress.md'de UC'yi işaretle: `- [x] UC-{MODUL}-{NNN}: {UC_ADI}`
3. Sonraki UC'ye geç

Ortak entity/DTO'ları tekrar oluşturma - mevcut olanı kullan.

## ADIM 3: Modül Düzeyinde Dosyalar

- `eIzin.Angular/src/app/features/{modülkodu}/{modülkodu}.routes.ts` - tüm route'lar
- Controller: 8'den fazla UC varsa controller'ı split et

## ADIM 4: DI Kaydı

```csharp
public static IServiceCollection Add{ModulKodu}Services(this IServiceCollection services)
{
    services.AddScoped<I{Entity1}Repository, {Entity1}Repository>();
    // ...tüm repository'ler
    return services;
}
```

## ADIM 5: Migration

Tüm migration'ları listele, uygulama önerisi sun:
```bash
dotnet ef database update --project eIzin.Infrastructure --startup-project eIzin.API
```

## ADIM 6: Tamamlama

1. progress.md: tüm UC'ler tamamlandı
2. Özet rapor: dosya sayısı, endpoint sayısı
3. progress.md sil
