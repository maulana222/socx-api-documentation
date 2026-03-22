# Contoh respons — produk game (top-up / voucher)

Pada level API SOCX, **format request/response sama** dengan pulsa: `code`, `msisdn`, `request_id`, dan field respons yang identik. Perbedaannya ada pada **arti bisnis** field:

| Field | Pulsa | Produk game (umum) |
|-------|--------|---------------------|
| `code` | SKU pulsa (mis. `HSA5`) | SKU game tertentu (**dari katalog — TBD**) |
| `msisdn` | Nomor HP | **ID pengguna game** sesuai aturan biller (hanya angka di API ini) |
| `sn` | Referensi / serial pulsa | Kode voucher / pin / referensi top-up game |

**Penting:** Spesifikasi `socx.md` tidak membedakan endpoint; pastikan untuk setiap produk game Anda memakai **`code` dan format `msisdn` yang benar** sesuai daftar dari SOCX (akan dilengkapi lewat inquiry / dokumen produk).

## Request (JSON POST) — contoh ilustratif

**Peringatan:** `GAME-TELOR-001` hanya contoh penamaan; ganti dengan kode resmi dari SOCX.

```json
{
  "code": "GAME-TELOR-001",
  "msisdn": "9876543210123456",
  "request_id": "GAME-20250322-0001"
}
```

Asumsi ilustratif: `msisdn` berisi **user ID numerik** dari game (bukan nomor telepon).

## Respons — pending (bentuk sama dengan pulsa)

```json
{
  "code": "GAME-TELOR-001",
  "msisdn": "9876543210123456",
  "request_id": "GAME-20250322-0001",
  "rc": "68",
  "trxid": 88102,
  "price": 10000,
  "balance": 50000000,
  "sn": "",
  "message": "PENDING, Transaksi sedang diproses"
}
```

## Respons — sukses (ilustrasi)

```json
{
  "code": "GAME-TELOR-001",
  "msisdn": "9876543210123456",
  "request_id": "GAME-20250322-0001",
  "rc": "00",
  "trxid": 88102,
  "price": 10000,
  "balance": 49990000,
  "sn": "VCR-GAME-AB12CD34EF",
  "message": "SUKSES"
}
```

| Field | Interpretasi game |
|-------|-------------------|
| `sn` | Sering dipakai sebagai **voucher / pin / bukti** untuk dikirim ke pemain — format bervariasi per SKU (**TBD** per produk) |

## Respons — ID game tidak valid (ilustrasi)

Jika biller mengembalikan setara `07` / `12` / `13`, bentuk JSON mengikuti pola umum:

```json
{
  "code": "GAME-TELOR-001",
  "msisdn": "0000000000000000",
  "request_id": "GAME-20250322-0002",
  "rc": "07",
  "trxid": 0,
  "price": 0,
  "balance": 50000000,
  "sn": "",
  "message": "Nomor pelanggan tidak ditemukan"
}
```

**TBD:** Pesan pasti per jenis game dan mapping RC — konfirmasi ke tim API.

## “List game”

- **Bukan bagian endpoint purchase:** daftar SKU game / denom diharapkan dari **inquiry / API katalog** (menyusul, dari tim SOCX).
- Dokumen ini hanya menjamin **kontrak respons transaksi** konsisten dengan pulsa; variasi isi `sn` dan `message` per produk didokumentasikan per SKU setelah katalog tersedia.

## Tabel checklist per SKU game (untuk diisi tim produk)

| `code` | Nama produk | Format `msisdn` | Isi `sn` saat sukses | Catatan |
|--------|-------------|-----------------|----------------------|---------|
| *TBD* | *TBD* | *TBD* | *TBD* | |

Salin tabel ini ke spreadsheet internal dan lengkapi bersama tim produk/API agar integrator punya **spesifikasi reply tertentu** per game.
