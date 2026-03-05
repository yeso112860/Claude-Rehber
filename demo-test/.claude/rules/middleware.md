---
paths:
  - "backend/src/middleware/**"
---

# Middleware Kurallari

- TypeScript tiplerini kullan: Request, Response, NextFunction
- `any` tipi YASAK
- next() cagrisi UNUTMA (aksi halde istek asili kalir)
- Async middleware'larda try-catch ZORUNLU
- Hata firlatma yerine next(error) kullan
- Uretimde stack trace GOSTERME (error.stack response'a koyma)
- Error middleware 4 parametre almali: (err, req, res, next)
- Middleware sirasi onemli: logger → auth → validate → route handler → error-handler
