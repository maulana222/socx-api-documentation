# Kode respons (RC)

Kode `rc` pada respons JSON (dan `RESPONSECODE` pada XML) menggambarkan hasil transaksi.

| RC | Keterangan | Status |
|----|------------|--------|
| `00` | Transaksi sukses | Berhasil |
| `01` | Invalid payload | Gagal |
| `02` | Invalid authorization | Gagal |
| `03` | Transaksi timeout | Gagal |
| `05` | Kode produk tidak dikenali | Gagal |
| `06` | Gangguan koneksi biller | Gagal |
| `07` | Nomor pelanggan tidak ditemukan | Gagal |
| `08` | Internal error | Gagal |
| `09` | Sistem dalam proses pemeliharaan | Gagal |
| `10` | Ref code tidak valid | Gagal |
| `11` | Transaksi tidak ditemukan | Gagal |
| `12` | Nomor pelanggan kadaluarsa | Gagal |
| `13` | Nomor pelanggan diblokir | Gagal |
| `14` | Gangguan biller | Gagal |
| `17` | Duplikasi transaksi reversal | Gagal |
| `18` | Saldo deposit tidak cukup | Gagal |
| `19` | Harga produk belum diset | Gagal |
| `21` | Refund | Gagal |
| `22` | Produk sementara ditutup | Gagal |
| `23` | Gagal biller | Gagal |
| `35` | System cut off in progress | Gagal |
| `36` | Akun mencapai batas maksimal limit provider | Gagal |
| `68` | Transaksi pending, menunggu callback | **Pending** |
| `70` | Gagal biller | Gagal |

## Catatan implementasi

- **`68`**: anggap transaksi **belum final**; lanjutkan dengan [cek status](./cek-status.md) dan/atau callback (**kontrak callback TBD**).
- **`00`**: simpan `sn` jika ada sebagai bukti ke pelanggan.
- **`18`**: cek [cek saldo](./cek-saldo.md) sebelum retry.

**TBD:** Mapping HTTP status code (4xx/5xx) vs body JSON saat error — tambahkan setelah konfirmasi tim API.
