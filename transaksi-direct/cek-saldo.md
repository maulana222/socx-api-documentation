# Cek saldo

Pengecekan saldo deposit reseller secara real-time.

## Request

| Properti | Nilai |
|----------|--------|
| Metode | `GET` |
| URL | `{base_url}/saldo` |
| Header | `Authorization: Bearer <JWT>` |

## Contoh cURL

```bash
curl -g --request GET \
  'https://indotechapi.socx.app/reseller/api/v1/saldo' \
  --header 'Authorization: Bearer REPLACE-WITH-YOUR-JWT-TOKEN'
```

## Response sukses (contoh)

```json
{
  "balance": 14450
}
```

| Field | Tipe | Keterangan |
|-------|------|------------|
| `balance` | number | Saldo tersedia (satuan sesuai kontrak internal SOCX — biasanya rupiah whole number) |

## Error umum

Lihat [kode respons](./kode-respons.md) untuk RC yang muncul pada endpoint transaksi. Untuk `/saldo`, kegagalan autentikasi mengikuti pola API umum (**TBD:** format body error jika bukan 200 — konfirmasi tim API).
