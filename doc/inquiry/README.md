# Inquiry & katalog produk

Bagian ini **belum ditulis** secara penuh karena menunggu:

- URL endpoint inquiry (jika sudah ada di backend, bisa langsung didokumentasikan di sini).
- Kontrak request/response dari **tim SOCX** (daftar harga, list game/pulsa, format filter, dll.).

## Yang akan dimuat di sini (rencana)

1. **Persiapan** — parameter auth, rate limit (**TBD**)
2. **Daftar produk / harga** — mapping ke field `code` untuk `POST /purchase`
3. **Kategori** — pulsa, game, e-wallet, dll. sesuai produk SOCX
4. **Contoh respons** — JSON konsisten untuk integrator

## Jika URL inquiry sudah tersedia

Anda dapat menambahkan file baru di folder ini, misalnya:

- `inquiry/daftar-harga.md`
- `inquiry/list-produk-game.md`

dengan template:

```markdown
## Endpoint
- Method:
- URL:

## Request
| Field | Tipe | Wajib |

## Response
Contoh JSON ...

## Catatan
Review internal — tanggal ___
```

Setelah itu ajukan **Pull Request** untuk review tim sebelum publikasi.

## Link ke transaksi

Setelah mendapat `code` dari inquiry, gunakan [direct purchase — JSON POST](../transaksi-direct/pembelian-json-post.md).
