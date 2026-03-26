# Flow Inquiry & Purchase (umum)

Diagram berikut menggambarkan alur sampai transaksi final untuk integrasi game.

Request detail (payload) mengikuti kontrak SOCX/API untuk `code` game Anda.

```mermaid
sequenceDiagram
  autonumber
  participant Client as Client/Reseller
  participant Indotech as Indotech
  participant Biller as Game Provider

  Client->>Indotech: POST /inquiry (code, msisdn)
  Indotech->>Biller: request inquiry (jika dibutuhkan)
  Biller-->>Indotech: response inquiry (info produk)
  Indotech-->>Client: response inquiry (info produk)

  Client->>Indotech: POST /purchase (code, msisdn, request_id)
  Indotech->>Biller: request topup / voucher (sesuai kategori)
  Indotech-->>Client: response purchase (rc=68, pending)
  Biller-->>Indotech: response (sn bila sukses)
  Indotech-->>Client: response purchase (rc=00 atau gagal)
```
/ End of Selection
```

## Catatan

- Jika respons `rc=68`, transaksi dianggap **pending**.
- Jika request purchase menggunakan `request_id` yang sama, maka SOCX akan mengembalikan data transaksi yang sudah ada sesuai data terakhir di sistem SOCX.

