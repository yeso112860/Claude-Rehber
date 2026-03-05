---
paths:
  - "frontend/src/**/schemas/**"
  - "frontend/src/**/*Form.tsx"
  - "frontend/src/**/*Form*.tsx"
---

# Form ve Validation Kurallari

## Zod Schema
- Hata mesajlari TURKCE: `z.string().min(1, "Bu alan zorunludur")`
- Schema dosyasi: `schemas/<ad>Schema.ts`
- Schema'yi export et (test'te de kullanilacak)
- E-posta: `z.string().email("Gecerli bir e-posta giriniz")`
- Fiyat: `z.number().positive("Sifirdan buyuk olmali")`
- Sayi: `z.number().min(0, "Negatif olamaz")`

## React Hook Form
- useForm + zodResolver entegrasyonu kullan
- useState ile form yonetimi YASAK
- register veya Controller kullan
- isValid ve isSubmitting state'lerini kullan

## MUI Form
- TextField: error + helperText ile hata gosterimi
- Select: MenuItem ile secenekler (serbest metin YASAK)
- Form layout: Stack spacing={2}
- Kaydet butonu: disabled={!isValid || isSubmitting}
- Inline validation YASAK (Zod schema'ya tasi)
