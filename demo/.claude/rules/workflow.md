---
# Bu dosyanin path'i yok — tum oturumlarda gecerlidir
---

# Oturum ve Ilerleme Kurallari

## progress.md Yonetimi

- **Oturum basinda:** `progress.md` varsa oku ve kaldigin yerden devam et
- **Her adim sonrasi:** yapilani ve degistirilen dosyalari yaz
- **Maksimum 200 satir:** tamamlanan eski adimlari kisalt, aktif gorev her zaman tam kalsin
- **Oturum sonunda** (tum gorevler bitti): `progress.md`'yi sil

## Format

```markdown
# Aktif Gorev: <istek ozeti>

## Tamamlandi
- [x] backend/src/models/User.ts olusturuldu
- [x] UserSearchService.ts olusturuldu

## Devam Ediyor
- [ ] UserOperationalService.ts — basladi, create metodu eksik

## Bekliyor
- [ ] Frontend RTK Query
- [ ] App.tsx guncelleme

## Son Degisiklikler
- backend/src/models/User.ts (olusturuldu)
- backend/src/services/search/UserSearchService.ts (olusturuldu)
```

## TypeScript Kontrol Zamanlari

Her entity veya feature tamamlandiginda (ara adimlarda degil):
```bash
cd backend && npx tsc --noEmit
cd frontend && npx tsc --noEmit
```
Hata varsa ilerlemeden once duzelt.