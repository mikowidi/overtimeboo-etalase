# SPEC v3.3 — POLISH GRAMMAR KARTU & PERBAIKAN PENULISAN

Tanggal: 14 Agustus 2026
Menemani: `spec-etalase-navy-v3.1.md` (token warna & kontras masih berlaku)
dan `spec-etalase-navy-v3.2.md` (tipografi Archivo masih berlaku)
Mockup: `Etalase Navy v3.3.dc.html` (v3.2 disimpan sebagai pembanding)

v3.3 **tidak menyentuh** tipografi §1 v3.2 maupun token warna §1 v3.1.
Yang diganti: **anatomi kartu/baris**, **format kapitalisasi**, dan
**hitungan merah** (sekarang benar-benar ≤3 per layar).

---

## 0. TIGA REFERENSI BARU, TIGA CIRI

| Ciri | Dari referensi |
|---|---|
| **Pill inset di dalam kartu** — konten kiri, tag kecil menempel di kanan | kartu task (baris avatar + tag `Medium`) |
| **CTA selebar kartu**, ikon lingkaran di kiri, label di tengah | kartu profil (`Get in Touch`), kartu task (`Create task`) |
| **Baris daftar**: kotak ikon membulat → judul + subjudul → chevron lingkaran | dashboard smart home (`Air Conditioner`, `2 unit 18kWh`, `›`) |
| Meta pakai **pasangan ikon lingkaran + label pendek** | kartu task (`26 Feb`, `10:15pm`) |
| Chip filter **terisi tanpa border**, aktif = blok gelap | kartu task (chip tanggal, `08 Apr Today`) |

Ikon: Lucide, inline SVG, stroke 2, ukuran 13/15/20px sesuai wadah.
Nol ikon dekoratif — setiap ikon menandai satu fakta (jumlah soal, format
file, kategori, arah navigasi).

---

## 1. ANATOMI KARTU PRODUK (BARU)

Lima blok, urut, `gap:16px`, `display:flex;flex-direction:column`:

```
1  header    : tag kategori (pill terisi)          ·  nomor (kanan, tabular)
2  judul     : 700 1.18rem/1.26, stretch 108%
3  meta      : [ikon] 40 soal   [ikon] PDF
4  pill inset: Rp10.000                            ·  tag "Kunci + pembahasan"
5  CTA       : [lingkaran ikon]        Checkout
```

```css
.card{ background:var(--card); border:1px solid var(--line);
  border-radius:26px; padding:20px; display:flex; flex-direction:column;
  gap:16px; transition:border-color .14s, transform .14s; }
.card:hover{ border-color:var(--navy); transform:translateY(-3px); }

/* 1 & 4 & 5 */
.tag{ background:var(--paper-2); color:var(--muted); border-radius:999px;
  padding:7px 14px; font:600 .66rem/1 var(--font); letter-spacing:.12em;
  text-transform:uppercase; white-space:nowrap; }
.tag--free{ background:#FBE9E6; color:var(--accent); }

.meta{ display:flex; flex-wrap:wrap; gap:10px 18px;
  font:500 .84rem/1 var(--font); color:var(--muted); }
.meta > span{ display:inline-flex; align-items:center; gap:8px; }
.meta i{ flex:none; width:26px; height:26px; border-radius:999px;
  background:var(--paper-2); color:var(--navy); display:grid; place-items:center; }

.inset{ display:flex; align-items:center; justify-content:space-between;
  gap:10px; background:var(--paper-2); border-radius:999px;
  padding:9px 9px 9px 18px; }
.inset .price{ font:800 1.24rem/1 var(--font); font-stretch:108%;
  font-variant-numeric:tabular-nums; letter-spacing:-.03em; }
.inset .tag{ background:var(--card); }

.cta{ display:flex; align-items:center; gap:10px; background:var(--navy);
  color:var(--on-navy); border-radius:999px; padding:7px;
  font:600 .88rem/1 var(--font); transition:background .14s; }
.cta:hover{ background:var(--navy-deep); }
.cta i{ flex:none; width:32px; height:32px; border-radius:999px;
  background:var(--card); color:var(--navy); display:grid; place-items:center; }
.cta span{ flex:1; text-align:center; padding-right:32px; }
```

`padding-right:32px` pada label = lebar ikon. Itu yang bikin label **optically
centered** terhadap pill, bukan tergeser ke kanan — sama seperti
`Get in Touch` di referensi. Kalau ukuran ikon diubah, ubah angka ini juga.

Varian: `.cta--ghost` (kartu gratis) = latar putih, border 1.5px navy,
ikon tetap terisi navy, hover → terisi navy penuh.

**Ini menggantikan** CTA pill kecil kanan-bawah v3.2 (§3.1). Alasan: dengan
CTA selebar kartu, area klik naik dari ±120×38 ke ±260×46 dan grid kartu
punya garis dasar yang rata — di v3.2 tinggi baris tombol berbeda-beda
tergantung panjang harga.

---

## 2. BARIS KATEGORI (BARU)

Ledger bergaris rambut v3.2 diganti **baris kartu** ala dashboard:

```css
.row{ display:grid; grid-template-columns:48px minmax(0,1fr) auto; gap:16px;
  align-items:center; background:var(--card); border:1px solid var(--line);
  border-radius:22px; padding:16px 18px; }
.row:hover{ border-color:var(--navy); transform:translateY(-2px); }
.row .ico{ width:48px; height:48px; border-radius:16px;
  background:var(--paper-2); color:var(--navy); display:grid; place-items:center; }
.row .t{ font:700 clamp(1.15rem,2.2vw,1.5rem)/1.2 var(--font);
  font-stretch:110%; letter-spacing:-.025em; }
.row .s{ margin-top:4px; font:500 .84rem/1.3 var(--font); color:var(--muted); }
.row .go{ width:36px; height:36px; border-radius:999px; background:var(--navy);
  color:var(--on-navy); display:grid; place-items:center; }
```

Baris "Segera" tidak punya `.go` (bukan link) — di tempatnya `.tag`
bertuliskan `Segera`, dan seluruh baris `opacity:.6`.
Subjudul menggantikan badge angka: `13 produk siap unduh` lebih informatif
daripada `13` + label `PRODUK` di v3.2.

Langkah "Cara pakai" ikut pola yang sama di atas navy: baris `border-radius:22px`
berlatar `--navy` di atas `--navy-deep`, nomor `01`–`04` dalam lingkaran
44px `#173A6B`. Panah `↗` di ujung baris dicabut — baris itu bukan link.

---

## 3. FORMAT KAPITALISASI (ATURAN BARU, WAJIB)

v3.1–v3.2 menulis **semua** teks huruf kecil, termasuk nama kategori dan
nama lembaga. Itu bikin `pcpm bank indonesia` dan `cpns skd 2026` terbaca
seperti salah tulis, bukan seperti gaya.

| Kelas teks | Aturan | Contoh |
|---|---|---|
| Judul (h1/h2/h3) | Kalimat, kapital awal | `Bank soal yang kunci sama pembahasannya lengkap.` |
| Nama produk | Judul (Title Case) | `Bank Soal PCPM BI — Paket 3` |
| Nama kategori | Judul + akronim kapital | `PCPM Bank Indonesia`, `GAT BUMN`, `CPNS SKD 2026` |
| Nav & footer nav | Kalimat | `Katalog`, `Cara pakai`, `Kategori` |
| Akronim | Selalu kapital | `PDF`, `TPU`, `FAQ`, `BI`, `BKN` |
| Label kecil / tag | HURUF BESAR via CSS `text-transform`, sumbernya tetap ditulis normal | `Bundle · hemat` → tampil `BUNDLE · HEMAT` |
| Body | Kalimat, tanda baca lengkap | `Soal, kunci, dan pembahasan tiap nomor.` |
| Nama merek | Sesuai merek | `overtimeboo`, `lynk.id`, `ChatGPT`, `Claude` |

Huruf besar **hanya** lewat `text-transform` di lapisan label, jadi teks
sumbernya tetap bisa dibaca dan di-copy dengan kapitalisasi normal.

Perbaikan penulisan yang sekaligus dilakukan:

| v3.2 | v3.3 |
|---|---|
| `kerjain dulu tanpa lihat kunci` | `Kerjakan dulu tanpa melihat kunci` |
| `minta 3 soal berpola sama` | `Minta 3 soal dengan pola yang sama` |
| `ulang besoknya` | `Ulangi besoknya` |
| `file-nya bisa dishare ke temen?` | `File-nya bisa dibagikan ke teman?` |
| `soal aslinya gak pernah sama persis` | `Soal aslinya tidak pernah sama persis.` |
| `pilih materi yang lagi kamu kejar` | `Pilih materi yang sedang kamu kejar` |
| `file PDF, download langsung baca. anywhere, anytime.` | `Format PDF, sekali unduh langsung bisa dibaca kapan saja.` |
| `career` (chip) | `Karier` |
| `mockup ini cuma memuat…` | `Mockup ini hanya memuat…` |

Nada tetap santai-langsung; yang dibuang cuma bentuk lisan (`gak`, `temen`,
`kerjain`) dan campur bahasa yang tidak perlu (`anywhere, anytime`).

---

## 4. `lynk.id` — MAKSIMAL 3 SEBUTAN

Aturan baru, dihitung per halaman:

| # | Tempat | Bentuk |
|---|---|---|
| 1 | CTA header | pill navy `lynk.id` + ikon `↗` |
| 2 | Footer nav | pill `#173A6B` `lynk.id` + ikon `↗` |
| 3 | — | *slot cadangan, kosong* |

Baris tagline footer sekarang `Bank soal digital, dikirim otomatis setelah
pembayaran.` — sebutan `lynk.id` ketiga dicabut supaya slot cadangan tetap
ada kalau nanti perlu satu di badan halaman.

**Semua tombol beli berbunyi `Checkout` saja.** `checkout di lynk` (3 tempat
di v3.2: kartu bundle lebar, kartu paket, kartu bundle grid) dihapus.
Ikon `↗` tetap ada di tombolnya, jadi sinyal "pindah domain" tidak hilang.

Kriteria uji: **teks yang terlihat** bertuliskan `lynk.id` → **≤3**
(`href="https://lynk.id/"` tidak dihitung; di v3.3 ada 2 label + 2 href),
dan `grep -c 'checkout di lynk' index.html` → **0**.

---

## 5. RADIUS — DIPETAKAN ULANG LAGI

| Elemen | v3.2 | v3.3 |
|---|---|---|
| hero, blok navy lebar, footer, kartu produk, empty state | 18px | **26px** |
| baris kategori, baris langkah, kartu kategori, FAQ | 18px | **22px** |
| kotak ikon di baris kategori | — | **16px** |
| kotak ikon di kartu kategori | — | **14px** |
| pill, chip, badge lingkaran | 999px | **999px** |

Dua nilai (26/22) menggantikan satu nilai 18px. Alasannya kartu di referensi
selalu **lebih membulat daripada elemen di dalamnya** — 26 di luar, 22 di
baris, 16/14 di kotak ikon, 999 di pill. Itu yang bikin susunannya terasa
bersarang, bukan rata.

---

## 6. ANGGARAN MERAH — SEKARANG ≤3 DI SEMUA LAYAR

| Layar | Merah | Jumlah |
|---|---|---|
| hero | titik monogram + border pill `lengkap.` | 2 |
| katalog | tag `GRATIS` + harga `Gratis` (satu kartu, dua penanda berdekatan) | 2 |
| cara pakai | nol | 0 |
| kategori | nol | 0 |
| faq | nol | 0 |
| footer | titik wordmark | 1 |

Yang dibuang dibanding v3.2, sesuai keputusan yang tertunda di §5 v3.2:

- nomor `00` merah di eyebrow katalog → `#8A96AC`
- nomor `00` merah di baris kategori pertama → dicabut bersama ledger
- **garis ambang batas + tick merah 2×11px** (`40 SOAL / PAKET`,
  `200 SOAL / BUNDLE`, `166 PASSING GRADE`) → dicabut seluruhnya

Catatan jujur soal yang terakhir: itu motif khas v3.1 dan pencabutannya
**bukan** permintaan eksplisit. Alasannya dua — grammar referensi memakai
kartu dan baris, bukan garis rambut bertick, dan tiga garis itu sendirian
menyumbang 3 merah. Kalau kamu masih mau angka ambang batasnya muncul,
tempat termurah menaruhnya kembali adalah **pill inset** di kartu
(`40 soal / paket` sebagai tag kanan) — nol garis baru, nol merah baru.

---

## 7. YANG DIHAPUS DARI v3.2

| Dihapus | Alasan |
|---|---|
| CTA pill kecil di kanan-bawah kartu | diganti CTA selebar kartu |
| daftar `<ul>` dua baris bergaris di kartu | diganti meta ikon + pill inset |
| ledger kategori bergaris rambut + badge angka | diganti baris kartu berkotak ikon |
| nomor superscript `.sup` di kategori | nomor pindah ke kanan-atas kartu produk saja |
| garis ambang batas bertick merah (3 tempat) | lihat §6 |
| `<details>` native + `.pm::after` | diganti akordeon `<button>` + state di logic (lihat §8) |
| chip filter berborder | chip terisi tanpa border, aktif = blok navy |
| border-bottom pada header | header menyatu dengan kertas, hero yang jadi batas |
| `letter-spacing` pada nav | nav jadi pill hover, tidak lagi label huruf besar |

---

## 8. AKORDEON FAQ — SEKARANG STATE, BUKAN `<details>`

`<details open>` dipakai sampai v3.2. Masalahnya: di halaman yang re-render
(chip filter diklik), atribut `open` yang dikendalikan React bisa menutup
kembali panel yang baru dibuka pengunjung. v3.3 memakai satu state:

```js
state = { open: 1 };                 // 1 = pertanyaan pertama terbuka saat muat
// tombol: setState(s => ({ open: s.open === i ? 0 : i }))
```

Satu panel terbuka pada satu waktu. Penanda terbuka: border `--navy` +
tanda `−`; tertutup: border `--line` + `+`. `aria-expanded` ikut state.

Untuk implementasi statis (tanpa framework) `<details>` **tetap boleh** —
di HTML biasa tidak ada re-render, jadi bug ini tidak muncul. Yang wajib
sama di kedua implementasi: tepat satu panel terbuka saat halaman dimuat,
dan penandanya border + `+`/`−`, bukan garis merah.

---

## 9. ANGGARAN UKURAN

| Bagian | Perkiraan |
|---|---|
| baseline v3.2 | ±25.5 KB |
| ikon SVG inline (±22 tempat, 6 bentuk berulang) | +1.6 KB |
| CSS `.inset`, `.cta`, `.row`, `.meta` | +0.7 KB |
| `<ul>` kartu + ledger + garis ambang yang dicabut | −1.0 KB |
| **perkiraan v3.3** | **±26.8 KB** |

Batas 30 KB masih aman. Kalau nanti mepet: ikon yang berulang (`file-text`,
`download`, `arrow-up-right`) dipindah ke satu `<svg>` `<symbol>` di awal
`<body>` lalu dipakai lewat `<use>` — hemat ±1.1 KB, nol permintaan baru.

---

## 10. KRITERIA TERIMA TAMBAHAN

- [ ] `lynk.id` maksimal 3 sebutan; `checkout di lynk` nol hasil
- [ ] setiap tombol beli berbunyi tepat `Checkout` + ikon `↗`
- [ ] label `.cta` optically centered: `padding-right` = lebar ikon
- [ ] nol teks kalimat yang dimulai huruf kecil di luar nama merah merek
      (`overtimeboo`, `lynk.id`)
- [ ] akronim kapital di seluruh berkas: `PDF PCPM TPU BI FAQ GAT BUMN CPNS SKD`
- [ ] huruf besar label hanya lewat `text-transform`, nol teks sumber
      yang diketik kapital
- [ ] merah ≤3 per layar (hitung ulang §6 setelah setiap penambahan)
- [ ] tepat satu panel FAQ terbuka saat muat; klik chip filter tidak
      menutupnya
- [ ] kartu produk rata dasarnya di grid 3 kolom, 2 kolom, dan 1 kolom
      (`margin-top:auto` pada blok pill+CTA)
- [ ] area klik CTA ≥44px tinggi di semua kartu
- [ ] setiap ikon menandai satu fakta; nol ikon dekoratif
- [ ] `radius` hanya empat nilai: 26 / 22 / 16–14 / 999
