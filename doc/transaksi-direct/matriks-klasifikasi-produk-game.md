# Matriks klasifikasi produk game

Halaman ini menjadi acuan untuk menentukan parameter transaksi game per `code`: apakah termasuk **topup** atau **voucher**, apakah butuh **area/zone/server**, dan field apa saja yang wajib.

Gunakan matriks ini bersama dokumen [Contoh respons — produk game](./contoh-respons-produk-game.md) saat implementasi integrasi.

## Aturan umum klasifikasi

| Item | Aturan |
|------|--------|
| `tipe_produk` | `TOPUP` jika mengisi saldo/item langsung ke akun game; `VOUCHER` jika menghasilkan kode `sn` untuk diredeem manual. |
| `area_required` | `YA` jika produk butuh area/zone/server tambahan; `TIDAK` jika cukup `user_id` saja. |
| `msisdn` | Tetap dipakai sebagai field payload utama di API purchase; isi mengikuti format produk (mis. `user_id` atau `user_id+zone`). |
| `request_id` | Wajib unik untuk tiap order baru. |
| `sn` saat sukses | Bisa kosong pada sebagian `TOPUP`; umumnya terisi pada `VOUCHER`. Tetap simpan jika ada. |

### Top-up game (`TOPUP`)

Tujuan transaksi adalah **mengisi saldo atau item langsung ke akun game** di server yang dimaksud. Parameter yang diteruskan ke biller pada umumnya berupa **ID pemain (user / game ID)**; jika produk membutuhkan pemisahan server, **ID server (zone / server id)** ikut disertakan dalam **satu rangkaian format** (misalnya `user_id` + delimiter + `server_id`) sebelum dimapping ke field **`msisdn`** di payload purchase — detail delimiter dan urutan field mengikuti **matriks per `code`** dan konfirmasi tim SOCX/API.

### Voucher game (`VOUCHER`)

Di jalur purchase, **`msisdn`** tetap field wajib di API (bisa berisi nomor HP mitra, channel, atau referensi lain sesuai kontrak SKU), sehingga transaksi tetap dapat diproses dan pelanggan “mendapat” produk lewat alur Anda.

Yang **menentukan isi ulang** bagi pemain adalah **`sn` / kode voucher** di respons transaksi **setelah sukses final** (mis. `rc = 00`, atau hasil final setelah `rc = 68`). **Redeem** dilakukan pemain dengan kode unik tersebut; **bukan** dengan menganggap nilai **`msisdn` pada request** sebagai kode isi ulang. Simpan `sn` untuk bukti, CS, dan rekonsiliasi.

## Matriks per kode game (isi operasional)

> Status awal: template siap pakai. Isi per `code` final dari tim SOCX/API saat katalog resmi sudah diterima.

| code | nama_produk | tipe_produk (`TOPUP`/`VOUCHER`) | area_required (`YA`/`TIDAK`) | format_parameter (sebelum mapping ke `msisdn`) | mapping ke field API purchase | contoh_nilai | contoh_reply_ringkas | status |
|------|-------------|----------------------------------|-------------------------------|-----------------------------------------------|-------------------------------|-------------|----------------------|--------|
| `TBD` | `TBD` | `TBD` | `TBD` | `TBD` | `msisdn=<TBD>` | `TBD` | `rc=00/68, sn=<opsional>` | Menunggu katalog resmi |

## Panduan pengisian cepat

1. **Identifikasi tipe produk**
   - Jika hasil akhir diberikan sebagai kode voucher/pin: isi `tipe_produk=VOUCHER`.
   - Jika saldo/item langsung masuk akun game: isi `tipe_produk=TOPUP`.
2. **Tentukan kebutuhan area**
   - Jika provider meminta server/zone/region: isi `area_required=YA`.
   - Jika provider hanya butuh satu ID pemain: isi `area_required=TIDAK`.
3. **Definisikan format parameter**
   - Contoh tanpa area: `user_id`.
   - Contoh dengan area: `user_id|zone_id` (delimiter final mengikuti ketentuan SOCX).
4. **Tetapkan mapping ke payload API**
   - Karena endpoint purchase standar memakai `msisdn`, tulis aturan mapping yang konsisten per `code`.
5. **Validasi contoh reply**
   - Minimal simpan contoh `rc=00` dan `rc=68`; tambahkan contoh gagal paling umum jika tersedia.

## Template siap-copy per produk

```md
### <code> - <nama produk>

- tipe_produk: TOPUP | VOUCHER
- area_required: YA | TIDAK
- format_parameter: <contoh: user_id atau user_id|zone_id>
- mapping_payload:
  - code: <code resmi>
  - msisdn: <aturan mapping>
  - request_id: <unik per transaksi>
- contoh_reply_sukses_ringkas: rc=00, sn=<terisi/kosong>, message=<...>
- catatan_operasional: <mis. retry, validasi panjang id, dll>
```

## Catatan untuk tim integrasi

- Jangan asumsikan semua produk game membutuhkan area.
- Jangan asumsikan semua produk game adalah voucher.
- Jadikan tabel ini sebagai **single source of truth** saat handover ke partner (mis. Paydia/LinkAja), agar tidak terjadi beda interpretasi parameter.
