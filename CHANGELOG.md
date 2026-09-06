# Riwayat versi

Repo ini dibuat setelah situsnya sudah jalan, jadi commit sebelum
15 Agustus 2026 **diimpor secara retroaktif** dari arsip berkas. Tanggal
pada commit mencerminkan kapan versi itu benar-benar dibuat, tapi
commit-nya sendiri baru dicatat belakangan. Ini disebutkan supaya
riwayatnya tidak dibaca sebagai jejak kerja harian.

---

## v3.7 — 6 September 2026 — **peta tahapan, rekomendasi per tahap, dan dua tema**
Pengunjung sekarang lihat dia lagi di tahap seleksi mana, lalu langsung dapat
produk untuk tahap itu, tanpa harus turun ke katalog. Penyaringan yang tadinya
dikerjakan pengunjung pindah ke halaman.

- **peta tahapan naik**, rel geser kiri-kanan berisi 5 tahap PCPM BI angkatan 41.
  Geser mendatar, bukan tumpuk vertikal: 5 kartu ditumpuk ke bawah cuma
  manjangin halaman tanpa nambah informasi. Waktu halaman dibuka, relnya sudah
  digeser sendiri supaya tahap yang lagi jalan ada di kiri layar — pengunjung
  nggak perlu nyari.
- **status nggak pernah ditulis manual.** `lewat / berlangsung / berikutnya /
  nanti` dihitung dari tanggal tiap halaman dibuka. Kalau semua tahap sudah
  lewat, seluruh blok ganti sendiri jadi kartu "angkatan 41 sudah selesai".
  Ini yang bikin fiturnya aman ditinggal tanpa diurus.
- **banner `<div class="ticker">` dihapus** berikut aturan CSS-nya. Isinya sama
  dengan peta tahapan, dan tanggal yang ditulis di dua tempat itu cara jadi
  salah. README bagian 12 ditulis ulang mengikuti ini.
- **blok "Siapin yang ini dulu"** di bawah peta, terikat ke tahap sorotan,
  maksimal 3 kartu. Kartunya dirender pakai fungsi kartu katalog yang sama —
  satu sumber, satu tampilan, bukan komponen kedua.
- **tahap tanpa listing nggak jadi jalan buntu.** `psiko` dan `wawancara`
  belum punya produk, jadi kartunya berbunyi "materi lagi disiapin" dengan
  tombol preview gratis, bukan tombol mati. Opsi B di handoff v3.7 §1.1.
- **strip dampak turun satu tingkat**, dari dalam hero ke bawah blok
  rekomendasi. Pertanyaan "kenapa beli di sini" baru relevan setelah pengunjung
  tahu "kapan tesnya".
- **tanggal `wawancara` belum diverifikasi** ke pengumuman resmi. Empat tahap
  lain cocok dengan tabel rujukan di README bagian 12. Betulkan sebelum deploy.

Tiga hal sengaja menyimpang dari handoff v3.7, alasannya dicatat di komentar
kode dan README bagian 12:
- **kartu `lewat` nggak dikasih opacity 0.55** seperti tabel §5. Di angka itu
  teksnya jatuh jauh di bawah lantai kontras 4.5:1 yang dipatok §5 juga.
  Kesan redupnya diambil dari permukaan navy-nya, kontras terendah 6.57:1.
- **geser awal nggak dibungkus `requestAnimationFrame`** seperti §5.
  rAF nggak jalan di tab latar, jadi relnya bakal diam di awal sampai tabnya
  dibuka lalu melompat. Lebar kartu dikunci `flex-basis`, nggak berubah waktu
  font masuk, jadi nggak ada yang perlu ditunggu.
- **tanggal dihitung lokal, bukan `toISOString()`** seperti §4. toISOString
  memakai UTC, jadi buat pemakai WIB/WITA/WIT timeline-nya telat sehari tiap pagi.

### Tema: style Noxfolio, tone biru, gelap + terang

Tampilannya dibangun ulang di atas style template Noxfolio, tapi aksen lime-nya
dibuang seluruhnya dan tone-nya tetap biru seperti v3.6, supaya nyambung sama
situs yang pembeli sudah lihat tiga minggu terakhir. Font pindah ke DM Sans +
Inter. Sekarang ada mode gelap dan terang, dua-duanya biru.

- **toggle tema di header**, sebelah pill lynk.id. Tiga keadaan: ikut sistem
  (default), dipaksa terang, dipaksa gelap. Pilihannya disimpan di
  `localStorage` dan dipasang lewat script inline di `<head>` sebelum paint,
  jadi tidak ada kedip putih waktu memuat. Semua akses `localStorage` dibungkus
  `try/catch` — di mode penyamaran dia melempar dan bisa mematikan seluruh
  script halaman.
- **22 nilai `rgba(201,243,29,a)` yang ditulis langsung** di luar `:root`
  diubah jadi `rgba(var(--primary-rgb), a)`. Kalau cuma `--primary` yang
  diganti, glow tombol, isian chip aktif, dan garis tepi kartu tetap lime.
  Ikut dibereskan: 6 `#070707`, 27 putih harfiah, 6 overlay putih, latar
  header `rgba(19,19,19,.92)`, dan 8 bayangan hitam — semuanya jadi token.
- **satu set token `--on-dark-*`** untuk bidang yang navy di kedua tema
  (band cara pakai, footer, kotak aksi bundle, kartu tahapan `lewat`). Tanpa
  ini, di mode terang teks footer cuma 1,39:1 dan harga di kotak bundle
  1,00:1 — navy di atas navy, betul-betul tak terlihat.
- **tombol transaksi tetap hijau** (`.theme-btn--buy`), tidak ikut biru.
  Biru = merek dan navigasi, hijau = beli dan ambil gratis. Kalau disamakan,
  tombol beli tidak terbedakan dari tautan biasa.
- **dua `<meta name="theme-color">`** dibedakan `media=`, menggantikan
  `#131313` sisa Noxfolio.

Yang diperbaiki sambil jalan, semuanya cacat yang terbawa dari berkas Antigravity:

- **klaim "tanpa pengulangan soal" dicabut** dari deskripsi bundle unggulan.
  Itu varian dari "nol pengulangan antar paket" yang sudah dicabut di v3.6
  karena tidak bisa dipertahankan lintas lini. Diganti klaim proses yang memang
  dikerjakan pipeline QC-nya.
- **fallback `|| TL[1]` di `SOROT` dihapus.** Dengan fallback itu, blok
  keadaan-habis tidak pernah jalan: kalau semua tahap sudah lewat halaman malah
  menyorot SPD 5–6 Sep selamanya dan menayangkan jadwal basi — persis masalah
  yang bikin peta tahapan ini dibuat. Kartu "Angkatan 41 sudah selesai"
  dikembalikan.
- **tahap tanpa listing tidak lagi menampilkan produk acak.** Sebelumnya
  `psiko` dan `wawancara` memunculkan tiga bundle yang tidak ada
  hubungannya, seolah-olah itu materi tahap tersebut. Sekarang kartunya bilang
  apa adanya + tombol preview gratis.
- **header meluber 256px di 390px** karena empat tautan jangkar + pill lynk.id
  + tombol tema baru. Tautan jangkar disembunyikan di bawah 860px, wordmark
  disembunyikan di bawah 400px. Nol scroll mendatar di 320/390/401/861/1280px.
- **opacity .65 dinaikkan ke .78** di kartu tahapan `nanti` dan baris
  kategori "Segera". Di .65 teks sekundernya cuma 2,78–3,62:1.
- **label harga 0,8rem (12,8px) dinaikkan ke `--fs-meta`**, mengembalikan
  lantai 14px yang berlaku di situs ini.

Kontras diukur ulang di halaman jadi, bukan disalin dari tabel. Terendah di
mode terang 4,95:1, di mode gelap 4,75:1. Rincian per keadaan di README bagian
12 dan 13.

Ukuran: `index.html` 40,5 KB (v3.6 live) → 66,4 KB, +25,9 KB mentah,
+5,7 KB setelah gzip. Naiknya dari dua hal yang memang ditambah — peta tahapan
plus rekomendasi, dan sistem dua tema — bukan dari template Noxfolio yang ikut
terbawa. Anggaran +8 KB di handoff v3.7 sebelumnya sudah tidak berlaku untuk
pass ini; kalau ukuran mau ditekan, komentar penjelas yang paling gampang
dipangkas dan hampir tidak berpengaruh setelah gzip.

---

## v3.6 — 4 September 2026 — **siap deploy**
Koreksi timeline SPD, pencabutan satu klaim yang tidak bisa dipertahankan, dan
lini Potensi Dasar naik. Listing Lynk-nya selesai dibuat Deus sore 4 Sep, jadi
enam kartu yang tadinya ditahan sekarang tayang. Katalog 15 kartu jadi 21.

- **banner diperbaiki.** `TPD ~1 Sep · Tes teknis 2–20 Sep` (dua-duanya salah,
  dan salah ke arah yang bikin pengunjung mengira tesnya sudah lewat) diganti
  jadi `SPD 5–6 Sep · Tes teknis minggu ke-3 Sep`. Masa kedaluwarsanya sekarang
  dicatat di README bagian 12 supaya tidak terulang.
- **klaim "200 soal, nol pengulangan antar paket" dicabut** dari hero. Angkanya
  cuma milik lini PCPM tapi posisinya terbaca sebagai klaim seluruh situs, dan
  "nol pengulangan" tidak bisa dipertahankan lintas lini (16 gambar spasial
  identik antara paket 1 dan 2, figural psikotes 24 unik dari 32). Situs ini
  menyatakan tidak menerima refund, jadi klaim yang bisa dibantah pembeli adalah
  risiko nyata. Diganti klaim proses, yang memang dikerjakan pipeline QC-nya.
- **array PRODUK mendukung banyak kategori per kartu** (`kategori:["pcpm","karier"]`).
  Materi potensi dasar melayani SPD PCPM sekaligus GAT BUMN. Tanpa chip keenam —
  chip keenam bikin barisnya penuh di layar HP.
- **kartu dengan `url` kosong tidak pernah dirender.** Aturan "jangan naikkan
  kartu tanpa URL Lynk yang valid" sekarang dijaga kode, bukan ingatan.
- **enam kartu Potensi Dasar naik** dengan URL Lynk asli, diverifikasi satu per
  satu terhadap salinan halaman etalase Lynk yang diunduh 4 Sep: 21 slug di
  situs, 21 slug di Lynk, nol selisih di kedua arah.
  Judul dan harga kartu disamakan dengan listing, dicek ulang terhadap salinan
  halaman Lynk kedua (15.08 WITA): 21 slug cocok dua arah, **nol** selisih harga.
  Harga mengikuti Lynk, bukan rencana lama: bundle **Rp30.000** (rencana Rp79.000,
  sempat Rp25.000) dan drill **Rp10.000** (rencana Rp15.000). Isi bundle
  terkonfirmasi 700 soal, jadi psikotes ikut. Konsekuensi harganya dicatat di
  `05-dokumen/instruksi-proyek/status-eksekusi-handoff-prioritas-040926.md`.
- **kartu Drill Spasial ditulis "84 soal unik", bukan "100 soal".** Berkasnya
  memuat 100 nomor tapi 16 di antaranya duplikat persis. Situs menyatakan tidak
  menerima refund, jadi angka yang dipakai adalah angka yang bisa dipertahankan.
- **baris kategori Potensi Dasar** berubah dari menunjuk preview gratis jadi
  menyaring katalog ke chip Karier, karena sekarang ada 7 kartu di belakangnya.
  TOEFL ITP digeser ke urutan terakhir supaya klik "Potensi Dasar" memunculkan
  kartu Potensi Dasar lebih dulu.
- **kartu preview diarahkan ke listing preview yang baru** (`jd979xm4m39x`,
  "Preview Gratis — 50 Soal Potensi Dasar PCPM BI"). Listing preview lama
  (`qymkqox1v251`, masih berjudul "50 Soal GAT BUMN") sudah tidak dirujuk situs
  sama sekali, tapi masih hidup di Lynk. Turunkan dari dashboard: dua preview
  gratis untuk isi yang sama bikin pengunjung ragu, dan judul lama itu satu-satunya
  tempat kata GAT masih terlihat pembeli.
- **seksi Kategori disusun ulang berdasarkan tanggal terdekat**, dari 4 baris
  jadi 5: Potensi Dasar (5–6 Sep) → PCPM BI + B. Inggris (minggu ke-3 Sep) →
  Psikotes (minggu ke-2 Okt) → CPNS SKD (22 Sep – 7 Okt) → TOEFL ITP (tanpa
  tanggal). Keterangan kartu CPNS `Nunggu jadwal resmi 2026` sudah basi karena
  jadwalnya keluar, diganti tanggal sebenarnya.
- **FAQ tes kepribadian ditambahkan di posisi kedua**, bukan paling bawah.
  Dengan kebijakan tanpa refund, batasan produk harus dinyatakan sebelum orang
  bayar. Ini batasan yang paling mungkin jadi keluhan.
- **`<title>` dan meta description** diganti ke kata yang benar-benar diketik
  orang. "Karier" dan "BUMN" bukan kata pencarian.
- **CTA penutup** menyebut dua preview gratis, bukan satu.

Sengaja tidak dikerjakan di versi ini: halaman artikel/SEO, perubahan hero, gaya
copy, dan tema warna.

## v3.5 — 15 Agustus 2026 (masih live)
Copy ditulis ulang dengan gaya Satu Persen. Nol perubahan desain.

Masalah yang diperbaiki: seluruh halaman memakai satu bentuk kalimat yang
sama (pertentangan dua klausa) sebanyak 12 kali, sehingga terdengar seperti
kumpulan pepatah. Pembaca juga hampir tidak pernah disapa.

- hero berubah dari pepatah jadi pertanyaan yang menyebut situasi pembaca
- sapaan `kamu` naik dari 1 slot ke 9 slot
- FAQ mengakui dulu hal yang tidak enak (tidak ada refund) sebelum
  menawarkan jalan keluar
- kalimat berpola pertentangan tersisa 2 dari 12
- tagline footer jadi `Your best study companion.`

## v3.4 — 14 Agustus 2026
Versi pertama yang dideploy ke Cloudflare Workers.

- placeholder `{{ }}` dari ekspor design canvas diganti render nyata
- 15 kartu produk digenerate dari array `PRODUK`, bukan ditulis manual
- copy dinaikkan dari tingkat fitur ke tingkat dampak
- bar statistik diganti strip dampak
- tombol penutup diubah jadi preview gratis, bukan beli sekarang
- instruksi menyalin pembahasan ke tool AI pihak ketiga dicabut, karena
  posisinya di bawah katalog justru mengarahkan pembeli untuk tidak
  membeli paket berikutnya

## v3.3 — 14 Agustus 2026 (mockup)
Desain dikunci di versi ini. Perubahan setelahnya hanya menyentuh teks.

- radius dibatasi 4 nilai
- anggaran warna merah maksimal 3 per layar
- kapitalisasi dinaikkan dari lowercase ke sentence case

## v3.2 — 13 Agustus 2026 (mockup)
Tipografi diganti dari Newsreader ke Archivo 800 dengan `font-stretch`.
Hero mendapat highlight berbingkai.

## v3.1 — 13 Agustus 2026 (mockup)
Palet navy dikunci, rasio kontras diaudit.

## v3 — 13 Agustus 2026 (mockup)
Eksplorasi awal peralihan dari warm cream ke navy.

## v1 — 12 Agustus 2026 — **tidak terarsip**
Versi pertama: tema warm cream (`#F4EDE0`), tipografi Space Mono +
IBM Plex Sans. Sudah dideploy dan sempat live, tapi berkasnya ditimpa
saat v3.4 naik dan tidak tersimpan di arsip mana pun yang tersedia.

Kalau berkas `index-v1-backup.html` masih ada di komputer (handoff waktu itu
meminta backup dibuat sebelum menimpa), simpan ke `design/v1-warm-cream.html`
lalu commit. Urutannya tidak akan kronologis, tapi artefaknya terselamatkan.
