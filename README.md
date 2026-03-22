# Dokumentasi API SOCX (Reseller H2H)

Repositori ini berisi **dokumentasi teknis** untuk integrasi **Host-to-Host (H2H)** antara sistem Anda dan platform **SOCX** sebagai reseller. Dokumen ditulis dalam Bahasa Indonesia, dengan struktur alur mirip dokumentasi API marketplace sejenis ([Digiflazz Developer](https://developer.digiflazz.com/)): pengenalan → persiapan → transaksi → kode respons → contoh → pengujian.

---

## Apa yang tercakup

| Area | Status | Keterangan |
|------|--------|------------|
| Direct purchase (JSON / HTTP / XML) | Dokumentasi tersedia | Pembelian produk dengan `code`, `msisdn`, `request_id` |
| Cek saldo | Tersedia | `GET /saldo` |
| Cek status transaksi | Tersedia | `POST /status` |
| Kode respons (RC) | Tersedia | Tabel lengkap sukses, gagal, pending |
| Contoh respons pulsa & game | Tersedia | Skenario + interpretasi field |
| Skenario pengujian | Tersedia | Checklist QA integrasi |
| Deposit tiket | Lampiran | Di luar fokus direct purchase |
| Inquiry / daftar harga / katalog | Menyusul | Menunggu kontrak & URL dari tim API |

---

## Base URL & autentikasi (ringkas)

**Base URL (production — contoh dokumen):**

```
https://indotechapi.socx.app/reseller/api/v1
```

Semua path endpoint di dokumen ditulis relatif terhadap base URL di atas (placeholder: `{base_url}`).

**Autentikasi**

- Endpoint JSON (`/saldo`, `/purchase`, `/status`, `/deposit_ticket`, …): header  
  `Authorization: Bearer <JWT>`
- Endpoint HTTP purchase (`/http/purchase`): JWT dikirim sebagai parameter `token` (selain `code`, `msisdn`, `request_id`).

Token JWT diperoleh dari portal reseller SOCX (menu **Settings** — nama pasti dapat disesuaikan jika berubah).

**Jaringan**

- Server integrasi sebaiknya memakai **IP statis** dan didaftarkan untuk **whitelist** di sisi SOCX.

Detail lengkap: [Pengenalan](doc/01-pengenalan.md) · [Persiapan integrasi](doc/02-persiapan-integrasi.md).

---

## Daftar endpoint (quick reference)

| Metode | Path | Fungsi | Dokumentasi |
|--------|------|--------|-------------|
| `GET` | `/saldo` | Cek saldo deposit | [doc/transaksi-direct/cek-saldo.md](doc/transaksi-direct/cek-saldo.md) |
| `POST` | `/purchase` | Transaksi pembelian (JSON) — **disarankan** | [doc/transaksi-direct/pembelian-json-post.md](doc/transaksi-direct/pembelian-json-post.md) |
| `GET` / `POST` | `/http/purchase` | Pembelian via query atau form + `token` | [doc/transaksi-direct/pembelian-http.md](doc/transaksi-direct/pembelian-http.md) |
| `POST` | `/xml/purchase` | Pembelian format XML (`topUpRequest`) | [doc/transaksi-direct/pembelian-xml.md](doc/transaksi-direct/pembelian-xml.md) |
| `POST` | `/status` | Cek status by `request_id` | [doc/transaksi-direct/cek-status.md](doc/transaksi-direct/cek-status.md) |
| `POST` | `/deposit_ticket` | Buat tiket deposit | [doc/appendix-deposit-ticket.md](doc/appendix-deposit-ticket.md) |

**Idempotensi:** jika Anda mengirim ulang **`request_id` yang sama** dan transaksi itu sudah pernah diproses, API mengembalikan **respons yang sama** dengan transaksi tersebut (bukan transaksi baru).

**Pending (`rc = 68`):** transaksi diterima tetapi belum final; lanjutkan dengan polling **`/status`** dan/atau mekanisme callback jika sudah disepakati dengan tim SOCX (detail callback/XML `topUpReport` — review internal).

---

## Alur integrasi (disarankan)

1. **Persiapan:** whitelist IP, simpan JWT dengan aman, tentukan konvensi `request_id` (unik per order baru).
2. **Opsional:** `GET /saldo` sebelum transaksi besar.
3. **Purchase:** `POST /purchase` (atau jalur HTTP/XML jika sistem Anda mengharuskan).
4. **Jika `rc = 68`:** panggil `POST /status` dengan interval wajar hingga status final (`00` atau kode gagal).
5. **Simpan** `trxid`, `request_id`, `sn` (jika ada) untuk rekonsiliasi dan dukungan pelanggan.

Diagram dan skenario detail: [doc/transaksi-direct/contoh-respons-pulsa.md](doc/transaksi-direct/contoh-respons-pulsa.md) · [doc/transaksi-direct/skenario-pengujian.md](doc/transaksi-direct/skenario-pengujian.md).

---

## Struktur repositori

```
.
├── README.md                 ← Anda di sini (pengantar GitHub)
├── mkdocs.yml                ← Konfigurasi situs dokumentasi (Material)
├── requirements-docs.txt     ← Dependensi Python untuk MkDocs
├── .gitignore                ← Mengabaikan site/, venv, dll.
└── doc/                      ← Sumber Markdown (isi situs)
    ├── index.md              ← Beranda dokumentasi (daftar isi)
    ├── 01-pengenalan.md
    ├── 02-persiapan-integrasi.md
    ├── appendix-deposit-ticket.md
    ├── inquiry/
    │   └── README.md         ← Placeholder inquiry / katalog
    └── transaksi-direct/     ← Direct purchase, RC, contoh, uji
```

---

## Cara membaca dokumentasi

### Di GitHub (tanpa install)

Buka file Markdown di folder [`doc/`](./doc/), mulai dari [`doc/index.md`](doc/index.md). Tautan antar-file relatif berfungsi di antarmuka GitHub.

### Sebagai situs lokal (sidebar & pencarian)

Membutuhkan Python 3.10+.

```bash
# Dari root repositori ini (folder yang berisi mkdocs.yml)
pip install -r requirements-docs.txt
mkdocs serve
```

Buka browser ke **http://127.0.0.1:8000** — navigasi kiri, pencarian, dan penyalinan blok kode didukung oleh [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

**Build statis (folder `site/`):**

```bash
mkdocs build
```

Output di `site/` (tidak di-commit; ada di `.gitignore`). Unggah isi `site/` ke hosting statis atau gunakan:

```bash
mkdocs gh-deploy
```

untuk menerbitkan ke branch GitHub Pages (sesuaikan pengaturan repo & permission).

---

## Konten per topik (tautan langsung)

- [Ringkasan transaksi direct purchase](doc/transaksi-direct/README.md)
- [Kode respons (RC) lengkap](doc/transaksi-direct/kode-respons.md)
- [Contoh respons — pulsa](doc/transaksi-direct/contoh-respons-pulsa.md)
- [Contoh respons — produk game](doc/transaksi-direct/contoh-respons-produk-game.md)
- [Inquiry & katalog (menyusul)](doc/inquiry/README.md)

---

## Status & review

- Materi utama diselaraskan dengan spesifikasi internal API H2H SOCX.
- Bagian yang masih perlu **konfirmasi tim API / Pak Leo** antara lain: format error HTTP, URL sandbox, kontrak callback setelah pending, daftar kode produk resmi (terutama game), serta dokumentasi inquiry ketika endpoint tersedia.

Saran kontribusi: kirim **Pull Request** untuk perbaikan teks, contoh request/response terbaru dari UAT, atau pengisian tabel SKU di halaman produk game setelah ada data resmi.

---

## Lisensi & kontak

Hak cipta dan kebijakan distribusi dokumen mengikuti kebijakan **SOCX / pemilik API**. Untuk akses sandbox, whitelist IP, atau pertanyaan integrasi, hubungi **tim SOCX / support** melalui kanal resmi yang diberikan kepada mitra.

---

**Repositori GitHub:** [maulana222/socx-api-documentation](https://github.com/maulana222/socx-api-documentation) — sesuaikan `repo_url` di `mkdocs.yml` jika repo dipindah atau diganti nama.
