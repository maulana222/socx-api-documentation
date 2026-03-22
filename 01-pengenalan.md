# Pengenalan

SOCX menyediakan API **reseller** untuk transaksi prabayar (termasuk **direct purchase**) melalui koneksi **IP host-to-host**. Setiap permintaan ditujukan ke host yang telah di-whitelist.

## Base URL

```
https://indotechapi.socx.app/reseller/api/v1
```

Di dokumen endpoint, `{base_url}` mengacu ke path di atas.

## Autentikasi

- **JSON API** (`/saldo`, `/purchase`, `/status`, `/deposit_ticket`, dll.): header  
  `Authorization: Bearer <JWT>`  
  Token diperoleh dari halaman **Settings** (portal reseller).
- **HTTP purchase** (`/http/purchase`): parameter `token` sama dengan JWT (lihat bagian pembelian HTTP).

## Jalur transaksi pembelian

| Jalur | Metode | Endpoint | Kapan dipakai |
|-------|--------|----------|----------------|
| **JSON (disarankan)** | POST | `{base_url}/purchase` | Integrasi modern, body JSON |
| HTTP query / form | GET / POST | `{base_url}/http/purchase` | Klien lama / gateway terbatas |
| XML-RPC style | POST | `{base_url}/xml/purchase` | Kompatibilitas sistem XML |

Detail field wajib dan contoh ada di masing-masing halaman [transaksi direct](transaksi-direct/README.md).

## Istilah

| Istilah | Arti |
|---------|------|
| `code` | Kode produk (SKU) dari SOCX |
| `msisdn` | Nomor tujuan; untuk beberapa produk non-pulsa dapat berisi **ID pengguna game** (hanya angka, sesuai kontrak produk) |
| `request_id` | ID unik dari sisi Anda untuk idempotensi / korelasi |
| `rc` | Response code hasil transaksi |
| `trxid` | ID transaksi internal SOCX |
| `sn` | Serial / kode voucher / bukti biller (jika ada) |

## Dokumen sumber

Spesifikasi awal diolah dari dokumen internal **API H2H SOCX** (`socx.md`). Bagian yang belum ada di sumber ditandai **TBD** agar bisa dilengkapi setelah konfirmasi tim API.
