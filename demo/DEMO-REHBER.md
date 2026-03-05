# Canli Demo Rehberi (Basit)

Hizli demo icin. Tahmini sure: 8-10 dakika.
`npm` ve `node` kurulu olmali.

---

## HAZIRLIK (Sunum oncesi)

```bash
cd demo && git init && git add -A && git commit -m "ilk commit"
```

---

## ADIM 1: Baslat (1 dk)

```bash
claude
```

Soyle:
```
CLAUDE.md dosyasini oku ve ozetle
```

> "Claude kurallarimizi biliyor" de.

---

## ADIM 2: Iskelet Olustur (3 dk)

```
Bu projenin backend ve frontend iskeletini olustur.
Backend: Express + TypeScript + better-sqlite3 (port 3001)
Frontend: React 18 + TypeScript + RTK Query + MUI (port 3000, Vite)
Temel dosyalari, package.json, tsconfig.json, baseApi.ts, store, App.tsx olustur.
```

---

## ADIM 3: /endpoint ile Modul Uret (3 dk)

```
/endpoint User
```

> 8 dosya uretir: model, searchService, operationalService, searchRoutes, operationalRoutes, userApi.ts, UserListPage.tsx, UserForm.tsx

```
/endpoint Product
```

> Ayni pattern, ikinci modul. "Kurallar otomatik uygulanir" de.

---

## ADIM 4: Calistir (2 dk)

```
Backend ve frontend'i kur ve calistir
```

> localhost:3000 → calisan uygulama!

---

## ADIM 5: Code Review (1 dk)

```
@code-reviewer tum kodu incele
```

---

## DOSYA YAPISI

```
demo/
├── CLAUDE.md                    ← Proje kurallari
├── .claude/
│   ├── settings.json            ← Izinler + hook
│   ├── agents/
│   │   └── code-reviewer.md     ← Kod inceleme
│   ├── rules/
│   │   ├── search-controller.md ← Search: sadece GET
│   │   ├── operational-controller.md ← Operational: sadece CUD
│   │   ├── react-component.md   ← React kurallari
│   │   └── rtk-query.md         ← RTK Query kurallari
│   └── skills/
│       ├── endpoint/SKILL.md    ← /endpoint → full-stack CRUD
│       └── feature-ui/SKILL.md  ← /feature-ui → sadece frontend
```

> Basit demo. Detayli versiyon icin demo-test/ klasorune bak.
