# Dokumentasi API SOCX (Reseller H2H)

Dokumen ini untuk integrasi **Host-to-Host (H2H)** reseller ke platform SOCX. Struktur mengikuti alur dokumentasi teknis sejenis [Digiflazz Developer](https://developer.digiflazz.com/) (pengenalan → persiapan → transaksi → kode respons → contoh → pengujian).

Gunakan **menu kiri** untuk navigasi (situs ini dihasilkan dengan [MkDocs Material](https://squidfunk.github.io/mkdocs-material/)).

## Isi dokumentasi

| Bagian | Keterangan |
|--------|------------|
| [Pengenalan](01-pengenalan.md) | Gambaran umum, base URL, autentikasi |
| [Persiapan integrasi](02-persiapan-integrasi.md) | IP statis, whitelist, token, format `request_id` |
| **Transaksi direct purchase** | |
| → [Cek saldo](transaksi-direct/cek-saldo.md) | `GET /saldo` |
| → [Pembelian JSON (POST)](transaksi-direct/pembelian-json-post.md) | `POST /purchase` — **jalur utama** |
| → [Pembelian HTTP](transaksi-direct/pembelian-http.md) | `GET` / `POST /http/purchase` |
| → [Pembelian XML](transaksi-direct/pembelian-xml.md) | `POST /xml/purchase` |
| → [Cek status](transaksi-direct/cek-status.md) | `POST /status` |
| → [Kode respons (RC)](transaksi-direct/kode-respons.md) | Tabel RC |
| → [Contoh respons — pulsa](transaksi-direct/contoh-respons-pulsa.md) | Payload & respons spesifik pulsa |
| → [Contoh respons — produk game](transaksi-direct/contoh-respons-produk-game.md) | Konvensi field & contoh (tanpa inquiry) |
| → [Skenario pengujian](transaksi-direct/skenario-pengujian.md) | Checklist QA integrasi |
| [Inquiry & katalog produk](inquiry/README.md) | **Menyusul** — koordinasi Pak Leo |
| [Lampiran — deposit tiket](appendix-deposit-ticket.md) | Di luar direct purchase; dari spesifikasi sumber |

## Jalankan dokumentasi secara lokal

Dari **root repositori** ini (folder yang sama dengan `mkdocs.yml`):

```bash
pip install -r requirements-docs.txt
mkdocs serve
```

Buka `http://127.0.0.1:8000` di browser.

## Deploy (GitHub Pages)

```bash
mkdocs gh-deploy
```

(Sesuaikan branch remote; atau build `mkdocs build` dan unggah isi folder `site/` ke hosting statis.)

## Status dokumen

- **Siap dipakai untuk integrasi:** direct purchase (JSON/HTTP/XML), cek saldo, cek status, tabel RC — sesuai sumber internal `socx.md`.
- **Perlu review Pak Leo / tim API:** callback setelah `rc = 68` (pending), verifikasi webhook/XML `topUpReport`, URL & kontrak **inquiry** / daftar harga, kode produk resmi untuk game.

## Repositori GitHub

Setelah direview, dokumen ini dapat dipindahkan ke repositori publik terpisah (mis. `socx/api-docs`). Edit `repo_url` di `mkdocs.yml` agar tombol GitHub di header benar.

### Paralel dengan organisasi DANA

Organisasi [dana-id di GitHub](https://github.com/dana-id/) memisahkan **SDK** (`dana-node`) dan **dokumentasi API** terpisah. Untuk SOCX Anda bisa meniru pola itu:

- Repo `socx-api-docs` — Markdown + MkDocs + GitHub Pages.
- Repo integrasi klien (opsional) — SDK Node/PHP terpisah jika nanti dibutuhkan.
