# Cek status

Mengambil status terkini transaksi berdasarkan `request_id` Anda.

## Request

| Properti | Nilai |
|----------|--------|
| Metode | `POST` |
| URL | `{base_url}/status` |
| Header | `Authorization: Bearer <JWT>` |
| Header | `Content-Type: application/json` |

## Body

| Field | Tipe | Wajib |
|-------|------|--------|
| `request_id` | string | Ya |

## Contoh cURL

```bash
curl -g --request POST \
  'https://indotechapi.socx.app/reseller/api/v1/status' \
  --header 'Authorization: Bearer REPLACE-WITH-YOUR-JWT-TOKEN' \
  --header 'Content-Type: application/json' \
  --data-raw '{"request_id":"999999"}'
```

## Response (contoh)

Format **sama** dengan respons purchase:

```json
{
  "code": "HSA5",
  "msisdn": "08121231231",
  "request_id": "999999",
  "rc": "68",
  "trxid": 16413,
  "price": 5400,
  "balance": 341890000,
  "sn": "",
  "message": "PENDING, Transaksi sedang diproses"
}
```

## Praktik integrasi

1. Setelah purchase mengembalikan `rc = 68`, lakukan **polling** `/status` dengan interval yang wajar (mis. 2–5 detik, backoff maksimal) hingga `rc` final (`00` sukses atau kode gagal lain).
2. **TBD:** Jika callback `topUpReport` aktif, sesuaikan agar tidak dobel-update status (gunakan `request_id` / `trxid` sebagai kunci idempotensi di DB Anda).

## Error

- `rc = 11` — transaksi tidak ditemukan (lihat [kode respons](./kode-respons.md)).
