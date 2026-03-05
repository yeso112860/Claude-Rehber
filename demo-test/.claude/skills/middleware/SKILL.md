---
name: middleware
description: Express middleware olusturur. Kullan: /middleware auth, /middleware error-handler, /middleware request-logger, /middleware rate-limiter
argument-hint: middleware-name
---

$ARGUMENTS icin Express middleware olustur.

## Dosya konumu

`backend/src/middleware/<ad>.ts`

## Middleware turleri

### auth
- JWT token dogrulama (Authorization: Bearer <token>)
- Token yoksa 401 don
- Token gecersizse 401 don
- req.user'a decode edilmis token bilgisini ekle
- Korunan route'lara `authMiddleware` olarak ekle

### error-handler
- Global error handling middleware
- Bilinen hatalari yakalanmis response'a donustur
- Bilinmeyen hatalarda 500 don
- Uretimde stack trace GOSTERME
- Hata log'la (console.error)
- Response: { success: false, message: "...", code?: "..." }

### request-logger
- Her istegi logla: method, url, status, sure (ms)
- Renkli cikti (GET=yesil, POST=mavi, DELETE=kirmizi)
- Body'yi loglama (guvenlik)
- Response time hesapla

### rate-limiter
- IP bazli istek siniri
- Yapilandirmali: windowMs, maxRequests
- Sinir asilinca 429 Too Many Requests don
- Map ile basit in-memory sayac (demo icin yeterli)

### validate
- Zod schema ile request body dogrulama
- Gecersiz body'de 400 + detayli hata mesajlari don
- Fabrika fonksiyonu: `validate(schema)` → middleware dondur

## Kurallar

- TypeScript tipli (Request, Response, NextFunction)
- `any` tipi YASAK
- next() cagrisi unutma
- Async hatalari yakala (async middleware icin try-catch)
- backend/src/index.ts'e middleware'i import edip uygun siraya ekle
