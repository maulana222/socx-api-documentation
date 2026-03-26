# Pembelian — JSON POST (direct purchase)

Jalur **utama** untuk transaksi pembelian: body JSON + Bearer token.

## Request

| Properti | Nilai |
|----------|--------|
| Metode | `POST` |
| URL | `{base_url}/purchase` |
| Header | `Authorization: Bearer <JWT>` |
| Header | `Content-Type: application/json` |

## Body

| Field | Tipe | Wajib | Keterangan |
|-------|------|-------|------------|
| `code` | string | Ya | Kode produk |
| `msisdn` | string | Ya | Hanya angka — nomor HP (pulsa) atau ID sesuai produk (mis. game) |
| `request_id` | string | Ya | Unik di sisi Anda |

## Contoh cURL

```bash
curl -g --request POST \
  'https://indotechapi.socx.app/reseller/api/v1/purchase' \
  --header 'Authorization: Bearer REPLACE-WITH-YOUR-JWT-TOKEN' \
  --header 'Content-Type: application/json' \
  --data-raw '{
    "code": "HSA5",
    "msisdn": "08121231231",
    "request_id": "999999"
  }'
```

## Response (contoh pending)

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

## Field response

| Field | Tipe | Keterangan |
|-------|------|------------|
| `code` | string | Kode produk yang diproses |
| `msisdn` | string | Nomor / ID tujuan |
| `request_id` | string | Echo dari request |
| `rc` | string | [Kode respons](./kode-respons.md) |
| `trxid` | number | ID transaksi SOCX |
| `price` | number | Harga yang dikenakan (contoh dari spesifikasi) |
| `balance` | number | Saldo setelah transaksi (contoh dari spesifikasi) |
| `sn` | string | Serial / voucher; bisa kosong saat pending |
| `message` | string | Deskripsi human-readable |

## Idempotensi

Jika Anda mengirim **`request_id` yang sama** dan transaksi itu **sudah pernah diproses** di server SOCX, respons yang dikembalikan adalah **status transaksi untuk `request_id` tersebut** (bukan transaksi baru).

## Lanjutan

- Respons detail per skenario: [contoh pulsa](./contoh-respons-pulsa.md), [klasifikasi & contoh game](./klasifikasi-produk-game.md)
- Finalisasi pending: [cek status](./cek-status.md)
- Jalur alternatif: [HTTP](./pembelian-http.md), [XML](./pembelian-xml.md)

