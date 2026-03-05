# e-İzin Sistemi

Çevre ve Şehircilik Bakanlığı e-İzin/Lisans başvuru ve değerlendirme sistemi.
Full-stack: .NET Core 9.0 Clean Architecture API + Angular 18 SPA.

## Proje Yapısı

```
eizin-solution/
├── eIzin.Domain/          # Entity, ValueObject, DomainEvent, Enum, Exception
├── eIzin.Application/     # Command, Query, Handler, DTO, Validator, Interface
├── eIzin.Infrastructure/  # DbContext, Repository, ExternalService, Migration
├── eIzin.API/             # Controller, Middleware, Filter, DI Config
└── eIzin.Angular/         # Angular 18+ SPA
```

## Hızlı Başlatma

- API: `cd eIzin.API && dotnet run`
- Angular: `cd eIzin.Angular && ng serve`
- Migration uygula: `dotnet ef database update --project eIzin.Infrastructure --startup-project eIzin.API`
- Migration oluştur: `dotnet ef migrations add MigrationAdi --project eIzin.Infrastructure --startup-project eIzin.API`

## Ortam

- API: https://localhost:7001
- Angular: http://localhost:4200
- PostgreSQL: localhost:5432/eizin
- Swagger: https://localhost:7001/swagger

## Modül Kodları

| Kod | Modül | UC Sayısı |
|-----|-------|-----------|
| SYS | Sistem Yönetimi ve Konfigürasyon | 8 |
| KUL | Kullanıcı ve Yetki Yönetimi | 6 |
| CG  | Çevre Görevlisi İşlemleri | 5 |
| BSV | Başvuru Süreçleri | 8 |
| DEG | Değerlendirme Süreçleri | 9 |
| YEN | Yenileme ve Güncelleme Süreçleri | 8 |
| MUA | Muafiyet İşlemleri | 4 |
| IPT | İptal Süreçleri | 7 |
| OZL | Görüş ve Özel İşlemler | 5 |
| BLG | Belge Yönetimi | 5 |
| RPR | Raporlama ve Analitik | 6 |
| ENT | Entegrasyon Yönetimi | 6 |
| BLD | Bildirim Sistemi | 4 |

## Modül Üretim Sırası (ZORUNLU)

SYS → KUL → ENT → BLD → BLG → CG → BSV → DEG → YEN → MUA → IPT → OZL → RPR

Nedenler:
- SYS: Temel parametreler, diğer her şey buna bağlı
- KUL: Auth/JWT altyapısı tüm modüllerce kullanılır
- ENT: Dış sistem client'ları BSV/DEG'den önce hazır olmalı
- BLD: Bildirim altyapısı tüm iş akışlarında kullanılır
- BLG: Belge yönetimi BSV'den önce hazır olmalı

## Aktörler

### Başvuru Yapanlar
- Çevre Görevlisi (ÇG) - Tesisler adına başvuru yapar
- Çevre Danışman Firması (ÇDF) - ÇG'leri istihdam eder
- Çevre Yönetim Birimi (ÇYB) - İşletme bünyesindeki birim
- Firma Yetkilisi - İşletme sahibi/yetkili temsilci

### Değerlendirme Yapanlar
- İl Müdürlüğü Personeli - EK-2 başvuru değerlendirme
- Bakanlık Personeli - EK-1 başvuru değerlendirme
- Şube Müdürü - Onay/red kararı

### Sistem
- Sistem Yöneticisi - Parametre, şablon, organizasyon yönetimi

### Dış Sistemler
- e-Yeterlik Sistemi, Belgenet, TOBB/TESK, e-ÇED, Döner Sermaye

## Clean Architecture Katman Bağımlılığı (KRİTİK)

```
Domain → (hiçbir katmana bağımlı değil)
Application → Domain
Infrastructure → Domain, Application
API → Application, Infrastructure
```

YASAK: Application → Infrastructure doğrudan bağımlılık

## Temel İş Kuralları

- İl Md. Uygunluk yazısı: 1 yıl geçerli
- GFB: 1 yıl geçerli, değerlendirme 30 takvim günü
- İzin/Lisans: 5 yıl geçerli, değerlendirme 60 takvim günü
- GFB'den sonra 180 gün içinde İzin/Lisans başvurusu yapılmalı
- EK-1: Bakanlık değerlendirmesi, EK-2: İl Müdürlüğü değerlendirmesi
- Audit logları 10 yıl saklanır
- 3 başarısız giriş → 30 dk hesap kilitleme

## Progress Tracking

Her `/usecase` veya `/modul` komutu başında `progress.md` oluşturulur.
Görev tamamlanınca `progress.md` silinir.
Oturum başında `progress.md` varsa devam edilir.

## UseCase Kaynağı

Use case tanımları: `/Users/karademir/ornek-calisma/ucbs/UseCase.md`
