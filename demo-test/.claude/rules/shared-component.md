---
paths:
  - "frontend/src/components/**"
---

# Paylasilan Component Kurallari

- Props icin ayri interface tanimla ve export et
- Function component kullan (React.FC KULLANMA, duz fonksiyon)
- defaultProps YASAK, parametre default degerleri kullan
- `any` tipi YASAK
- children tipi: React.ReactNode
- Stil: MUI sx prop kullan (inline style YASAK)
- Her component icin test dosyasi: `<Ad>.test.tsx`
- Component dosya adi PascalCase: ConfirmDialog.tsx, StatusChip.tsx
- Re-export: components/index.ts'den export et (barrel file)
