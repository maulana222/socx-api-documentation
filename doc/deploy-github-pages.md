# Deploy dokumentasi ke GitHub Pages (situs statis)

Panduan ini memublikasikan situs MkDocs (HTML statis) ke **GitHub Pages** agar bisa diakses lewat URL publik, misalnya:

`https://<username>.github.io/<nama-repo>/`

Ganti `<username>` dan `<nama-repo>` sesuai akun dan repositori Anda.

---

## Prasyarat

1. Repositori Git di GitHub sudah ada (contoh: `socx-api-documentation`).
2. Di komputer lokal terpasang **Python 3.10+** dan **Git**.
3. Anda punya hak **push** ke repo tersebut.
4. File `mkdocs.yml` dan folder `doc/` sudah ada di **root** repositori (seperti struktur repo dokumentasi SOCX ini).

---

## Langkah 1 — Pastikan `mkdocs.yml` benar

Di root repo, periksa:

- `repo_url` mengarah ke URL GitHub repo Anda (tombol “repo” di header tema).
- Opsional tapi dianjurkan: `site_url` diisi URL Pages nanti, contoh:

```yaml
site_url: https://maulana222.github.io/socx-api-documentation/
```

(Sesuaikan username dan nama repo.)

---

## Langkah 2 — Install dependensi lokal

Dari **root repositori** (folder yang berisi `mkdocs.yml`):

```bash
pip install -r requirements-docs.txt
```

Uji build:

```bash
mkdocs build
```

Jika sukses, folder `site/` berisi HTML siap unggah (`site/` biasanya di `.gitignore` dan **tidak** perlu di-commit ke branch `main`).

---

## Langkah 3 — Deploy dengan `mkdocs gh-deploy` (cara cepat)

Perintah ini akan:

1. Menjalankan `mkdocs build`.
2. Membuat atau memperbarui branch **`gh-pages`** di remote `origin`.
3. Mengunggah isi `site/` ke branch tersebut.

Jalankan dari root repo:

```bash
mkdocs gh-deploy
```

Jika diminta konfirmasi atau ada error autentikasi, pastikan:

- `git remote -v` menunjuk ke repo GitHub yang benar.
- Anda sudah login GitHub di Git (`credential` / SSH / token).

Variasi berguna:

```bash
# Pesan commit kustom pada branch gh-pages
mkdocs gh-deploy -m "docs: update"

# Jika remote bukan origin
mkdocs gh-deploy --remote-name origin
```

---

## Langkah 4 — Aktifkan GitHub Pages di pengaturan repo

1. Buka repositori di GitHub → **Settings**.
2. Menu kiri → **Pages** (di bagian *Code and automation*).
3. Di **Build and deployment** → **Source**: pilih **Deploy from a branch**.
4. **Branch**: pilih **`gh-pages`** dan folder **`/ (root)`**, lalu **Save**.

Tunggu beberapa menit. Status build muncul di tab **Actions** atau di halaman Pages.

---

## Langkah 5 — Buka situs

URL umum:

`https://<username>.github.io/<nama-repo>/`

Contoh: `https://maulana222.github.io/socx-api-documentation/`

Jika halaman kosong atau 404:

- Pastikan branch `gh-pages` ada dan berisi file `index.html` di root.
- Tunggu 1–10 menit setelah deploy pertama.
- Cek **Settings → Pages** apakah ada error.

---

## Opsi: deploy otomatis dengan GitHub Actions

Di repo ini sudah disediakan workflow **`.github/workflows/deploy-docs.yml`**: setiap **push** ke branch **`main`** akan menjalankan `mkdocs build` dan mendorong isi `site/` ke branch **`gh-pages`**.

Yang perlu Anda lakukan:

1. Commit dan push file `.github/workflows/deploy-docs.yml` ke GitHub.
2. Buka tab **Actions** di repo — pastikan workflow berjalan tanpa error (permission `contents: write` sudah disetel di file).
3. Di **Settings → Pages**, pilih sumber **Deploy from a branch** → branch **`gh-pages`**, folder **`/ (root)`** (sama seperti deploy manual).

Jika Anda menghapus workflow dan ingin menambah lagi, isi file-nya seperti berikut:

```yaml
name: Deploy MkDocs to GitHub Pages

on:
  push:
    branches: [main]

permissions:
  contents: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"

      - run: pip install -r requirements-docs.txt

      - run: mkdocs build

      - uses: peaceiris/actions-gh-pages@v4
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./site
          force_orphan: true
```

Sesuaikan nama branch (`main` vs `master`) dengan repo Anda.

---

## Domain kustom (opsional)

Di **Settings → Pages**, bagian **Custom domain**, isi domain Anda dan ikuti instruksi DNS dari GitHub. File `CNAME` biasanya ditambahkan otomatis atau bisa dikelola lewat hosting DNS Anda.

---

## Ringkasan perintah

| Tujuan | Perintah |
|--------|----------|
| Cek di lokal | `mkdocs serve` |
| Build saja | `mkdocs build` |
| Build + push ke `gh-pages` | `mkdocs gh-deploy` |

Setelah **Langkah 4** selesai sekali, untuk pembaruan berikutnya cukup edit Markdown di `doc/`, commit ke `main`, lalu jalankan lagi **`mkdocs gh-deploy`** (atau andalkan GitHub Actions jika sudah disetel).
