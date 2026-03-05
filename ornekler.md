# Claude Code Pratik Ornekler - Spring Boot (Gradle) + React

Hizlica kopyala-yapistir ile uygulanabilir ornekler.
Backend: Spring Boot 3.x + Java 21 + Gradle + Shadow JAR (Multi-module: common + server)
Frontend: React 18.3 + TypeScript + RTK Query + MUI + Golge UI

---

## 1. CLAUDE.md ORNEKLERI

### 1a. Spring Boot Backend - Multi Module Projesi

Dosya: `backend/CLAUDE.md`

```markdown
# Golge Backend API - Spring Boot

Spring Boot 3.x + Java 21 + Gradle multi-module proje.

## Teknoloji Stack

- Java 21
- Spring Boot 3.3
- Gradle 8.x (Kotlin DSL - build.gradle.kts)
- Shadow JAR (com.github.johnrengelman.shadow plugin)
- Spring Data JPA + Hibernate
- PostgreSQL
- Flyway (migration)
- Spring Security + JWT
- MapStruct (DTO mapping)
- Swagger/OpenAPI (springdoc-openapi)

## Multi-Module Yapisi

- `common/` - DTO, Enum, Constants, Exception, Util (paylasilan)
- `server/` - Entity, Repository, Service, Controller, Config

common modulu HICBIR Spring veya JPA bagimliligi icermez.
server modulu common'a bagimlidir.

## Komutlar

- `./gradlew bootRun` - gelistirme sunucusu
- `./gradlew test` - tum testleri calistir
- `./gradlew :server:test` - sadece server testleri
- `./gradlew :common:test` - sadece common testleri
- `./gradlew build` - uretim derlemesi
- `./gradlew shadowJar` - fat JAR olustur (Shadow plugin)
- `./gradlew flywayMigrate` - veritabani migration
- `./gradlew dependencies` - bagimlilik agaci
- `./gradlew clean build -x test` - test atlanarak derleme

## Mimari (Katmanli Yapi)

Controller → Service → Repository
Her katman sadece bir alt katmani cagirir.

### Service Ayirimi (ONEMLI!)

- **SearchService / SearchServiceImpl**: GET ve filtreleme islemleri
  - findAll, findById, findByFilter, search, list
- **OperationalService / OperationalServiceImpl**: CUD (Create, Update, Delete)
  - create, update, delete, activate, deactivate

Bu ayirim hem Controller hem Service katmaninda gecerli:
- `UserSearchController` + `UserSearchService` → GET islemleri
- `UserOperationalController` + `UserOperationalService` → POST/PUT/DELETE

## Dosya Yapisi ve Isimlendirme

### common modulu (DTO, Enum, Constants)
- DTO: `common/src/main/java/com/golge/common/dto/user/UserDTO.java`
- Request: `common/src/main/java/com/golge/common/dto/user/CreateUserRequest.java`
- Response: `common/src/main/java/com/golge/common/dto/user/UserResponse.java`
- Filter: `common/src/main/java/com/golge/common/dto/user/UserFilterRequest.java`
- Enum: `common/src/main/java/com/golge/common/enums/UserStatus.java`
- Constants: `common/src/main/java/com/golge/common/constants/ApiConstants.java`
- Exception: `common/src/main/java/com/golge/common/exception/ResourceNotFoundException.java`
- Util: `common/src/main/java/com/golge/common/util/DateUtils.java`

### server modulu (Entity, Service, Controller)
- Entity: `server/src/main/java/com/golge/server/entity/User.java`
- Repository: `server/src/main/java/com/golge/server/repository/UserRepository.java`
- Search Service: `server/src/main/java/com/golge/server/service/search/UserSearchService.java`
- Search Impl: `server/src/main/java/com/golge/server/service/search/impl/UserSearchServiceImpl.java`
- Operational Service: `server/src/main/java/com/golge/server/service/operational/UserOperationalService.java`
- Operational Impl: `server/src/main/java/com/golge/server/service/operational/impl/UserOperationalServiceImpl.java`
- Search Controller: `server/src/main/java/com/golge/server/controller/search/UserSearchController.java`
- Operational Controller: `server/src/main/java/com/golge/server/controller/operational/UserOperationalController.java`
- Mapper: `server/src/main/java/com/golge/server/mapper/UserMapper.java`
- Config: `server/src/main/java/com/golge/server/config/SecurityConfig.java`
- Test: `server/src/test/java/com/golge/server/service/UserSearchServiceTest.java`
- Migration: `server/src/main/resources/db/migration/V1__create_users_table.sql`

## Kurallar

- @RestController + @RequestMapping("/api/v1/...") kullan
- ResponseEntity<ApiResponse<T>> ile sarmalanan response
- @Valid ile input validation (Jakarta Validation)
- Global exception handler: @ControllerAdvice
- Lombok @Data, @Builder, @AllArgsConstructor kullan
- Constructor injection kullan (@Autowired field injection YASAK)
- application.yml kullan (application.properties degil)
- Profile'lar: dev, test, prod
- Logback ile loglama, @Slf4j annotation
- DTO siniflarini SADECE common modulunde tanimla
- Entity siniflarini SADECE server modulunde tanimla
```

### 1b. React Frontend Projesi

Dosya: `frontend/CLAUDE.md`

```markdown
# Golge Frontend - React

React 18.3 + TypeScript + Vite SPA.

## Teknoloji Stack

- React 18.3 + TypeScript 5 (strict mode)
- Vite (build tool)
- React Router v6
- RTK Query (Redux Toolkit Query - API state yonetimi)
- Redux Toolkit (client state)
- MUI (Material UI) - ana component kutuphanesi
- Golge UI - ozel component kutuphanesi (MUI uzerine insa)
- React Hook Form + Zod (form validation)
- Vitest + React Testing Library (test)

## Komutlar

- `npm run dev` - gelistirme sunucusu (Vite)
- `npm run build` - uretim derlemesi (tsc && vite build)
- `npm run preview` - uretim onizlemesi
- `npm run test` - vitest
- `npm run test:ui` - vitest UI
- `npm run lint` - ESLint
- `npm run format` - Prettier
- `npm run type-check` - TypeScript tip kontrolu (tsc --noEmit)

## Dosya Yapisi

- `src/pages/` - Sayfa componentleri (route basina)
- `src/components/` - Paylasilan UI componentleri
- `src/features/` - Feature-based moduller (her feature kendi slice + api)
- `src/features/<feature>/api.ts` - RTK Query endpoint tanimlari
- `src/features/<feature>/slice.ts` - Redux slice (client state)
- `src/features/<feature>/types.ts` - Feature-specific tipler
- `src/features/<feature>/components/` - Feature-specific componentler
- `src/hooks/` - Custom hook'lar
- `src/services/` - RTK Query baseApi yapilandirmasi
- `src/store/` - Redux store yapilandirmasi
- `src/types/` - Global TypeScript tipleri
- `src/utils/` - Yardimci fonksiyonlar
- `src/lib/` - 3rd party yapilandirmalari
- `src/theme/` - MUI tema ve Golge UI tema ozellestimeleri

## Kurallar

- TypeScript strict mode ZORUNLU, any tipi YASAK
- Fonksiyonel componentler (class component YASAK)
- Named export kullan (default export YASAK)
- Interface kullan (type degil, union type haric)
- API URL'leri .env dosyasindan: VITE_API_BASE_URL
- API islemleri icin SADECE RTK Query kullan (axios/fetch dogrudan YASAK)
- Client state icin Redux Toolkit slice kullan
- Server state icin RTK Query kullan (cache, invalidation, polling)
- UI componentleri: once Golge UI, yoksa MUI, son care custom
- Form yonetimi: React Hook Form + Zod schema ZORUNLU
- Loading/Error/Empty state her sayfada handle edilmeli
- 2 space indentation, trailing comma, single quote
- import siralama: react > redux/rtk > mui > golge-ui > yerel
```

### 1c. Monorepo (Full-Stack) CLAUDE.md

Dosya: `CLAUDE.md` (proje koku)

```markdown
# Golge Platform

Full-stack monorepo: Spring Boot API (multi-module) + React SPA.

## Proje Yapisi

- `backend/` - Spring Boot 3 + Java 21 + Gradle + Shadow JAR
  - `backend/common/` - DTO, Enum, Constants (paylasilan)
  - `backend/server/` - Entity, Service, Controller
- `frontend/` - React 18.3 + TypeScript + RTK Query + MUI + Golge UI
- `docker/` - Docker Compose yapilandirmasi
- `docs/` - API ve mimari dokumantasyonu

## Hizli Baslatma

- Backend: `cd backend && ./gradlew bootRun`
- Frontend: `cd frontend && npm run dev`
- Full stack: `docker compose up`

## Ortam

- Backend: http://localhost:8080
- Frontend: http://localhost:5173
- PostgreSQL: localhost:5432
- Swagger UI: http://localhost:8080/swagger-ui.html

@backend/CLAUDE.md
@frontend/CLAUDE.md
```

### 1d. Path-Specific Kurallar

Dosya: `.claude/rules/spring-search-controller.md`

```markdown
---
paths:
  - "backend/server/src/main/java/**/controller/search/**"
---

# Search Controller Kurallari

- @RestController + @RequestMapping("/api/v1/<kaynak>/search") kullan
- SADECE GET metodlari: findAll, findById, search, filter
- @Operation (OpenAPI) annotation her metoda
- Sayfalama: Pageable parametresi, Page<DTO> donusu
- Filtreleme: @ModelAttribute FilterRequest DTO kullan
- ResponseEntity<ApiResponse<T>> don
- Controller'da business logic YAZMA, SearchService'e delege et

Ornek:
@GetMapping
public ResponseEntity<ApiResponse<Page<UserResponse>>> findAll(Pageable pageable) {
    return ResponseEntity.ok(ApiResponse.success(userSearchService.findAll(pageable)));
}

@GetMapping("/filter")
public ResponseEntity<ApiResponse<Page<UserResponse>>> filter(
        @ModelAttribute UserFilterRequest filter, Pageable pageable) {
    return ResponseEntity.ok(ApiResponse.success(userSearchService.filter(filter, pageable)));
}
```

Dosya: `.claude/rules/spring-operational-controller.md`

```markdown
---
paths:
  - "backend/server/src/main/java/**/controller/operational/**"
---

# Operational Controller Kurallari

- @RestController + @RequestMapping("/api/v1/<kaynak>") kullan
- SADECE CUD metodlari: create (POST), update (PUT), delete (DELETE)
- @Valid ile request body dogrulama
- @PreAuthorize ile yetkilendirme
- ResponseEntity<ApiResponse<T>> don
- 201 CREATED (create), 200 OK (update), 204 NO_CONTENT (delete)
- Controller'da business logic YAZMA, OperationalService'e delege et

Ornek:
@PostMapping
public ResponseEntity<ApiResponse<UserResponse>> create(@Valid @RequestBody CreateUserRequest request) {
    return ResponseEntity.status(HttpStatus.CREATED)
        .body(ApiResponse.success(userOperationalService.create(request)));
}
```

Dosya: `.claude/rules/spring-search-service.md`

```markdown
---
paths:
  - "backend/server/src/main/java/**/service/search/**"
---

# Search Service Kurallari

- Interface + Impl pattern: UserSearchService + UserSearchServiceImpl
- @Service annotation impl sinifina
- @Transactional(readOnly = true) TUM metodlara (okuma islemi)
- Constructor injection (final field + @RequiredArgsConstructor)
- Sonuc bulunamazsa: ResourceNotFoundException firlat
- Entity → DTO donusumu MapStruct mapper ile
- Filtreleme: Specification pattern veya QueryDSL kullan
- Loglama: log.debug istek, log.warn bulunamadi durumu
```

Dosya: `.claude/rules/spring-operational-service.md`

```markdown
---
paths:
  - "backend/server/src/main/java/**/service/operational/**"
---

# Operational Service Kurallari

- Interface + Impl pattern: UserOperationalService + UserOperationalServiceImpl
- @Service annotation impl sinifina
- @Transactional (varsayilan, readOnly DEGIL - yazma islemi)
- Constructor injection (final field + @RequiredArgsConstructor)
- Create: DTO → Entity donusumu, save, Entity → Response DTO
- Update: findById (yoksa exception), guncelle, save
- Delete: soft delete tercih et (isActive = false), hard delete gerekirse
- Event publish: onemli islemlerde ApplicationEventPublisher kullan
- Loglama: log.info basarili islem, log.error hata durumu
```

Dosya: `.claude/rules/common-dto.md`

```markdown
---
paths:
  - "backend/common/src/main/java/**/dto/**"
---

# Common DTO Kurallari

- DTO siniflarini SADECE common modulunde tanimla
- Request DTO: CreateUserRequest, UpdateUserRequest (Jakarta Validation ile)
- Response DTO: UserResponse (@Builder ile)
- Filter DTO: UserFilterRequest (nullable alanlar)
- Lombok: @Data, @Builder, @NoArgsConstructor, @AllArgsConstructor
- DTO'larda JPA/Spring annotation KULLANMA (common bagimsiz olmali)
- Nested DTO icin ayri sinif: UserResponse.AddressDTO seklinde DEGIL, AddressResponse ayri dosya
- record kullanabilirsin (Java 21): public record UserResponse(Long id, String name) {}
```

Dosya: `.claude/rules/react-rtk-query.md`

```markdown
---
paths:
  - "frontend/src/features/**/api.ts"
  - "frontend/src/services/**"
---

# RTK Query Kurallari

- Her feature kendi api.ts dosyasinda endpoint tanimlar
- baseApi: src/services/baseApi.ts'den inject et
- Tag-based invalidation ZORUNLU (cache tutarliligi icin)
- GET islemleri: builder.query
- CUD islemleri: builder.mutation + invalidatesTags
- Response tipi: ApiResponse<T> wrapper
- Error handling: transformErrorResponse ile standart hata formati
- Polling: ihtiyac varsa pollingInterval kullan

Ornek (src/features/user/api.ts):
```typescript
import { baseApi } from '@/services/baseApi';
import { UserResponse, CreateUserRequest, UserFilterRequest } from './types';
import { ApiResponse, PageResponse } from '@/types/api';

export const userApi = baseApi.injectEndpoints({
  endpoints: (builder) => ({
    getUsers: builder.query<ApiResponse<PageResponse<UserResponse>>, { page: number; size: number }>({
      query: ({ page, size }) => `/api/v1/user/search?page=${page}&size=${size}`,
      providesTags: ['User'],
    }),
    getUserById: builder.query<ApiResponse<UserResponse>, number>({
      query: (id) => `/api/v1/user/search/${id}`,
      providesTags: (result, error, id) => [{ type: 'User', id }],
    }),
    filterUsers: builder.query<ApiResponse<PageResponse<UserResponse>>, UserFilterRequest>({
      query: (filter) => ({
        url: '/api/v1/user/search/filter',
        params: filter,
      }),
      providesTags: ['User'],
    }),
    createUser: builder.mutation<ApiResponse<UserResponse>, CreateUserRequest>({
      query: (body) => ({ url: '/api/v1/user', method: 'POST', body }),
      invalidatesTags: ['User'],
    }),
    updateUser: builder.mutation<ApiResponse<UserResponse>, { id: number; body: UpdateUserRequest }>({
      query: ({ id, body }) => ({ url: `/api/v1/user/${id}`, method: 'PUT', body }),
      invalidatesTags: (result, error, { id }) => [{ type: 'User', id }, 'User'],
    }),
    deleteUser: builder.mutation<void, number>({
      query: (id) => ({ url: `/api/v1/user/${id}`, method: 'DELETE' }),
      invalidatesTags: ['User'],
    }),
  }),
});

export const {
  useGetUsersQuery,
  useGetUserByIdQuery,
  useFilterUsersQuery,
  useCreateUserMutation,
  useUpdateUserMutation,
  useDeleteUserMutation,
} = userApi;
```
```

Dosya: `.claude/rules/react-component.md`

```markdown
---
paths:
  - "frontend/src/components/**/*.tsx"
  - "frontend/src/pages/**/*.tsx"
  - "frontend/src/features/**/components/**/*.tsx"
---

# React Component Kurallari

- Fonksiyonel component + named export
- Props icin interface tanimla: interface UserCardProps { ... }
- any tipi YASAK, uygun tip tanimla veya unknown + type guard kullan
- UI onceligi: Golge UI > MUI > custom component
- MUI import: @mui/material, @mui/icons-material
- Golge UI import: @golge/ui (veya projedeki golge-ui yolu)
- Tailwind CSS + MUI sx prop birlikte kullanilabilir
- Form: React Hook Form + Zod schema ZORUNLU
- API state: RTK Query hook'lari (useGetXxxQuery, useCreateXxxMutation)
- Loading: MUI Skeleton veya CircularProgress
- Error: Alert veya Snackbar ile goster
- Empty: Typography ile "Kayit bulunamadi" mesaji
- Event handler isimlendirme: handleSubmit, handleClick, handleChange
- Memo: React.memo sadece gercek performans sorunu varsa kullan
```

Dosya: `.claude/rules/react-test.md`

```markdown
---
paths:
  - "frontend/src/**/*.test.tsx"
  - "frontend/src/**/*.test.ts"
---

# React Test Kurallari

- Vitest + @testing-library/react + @testing-library/user-event
- RTK Query mock: setupServer (MSW) ile API mock
- Redux store: renderWithProviders helper kullan
- DOM sorgulari: getByRole > getByLabelText > getByText > getByTestId
- Async: waitFor, findByRole kullan
- MUI component testi: role-based query kullan (button, textbox, combobox)
- Snapshot test YAZMA, davranis testi yaz
```

Dosya: `.claude/rules/spring-test.md`

```markdown
---
paths:
  - "backend/server/src/test/**"
---

# Spring Boot Test Kurallari

- Unit test: @ExtendWith(MockitoExtension.class), @Mock, @InjectMocks
- Integration test: @SpringBootTest + @AutoConfigureMockMvc
- Repository test: @DataJpaTest (in-memory H2)
- WebMvc test: @WebMvcTest(UserSearchController.class)
- Test isimlendirme: shouldReturnUser_whenValidId / should_throw_whenNotFound
- Given-When-Then pattern
- Testcontainers kullan (PostgreSQL integration test)
- SearchService ve OperationalService AYRI test siniflarinda test et

Ornek:
@Test
void shouldReturnPagedUsers_whenFilterApplied() {
    // given
    UserFilterRequest filter = UserFilterRequest.builder().status(UserStatus.ACTIVE).build();
    Page<User> mockPage = new PageImpl<>(List.of(testUser));
    when(userRepository.findAll(any(Specification.class), any(Pageable.class))).thenReturn(mockPage);

    // when
    Page<UserResponse> result = userSearchService.filter(filter, PageRequest.of(0, 10));

    // then
    assertThat(result.getContent()).hasSize(1);
    assertThat(result.getContent().get(0).getName()).isEqualTo("Test User");
}
```

---

## 2. MCP ORNEKLERI

### 2a. Proje Bazli MCP Yapilandirmasi

Dosya: `.mcp.json` (proje koku)

```json
{
  "mcpServers": {
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "postgres": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "${DATABASE_URL}"
      }
    }
  }
}
```

Kullanim:

```
"users tablosunun semasini goster"
"orders tablosundaki son 10 siparis kaydini getir"
"GitHub'da #23 numarali issue'yu oku"
"Son acik PR'larin durumunu kontrol et"
```

### 2b. MCP Hizli Komutlar

```bash
# PostgreSQL ekle
claude mcp add --transport stdio postgres \
  --env DATABASE_URL=postgresql://user:pass@localhost:5432/golge \
  -- npx -y @anthropic-ai/mcp-server-postgres

# GitHub ekle
claude mcp add --transport stdio --env GITHUB_TOKEN=ghp_xxxx github \
  -- npx -y @anthropic-ai/mcp-server-github

# Notion ekle (HTTP)
claude mcp add --transport http notion https://mcp.notion.com/mcp

# Listele / Kaldir / Durum
claude mcp list
claude mcp remove postgres
/mcp  # oturum icinde
```

> **Best Practice:** Token'lari `.mcp.json`'a dogrudan yazma.
> `${ENV_VAR}` kullan, gercek degerler shell ortaminda kalsin.

---

## 3. SUBAGENT (AJAN) ORNEKLERI

### 3a. Spring Boot Reviewer (Search + Operational Pattern)

Dosya: `.claude/agents/spring-reviewer.md`

```markdown
---
name: spring-reviewer
description: Spring Boot kodunu Search/Operational pattern, multi-module yapisi ve Spring best practice'leri acisindan inceler
tools: Read, Glob, Grep, Bash
model: sonnet
---

Spring Boot kodu icin detayli code review yap.

## Mimari Kontrol

- SearchService SADECE okuma islemi mi yapiyor? (yazma islemi varsa UYAR)
- OperationalService SADECE CUD islemi mi yapiyor? (okuma varsa UYAR)
- DTO siniflarinin TAMAMI common modulunde mi? (server'da DTO varsa UYAR)
- Entity siniflarinin TAMAMI server modulunde mi? (common'da entity varsa UYAR)
- Controller → dogru Service'e mi yonlendiriyor?
  - SearchController → SearchService
  - OperationalController → OperationalService
- Field injection (@Autowired) kullanilmis mi? (Constructor injection olmali)
- Entity dogrudan response olarak donuluyor mu? (DTO + MapStruct kullanilmali)

## Spring Security
- Endpoint'ler yetkilendirilmis mi?
- OperationalController'da @PreAuthorize var mi? (yazma islemleri korunmali)
- JWT token dogrulama eksik mi?
- Hassas veri response'ta ifsa ediliyor mu?

## Veritabani / JPA
- N+1 sorgu problemi: @EntityGraph veya JOIN FETCH gerekli mi?
- @Transactional(readOnly=true) SearchService'te kullanilmis mi?
- @Transactional OperationalService'te var mi?
- Index eksik mi?

## Common Module
- DTO'da JPA annotation var mi? (OLMAMALI)
- Enum'larda gereksiz Spring dependency var mi?
- Constants sinifi statik final mi?

## Raporlama
- KRITIK: Guvenlik acigi, veri kaybi, pattern ihlali
- UYARI: Best practice ihlali, performans sorunu
- ONERI: Iyilestirme firsati
```

### 3b. React + RTK Query Reviewer

Dosya: `.claude/agents/react-reviewer.md`

```markdown
---
name: react-reviewer
description: React + TypeScript + RTK Query + MUI kodunu inceler
tools: Read, Glob, Grep
model: sonnet
---

React kodunu incele.

## TypeScript
- any tipi kullanilmis mi? (YASAK - uygun tip tanimla)
- as type assertion gereksiz kullanilmis mi?
- Interface vs Type dogru kullanilmis mi?
- Strict mode ihlali var mi?

## RTK Query
- API cagrilari RTK Query disinda mi yapilmis? (axios/fetch dogrudan YASAK)
- Tag invalidation dogru yapilandirilmis mi?
- Cache stratejisi uygun mu?
- Error handling var mi? (isError, error kontrolu)
- Loading state handle ediliyor mu? (isLoading, isFetching)

## Component
- Golge UI / MUI component varken custom component mi yazilmis?
- Gereksiz re-render var mi?
- Props drilling var mi? (Redux/Context kullanilmali)
- Key prop dogru kullanilmis mi? (index key YASAK)
- Form'da React Hook Form + Zod kullanilmis mi?

## Guvenlik
- XSS: dangerouslySetInnerHTML YASAK
- API token client-side'da ifsa ediliyor mu?
- Kullanici girdisi sanitize ediliyor mu?
```

### 3c. Spring Boot Test Yazici (Search + Operational)

Dosya: `.claude/agents/spring-test-writer.md`

```markdown
---
name: spring-test-writer
description: Spring Boot Search/Operational servisler icin JUnit 5 + Mockito testleri yazar
tools: Read, Glob, Grep, Write, Edit, Bash
model: sonnet
---

Verilen Spring Boot sinifi icin kapsamli testler yaz.

## SearchService Test
- @ExtendWith(MockitoExtension.class)
- @Mock UserRepository, @InjectMocks UserSearchServiceImpl
- findAll: bos liste, dolu liste, sayfalama
- findById: bulundu, bulunamadi (ResourceNotFoundException)
- filter: farkli filtre kombinasyonlari, bos filtre
- @Transactional(readOnly=true) oldugunu dogrula (bilinc kontrolu)

## OperationalService Test
- create: basarili, validation hatasi, duplicate
- update: basarili, bulunamadi, kismen guncelleme
- delete: basarili, bulunamadi
- verify(repository.save()) cagrildigi dogrula
- Event publish dogrula (gerekiyorsa)

## Controller Test
- @WebMvcTest(UserSearchController.class) veya @WebMvcTest(UserOperationalController.class)
- MockMvc ile HTTP isteklarini simule et
- JSON response icerigi ve HTTP durum kodu
- @WithMockUser ile security testleri
- SearchController: GET /api/v1/user/search, GET /api/v1/user/search/{id}, GET /api/v1/user/search/filter
- OperationalController: POST /api/v1/user, PUT /api/v1/user/{id}, DELETE /api/v1/user/{id}

Test bittikten sonra `cd backend && ./gradlew test --tests "*.XxxTest"` calistir.
```

### 3d. React Test Yazici (RTK Query + MUI)

Dosya: `.claude/agents/react-test-writer.md`

```markdown
---
name: react-test-writer
description: React + RTK Query + MUI componentleri icin test yazar
tools: Read, Glob, Grep, Write, Edit, Bash
model: sonnet
---

## Kurallar
- Vitest + @testing-library/react + @testing-library/user-event
- RTK Query mock: MSW ile API mock VEYA vi.mock ile api dosyasini mock'la
- Redux store: test icin ozel store olustur (configureStore)
- MUI component: role-based query (button, textbox, dialog, alert)
- Snapshot test YAZMA, davranis testi yaz

## Wrapper

```tsx
import { configureStore } from '@reduxjs/toolkit';
import { Provider } from 'react-redux';
import { baseApi } from '@/services/baseApi';
import { BrowserRouter } from 'react-router-dom';

function renderWithProviders(ui: React.ReactElement, options = {}) {
  const store = configureStore({
    reducer: { [baseApi.reducerPath]: baseApi.reducer },
    middleware: (getDefault) => getDefault().concat(baseApi.middleware),
  });

  return render(
    <Provider store={store}>
      <BrowserRouter>{ui}</BrowserRouter>
    </Provider>,
    options,
  );
}
```

Test bittikten sonra `cd frontend && npm run test -- --run` calistir.
```

### 3e. Gradle/Shadow Uzmani

Dosya: `.claude/agents/gradle-expert.md`

```markdown
---
name: gradle-expert
description: Gradle build, Shadow JAR, multi-module yapilandirma ve dependency yonetimi
tools: Read, Glob, Grep, Bash
model: sonnet
---

Gradle ve Shadow JAR konularinda uzman.

## Multi-Module Yapisi
- settings.gradle.kts: include(":common", ":server")
- server depends on common: implementation(project(":common"))
- common modulunde Spring dependency OLMAMALI

## Shadow JAR
- Plugin: com.github.johnrengelman.shadow
- relocate: package conflict cozumu
- mergeServiceFiles(): META-INF/services birlestirme
- exclude: gereksiz dosyalari cikar

## Sorun Giderme
1. `./gradlew dependencies --configuration runtimeClasspath`
2. `./gradlew build --scan`
3. `./gradlew shadowJar --info`
4. Conflict: exclude veya relocate kullan
```

### 3f. Flyway Migration Yazici

Dosya: `.claude/agents/migration-writer.md`

```markdown
---
name: migration-writer
description: Flyway SQL migration dosyalari olusturur
tools: Read, Glob, Grep, Write, Bash
model: sonnet
---

## Kurallar
- Dosya adi: V<numara>__<aciklama>.sql (cift alt cizgi!)
- Konum: server/src/main/resources/db/migration/
- Mevcut migration'lari kontrol et, siradaki numarayi bul
- NOT NULL eklerken DEFAULT deger belirle
- Index eklemeyi unutma
- FK isimlendirme: fk_<tablo>_<referans>
- Migration sonrasi `./gradlew flywayInfo` calistir
```

### 3g. Izole Refactoring Agent

Dosya: `.claude/agents/refactorer.md`

```markdown
---
name: refactorer
description: Izole worktree'de guvenli refactoring yapar
tools: Read, Glob, Grep, Edit, Write, Bash
model: sonnet
isolation: worktree
---

Refactoring kurallari:
- Davranisi degistirme, sadece yapiyi iyilestir
- Backend: her adimda `./gradlew test`
- Frontend: her adimda `cd frontend && npm run test -- --run`
- Mevcut API kontratini bozma
- Search/Operational ayrimina uy
```

---

## 3+. AGENT TEAMS ORNEKLERI (Deneysel)

Agent Teams, birden fazla Claude oturumunun paralel ve koordineli calismasini saglar.
Subagent'lardan farki: teammate'ler bagimsiz calisan tam Claude oturumlaridir.

### 3+a. Paralel Modul Gelistirme

```bash
# Aktiflestime
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
claude
```

Team Lead'e yazdiginiz:
```
User ve Basvuru modulleri icin backend + frontend kodunu paralel gelistir.
Her modul icin Search/Operational pattern uygula.
common modulune DTO'lari, server modulune entity/service/controller yaz.
Frontend icinde RTK Query api ve MUI sayfalari olustur.
```

Team Lead'in olusturacagi gorev dagitimi:
```
Task 1: [Teammate-User-Backend]
  "common/dto/user/ altinda DTO'lar, server/entity/User.java,
   UserSearchServiceImpl, UserOperationalServiceImpl,
   UserSearchController, UserOperationalController olustur"

Task 2: [Teammate-Basvuru-Backend]
  "common/dto/basvuru/ altinda DTO'lar, server/entity/Basvuru.java,
   BasvuruSearchServiceImpl, BasvuruOperationalServiceImpl,
   BasvuruSearchController, BasvuruOperationalController olustur"

Task 3: [Teammate-User-Frontend]  blockedBy: [Task 1]
  "userApi.ts (RTK Query), UserListPage.tsx, UserDetailPage.tsx,
   UserForm.tsx olustur. MUI DataGrid + Golge UI kullan"

Task 4: [Teammate-Basvuru-Frontend]  blockedBy: [Task 2]
  "basvuruApi.ts (RTK Query), BasvuruListPage.tsx, BasvuruWizard.tsx
   olustur. MUI Stepper + Golge UI kullan"

Task 5: [Teammate-Test]  blockedBy: [Task 1, Task 2, Task 3, Task 4]
  "Tum modullerin backend unit test + frontend component testlerini yaz"
```

### 3+b. Paralel Code Review Takimi

Team Lead'e yazdiginiz:
```
Son commit'teki degisiklikleri 3 farkli acidan paralel incele:
1. Spring Boot best practices ve Search/Operational pattern uyumu
2. React + TypeScript + RTK Query best practices
3. Guvenlik ve performans
```

Sonuc: Her teammate bagimsiz inceleme yapar, bulgulari gorev listesine yazar.
Team Lead sonuclari birlestirir.

### 3+c. Competing Hypotheses (Rakip Cozumler)

Team Lead'e yazdiginiz:
```
Basvuru modulu icin dosya yukleme mimarisini 2 farkli yaklasmla tasarla:
Teammate-1: Spring Content + S3 ile
Teammate-2: Dogrudan MinIO client ile
Her ikisi de plan yazsin, ben secerim.
```

Sonuc: Iki teammate paralel olarak iki farkli mimari plan yazar.
Siz karsilastirip hangisini uygulayacaginizi secersiniz.

### Klavye Kisayollari (Agent Teams)

| Kisayol | Islem |
|---------|-------|
| `Shift+Down` | Teammate'ler arasi gecis (in-process mod) |
| `Ctrl+T` | Gorev listesini goster |
| `Enter` | Secili teammate oturumunu gor |
| `Escape` | Teammate'i durdur |

> **Dikkat:** Agent Teams deneysel ozelliktir.
> `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` env var gereklidir.
> Maliyet takim boyutuyla dogrusal artar - her teammate ayri context kullanir.

---

## 4. SKILLS ORNEKLERI

### 4a. Spring Boot Endpoint Seti Olusturucu

Dosya: `.claude/skills/endpoint.md`

```markdown
---
name: endpoint
description: Spring Boot Search + Operational endpoint seti olusturur (common DTO + server entity/service/controller + test)
tools: Write, Read, Glob, Edit
---

Verilen entity adi icin tum katmanlari olustur.

## common modulunde olustur:
1. `dto/<ad>/Create<Ad>Request.java` - Create DTO (@NotBlank, @Size vb.)
2. `dto/<ad>/Update<Ad>Request.java` - Update DTO
3. `dto/<ad>/<Ad>Response.java` - Response DTO (@Builder)
4. `dto/<ad>/<Ad>FilterRequest.java` - Filtreleme DTO (nullable alanlar)

## server modulunde olustur:
5. `entity/<Ad>.java` - JPA Entity (@Entity, @Table, Lombok, audit alanlari)
6. `repository/<Ad>Repository.java` - Spring Data JPA + custom query
7. `mapper/<Ad>Mapper.java` - MapStruct mapper
8. `service/search/<Ad>SearchService.java` - Search interface
9. `service/search/impl/<Ad>SearchServiceImpl.java` - Search impl (@Transactional readOnly)
10. `service/operational/<Ad>OperationalService.java` - Operational interface
11. `service/operational/impl/<Ad>OperationalServiceImpl.java` - Operational impl (@Transactional)
12. `controller/search/<Ad>SearchController.java` - GET endpoints
13. `controller/operational/<Ad>OperationalController.java` - POST/PUT/DELETE endpoints
14. `test: <Ad>SearchServiceTest.java` - Search service test
15. `test: <Ad>OperationalServiceTest.java` - Operational service test

Mevcut package yapisi ve pattern'lari kontrol et, ona uygun olustur.
```

Kullanim:
```
/endpoint Product
/endpoint OrderItem
```

### 4b. React Feature Olusturucu (RTK Query)

Dosya: `.claude/skills/feature-ui.md`

```markdown
---
name: feature-ui
description: React feature modulu olusturur (RTK Query api + Redux slice + sayfa + componentler + test)
tools: Write, Read, Glob, Edit
---

Verilen feature adi icin tum dosyalari olustur.

## Olusturulacak Dosyalar

1. `src/features/<feature>/types.ts` - TypeScript tipleri (backend DTO'larla eslesir)
2. `src/features/<feature>/api.ts` - RTK Query endpoints (inject to baseApi)
3. `src/features/<feature>/slice.ts` - Redux slice (client state, opsiyonel)
4. `src/features/<feature>/components/<Feature>List.tsx` - Liste componenti (MUI DataGrid)
5. `src/features/<feature>/components/<Feature>Form.tsx` - Form (React Hook Form + Zod + MUI)
6. `src/features/<feature>/components/<Feature>Detail.tsx` - Detay componenti
7. `src/pages/<Feature>Page.tsx` - Sayfa (list + routing)
8. `src/pages/<Feature>DetailPage.tsx` - Detay sayfasi
9. `src/features/<feature>/__tests__/<Feature>List.test.tsx` - Test

RTK Query tag invalidation, MUI/Golge UI componentleri ve TypeScript strict mode'a dikkat et.
React Router'a route eklemeyi unutma.
```

Kullanim:
```
/feature-ui user
/feature-ui product
```

### 4c. Flyway Migration Skill

Dosya: `.claude/skills/migration.md`

```markdown
---
name: migration
description: Flyway SQL migration dosyasi olusturur
tools: Write, Read, Glob, Bash
---

1. `server/src/main/resources/db/migration/` dizinindeki mevcut migration'lari oku
2. Son version numarasini bul ve bir artir
3. Migration dosyasini olustur: `V{N}__{aciklama}.sql`
4. `cd backend && ./gradlew flywayInfo` ile dogrula

Kurallar:
- NOT NULL eklerken DEFAULT deger ver
- Index ekle (sik sorgulanan alanlar icin)
- FK: ON DELETE SET NULL veya CASCADE belirt
- COMMENT ON COLUMN ile alan aciklamasi
```

Kullanim:
```
/migration users tablosuna phone_number alani ekle
/migration orders ve order_items tablolarini olustur
```

### 4d. Feature Branch Workflow

Dosya: `.claude/skills/feature.md`

```markdown
---
name: feature
description: Yeni ozellik icin branch olusturur ve ortami hazirlar
tools: Bash, Read, Glob
---

1. `git checkout main && git pull origin main`
2. `git checkout -b feature/<ozellik-adi>`
3. Backend test: `cd backend && ./gradlew test`
4. Frontend test: `cd frontend && npm run test -- --run`
5. Baslangic durumunu raporla
```

Kullanim:
```
/feature siparis takip sistemi
```

### 4e. Gradle Dependency Ekleyici

Dosya: `.claude/skills/dep.md`

```markdown
---
name: dep
description: Gradle dependency ekler (common veya server modulune)
tools: Read, Edit, Bash
---

1. Hangi module eklenecegini belirle:
   - common'a: saf Java kutuphaneleri (validation, util)
   - server'a: Spring, JPA, veritabani kutuphaneleri
2. Uygun build.gradle.kts dosyasini oku ve dependency ekle
3. `./gradlew dependencies --configuration runtimeClasspath | grep <kutuphane>` ile dogrula
4. Gerekiyorsa application.yml veya Config sinifi guncelle
```

Kullanim:
```
/dep Redis cache ekle server modulune
/dep commons-lang3 ekle common modulune
```

---

## 5. HOOK ORNEKLERI

### 5a. Java Dosyalarinda Checkstyle

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "FILE=$(jq -r '.tool_input.file_path // empty'); if echo \"$FILE\" | grep -qE '\\.java$'; then cd backend && ./gradlew checkstyleMain --quiet 2>/dev/null || true; fi"
          }
        ]
      }
    ]
  }
}
```

### 5b. Frontend Otomatik Prettier + ESLint

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "FILE=$(jq -r '.tool_input.file_path // empty'); if echo \"$FILE\" | grep -qE '\\.(tsx?|jsx?|css)$'; then cd frontend && npx prettier --write \"$FILE\" && npx eslint --fix \"$FILE\" 2>/dev/null || true; fi"
          }
        ]
      }
    ]
  }
}
```

### 5c. Tehlikeli Komutlari Engelle

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "CMD=$(jq -r '.tool_input.command'); if echo \"$CMD\" | grep -qiE 'drop table|drop database|truncate|flywayClean|git push.*--force|rm -rf /'; then echo 'Tehlikeli komut engellendi' >&2; exit 2; fi"
          }
        ]
      }
    ]
  }
}
```

### 5d. Commit Oncesi Backend + Frontend Test

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "CMD=$(jq -r '.tool_input.command'); if echo \"$CMD\" | grep -q 'git commit'; then cd backend && ./gradlew test --quiet 2>&1 || { echo 'Backend testleri basarisiz!' >&2; exit 2; }; cd ../frontend && npm run test -- --run 2>&1 || { echo 'Frontend testleri basarisiz!' >&2; exit 2; }; fi"
          }
        ]
      }
    ]
  }
}
```

### 5e. Oturum Basinda Ortam Kontrolu

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "echo '=== Ortam Kontrolu ==='; java -version 2>&1 | head -1; echo \"Gradle: $(cd backend 2>/dev/null && ./gradlew --version 2>/dev/null | grep 'Gradle' | head -1)\"; node -v 2>/dev/null && echo 'Node.js OK' || echo 'Node.js BULUNAMADI'; echo \"Git: $(git branch --show-current 2>/dev/null)\"; git status --short 2>/dev/null | head -5"
          }
        ]
      }
    ]
  }
}
```

### 5f. Migration Dosyasi Duzenleme Uyarisi

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "FILE=$(jq -r '.tool_input.file_path // empty'); if echo \"$FILE\" | grep -q 'db/migration/V'; then echo 'UYARI: Mevcut migration dosyasi DUZENLENMEMELI! Yeni migration dosyasi olustur.' >&2; exit 2; fi"
          }
        ]
      }
    ]
  }
}
```

---

## 6. IZIN KURALLARI

### 6a. Full-Stack (Backend + Frontend)

Dosya: `.claude/settings.json`

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Bash(cd backend && ./gradlew *)",
      "Bash(cd frontend && npm run *)",
      "Bash(./gradlew *)",
      "Bash(git status)",
      "Bash(git log *)",
      "Bash(git diff *)",
      "Bash(git add *)",
      "Bash(git checkout -b *)",
      "Bash(git branch *)",
      "Edit(backend/common/src/**)",
      "Edit(backend/server/src/**)",
      "Edit(frontend/src/**)",
      "Write(backend/common/src/**)",
      "Write(backend/server/src/**)",
      "Write(frontend/src/**)",
      "Edit(backend/**/build.gradle.kts)",
      "Edit(backend/server/src/main/resources/application*.yml)",
      "Write(backend/server/src/main/resources/db/migration/*.sql)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force *)",
      "Bash(git reset --hard *)",
      "Bash(./gradlew flywayClean*)",
      "Edit(backend/server/src/main/resources/application-prod.yml)",
      "Edit(.env*)",
      "Write(.env*)",
      "Edit(**/*secret*)",
      "Edit(**/*credential*)"
    ]
  }
}
```

### 6b. Code Review Modu (Salt Okuma)

```json
{
  "permissions": {
    "defaultMode": "plan",
    "allow": [
      "Read", "Glob", "Grep",
      "Bash(git log *)", "Bash(git diff *)", "Bash(git status)",
      "Bash(./gradlew dependencies *)"
    ],
    "deny": [
      "Edit(*)", "Write(*)", "Bash(git commit *)", "Bash(git push *)"
    ]
  }
}
```

---

## 7. PROGRESS.MD - IS TAKIP PATTERN'I

Her gorev icin `progress.md` olustur, bitince sil. Claude'a bu pattern'i ogret:

Dosya: `.claude/rules/progress-tracking.md`

```markdown
# Progress Tracking Kurali

Her yeni gorev basladiginda:

1. Proje kokunde `progress.md` dosyasi OLUSTUR:

```markdown
# Gorev: <gorev basligi>
Baslangic: <tarih/saat>

## Yapilacaklar
- [ ] Adim 1 aciklama
- [ ] Adim 2 aciklama
- [ ] Adim 3 aciklama

## Tamamlananlar
(henuz yok)

## Notlar
- Onemli kararlar veya sorunlar buraya yazilir
```

2. Her adim tamamlandiginda progress.md'yi GUNCELLE:
   - Tamamlanan adimi [x] olarak isaretle ve "Tamamlananlar" bolumune tasi
   - Yeni kesfedilen adimlari "Yapilacaklar"a ekle
   - Sorunlari "Notlar"a yaz

3. TUM adimlar tamamlandiginda:
   - progress.md dosyasini SIL
   - Kullaniciya ozet rapor ver

Bu pattern sayesinde:
- Uzun gorevlerde nerede kaldigimizi biliriz
- Baglam kaybi olmaz (compact/session restart durumunda)
- Kullanici ilerlemeyi takip edebilir
```

### Progress.md Ornek Kullanim

```
"Siparis modulu icin backend + frontend endpoint'lerini olustur"
```

Claude otomatik olarak olusturur:

```markdown
# Gorev: Siparis Modulu Olusturma

## Yapilacaklar
- [ ] common: Order DTO'lari olustur (CreateOrderRequest, OrderResponse, OrderFilterRequest)
- [ ] common: OrderStatus enum olustur
- [ ] server: Order entity olustur
- [ ] server: Flyway migration yaz (V5__create_orders_table.sql)
- [ ] server: OrderRepository olustur
- [ ] server: OrderSearchService + Impl olustur
- [ ] server: OrderOperationalService + Impl olustur
- [ ] server: OrderSearchController olustur
- [ ] server: OrderOperationalController olustur
- [ ] server: OrderMapper (MapStruct) olustur
- [ ] server: Test yaz ve calistir
- [ ] frontend: Order feature modulu olustur (RTK Query api.ts)
- [ ] frontend: OrderList component (MUI DataGrid)
- [ ] frontend: OrderForm component (React Hook Form + Zod)
- [ ] frontend: OrderPage ve routing
- [ ] frontend: Test yaz ve calistir

## Tamamlananlar
(henuz yok)

## Notlar
(henuz yok)
```

Her adim tamamlandikca guncellenir, tum adimlar bitince dosya silinir.

---

## 8. GUNLUK IS AKISI

```bash
# 1. Sabah: Durumu kontrol et
claude -c
# > "Git status, son commitler ve testlerin durumunu goster"

# 2. Yeni gorev: Plan Mode'da analiz
# > Shift+Tab (Plan Mode)
# > "Siparis modulu icin backend ve frontend degisikliklerini planla"
# (Claude progress.md olusturur)

# 3. Backend: common + server
# > Shift+Tab (Normal Mode)
# > "/endpoint Order"
# > "OrderSearchService'e filtreleme ekle"

# 4. Migration
# > "/migration orders tablosunu olustur"

# 5. Backend test
# > "OrderSearchService ve OrderOperationalService testlerini yaz ve calistir"

# 6. Frontend
# > "/feature-ui order"
# > "RTK Query endpoint'lerini backend API'ye bagla"

# 7. Frontend test
# > "OrderList ve OrderForm testlerini yaz"

# 8. Review
# > "Tum degisiklikleri incele"
# (Claude progress.md'yi siler, ozet verir)

# 9. Commit
# > "Commit et: siparis modulu eklendi"

# 10. Baglam temizle
# > /compact
```

---

## 9. GRADLE MULTI-MODULE YAPILANDIRMA ORNEGI

### settings.gradle.kts

```kotlin
rootProject.name = "golge-platform"

include(":common", ":server")

project(":common").projectDir = file("common")
project(":server").projectDir = file("server")
```

### backend/build.gradle.kts (root)

```kotlin
plugins {
    java
    id("org.springframework.boot") version "3.3.0" apply false
    id("io.spring.dependency-management") version "1.1.5" apply false
}

subprojects {
    apply(plugin = "java")

    group = "com.golge"
    version = "0.0.1-SNAPSHOT"

    java {
        sourceCompatibility = JavaVersion.VERSION_21
    }

    repositories {
        mavenCentral()
    }
}
```

### common/build.gradle.kts

```kotlin
// common: SADECE saf Java kutuphaneleri, Spring YOK

dependencies {
    // Validation (Jakarta - Spring'siz)
    implementation("jakarta.validation:jakarta.validation-api:3.1.0")

    // Lombok
    compileOnly("org.projectlombok:lombok:1.18.32")
    annotationProcessor("org.projectlombok:lombok:1.18.32")

    // Test
    testImplementation("org.junit.jupiter:junit-jupiter:5.10.2")
}

tasks.withType<Test> {
    useJUnitPlatform()
}
```

### server/build.gradle.kts

```kotlin
plugins {
    id("org.springframework.boot")
    id("io.spring.dependency-management")
    id("com.github.johnrengelman.shadow") version "8.1.1"
}

dependencies {
    // Common module
    implementation(project(":common"))

    // Spring Boot
    implementation("org.springframework.boot:spring-boot-starter-web")
    implementation("org.springframework.boot:spring-boot-starter-data-jpa")
    implementation("org.springframework.boot:spring-boot-starter-validation")
    implementation("org.springframework.boot:spring-boot-starter-security")
    implementation("org.springframework.boot:spring-boot-starter-cache")
    implementation("org.springframework.boot:spring-boot-starter-actuator")

    // Database
    runtimeOnly("org.postgresql:postgresql")
    implementation("org.flywaydb:flyway-core")
    implementation("org.flywaydb:flyway-database-postgresql")

    // JWT
    implementation("io.jsonwebtoken:jjwt-api:0.12.5")
    runtimeOnly("io.jsonwebtoken:jjwt-impl:0.12.5")
    runtimeOnly("io.jsonwebtoken:jjwt-jackson:0.12.5")

    // MapStruct
    implementation("org.mapstruct:mapstruct:1.5.5.Final")
    annotationProcessor("org.mapstruct:mapstruct-processor:1.5.5.Final")

    // Lombok
    compileOnly("org.projectlombok:lombok")
    annotationProcessor("org.projectlombok:lombok")
    annotationProcessor("org.projectlombok:lombok-mapstruct-binding:0.2.0")

    // OpenAPI
    implementation("org.springdoc:springdoc-openapi-starter-webmvc-ui:2.5.0")

    // Test
    testImplementation("org.springframework.boot:spring-boot-starter-test")
    testImplementation("org.springframework.security:spring-security-test")
    testImplementation("org.testcontainers:junit-jupiter:1.19.7")
    testImplementation("org.testcontainers:postgresql:1.19.7")
    testRuntimeOnly("com.h2database:h2")
}

// Shadow JAR
tasks.shadowJar {
    archiveBaseName.set("golge-api")
    archiveClassifier.set("all")
    mergeServiceFiles()
    manifest {
        attributes["Main-Class"] = "com.golge.server.GolgeApplication"
    }
}

tasks.withType<Test> {
    useJUnitPlatform()
    testLogging {
        events("passed", "skipped", "failed")
    }
}
```

---

## 10. FRONTEND YAPILANDIRMA ORNEGI

### RTK Query baseApi

Dosya: `frontend/src/services/baseApi.ts`

```typescript
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

export const baseApi = createApi({
  reducerPath: 'api',
  baseQuery: fetchBaseQuery({
    baseUrl: import.meta.env.VITE_API_BASE_URL || 'http://localhost:8080',
    prepareHeaders: (headers) => {
      const token = localStorage.getItem('token');
      if (token) {
        headers.set('Authorization', `Bearer ${token}`);
      }
      return headers;
    },
  }),
  tagTypes: ['User', 'Product', 'Order'],
  endpoints: () => ({}),
});
```

### Redux Store

Dosya: `frontend/src/store/index.ts`

```typescript
import { configureStore } from '@reduxjs/toolkit';
import { baseApi } from '@/services/baseApi';

export const store = configureStore({
  reducer: {
    [baseApi.reducerPath]: baseApi.reducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(baseApi.middleware),
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

### Ornek Feature: User (RTK Query + MUI)

Dosya: `frontend/src/features/user/api.ts`

```typescript
import { baseApi } from '@/services/baseApi';
import type { ApiResponse, PageResponse } from '@/types/api';
import type { UserResponse, CreateUserRequest, UpdateUserRequest, UserFilterRequest } from './types';

export const userApi = baseApi.injectEndpoints({
  endpoints: (builder) => ({
    // Search endpoints (GET)
    getUsers: builder.query<ApiResponse<PageResponse<UserResponse>>, { page: number; size: number }>({
      query: ({ page, size }) => `/api/v1/user/search?page=${page}&size=${size}`,
      providesTags: ['User'],
    }),
    getUserById: builder.query<ApiResponse<UserResponse>, number>({
      query: (id) => `/api/v1/user/search/${id}`,
      providesTags: (result, error, id) => [{ type: 'User', id }],
    }),
    filterUsers: builder.query<ApiResponse<PageResponse<UserResponse>>, UserFilterRequest>({
      query: (filter) => ({ url: '/api/v1/user/search/filter', params: filter }),
      providesTags: ['User'],
    }),

    // Operational endpoints (CUD)
    createUser: builder.mutation<ApiResponse<UserResponse>, CreateUserRequest>({
      query: (body) => ({ url: '/api/v1/user', method: 'POST', body }),
      invalidatesTags: ['User'],
    }),
    updateUser: builder.mutation<ApiResponse<UserResponse>, { id: number; body: UpdateUserRequest }>({
      query: ({ id, body }) => ({ url: `/api/v1/user/${id}`, method: 'PUT', body }),
      invalidatesTags: (result, error, { id }) => [{ type: 'User', id }, 'User'],
    }),
    deleteUser: builder.mutation<void, number>({
      query: (id) => ({ url: `/api/v1/user/${id}`, method: 'DELETE' }),
      invalidatesTags: ['User'],
    }),
  }),
});

export const {
  useGetUsersQuery,
  useGetUserByIdQuery,
  useFilterUsersQuery,
  useCreateUserMutation,
  useUpdateUserMutation,
  useDeleteUserMutation,
} = userApi;
```

Dosya: `frontend/src/pages/UserPage.tsx`

```tsx
import { useState } from 'react';
import {
  Box, Typography, Button, CircularProgress, Alert,
  Paper, IconButton, Tooltip,
} from '@mui/material';
import { DataGrid, type GridColDef } from '@mui/x-data-grid';
import { Add as AddIcon, Edit as EditIcon, Delete as DeleteIcon } from '@mui/icons-material';
import { useGetUsersQuery, useDeleteUserMutation } from '@/features/user/api';
import type { UserResponse } from '@/features/user/types';

export function UserPage() {
  const [page, setPage] = useState(0);
  const [pageSize, setPageSize] = useState(10);

  const { data, isLoading, error } = useGetUsersQuery({ page, size: pageSize });
  const [deleteUser] = useDeleteUserMutation();

  const columns: GridColDef<UserResponse>[] = [
    { field: 'id', headerName: 'ID', width: 70 },
    { field: 'name', headerName: 'Ad', flex: 1 },
    { field: 'email', headerName: 'E-posta', flex: 1 },
    { field: 'status', headerName: 'Durum', width: 120 },
    {
      field: 'actions',
      headerName: 'Islemler',
      width: 120,
      renderCell: (params) => (
        <Box>
          <Tooltip title="Duzenle">
            <IconButton size="small"><EditIcon /></IconButton>
          </Tooltip>
          <Tooltip title="Sil">
            <IconButton size="small" color="error" onClick={() => deleteUser(params.row.id)}>
              <DeleteIcon />
            </IconButton>
          </Tooltip>
        </Box>
      ),
    },
  ];

  if (error) return <Alert severity="error">Veriler yuklenemedi.</Alert>;

  return (
    <Box sx={{ p: 3 }}>
      <Box sx={{ display: 'flex', justifyContent: 'space-between', mb: 2 }}>
        <Typography variant="h5">Kullanicilar</Typography>
        <Button variant="contained" startIcon={<AddIcon />}>Yeni Kullanici</Button>
      </Box>
      <Paper>
        <DataGrid
          rows={data?.data?.content ?? []}
          columns={columns}
          loading={isLoading}
          paginationMode="server"
          rowCount={data?.data?.totalElements ?? 0}
          pageSizeOptions={[10, 25, 50]}
          paginationModel={{ page, pageSize }}
          onPaginationModelChange={(model) => {
            setPage(model.page);
            setPageSize(model.pageSize);
          }}
        />
      </Paper>
    </Box>
  );
}
```

---

## HIZLI BASLANGIC

Tum yapilandirmayi tek seferde olusturmak icin:

```bash
mkdir -p .claude/agents .claude/rules .claude/skills

claude -p "Bu Spring Boot (multi-module: common + server) + React projesini analiz et.
Su dosyalari olustur:
1. CLAUDE.md - monorepo rehberi
2. backend/CLAUDE.md - Spring Boot kurallari (Search/Operational pattern)
3. frontend/CLAUDE.md - React + RTK Query + MUI kurallari
4. .claude/settings.json - izin ve hook kurallari
5. .claude/agents/spring-reviewer.md
6. .claude/agents/react-reviewer.md
7. .claude/rules/spring-search-controller.md
8. .claude/rules/spring-operational-controller.md
9. .claude/rules/common-dto.md
10. .claude/rules/react-rtk-query.md
11. .claude/rules/progress-tracking.md
12. .claude/skills/endpoint.md - Spring endpoint olusturucu
13. .claude/skills/feature-ui.md - React feature olusturucu
14. .claude/skills/migration.md - Flyway migration olusturucu
Her dosyayi projenin mevcut yapisina uygun olustur."
```
