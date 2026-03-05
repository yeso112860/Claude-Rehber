---
name: component
description: Paylasilan React MUI component olusturur. Kullan: /component ConfirmDialog, /component StatusChip, /component DataCard, /component PageHeader
argument-hint: ComponentName
---

$ARGUMENTS icin paylasilan (shared) React component olustur.

## Dosya konumu

`frontend/src/components/<Ad>.tsx`

## Component turleri

### ConfirmDialog
- Silme/iptal gibi tehlikeli islemler icin onay dialog'u
- Props: open, title, message, confirmText, cancelText, onConfirm, onCancel, severity
- MUI Dialog + DialogTitle + DialogContent + DialogActions
- Severity'ye gore renk: error(kirmizi), warning(sari), info(mavi)

### StatusChip
- Durum gostergesi (Chip component)
- Props: status, statusMap (her status icin label + color)
- Ornek: { DRAFT: { label: "Taslak", color: "default" }, CONFIRMED: { label: "Onaylandi", color: "info" } }
- MUI Chip component kullan

### DataCard
- Istatistik karti (Dashboard icin)
- Props: title, value, icon, color, subtitle
- MUI Card + CardContent + Typography + Box
- Icon sol tarafta buyuk, deger sag tarafta

### PageHeader
- Sayfa baslik bilesepi
- Props: title, subtitle, action (ReactNode - buton vb.)
- MUI Box + Typography + sag tarafa action butonu

### EmptyState
- Bos veri durumu gosterimi
- Props: icon, title, description, actionLabel, onAction
- MUI Box + Typography + merkeze hizali gorsel

### LoadingOverlay
- Yukleme gostergesi
- Props: loading, children
- CircularProgress overlay (children uzerine)

## Kurallar

- Props icin ayri interface tanimla ve export et
- Function component + React.FC KULLANMA (duz fonksiyon)
- Varsayilan degerler icin defaultProps YASAK, parametre default kullan
- `any` tipi YASAK
- children tipi ReactNode olsun
- MUI sx prop kullan (inline style YASAK)
- Test dosyasi olustur: `<Ad>.test.tsx`
