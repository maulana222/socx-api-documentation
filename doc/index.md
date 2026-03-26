# Dokumentasi API SOCX (Reseller H2H)

Dokumen ini untuk integrasi **Host-to-Host (H2H)** reseller ke platform SOCX. Isi disusun bertahap: **pengenalan → persiapan integrasi → transaksi → kode respons → contoh → pengujian**.

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
| → [Klasifikasi & contoh respons produk game](transaksi-direct/klasifikasi-produk-game.md) | Acuan per `code`, request, respons, dan mapping `msisdn` |
| → [Skenario pengujian](transaksi-direct/skenario-pengujian.md) | Checklist QA integrasi |
| [Inquiry](inquiry/README.md) | `POST /inquiry` — contoh PLN prabayar (`CPLN`) |
| [Lampiran — deposit tiket](appendix-deposit-ticket.md) | Di luar direct purchase; dari spesifikasi sumber |

## Status dokumen

- **Siap dipakai untuk integrasi:** direct purchase (JSON/HTTP/XML), cek saldo, cek status, tabel RC — sesuai sumber internal `socx.md`.
- **Perlu review tim SOCX / API:** callback setelah `rc = 68` (pending), verifikasi webhook/XML `topUpReport`, URL & kontrak **inquiry** / daftar harga, finalisasi parameter game per `code` di tabel klasifikasi.

