---
paths:
  - "frontend/src/features/**"
  - "frontend/src/components/**"
---

# React Component Kurallari

- Function component kullan (class component YASAK)
- Props icin interface tanimla
- `any` tipi YASAK
- API cagrilari SADECE RTK Query hook'lari ile
- Dogrudan fetch/axios YASAK
- Loading: MUI CircularProgress veya Skeleton
- Error: MUI Alert
- Liste: MUI DataGrid
- Form: MUI TextField, Select, Dialog
