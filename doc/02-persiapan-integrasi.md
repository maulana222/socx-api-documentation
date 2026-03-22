# Persiapan integrasi

Checklist sebelum mulai hit API production.

## 1. IP statis & whitelist

- Pastikan server integrasi memakai **IP statis** (atau rentang yang disepakati).
- Kirim IP ke tim SOCX untuk **whitelist** di sisi API.
- Tanpa whitelist, request dapat ditolak di layer jaringan atau aplikasi.

## 2. Token (JWT)

1. Login ke portal reseller SOCX.
2. Buka **Settings** (atau menu yang menyediakan token API — **TBD** jika nama menu berubah).
3. Salin token dan simpan sebagai rahasia (environment variable / secret manager).
4. Header: `Authorization: Bearer <token>`.

**Jangan** menaruh token di repositori Git atau log aplikasi.

## 3. Konvensi `request_id`

- Harus **unik per percobaan transaksi baru** dari sisi Anda.
- Jika Anda mengirim ulang `request_id` yang **sudah pernah diproses** di server SOCX, API mengembalikan **respons yang sama** dengan transaksi tersebut (idempotensi).

Disarankan: prefix lingkungan + timestamp + random, contoh `STG-20250322-abc12`.

## 4. Lingkungan

| Lingkungan | Base URL | Catatan |
|------------|----------|---------|
| Production (contoh dokumen) | `https://indotechapi.socx.app/reseller/api/v1` | Sesuai `socx.md` |

**TBD:** URL sandbox / UAT dan kredensial uji — konfirmasi ke tim SOCX.

## 5. HTTPS

Selalu gunakan **HTTPS** untuk semua panggilan API.

## 6. Yang akan menyusul (inquiry)

Daftar harga / katalog produk / **inquiry** — tunggu URL dan kontrak dari tim SOCX, lalu tambahkan halaman di folder [inquiry](inquiry/README.md).
