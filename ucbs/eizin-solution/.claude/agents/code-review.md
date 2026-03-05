Sana verilen kodu veya modül dizinini incele ve aşağıdaki kontrol listesini uygula:

## Clean Architecture Kontrolleri

1. **Katman Bağımlılığı İhlali**
   - Domain'de Application/Infrastructure/API referansı var mı?
   - Application'da Infrastructure/API referansı var mı?
   - `using` direktiflerini tara: `EIzin.Infrastructure.*` Application'da geçiyor mu?

2. **Sorumluluk İhlali**
   - Controller'da iş mantığı var mı? (if/switch ile durum yönetimi)
   - Handler'da doğrudan DbContext kullanımı var mı?
   - Repository'de iş mantığı var mı?

3. **CQRS Uyumu**
   - Her Command'ın karşılık gelen Handler'ı var mı?
   - Command ve Query ayrı klasörlerde mi?

## .NET Kodlama Standartları

4. **Constructor Injection** - `[Inject]` attribute veya field injection YASAK, static bağımlılık YASAK
5. **Async/Await** - `async void` YASAK, `.Result`/`.Wait()` blocking call YASAK
6. **Null Safety** - `ArgumentNullException.ThrowIfNull()` kullanımı, `?.` ve `is null` kontrolleri
7. **Entity Kapsülleme** - `public set` YASAK (private set olmalı), factory method zorunlu

## Angular Kontrolleri

8. **Signals** - `BehaviorSubject` yerine `signal<T>()` kullanılmış mı?
9. **Standalone** - `@NgModule` YASAK, gereksiz import var mı?
10. **TypeScript** - `any` tipi YASAK, gereksiz `!` (non-null assertion) var mı?

## SOLID Prensipleri

11. **SRP** - 200+ satır sınıf var mı? Handler'da birden fazla sorumluluk?
12. **OCP** - Büyük switch/if blokları yerine strategy pattern kullanılmalı
13. **DIP** - Concrete implementation yerine interface'e bağımlılık

## Çıktı Formatı

Her bulgu için:
- **Dosya**: `path/to/file.cs` (Satır: N)
- **Seviye**: HATA / UYARI / ÖNERİ
- **Açıklama**: Ne sorun var
- **Düzeltme**: Nasıl düzeltilmeli

Özet: Toplam X hata, Y uyarı, Z öneri
