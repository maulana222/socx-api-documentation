# Inquiry & katalog

Bagian ini mendokumentasikan endpoint **inquiry** (cek data pelanggan / informasi produk sebelum transaksi) dan akan diperluas untuk katalog produk lain jika tersedia.

## Isi

| Halaman | Keterangan |
|---------|------------|
| [Inquiry POST (JSON)](inquiry-post.md) | `POST /inquiry` — contoh **PLN Prabayar** (`CPLN`), request/response, cURL. |

## Autentikasi & jaringan

Sama seperti API reseller lainnya:

- Header **`Authorization: Bearer <JWT>`**
- **Whitelist IP** untuk H2H production (lihat [Persiapan integrasi](../02-persiapan-integrasi.md))

## Alur disarankan

1. **Inquiry** → validasi pelanggan / ambil info tampilan (`info[]`).
2. **Purchase** → gunakan `code` yang sesuai dan field tujuan (`msisdn` atau setara) sesuai [pembelian JSON](../transaksi-direct/pembelian-json-post.md).

## Menyusul

- Daftar lengkap `code` inquiry per kategori (game, pulsa, e-wallet, dll.) — dari tim SOCX.
- Varian response untuk produk non-PLN.

## Link ke transaksi

Setelah inquiry, lanjut ke [direct purchase — JSON POST](../transaksi-direct/pembelian-json-post.md).
