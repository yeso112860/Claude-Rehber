e-İzin sisteminin güvenlik denetimini yap:

## 1. Kimlik Doğrulama ve JWT

- JWT token süresi konfigürasyonda mı? (hard-coded değil)
- Refresh token mekanizması var mı?
- Token imzalama anahtarı environment variable'dan mı okunuyor?
- `[AllowAnonymous]` gereksiz kullanım var mı?
- Hassas endpoint'lerde `[Authorize]` attribute eksik mi?

## 2. Yetkilendirme (e-İzin'e Özel)

Aktör-Endpoint eşlemesini kontrol et:
- CevreGorevlisi → Yalnızca kendi tesislerine ait işlemler
- IlMudurlugu → Yalnızca kendi iline ait EK-2 başvuruları
- BakanlikPersoneli → Yalnızca EK-1 başvuruları
- FirmaYetkilisi → Yalnızca kendi firmasına ait veriler (salt okunur)

Her Controller action'ında `[Authorize(Roles = "...")]` var mı?

## 3. OWASP Top 10

**SQL Injection**: Ham SQL'de parametre geçişi, string interpolation ile SQL YASAK
**XSS (Angular)**: `innerHTML` binding DomSanitizer olmadan YASAK, `bypassSecurityTrust*` gerekçesiz YASAK
**Sensitive Data**: TC Kimlik loglarda maskelenmeli, API response'da parola/token dönmemeli, HTTPS zorunlu
**IDOR**: `GetById(id)` sorgularında erişim yetkisi kontrolü, repository'de kullanıcı filtresi
**Misconfiguration**: CORS `AllowAnyOrigin` prod'da YASAK, Swagger prod'da devre dışı, detaylı exception mesajları prod'a sızmamalı

## 4. e-İzin Spesifik

**e-İmza**: Belge onaylarında e-imza doğrulama zorunlu, imzasız onay mümkün olmamalı
**Belge Güvenliği**: Dosya boyut sınırı (10MB), izin verilen uzantılar (pdf, jpg, png, docx), virüs taraması
**Audit Log**: Tüm CUD işlemleri loglanmalı, kullanıcı ID + timestamp + değişen alan bilgisi, 10 yıl saklama
**Brute Force**: 3 başarısız giriş → hesap kilitleme, 30 dk kilitleme süresi

## 5. Dış Sistem Entegrasyonu

- API key/token → environment variable (hard-coded değil)
- Circuit breaker pattern (Polly)
- Merkezi hata loglama
- Timeout değerleri konfigürasyonda

## Çıktı

Her bulgu:
- **Dosya + Satır**: `eIzin.API/Controllers/BasvuruController.cs:45`
- **Seviye**: KRİTİK / YÜKSEK / ORTA / DÜŞÜK
- **OWASP Kategorisi**: A01:2021-Access Control vb.
- **Açıklama + Düzeltme**
