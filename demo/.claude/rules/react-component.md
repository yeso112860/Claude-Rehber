---
paths:
  - "frontend/src/features/**"
  - "frontend/src/components/**"
---

# React Component Kurallari

## Zorunlular
- Sadece **function component** kullan (class component **YASAK**)
- Her component icin **props interface** tanimla
- `any` tipi **YASAK** — TypeScript strict zorunlu
- API cagrisi **sadece RTK Query hook'lari** ile (fetch / axios **YASAK**)

## MUI Kullanim
| Durum   | Component                          |
|---------|------------------------------------|
| Liste   | `MUI DataGrid`                     |
| Form    | `MUI TextField`, `Select`, `Dialog`|
| Loading | `CircularProgress` veya `Skeleton` |
| Hata    | `MUI Alert`                        |

## Hata Gosterimi
- RTK Query `isError` durumunda `MUI Alert` goster
- Backend'den gelen `message` alanini kullaniciya ilet:
```tsx
{isError && <Alert severity="error">{error?.data?.message ?? 'Bir hata olustu'}</Alert>}
```
- HTTP 404 ve 500 ayirt edilmeksizin ayni Alert pattern'i kullan

## Loading Gosterimi
- Veri yuklenirken liste icin `Skeleton`, modal/form icin `CircularProgress` kullan
- RTK Query `isLoading` flag'ini kullan, manuel state **YASAK**

## Yeni Feature Ekleme
- `frontend/src/features/<entity>/` klasoru olustur
- `<Entity>ListPage.tsx` ve `<Entity>Form.tsx` dosyalarini ekle
- `App.tsx` icindeki `ROUTES` dizisine entry ekle (baska yere dokunma)