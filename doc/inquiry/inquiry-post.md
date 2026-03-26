# Inquiry (POST JSON)

Endpoint untuk **inquiry** (cek tagihan / validasi pelanggan / informasi produk) sebelum atau bersamaan perencanaan transaksi. Kontrak di bawah mengacu pada contoh **Inquiry PLN Prabayar** dari lingkungan demo.

## URL & lingkungan

| Lingkungan | Contoh host | Catatan |
|------------|-------------|---------|
| Demo (contoh integrasi) | `http://demoapi.socx.app` | HTTP sesuai contoh; gunakan hanya untuk uji. |
| Production (dokumen umum API H2H) | `https://indotechapi.socx.app` | Gunakan **HTTPS** di production. |

Path umum:

```
/reseller/api/v1/inquiry
```

**Base lengkap (demo):** `http://demoapi.socx.app/reseller/api/v1/inquiry`  
**Base lengkap (production):** `https://indotechapi.socx.app/reseller/api/v1/inquiry`

Sesuaikan dengan base URL yang diberikan tim SOCX untuk akun Anda.

## Request

| Properti | Nilai |
|----------|--------|
| Metode | `POST` |
| Header | `Content-Type: application/json` |
| Header | `Authorization: Bearer <JWT>` |

### Body JSON

| Field | Tipe | Wajib | Keterangan |
|-------|------|-------|------------|
| `code` | string | Ya | Kode produk / jenis inquiry (contoh: `CPLN` — PLN prabayar). |
| `idpel` | string | Ya | ID pelanggan sesuai aturan produk (contoh nomor meter / ID PLN). |
| `request_id` | string | Ya | ID unik dari sisi Anda (idempotensi / korelasi). |

### Contoh cURL

```bash
curl -L -X POST 'http://demoapi.socx.app/reseller/api/v1/inquiry' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_JWT_TOKEN' \
  -d '{"code":"CPLN","idpel":"121021034831","request_id":"250717000000"}'
```

Ganti `YOUR_JWT_TOKEN` dengan token valid dari portal reseller.

### Contoh cURL (production — DANA)

```bash
curl -L -X POST 'http://indotechapi.socx.app/reseller/api/v1/inquiry' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer xxxx' \
  -d '{"code":"DANA","idpel":"08996647676#1000","request_id":"26032600000"}'
```

## Response sukses (contoh — `rc: "00"`)

```json
{
  "code": "CPLN",
  "idpel": "121021034831",
  "request_id": "250717000000",
  "product_name": "Inquiry PLN Prabayar",
  "client_name": "NRML~!@ $^*}?-&.UAT#TEST2",
  "info": [
    { "name": "Nomor", "value": "08102138831" },
    { "name": "Nama", "value": "NRML~!@ $^*}?-&.UAT#TEST2" },
    { "name": "Tarif/Daya", "value": "R1/900" },
    { "name": "Stroom/Token Unsold", "value": "0" }
  ],
  "rc": "00",
  "msg": "SUKSES",
  "jumlah_tagihan": "",
  "denda": "",
  "tagihan": "",
  "admin": "",
  "total": "",
  "balance": 29986594769
}
```

### Response sukses (contoh — `DANA`)

```json
{
  "code": "DANA",
  "idpel": "08996647676#1000",
  "request_id": "26032600000",
  "product_name": "DANA",
  "client_name": "DNID aXX danX",
  "info": null,
  "rc": "00",
  "msg": "SUKSES",
  "jumlah_tagihan": "1",
  "denda": "",
  "tagihan": "1000",
  "admin": "60",
  "total": "1060",
  "balance": 29986574717
}
```

### Field response

| Field | Tipe | Keterangan |
|-------|------|------------|
| `code` | string | Echo / konfirmasi kode produk. |
| `idpel` | string | Echo ID pelanggan. |
| `request_id` | string | Echo `request_id` permintaan. |
| `product_name` | string | Nama produk / jenis inquiry. |
| `client_name` | string | Nama pelanggan (jika tersedia dari biller). |
| `info` | array | Daftar pasangan `name` / `value` untuk ditampilkan (nomor, nama, tarif/daya, stroom, dll.). |
| `rc` | string | Kode hasil; `00` = sukses (selaras konvensi [kode respons](../transaksi-direct/kode-respons.md) untuk transaksi). |
| `msg` | string | Pesan singkat. |
| `jumlah_tagihan`, `denda`, `tagihan`, `admin`, `total` | string | Untuk skenario inquiry tagihan dapat terisi; pada contoh PLN prabayar kosong. |
| `balance` | number | Saldo deposit Anda setelah inquiry (jika dikembalikan oleh API). |

## Hubungan dengan direct purchase

- Inquiry memakai field **`idpel`** (dan `code`) sesuai produk.
- Transaksi **purchase** pada dokumen H2H umumnya memakai **`msisdn`** + **`code`** + **`request_id`**.  
  Pastikan mapping **`idpel` ↔ `msisdn`** atau field lain mengikuti **daftar produk** dari SOCX untuk alur lengkap (inquiry → purchase).

## Catatan integrasi

1. Gunakan **`request_id` unik** per permintaan inquiry baru; perilaku idempotensi jika sama persis dengan transaksi — konfirmasi ke tim SOCX jika diperlukan.
2. **Demo** memakai HTTP; **production** harus **HTTPS**.
3. Produk selain PLN dapat punya **`code`** lain dan struktur `info` berbeda — tambahkan halaman/appendix per produk ketika contoh resmi tersedia.

## Error & RC lain

Mapping lengkap ke **`rc`** selain `00` mengikuti tabel [Kode respons (RC)](../transaksi-direct/kode-respons.md) dan penyesuaian khusus produk inquiry jika ada dari tim API.
