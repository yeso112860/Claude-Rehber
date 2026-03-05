---
name: form
description: "Angular Reactive Form + PrimeNG ile form component oluşturur. Kullanım: /form BasvuruForm bsv"
user_invocable: true
---

Verilen form adı ve modül kodu için Angular form component oluştur:

## Oluşturulacak Dosyalar

### 1. Component TS
`eIzin.Angular/src/app/features/{modul}/components/{form-adi}/{form-adi}.component.ts`

```typescript
@Component({
  selector: 'app-{form-adi}',
  standalone: true,
  imports: [CommonModule, ReactiveFormsModule, ButtonModule, InputTextModule, DropdownModule, CalendarModule],
  templateUrl: './{form-adi}.component.html',
  styleUrl: './{form-adi}.component.scss'
})
export class {FormAdi}Component {
  private readonly fb = inject(FormBuilder);
  private readonly service = inject({Service});
  private readonly router = inject(Router);

  isSubmitting = signal<boolean>(false);
  submitError = signal<string | null>(null);

  form = this.fb.group({
    {alan}: ['', [Validators.required]],
  });

  onSubmit(): void {
    if (this.form.invalid) { this.form.markAllAsTouched(); return; }
    this.isSubmitting.set(true);
    this.submitError.set(null);
    this.service.create(this.form.value).subscribe({
      next: () => this.router.navigate(['/{modul}/{kaynak}']),
      error: (err) => {
        this.submitError.set(err.error?.message ?? 'Bir hata oluştu');
        this.isSubmitting.set(false);
      }
    });
  }

  getFieldError(fieldName: string): string | null {
    const control = this.form.get(fieldName);
    if (control?.errors && control.touched) {
      if (control.errors['required']) return 'Bu alan zorunludur';
      if (control.errors['maxlength']) return `En fazla ${control.errors['maxlength'].requiredLength} karakter`;
    }
    return null;
  }
}
```

### 2. Component HTML
PrimeNG bileşenleri ile:
```html
<div class="card">
  <h2>{Form Başlığı}</h2>
  @if (submitError()) {
    <p-message severity="error" [text]="submitError()!" />
  }
  <form [formGroup]="form" (ngSubmit)="onSubmit()">
    <div class="grid">
      <div class="col-12 md:col-6">
        <p-floatLabel>
          <input pInputText id="{alan}" formControlName="{alan}" />
          <label for="{alan}">{Etiket} *</label>
        </p-floatLabel>
        @if (getFieldError('{alan}')) {
          <small class="p-error">{{ getFieldError('{alan}') }}</small>
        }
      </div>
    </div>
    <div class="flex gap-2 mt-4">
      <p-button type="submit" label="Kaydet" [loading]="isSubmitting()" [disabled]="form.invalid" />
      <p-button type="button" label="İptal" severity="secondary" routerLink="../" />
    </div>
  </form>
</div>
```

### 3. Component SCSS
BEM convention, component-scoped

## PrimeNG Bileşen Eşlemeleri

```
Metin       → pInputText
Sayı        → p-inputnumber
Tarih       → p-calendar
Dropdown    → p-dropdown
Çoklu seçim → p-multiselect
Textarea    → p-inputtextarea
Checkbox    → p-checkbox
Dosya       → p-fileupload
Arama       → p-autocomplete
```
