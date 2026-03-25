# Transaksi — Direct purchase

Bagian ini fokus pada alur **pembelian langsung** (top-up / voucher) tanpa alur inquiry terpisah.

## Alur disarankan

1. **Cek saldo** (opsional tapi dianjurkan sebelum transaksi besar): [`GET /saldo`](./cek-saldo.md)
2. **Purchase** — pilih satu jalur:
   - [`POST /purchase`](./pembelian-json-post.md) (disarankan)
   - [`/http/purchase`](./pembelian-http.md)
   - [`POST /xml/purchase`](./pembelian-xml.md)
3. Jika `rc = 68` (**pending**): polling [`POST /status`](./cek-status.md) sampai final, atau tunggu **callback** (kontrak **TBD** — lihat catatan di [contoh pulsa](./contoh-respons-pulsa.md)).

## Isi folder

| File | Isi |
|------|-----|
| [cek-saldo.md](./cek-saldo.md) | GET saldo |
| [pembelian-json-post.md](./pembelian-json-post.md) | POST purchase JSON |
| [pembelian-http.md](./pembelian-http.md) | GET/POST http purchase |
| [pembelian-xml.md](./pembelian-xml.md) | XML topUpRequest |
| [cek-status.md](./cek-status.md) | POST status |
| [kode-respons.md](./kode-respons.md) | Tabel RC |
| [contoh-respons-pulsa.md](./contoh-respons-pulsa.md) | Contoh lengkap pulsa |
| [contoh-respons-produk-game.md](./contoh-respons-produk-game.md) | Konvensi & contoh game |
| [matriks-klasifikasi-produk-game.md](./matriks-klasifikasi-produk-game.md) | Klasifikasi game: topup/voucher, butuh area/tidak, mapping parameter |
| [skenario-pengujian.md](./skenario-pengujian.md) | Skenario QA |

Deposit tiket (`/deposit_ticket`) ada di spesifikasi sumber tetapi di luar fokus **direct purchase**; dapat ditambahkan halaman terpisah jika dibutuhkan.
