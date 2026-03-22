# Pembelian — HTTP GET / POST

Untuk klien yang tidak memakai JSON body dengan Bearer header saja, SOCX menyediakan endpoint `/http/purchase` dengan parameter `token` (JWT).

## GET

| Properti | Nilai |
|----------|--------|
| Metode | `GET` |
| URL | `{base_url}/http/purchase` |

### Query parameters

| Parameter | Wajib | Keterangan |
|-----------|-------|------------|
| `code` | Ya | Kode produk |
| `msisdn` | Ya | Hanya angka |
| `request_id` | Ya | ID unik Anda |
| `token` | Ya | JWT (sama seperti Bearer) |

### Contoh cURL

```bash
curl -g --request GET \
  'https://indotechapi.socx.app/reseller/api/v1/http/purchase?code=HSA5&request_id=123456&msisdn=081234567890&token=REPLACE-WITH-YOUR-JWT-TOKEN'
```

## POST (form)

| Properti | Nilai |
|----------|--------|
| Metode | `POST` |
| URL | `{base_url}/http/purchase` |
| Body | `multipart/form-data` atau `application/x-www-form-urlencoded` (sesuai implementasi server — field di bawah) |

### Field form

| Field | Wajib |
|-------|--------|
| `code` | Ya |
| `msisdn` | Ya |
| `request_id` | Ya |
| `token` | Ya |

### Contoh cURL (multipart)

```bash
curl -g --request POST \
  'https://indotechapi.socx.app/reseller/api/v1/http/purchase' \
  --form 'code=HSA5' \
  --form 'request_id=13013' \
  --form 'msisdn=081471384' \
  --form 'token=REPLACE-WITH-YOUR-JWT-TOKEN'
```

**Catatan:** Contoh asli spesifikasi memakai tanda kutip pada nilai form (`code="HSA5"`); banyak server menerima tanpa kutip. Samakan dengan perilaku server SOCX saat uji.

## Response

Format JSON **sama** dengan [pembelian JSON POST](./pembelian-json-post.md), contoh:

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

## Idempotensi

Perilaku sama: `request_id` yang sudah diproses akan mengembalikan respons transaksi yang sama.
