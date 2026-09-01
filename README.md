# overtimeboo — etalase

Landing page statis untuk katalog produk bank soal digital
**overtimeboo**. Fungsinya satu: mengarahkan pengunjung dari bio media sosial ke
listing yang tepat di lynk.id.

**Live:** https://overtime-boo.gulliveragent.workers.dev
**Hosting:** Cloudflare Workers (static assets)
**Stack:** satu berkas HTML. Nol framework, nol dependensi runtime, nol build step.

---

## 1. Ringkasan

Produknya dijual lewat lynk.id. Lynk hanya menyediakan dua halaman kustom,
sedangkan listing sudah 15 dan akan bertambah. Halaman ini jadi lapisan navigasi
yang Lynk tidak sediakan: pengunjung bisa menyaring produk per kategori dulu,
baru diarahkan ke halaman checkout yang benar.

Yang **tidak** dilakukan halaman ini: menyimpan data, memproses pembayaran,
mengelola akun, atau mengirim berkas. Semua itu tetap di Lynk. Halaman ini murni
etalase.

Konsekuensinya penting untuk dipahami: **tidak ada backend, tidak ada basis data,
tidak ada rahasia yang disimpan.** Itu yang bikin pilihan hostingnya jadi
sesederhana ini.

---

## 2. Cloudflare itu apa

Cloudflare punya jaringan server yang tersebar di 300+ kota di seluruh dunia.
Jaringan itu awalnya dipakai sebagai CDN dan perisai keamanan: konten disalin ke
banyak lokasi supaya pengunjung dilayani dari server terdekat, bukan dari satu
server pusat yang jauh.

Belakangan mereka membuka jaringan yang sama untuk menjalankan kode dan menyimpan
berkas. Dua produknya yang relevan di sini:

| Produk | Fungsi |
|---|---|
| **Workers** | menjalankan kode JavaScript di server terdekat dengan pengunjung, dan sejak 2024 juga bisa menyajikan berkas statis (HTML, CSS, gambar) langsung |
| **Pages** | hosting statis yang terhubung ke repositori Git, dengan CI/CD bawaan |

Istilah **edge** yang sering muncul artinya sederhana: kode dan berkas dijalankan
di titik jaringan yang paling dekat dengan pengunjung, bukan di satu pusat data.
Pengunjung dari Surabaya dilayani dari node Asia Tenggara, bukan dari server di
Amerika.

**Yang kita pakai: Workers.** Berkas `index.html` disimpan sebagai static asset,
lalu Cloudflare menyajikannya dari edge. Tidak ada kode server yang berjalan sama
sekali, walaupun secara teknis ini "Worker".

---

## 3. Kenapa Cloudflare, bukan yang lain

Kebutuhannya: satu berkas HTML statis, gratis, HTTPS otomatis, bisa dipasang
domain sendiri nanti, dan yang paling penting **nol perawatan** karena ini proyek
sampingan yang dikerjakan sambil jalan.

| Pilihan | Kenapa tidak / kenapa iya |
|---|---|
| VPS (DigitalOcean, dsb.) | berbayar bulanan, harus urus Nginx, sertifikat SSL, patch keamanan OS. Perawatannya jauh lebih besar dari nilai proyeknya |
| GitHub Pages | gratis dan gampang, tapi jaringannya tidak sekuat Cloudflare dan kontrol cache-nya terbatas |
| Netlify / Vercel | bagus, tapi kuota bandwidth gratisannya lebih ketat dan modelnya berorientasi build pipeline yang tidak dibutuhkan di sini |
| **Cloudflare Workers** | **dipakai.** Gratis di skala ini, HTTPS otomatis, jaringan global, deploy satu perintah, domain kustom kapan saja |

Catatan sejarah: rencana awal memakai Cloudflare **Pages**, tapi yang jadi
dideploy adalah **Worker**. Ternyata itu justru pilihan yang benar. Sejak Workers
bisa menyajikan static assets, dokumentasi Cloudflare mengarahkan proyek baru ke
Workers, dan Pages tetap didukung tapi tidak lagi jadi arah pengembangan mereka.
Tidak ada yang perlu dipindah.

**Kenapa gratis?** Paket gratis Workers memberi 100.000 permintaan per hari, dan
permintaan untuk berkas statis tidak ditagih. Traffic landing page dari bio media
sosial tidak akan mendekati angka itu. Kalau suatu saat mendekati, itu masalah
yang menyenangkan.

---

## 4. Arsitektur & alur

```
   Bio Threads / Instagram / X
              │
              ▼
   overtime-boo.gulliveragent.workers.dev
   (Cloudflare Worker, static asset)
              │
              │  pengunjung menyaring kategori,
              │  membaca cara pakai & FAQ
              ▼
   lynk.id/overtimeboo/<slug>
   (checkout, pembayaran, kirim berkas)
              │
              ▼
   PDF masuk ke email pembeli
   (watermark nama pembeli, otomatis dari Lynk)
```

Yang perlu ditekankan saat menjelaskan ke orang lain: **halaman ini tidak pernah
menyentuh uang atau data pembeli.** Begitu pengunjung menekan tombol beli, dia
keluar ke Lynk. Tidak ada formulir, tidak ada cookie pelacak, tidak ada
penyimpanan apa pun di sisi kita.

Isi berkas:

```
index.html
├── <style>   token warna, tipografi, layout  (inline)
├── <body>    header, hero, katalog, cara pakai, kategori, FAQ, penutup, footer
└── <script>  array PRODUK & KATEGORI + logika filter chip dan akordeon FAQ
```

Kartu produk tidak ditulis manual. Semuanya dirender dari array `PRODUK` di
dalam `<script>`. Menambah listing baru = menambah satu objek di array itu.

---

## 5. Struktur repositori

```
overtimeboo-etalase/
├── public/
│   ├── index.html        ← seluruh situs ada di sini
│   ├── og.png            ← gambar pratinjau saat link dibagikan (1200×630)
│   └── favicon.png
├── wrangler.jsonc        ← konfigurasi Cloudflare
├── .gitignore
├── .assetsignore         ← opsional, berkas yang tidak ikut diunggah
└── README.md
```

`wrangler.jsonc`:

```jsonc
{
  "name": "overtime-boo",              // jadi subdomain: overtime-boo.<akun>.workers.dev
  "compatibility_date": "2026-08-28",  // mengunci perilaku runtime ke tanggal ini
  "assets": {
    "directory": "./public"            // folder yang isinya disajikan sebagai situs
  }
}
```

Tidak ada kunci `main`, karena tidak ada kode server. Ini Worker yang isinya
hanya berkas statis.

`compatibility_date` gunanya mengunci versi perilaku runtime Cloudflare. Kalau
mereka mengubah sesuatu tahun depan, situs ini tetap berjalan dengan perilaku
tanggal itu. Jangan diubah tanpa alasan.

---

## 6. Alur kerja: dari edit sampai live

```
1. edit public/index.html
2. npx wrangler dev          → pratinjau lokal, cek di 360px & zoom 200%
3. git commit                → catat perubahan
4. npx wrangler deploy       → naik ke produksi, beberapa detik
5. buka di jendela penyamaran → pastikan versi baru yang muncul
```

Langkah 2 tidak boleh dilewat. Pratinjau lokal itu satu perintah, dan dia yang
menangkap kesalahan sebelum kelihatan pengunjung.

---

## 7. Menyambungkan ke Cloudflare

Sekali saja per komputer.

### 7.1 Prasyarat
Node.js versi 18 ke atas. Cek: `node -v`

### 7.2 Login
```bash
npx wrangler login
```
Membuka browser, minta izin ke akun Cloudflare. Setelah itu token tersimpan di
komputer, tidak perlu diulang.

Verifikasi:
```bash
npx wrangler whoami
```
Harus muncul akun `gulliveragent`.

### 7.3 Deploy pertama
```bash
npx wrangler deploy
```
Cloudflare membaca `wrangler.jsonc`, mengunggah isi `public/`, dan menyalakan
Worker bernama `overtime-boo`. Alamatnya otomatis
`https://overtime-boo.gulliveragent.workers.dev`.

Subdomain `gulliveragent` itu nama akun, ditentukan sekali saat pertama memakai
Workers dan berlaku untuk semua Worker di akun yang sama.

### 7.4 Kalau nanti punya domain sendiri
Dasbor Cloudflare → **Workers & Pages** → pilih `overtime-boo` → **Settings** →
**Domains & Routes** → **Add custom domain**. Alamat `workers.dev` tetap hidup
berdampingan, tidak mati.

Syaratnya domainnya sudah didaftarkan dan nameserver-nya diarahkan ke Cloudflare.

---

## 8. Push ke GitHub

### 8.1 Inisialisasi
```bash
cd overtimeboo-etalase
git init
git branch -M main
git add .
git commit -m "Etalase overtimeboo: halaman statis + konfigurasi Workers"
```

### 8.2 Buat repositori dan dorong
```bash
git remote add origin https://github.com/<username>/overtimeboo-etalase.git
git push -u origin main
```

### 8.3 `.gitignore`
```gitignore
node_modules/
.wrangler/
.dev.vars
.env
.DS_Store
*.log
```

### 8.4 Yang tidak boleh ikut ter-commit
- **API token Cloudflare.** Jangan pernah ditulis di berkas mana pun di repo.
  `wrangler login` menyimpan kredensial di luar folder proyek, jadi selama pakai
  cara itu, aman.
- `.dev.vars` dan `.env`, kalau suatu saat dipakai.
- Isi folder `.wrangler/`, itu cache lokal.

`account_id` di `wrangler.jsonc` bukan rahasia, aman ikut ter-commit. Tapi tanpa
token, orang lain tidak bisa deploy ke akun ini.

### 8.5 Publik atau privat?
Isinya hanya HTML statis dan tautan produk yang memang sudah publik, jadi
repositori publik tidak membocorkan apa pun. Privat juga tidak masalah. Pilihan
bebas.

---

## 9. Opsional: deploy otomatis dari Git

Sekarang deploy-nya manual (`wrangler deploy`), dan untuk proyek sekecil ini itu
cukup. Kalau nanti ingin setiap `git push` otomatis naik ke produksi:

Dasbor Cloudflare → Worker `overtime-boo` → **Settings** → **Build** → hubungkan
repositori GitHub. Setelah itu setiap push ke `main` memicu deploy.

**Jangan dipasang sekarang.** Untuk satu berkas yang jarang berubah, satu
perintah manual lebih sederhana dan lebih mudah dibatalkan daripada pipeline yang
harus diurus.

---

## 10. Rollback & pemecahan masalah

**Kembali ke versi sebelumnya:**
```bash
npx wrangler deployments list     # lihat riwayat
npx wrangler rollback             # kembali ke deployment sebelumnya
```
Cloudflare menyimpan riwayat deployment, jadi salah deploy bukan kejadian fatal.

| Gejala | Penyebab & solusi |
|---|---|
| yang muncul masih versi lama | cache browser. Muat ulang paksa (Ctrl+Shift+R) atau buka di jendela penyamaran |
| `wrangler: command not found` | pakai `npx wrangler`, atau `npm install -D wrangler` |
| error otentikasi | `npx wrangler logout` lalu `npx wrangler login` |
| berkas tidak ikut naik | cek `assets.directory` di `wrangler.jsonc` menunjuk folder yang benar, dan `index.html` memang ada di dalamnya |
| halaman kosong / 404 | nama berkasnya harus persis `index.html`, huruf kecil semua |

---

## 11. Biaya

| Item | Sekarang |
|---|---|
| Hosting Cloudflare Workers | Rp0, paket gratis |
| Subdomain `workers.dev` | Rp0 |
| Sertifikat HTTPS | Rp0, otomatis |
| Domain sendiri | belum dibeli, opsional |

Selama masih di paket gratis, tidak ada kartu kredit yang perlu didaftarkan dan
tidak ada tagihan yang bisa membengkak diam-diam.

---

## 12. Belum dikerjakan

Sengaja ditunda, bukan kelupaan:

- domain sendiri
- analitik pengunjung
- kolom pencarian dan halaman per kategori (belum perlu di 15 produk)
- penangkap email
- deploy otomatis dari Git

Prinsipnya: tambah lapisan hanya kalau sudah terasa kurang, bukan karena bisa.
