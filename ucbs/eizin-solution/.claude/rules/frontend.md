---
description: "Angular 18+ frontend kuralları - Standalone, Signals, PrimeNG"
paths:
  - "**/*.ts"
  - "**/*.html"
  - "**/*.scss"
---

# Angular 18+ Frontend Kuralları

## Teknoloji

- Angular 18+ Standalone Components (NgModule YASAK)
- Angular Signals (signal, computed, effect) - state management
- PrimeNG 17+ UI bileşenleri
- Angular Reactive Forms
- provideHttpClient (HttpClientModule değil)
- RxJS 7+, TypeScript 5.x strict mode

## Dosya Yapısı

```
eIzin.Angular/src/app/
├── core/
│   ├── auth/ (auth.service.ts, auth.guard.ts, jwt.interceptor.ts)
│   ├── interceptors/ (loading, error)
│   ├── models/ (api-response, pagination, result)
│   └── services/ (notification.service.ts)
├── shared/
│   ├── components/ (basvuru-stepper, durum-badge, belge-yukleme, eimza-dialog, onay-timeline)
│   ├── pipes/
│   └── directives/
└── features/
    ├── sys/, kul/, cg/, bsv/, deg/, yen/, mua/, ipt/, ozl/, blg/, rpr/, ent/, bld/
```

Her feature modülü:
```
features/{modul}/
├── models/{entity}.model.ts
├── services/{entity}.service.ts
├── components/
│   ├── {entity}-list/ (.ts, .html, .scss)
│   └── {entity}-form/ (.ts, .html, .scss)
└── {modul}.routes.ts
```

## Standalone Component Şablonu

```typescript
@Component({
  selector: 'app-basvuru-list',
  standalone: true,
  imports: [CommonModule, TableModule, ButtonModule],
  templateUrl: './basvuru-list.component.html',
  styleUrl: './basvuru-list.component.scss'
})
export class BasvuruListComponent implements OnInit {
  private readonly basvuruService = inject(BasvuruService);

  basvurular = signal<BasvuruModel[]>([]);
  isLoading = signal<boolean>(false);
  errorMessage = signal<string | null>(null);
  toplamBasvuru = computed(() => this.basvurular().length);

  ngOnInit(): void { this.loadBasvurular(); }

  private loadBasvurular(): void {
    this.isLoading.set(true);
    this.basvuruService.getAll().subscribe({
      next: (response) => { this.basvurular.set(response.data); this.isLoading.set(false); },
      error: () => { this.errorMessage.set('Yüklenemedi'); this.isLoading.set(false); }
    });
  }
}
```

## Service Şablonu

```typescript
@Injectable({ providedIn: 'root' })
export class BasvuruService {
  private readonly http = inject(HttpClient);
  private readonly apiUrl = '/api/bsv/basvurular';

  getAll(params?: any): Observable<ApiResponse<PageResponse<BasvuruModel>>> {
    return this.http.get<ApiResponse<PageResponse<BasvuruModel>>>(this.apiUrl, { params });
  }
  getById(id: string): Observable<ApiResponse<BasvuruModel>> {
    return this.http.get<ApiResponse<BasvuruModel>>(`${this.apiUrl}/${id}`);
  }
  create(request: CreateBasvuruRequest): Observable<ApiResponse<BasvuruModel>> {
    return this.http.post<ApiResponse<BasvuruModel>>(this.apiUrl, request);
  }
  update(id: string, request: Partial<BasvuruModel>): Observable<ApiResponse<BasvuruModel>> {
    return this.http.put<ApiResponse<BasvuruModel>>(`${this.apiUrl}/${id}`, request);
  }
  delete(id: string): Observable<ApiResponse<void>> {
    return this.http.delete<ApiResponse<void>>(`${this.apiUrl}/${id}`);
  }
}
```

## Model Şablonu

```typescript
export interface BasvuruModel {
  id: string;
  tesisId: string;
  ekKapsami: EkKapsami;
  basvuruDurumu: BasvuruDurumu;
  basvuruTarihi: Date;
}

export interface CreateBasvuruRequest {
  tesisId: string;
  ekKapsami: EkKapsami;
}

export enum EkKapsami { EK_1 = 'EK_1', EK_2 = 'EK_2' }
```

## Template Kuralları

```html
<p-table [value]="basvurular()" [loading]="isLoading()" [paginator]="true" [rows]="10">
  <ng-template pTemplate="body" let-basvuru>
    <tr>
      <td>{{ basvuru.basvuruNo }}</td>
      <td><p-tag [value]="basvuru.durumLabel" [severity]="getDurumSeverity(basvuru.durum)"/></td>
    </tr>
  </ng-template>
  <ng-template pTemplate="emptymessage">
    <tr><td colspan="3">Kayıt bulunamadı.</td></tr>
  </ng-template>
</p-table>
```

## Form Kuralları

```typescript
form = this.fb.group({
  tesisId: ['', [Validators.required]],
  ekKapsami: ['', [Validators.required]],
  aciklama: ['', [Validators.maxLength(2000)]]
});

onSubmit(): void {
  if (this.form.invalid) { this.form.markAllAsTouched(); return; }
  // submit logic
}
```

## Temel Kurallar

- `any` tipi YASAK, her değişkene tip ver
- `inject()` fonksiyonu kullan (constructor injection ikincil)
- `NgModule` YASAK, sadece Standalone Components
- Signal kullan: `signal<T>()`, `computed()`, `effect()`
- `@if` / `@for` / `@switch` kullan (`*ngIf`, `*ngFor` değil - Angular 17+ control flow)
- Her component'te Loading/Error/Empty state yönet
- SCSS: BEM naming, component-scoped
- PrimeNG bileşenlerini tercih et
- 2 space indent, single quote, trailing comma
