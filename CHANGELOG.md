# Riwayat versi

Repo ini dibuat setelah situsnya sudah jalan, jadi commit sebelum
15 Agustus 2026 **diimpor secara retroaktif** dari arsip berkas. Tanggal
pada commit mencerminkan kapan versi itu benar-benar dibuat, tapi
commit-nya sendiri baru dicatat belakangan. Ini disebutkan supaya
riwayatnya tidak dibaca sebagai jejak kerja harian.

---

## v3.8 — 18 September 2026 — **timeline sampai Desember, dan dua klaim yang dicabut**
Hasil audit 18 Sep (laporan lengkap di `04-web-etalase/audit-180926/`). Tiga
masalah yang kelihatan pengunjung diperbaiki, plus urutan katalog sekarang ikut
tahap yang lagi jalan.

- **tanggal tes teknis diperbaiki.** Array masih pakai jendela lama 14–20 Sep
  warisan "minggu ke-3 Sep", jadi halaman masang badge BERLANGSUNG buat tes yang
  baru mulai besoknya. Jadwal resmi 19–20 Sep, dan labelnya sekarang tanggal
  pasti, bukan nama minggu.
- **timeline diperpanjang sampai tahap akhir Desember**, dari 5 kartu jadi 7.
  Tahap terakhir yang lama selesai 31 Okt, artinya sejak 1 Nov seluruh blok
  ganti sendiri jadi kartu "angkatan 41 sudah selesai" — lima minggu sebelum
  seleksinya benar-benar kelar. Yang ditambah: kesehatan & psikiatri, wawancara
  pra tahap akhir, dan tahap akhir. Tahap pengumuman sengaja tidak dimasukkan;
  yang ditampilkan cuma tahap yang peserta datangi.
- **kartu preview gratis Bahasa Inggris masuk katalog** dan masuk daftar produk
  tahap tes teknis. Listing-nya sudah hidup di Lynk tapi belum pernah muncul di
  etalase. Daftar rekomendasi per tahap naik dari 3 ke 4 kartu supaya dua
  preview dan dua bundle muat sekaligus di minggu tes.
- **dua klaim dicabut.** "Dicek kemiripannya satu per satu sebelum rilis"
  (muncul di strip dampak dan kartu unggulan) tidak berlaku untuk semua produk:
  drill spasial 84 unik dari 100, figural psikotes 24 dari 32, dan keunikan
  gambar tidak pernah dicek sebelum 4 Sep. Diganti klaim yang lolos audit:
  kunci disebar rata per huruf, dan trik nebak opsi terpanjang tidak lebih baik
  daripada nebak acak. FAQ "boleh dibagi ke temen" berhenti menjanjikan
  watermark nama pembeli di tiap PDF — produk lini Potensi Dasar dikirim sebagai
  ZIP, dan Lynk mem-watermark PDF, bukan isi arsip. Belum terbukti, jadi tidak
  diklaim.
- **urutan katalog ikut tahapan.** Diturunkan dari tahap sorotan di array
  `TAHAPAN` yang sama, bukan dari logika tanggal kedua: selama tes teknis
  urutannya PCPM, English satuan, Potensi Dasar, lain-lain; setelah tahap itu
  lewat, English satuan turun paling bawah dan Potensi Dasar naik ke tengah.
  Preview gratis selalu duluan di dalam lininya.
- **kartu unggulan dikunci ke slug**, bukan ke `PRODUK[0]`. Begitu urutannya
  bisa berubah, posisi array berhenti berarti "kartu unggulan". Pencarian
  preview juga pindah dari pola judul ke slug — pola `/PCPM BI/` pecah begitu
  preview kedua yang judulnya juga memuat "PCPM BI" masuk array.
- **jumlah soal unik dicantumkan** di kartu bundle Potensi Dasar: 700 nomor,
  676 unik. Dihitung ulang dari JSON sumber hari ini, bukan disalin dari
  dokumen handoff. Irisan antara set Potensi Dasar dan set psikotes nol, jadi
  676 itu angka final, bukan batas atas.

Belum dikerjakan, menunggu keputusan: teks `<title>`/meta dan hero eyebrow yang
masih menyebut CPNS, eyebrow "Coming Next", CTA penutup yang menjanjikan dua
preview lewat satu tombol, dan tanggal yang masih ditulis kedua kali di
deskripsi kartu Kategori.

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

### Putaran umpan balik Deus (7 Sep)

- **default TERANG, bukan ikut setelan OS.** Pengunjung yang HP-nya gelap sempat
  langsung dapat situs gelap. Sekarang gelap cuma aktif lewat toggle. Seluruh
  `@media (prefers-color-scheme)` dan `matchMedia` warna dicabut, dan
  `<meta name="theme-color">` jadi satu yang diperbarui JS tiap toggle.
- **garis bawah sorotan hero jadi merah** dengan nada lebih muda dari teksnya
  (`rgba(var(--accent-rgb), .38)`). Sebelumnya kebiruan dan nggak nyambung.
- **ikon bintang dihapus** dari semua sub-judul (8 di HTML, 1 di template JS).
- **animasi tombol dipisah dua gaya.** Navigasi: cincin cahaya + ikon maju ke
  kanan. Checkout: pil padat, nol cincin, ikon diam.
- **"Siapin Yang Ini Dulu" jadi "Coming Next"**, dan judul di bawahnya cuma nama
  materinya. Kata "lagi berlangsung" dibuang, `jarakTeks()` ikut dihapus.
- **kata "Investasi" dan "Akses" dicabut** dari kartu produk dan kartu unggulan.
- **seksi timeline dirapikan**: sub-judul "Peta Tahapan" dan pill "Jadwal PCPM
  Bank Indonesia Angkatan 41" dihapus, judulnya jadi "Timeline PCPM BI".
- **kartu tahap yang sudah lewat jadi outline muted**, bukan blok navy penuh.
  Yang dapat blok cuma tahap sorotan, dan bloknya selalu kebalikan halaman:
  navy di tema terang, terang di tema gelap.
- **kontras katalog di tema gelap dibetulkan.** `--surface` dinaikkan dari
  #0E2A55 ke #143866 (1,22:1 -> 1,48:1 terhadap latar), kartu unggulan jadi blok
  terang (15,15:1), dan gradien kartu gratis ditumpuk di atas `--surface`
  karena versi lama mulai dari transparan dan bagian atasnya nggak punya isian.

### Aset: favicon & OG (7 Sep)

- **klaim "Nol pengulangan antar paket" dicabut dari og.png.** Klaim itu sudah
  dicabut dari situs sejak v3.6 karena tidak bisa dipertahankan lintas lini,
  tapi masih ikut terbawa tiap kali tautannya dibagikan ke WhatsApp atau
  Threads. Diganti tiga klaim proses: pembahasan tiap nomor, dicek
  kemiripannya sebelum rilis, file langsung masuk email.
- **og.png pakai TEMA TERANG**, mengikuti tema default situs. Latar `--bg`,
  kartu `--surface`, sorotan `--accent` dengan garis bawah merah muda,
  badge isian padat `--primary`. Font DM Sans + Inter.
- **favicon.png**: font DM Sans, gugus huruf + titik ditengahkan (sebelumnya
  bergeser ~25px ke kanan). Tetap navy beraksen merah supaya kebaca di bilah
  tab terang maupun gelap.
- keduanya di-recompress lossless: og 103 KB -> 76,6 KB, favicon 20,3 -> 13,7 KB.
- **berkas OG dinamai berversi: `og.png` -> `og-v2.png`.** WhatsApp, Threads,
  dan Facebook nge-cache gambar OG lama dan nggak peduli isinya sudah diganti.
  Karena gambar lama memuat klaim yang sudah dicabut, nunggu cache lewat sendiri
  bukan pilihan. Kalau isi OG berubah lagi, **naikkan lagi angkanya** — jangan
  timpa berkas dengan nama yang sama.
- **`.assetsignore` dipindah ke `public/`.** Cloudflare Workers cuma membaca
  berkas itu dari dalam direktori aset, jadi di akar repo dia tidak pernah
  berlaku. Catatan: kalau deploy-nya lewat drag-drop dashboard, `.assetsignore`
  sama sekali tidak dipakai — berkas yang tidak ingin naik harus benar-benar
  tidak ada di `public/`.

### Putaran umpan balik kedua (7 Sep)

- **badge seksi dipadatkan.** Isian `--primary` dengan teks `--on-primary`,
  ukuran naik ke `--fs-ui`. Versi 8% alpha sebelumnya nyaris hilang di halaman.
  "COMING NEXT" sekarang navy berteks putih, kebalik sendiri di tema gelap.
- **label "Berikutnya" dan "Nanti" dihapus dari layar.** Blok sorotan sudah
  bilang mana yang berikutnya dan sisanya jelas belakangan. Labelnya tetap
  ditulis buat pembaca layar — status nggak boleh cuma lewat warna.
- **"Kamu lagi ngejar tes yang mana?" jadi "Mau belajar apa hari ini?"**
- **kartu bundle unggulan jadi hijau** lewat set token `--flag-*` sendiri,
  lepas dari `--hi-*` milik timeline. Dua blok itu bawa pesan beda — "kamu
  lagi di sini" vs "ini yang direkomendasiin" — dan kalau dua-duanya navy
  mereka rebutan mata. Badge "BUNDLE UTAMA" dan "Hemat 30%" ikut dipadatkan;
  keduanya sebelumnya pakai isian alpha yang jadi putih-di-atas-putih begitu
  kartunya hijau.
- **"Beli di Lynk" jadi "Checkout", "Ambil gratis" jadi "Download"**, termasuk
  tombol kartu unggulan dan kartu "Segera".

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
