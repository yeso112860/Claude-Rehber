---
name: form
description: Zod validation semasi + React form component olusturur. Kullan: /form UserForm, /form ProductForm, /form LoginForm
argument-hint: FormName
---

$ARGUMENTS icin validasyonlu form component olustur.

## Olusturulacak dosyalar

1. `frontend/src/features/<feature>/schemas/<ad>Schema.ts`
   - Zod schema tanimi
   - Turkce hata mesajlari
   - Her alan icin uygun validasyon

2. `frontend/src/features/<feature>/<Ad>.tsx`
   - React Hook Form + zodResolver entegrasyonu
   - MUI form component'lari (TextField, Select, Checkbox, DatePicker)
   - Submit islemi RTK Query mutation hook ile
   - Loading state (buton disabled + CircularProgress)
   - Error state (her alan altinda hata mesaji)
   - Basarili islem sonrasi bildirim

## Zod Schema Kurallari

```typescript
// Her alan icin Turkce hata mesaji ZORUNLU
z.string().min(1, "Bu alan zorunludur")
z.string().email("Gecerli bir e-posta giriniz")
z.number().positive("Sifirdan buyuk olmali")
z.number().min(0, "Negatif olamaz")
```

## MUI Form Kurallari

- TextField: `error` ve `helperText` prop'lari ile hata gosterimi
- Select: MenuItem ile secenekler
- Form layout: MUI Stack spacing={2}
- Butonlar: DialogActions icinde Iptal + Kaydet
- Kaydet butonu: disabled={!isValid || isSubmitting}

## Kurallar

- `any` tipi YASAK
- Inline validation YASAK (Zod'a tasi)
- Form state icin React Hook Form kullan (useState ile form yonetme YASAK)
