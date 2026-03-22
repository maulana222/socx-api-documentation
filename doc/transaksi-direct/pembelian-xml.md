# Pembelian — XML

Transaksi melalui **POST** body XML ( pola mirip XML-RPC ).

## Request

| Properti | Nilai |
|----------|--------|
| Metode | `POST` |
| URL | `{base_url}/xml/purchase` |
| Body | XML `methodCall` |

## Request — `topUpRequest`

```xml
<?xml version="1.0"?>
<methodCall>
  <methodName>topUpRequest</methodName>
  <params>
    <param>
      <value>
        <struct>
          <member>
            <name>NOHP</name>
            <value><string>0818111222</string></value>
          </member>
          <member>
            <name>REQUESTID</name>
            <value><string>123456</string></value>
          </member>
          <member>
            <name>PIN</name>
            <value><string></string></value>
          </member>
          <member>
            <name>NOM</name>
            <value><string>HSA5</string></value>
          </member>
        </struct>
      </value>
    </param>
  </params>
</methodCall>
```

| Field XML | Setara JSON | Keterangan |
|-----------|-------------|------------|
| `NOM` | `code` | Kode produk |
| `NOHP` | `msisdn` | Nomor / ID tujuan |
| `REQUESTID` | `request_id` | ID unik Anda |
| `PIN` | — | Kosong jika tidak dipakai (**TBD** jika ada skenario PIN) |

## Response — `methodResponse` (contoh)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<methodResponse>
  <params>
    <param>
      <value>
        <struct>
          <member>
            <name>RESPONSECODE</name>
            <value><string>68</string></value>
          </member>
          <member>
            <name>REQUESTID</name>
            <value><string>123456</string></value>
          </member>
          <member>
            <name>MESSAGE</name>
            <value><string>PENDING, Transaksi sedang diproses</string></value>
          </member>
          <member>
            <name>TRANSACTIONID</name>
            <value><string>5483</string></value>
          </member>
        </struct>
      </value>
    </param>
  </params>
</methodResponse>
```

| Field | Keterangan |
|-------|------------|
| `RESPONSECODE` | Setara `rc` di JSON |
| `REQUESTID` | Echo `request_id` |
| `MESSAGE` | Setara `message` |
| `TRANSACTIONID` | Setara `trxid` (string di XML) |

**TBD:** Mapping lengkap ke field `price`, `balance`, `sn` di respons XML synchronous — konfirmasi tim API jika ada versi respons diperluas.

## Laporan / callback XML (`topUpReport`)

Spesifikasi sumber menyertakan contoh **`topUpReport`** (bukan request dari reseller, melainkan **laporan hasil** ke integrator):

```xml
<?xml version="1.0"?>
<methodCall>
  <methodName>topUpReport</methodName>
  <params>
    <param>
      <value>
        <struct>
          <member>
            <name>RESPONSECODE</name>
            <value><string>00</string></value>
          </member>
          <member>
            <name>REQUESTID</name>
            <value><string>12345</string></value>
          </member>
          <member>
            <name>MESSAGE</name>
            <value><string>SUKSES</string></value>
          </member>
          <member>
            <name>TRANSACTIONID</name>
            <value><string>123123</string></value>
          </member>
          <member>
            <name>SN</name>
            <value><string>02123123123123123</string></value>
          </member>
          <member>
            <name>MSISDN</name>
            <value><string>08123123123</string></value>
          </member>
          <member>
            <name>PRICE</name>
            <value><string>5500</string></value>
          </member>
        </struct>
      </value>
    </param>
  </params>
</methodCall>
```

**Wajib diklarifikasi ke Pak Leo / tim API:**

- URL endpoint di sisi **Anda** yang menerima `topUpReport`
- Metode autentikasi (IP whitelist, signature, basic auth, dll.)
- Apakah callback ini menggantikan atau melengkapi polling `/status` untuk `rc = 68`
