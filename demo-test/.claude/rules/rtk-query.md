---
paths:
  - "frontend/src/api/**"
---

# RTK Query Kurallari

- baseApi'yi injectEndpoints ile genislet
- Her feature icin ayri API dosyasi
- Tag-based cache invalidation ZORUNLU (providesTags + invalidatesTags)
- Endpoint isimlendirme: getUsers, getUserById, createUser, updateUser, deleteUser
- Response tipi icin interface tanimla
- transformResponse ile backend response'unu unwrap et
