# Skenario pengujian integrasi (direct purchase)

Checklist untuk QA sebelum production. Sesuaikan dengan **sandbox** begitu URL & kredensial UAT tersedia (**TBD**).

## 1. Autentikasi & jaringan

| ID | Skenario | Ekspektasi |
|----|----------|------------|
| A1 | GET `/saldo` dengan JWT valid | HTTP 200, field `balance` numerik |
| A2 | GET `/saldo` tanpa token / token salah | Gagal auth (detail **TBD**: status code & body) |
| A3 | Request dari IP **tidak** di-whitelist | Ditolak (**TBD** format) |

## 2. Pembelian JSON — happy path

| ID | Skenario | Ekspektasi |
|----|----------|------------|
| P1 | `POST /purchase` dengan `code`, `msisdn`, `request_id` valid | `rc` `00` atau `68` sesuai lingkungan; `trxid` terisi jika relevan |
| P2 | Ulangi **persis** `request_id` yang sama setelah sukses | Respons **sama** (idempotensi) dengan transaksi pertama |
| P3 | Setelah `rc = 68`, `POST /status` dengan `request_id` sama | `rc` terbaru; akhirnya `00` atau gagal |

## 3. Validasi payload

| ID | Skenario | Ekspektasi |
|----|----------|------------|
| V1 | Hilangkan field wajib (`code` / `msisdn` / `request_id`) | `rc = 01` atau error validasi (**TBD**) |
| V2 | `msisdn` berisi huruf / spasi | Gagal validasi |
| V3 | `code` tidak terdaftar | `rc = 05` |

## 4. Saldo & limit

| ID | Skenario | Ekspektasi |
|----|----------|------------|
| S1 | Saldo lebih kecil dari harga produk | `rc = 18` |
| S2 | Limit provider / cut off jika dapat disimulasikan | `rc = 36` / `35` (**TBD** di sandbox) |

## 5. Jalur alternatif

| ID | Skenario | Ekspektasi |
|----|----------|------------|
| H1 | `GET /http/purchase` dengan query lengkap | JSON respons setara `POST /purchase` |
| H2 | `POST /http/purchase` form dengan field sama | Sama dengan H1 |
| X1 | `POST /xml/purchase` `topUpRequest` | XML `methodResponse` dengan `RESPONSECODE` konsisten dengan RC |

## 6. Pulsa vs game

| ID | Skenario | Ekspektasi |
|----|----------|------------|
| G1 | Purchase pulsa dengan kode resmi sandbox | `sn` / `message` sesuai contoh [pulsa](./contoh-respons-pulsa.md) |
| G2 | Purchase game dengan kode resmi sandbox (**TBD** kode) | `sn` mengikuti spesifikasi SKU di tabel [game](./klasifikasi-produk-game.md) |

## 7. Callback / webhook (**TBD**)

| ID | Skenario | Ekspektasi |
|----|----------|------------|
| C1 | Server SOCX POST `topUpReport` ke URL Anda | Badan sesuai contoh XML; HTTP 200 dari Anda |
| C2 | Duplikasi callback untuk `REQUESTID` sama | Idempotent di sisi Anda (tidak dobel kredit user) |

Isi baris C1–C2 setelah tim API menetapkan URL, auth, dan retry policy.

## 8. Regression cepat (smoke)

Urutan minimal sebelum rilis integrasi:

1. `GET /saldo`
2. `POST /purchase` (satu transaksi uji)
3. Jika `68` → `POST /status` hingga final
4. Ulangi `purchase` dengan `request_id` sama → sama dengan langkah 3

---

**Untuk owner:** serahkan dokumen ini ke tim API dengan pertanyaan terbuka: sandbox URL, format error HTTP, kontrak callback, dan daftar `code` untuk tabel pengujian G2.

