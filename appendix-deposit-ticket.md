# Lampiran — Create tiket deposit

Di luar fokus **direct purchase**, tetapi tercantum di spesifikasi sumber untuk kelengkapan integrator.

## Request

| Properti | Nilai |
|----------|--------|
| Metode | `POST` |
| URL | `{base_url}/deposit_ticket` |
| Header | `Authorization: Bearer <JWT>` |
| Header | `Content-Type: application/json` |

## Body

| Field | Tipe | Wajib |
|-------|------|--------|
| `amount` | number | Ya |

## Contoh cURL

```bash
curl -g --request POST \
  'https://indotechapi.socx.app/reseller/api/v1/deposit_ticket' \
  --header 'Authorization: Bearer REPLACE-WITH-YOUR-JWT-TOKEN' \
  --header 'Content-Type: application/json' \
  --data-raw '{"amount":1000000}'
```

## Response (contoh)

```json
{
  "instruction": "Mohon transfer nominal Rp. 1,000,101 (harus sama), ke rekening BCA 1234567890, Mandiri 0987654321 sebelum 17/05/2024 01:37:08 Terima kasih",
  "amount": 1000000,
  "banks": [
    {
      "id": 1,
      "account_no": "1234567890",
      "bank_name": "BCA",
      "beneficiary_name": "Name",
      "status": 1
    },
    {
      "id": 2,
      "account_no": "0987654321",
      "bank_name": "Mandiri",
      "beneficiary_name": "Name",
      "status": 1
    }
  ],
  "unique": 101,
  "admin_fee": 0,
  "grand_total": 100101,
  "valid_before": "17/05/2024 01:37:08"
}
```

**TBD:** Validasi field `banks` di production, format tanggal, dan apakah respons persis sama — review dengan tim SOCX.
