# HANDOFF — WEB ETALASE OVERTIMEBOO v3: ROMBAK TEMA NAVY

Tanggal: 13 Agustus 2026
Status: **DRAFT — belum dieksekusi**
Menggantikan: lapisan tampilan dari `handoff-web-etalase-overtimeboo-v2.md`
URL target: `https://overtime-boo.gulliveragent.workers.dev/`

Dokumen ini **hanya mengganti lapisan tampilan**. Semua fakta operasional,
alur deploy, cara nambah listing, dan masalah terbuka non-visual di v2
**masih berlaku persis**. Kalau v3 tidak menyebut sesuatu, ikuti v2.

Target visual ada di berkas pendamping: `mockup-tema-navy-v3.html`.
Buka berkas itu di browser sebelum menulis kode. Angka, jarak, dan warna
di sana adalah spesifikasi, bukan ilustrasi.

---

## 0. RUANG LINGKUP — BACA DULU

**Yang dikerjakan:** token warna, tipografi, bentuk komponen, jarak,
`og.png`, `favicon.png`.

**Yang TIDAK BOLEH disentuh:**

1. Array `PRODUK` dan array `KATEGORI` beserta isinya.
2. Logika chip filter, status bar, dan badge jumlah di kartu kategori.
3. Struktur `<details>/<summary>` untuk FAQ (aksesibilitas native, jangan
   diganti accordion JS).
4. Tiga URL hardcode: `canonical`, `og:url`, `og:image`.
5. Alur deploy Workers dan riwayat versi.

Ini rombak lapisan tampilan. Kalau lapisan data ikut diaduk, risiko
regresinya tidak sebanding dengan hasilnya. Kalau ada bagian tampilan yang
kelihatannya butuh ubah data, **berhenti dan tanya**, jangan diakali.

**Anggaran ukuran:** `index.html` sekarang 22.5 KB. Batas atas v3 = **30 KB**.
Kalau tembus, buang dekorasi, bukan buang aksesibilitas.

---

## 1. KENAPA DIROMBAK

Riso Pop (kuning asam `#EDF23C` + indigo `#4747D1`, sudut tebal, bayangan
offset keras, Space Mono) itu bahasa visual Y2K yang main-main. Pembelinya
orang yang lagi ngelamar bank sentral dan instansi negara, bayar untuk
merasa siap. Tampilan main-main memotong sinyal kredibilitas tepat di
detik orang mutusin mau bayar atau nggak.

Arah baru: **institusional, tenang, presisi.** Bukan mewah, bukan korporat
dingin. Kesan yang dikejar: dokumen resmi yang enak dibaca.

**Bonus yang ikut kebeli:** masalah terbuka #1 di v2 (tombol beli
`#C45808` + putih = 4.29:1, gagal AA) hilang total. Tombol navy + putih =
14.1:1.

---

## 2. YANG DIAMBIL DARI REFERENSI, DAN YANG TIDAK

Enam gambar referensi yang dikasih Deus mayoritas berpalet hangat
(krem + oranye/terakota). **Warnanya jangan diambil.** Yang diambil bahasa
tata letaknya:

| Diambil | Dari referensi |
|---|---|
| Tipografi display besar huruf kecil | Lautsprecher (pure audio) |
| Blok gelap penuh sebagai pembuka | Femmefit |
| Kartu rounded bergaris rambut, tanpa bayangan | Femmefit, Alex Romanov |
| Daftar bernomor dengan garis tipis antarbaris | Q Industrial, Zanaveska |
| FAQ akordion berjudul serif | Zanaveska |
| Tombol pill + panah | Femmefit, Alex Romanov |
| Strip marquee tipis | Femmefit, Lautsprecher |

**Yang sengaja tidak diambil:** dasar krem hangat, aksen terakota, foto
gaya hidup, serif kontras tinggi bergaya fesyen. Krem + serif kontras
tinggi + terakota itu tampilan default yang muncul di mana-mana sekarang;
untuk produk yang mau kebaca institusional, dia malah terbaca artisanal.

---

## 3. TOKEN WARNA

Ganti seluruh blok `:root` yang lama. Jangan sisakan token Riso Pop yang
tidak terpakai — kalau ketinggalan, sesi berikutnya bakal mikir itu masih
aktif.

```css
:root{
  /* ground */
  --navy:      #0E2A55;   /* blok hero, footer, tombol, title bar */
  --navy-deep: #071A36;   /* lapisan paling dalam, garis di atas navy */
  --ink:       #0A1D3A;   /* teks utama di atas kertas */

  /* paper */
  --paper:     #ECF0F6;   /* dasar halaman */
  --card:      #FFFFFF;   /* permukaan kartu */
  --line:      #D3DCE8;   /* garis rambut di atas kertas */
  --line-dark: #23406E;   /* garis rambut di atas navy */

  /* text */
  --muted:     #56637A;   /* teks sekunder di atas kertas */
  --on-navy:   #FFFFFF;
  --on-navy-2: #B9C6DC;   /* teks sekunder di atas navy */

  /* accent */
  --accent:    #C0392B;   /* merah rumah, dari PDF produk */
}
```

**Rasio kontras — sudah dihitung, jangan diubah tanpa hitung ulang:**

| Pasangan | Rasio | Status |
|---|---|---|
| `--ink` di `--paper` | 14.7:1 | lolos AAA |
| putih di `--navy` | 14.1:1 | lolos AAA |
| `--on-navy-2` di `--navy` | 8.1:1 | lolos AAA |
| `--muted` di `--paper` | 5.2:1 | lolos AA |
| `--accent` di `--paper` | 4.8:1 | lolos AA |
| **`--accent` di `--navy`** | **2.6:1** | **GAGAL** |

**Aturan keras:** `--accent` tidak boleh jadi teks di atas navy. Di atas
navy dia cuma boleh jadi bentuk — tick, garis, titik, kotak kecil.

**Tombol beli = `--navy` + teks putih.** Bukan merah. Merah dipakai hemat:
harga, tick garis ambang batas, badge "gratis". Kalau merah muncul lebih
dari tiga tempat dalam satu layar, ada yang salah.

`--accent` sengaja `#C0392B` karena itu merah rumah yang sudah dipakai di
seluruh PDF produk. Pembeli lihat cover PDF navy-merah, lalu situs
navy-merah. Konsistensi itu lebih berharga daripada warna baru yang lebih
cantik.

---

## 4. TIPOGRAFI

Space Mono **dicabut sepenuhnya**. Monospace retro itu lawan langsung dari
"elegan".

```
Display : Newsreader  — 400, 500 — serif editorial-cetak
UI/Body : Archivo     — 400, 500, 600 — grotesk, angka tabular bagus
```

Dua keluarga, cukup. Jangan tambah yang ketiga.

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Archivo:wght@400;500;600&family=Newsreader:opsz,wght@6..72,400;6..72,500&display=swap" rel="stylesheet">
```

**Skala tipe:**

| Peran | Font | Ukuran | Catatan |
|---|---|---|---|
| Hero | Newsreader 400 | `clamp(2.4rem, 7vw, 4.6rem)` | huruf kecil, `line-height:1.05`, `letter-spacing:-.02em` |
| Judul section | Newsreader 500 | `clamp(1.5rem,3.6vw,2.1rem)` | huruf kecil |
| Judul kartu | Archivo 600 | `1.02rem` | `line-height:1.35` |
| Body | Archivo 400 | `1.0625rem` | `line-height:1.65` |
| Meta / label | Archivo 500 | `.78rem` | `letter-spacing:.09em`, HURUF BESAR |
| Harga & angka | Archivo 600 | `1.12rem` | **wajib** `font-variant-numeric: tabular-nums` |

Halaman ini isinya harga dan jumlah soal terus-terusan. Angka tabular
bikin kolom harga di kartu sejajar; tanpa itu kelihatan goyang.

Aturan brand voice v2 **masih berlaku**: huruf kecil semua kecuali akronim,
nol em dash di copy sendiri (judul produk dari Lynk tetap boleh, itu data),
maksimal satu emoji.

---

## 5. ELEMEN TANDA TANGAN — GARIS AMBANG BATAS

Ini satu-satunya hal yang harus dieksekusi presisi. Semua yang lain boleh
tenang.

Pemisah antar section **bukan** garis dekoratif. Dia meniru garis nilai
ambang batas di grafik skor: garis tipis penuh lebar, dengan tick pendek
dan label kecil di ujung kiri.

```
──────────────────────────────────────────────
│ 65        ← tick merah 2px, tinggi 10px
```

```css
.batas{ position:relative; height:1px; background:var(--line);
        margin:clamp(52px,8vw,84px) 0; }
.batas::before{ content:""; position:absolute; left:0; top:-5px;
        width:2px; height:11px; background:var(--accent); }
.batas__label{ position:absolute; left:10px; top:-9px;
        font:500 .68rem/1 Archivo, sans-serif; letter-spacing:.12em;
        color:var(--muted); background:var(--paper); padding-right:8px; }
```

**Labelnya harus angka yang benar-benar berarti**, bukan hiasan. Contoh
yang sah: `65` `80` `166` (passing grade SKD), `40` (soal per paket PCPM),
`50` (soal per paket psikotes). Contoh yang dilarang: `01` `02` `03`.
Kalau di satu tempat tidak ada angka yang jujur, pakai garis polos tanpa
label. Jangan dikarang.

Di atas navy, garis ini pakai `--line-dark` dan ticknya tetap `--accent`
(boleh, karena bentuk bukan teks).

---

## 6. PETA KOMPONEN — LAMA KE BARU

| Komponen | v2 sekarang | v3 |
|---|---|---|
| Title bar window | garis kuning + kotak kontrol palsu | dicabut. Ganti jadi label section huruf besar `.78rem` + garis rambut di bawahnya |
| Kartu produk | window retro, bayangan offset keras, sudut siku | `--card` putih, `border:1px solid var(--line)`, `border-radius:10px`, tanpa bayangan. Hover: border jadi `--navy`, `transform:translateY(-2px)`, transisi 140ms |
| Tombol beli | pill oranye `#C45808` | pill `--navy`, teks putih, `border-radius:999px`, panah `↗` inline. Hover: `--navy-deep` |
| Tombol gratis | sama | pill garis: `background:transparent`, `border:1.5px solid var(--navy)`, teks `--navy` |
| Chip filter | kotak siku | pill `border-radius:999px`, `border:1px solid var(--line)`. Aktif: `--navy` isi penuh + teks putih |
| Hero | latar kertas | **blok `--navy` penuh lebar**, sudut bawah `border-radius:0 0 18px 18px`, teks putih |
| Strip marquee | tidak ada | strip tipis `--navy-deep` di bawah topbar, tinggi 34px, teks `--on-navy-2` `.74rem`. Isi = jadwal PCPM 41 |
| FAQ terbuka | header mustard | header `--card`, garis kiri 3px `--accent`, judul Newsreader |
| Kartu kategori | kotak siku | sama seperti kartu produk. Yang `segera` opasitas .55 + badge "segera" |
| Pemisah section | tidak ada | garis ambang batas (section 5) |
| Footer | ringkas | blok `--navy`, disclaimer wajib lengkap, tetap terbaca |

**Sudut membulat, sekali saja diputuskan:** kartu & blok `10px`, hero
bawah `18px`, tombol & chip `999px`. Tidak ada nilai lain di seluruh
berkas.

---

## 7. STRIP MARQUEE — DAN JEBAKANNYA

Strip ini berguna karena isinya informasi nyata, bukan hiasan:

```
TPD ~1 SEP · TES TEKNIS 2–20 SEP · PCPM BI ANGKATAN 41
```

**Jebakan:** ini bakal basi lewat September, persis seperti tulisan
"15 PRODUK" yang kepanggang di `og.png` (masalah terbuka #4 di v2).
Jangan ulangi kesalahan yang sama dua kali dalam satu situs.

Wajib:
- taruh isinya di **satu konstanta JS** di paling atas `<script>`, dengan
  komentar `// GANTI/KOSONGKAN SETELAH SEPTEMBER 2026`
- kalau konstantanya string kosong, strip **tidak dirender sama sekali**,
  bukan dirender kosong
- jangan pakai animasi jalan. Teks statis di tengah. Animasi bikin orang
  nunggu kalimat lewat, dan `prefers-reduced-motion` jadi urusan tambahan

---

## 8. og.png DAN favicon.png

Regenerate dua-duanya dengan palet baru.

**`og.png` (1200x630):**
- latar `--navy`, teks putih
- **buang tulisan "15 PRODUK"**. Ini akar masalah terbuka #4 di v2 — angka
  kepanggang di gambar yang di-cache scraper. Ganti jadi teks yang tidak
  pernah basi: `bank soal · kunci · pembahasan`
- satu garis ambang batas tipis merah sebagai penanda merek
- tanpa emoji (tidak ada font emoji warna di lingkungan generate — ini
  sudah kejadian di v2)

**`favicon.png` (512x512):** kotak `--navy` sudut membulat, tick merah,
huruf `o` Newsreader putih.

**Font di aset gambar:** Newsreader dan Archivo mungkin tidak tersedia di
lingkungan generate. Kalau tidak ada, pakai DejaVu Serif / DejaVu Sans
(preseden v2) dan **catat di handoff berikutnya** bahwa huruf aset gambar
tidak identik dengan halaman. Jangan diam-diam.

---

## 9. KRITERIA TERIMA

Belum selesai kalau ada satu pun yang belum kelar.

- [ ] Nol token Riso Pop tersisa di berkas (cari `EDF23C`, `4747D1`,
      `C45808`, `Space Mono` — harus nol hasil)
- [ ] Semua pasangan warna sesuai tabel section 3. Hitung ulang, jangan
      percaya dokumen ini
- [ ] `--accent` tidak dipakai sebagai teks di atas navy di mana pun
- [ ] Harga dan jumlah soal pakai `tabular-nums`
- [ ] Chip filter, status bar, dan badge kategori masih jalan persis
      seperti sebelum rombak. **Tes dengan cara nambah 2 listing dummy ke
      array, cek angkanya ikut naik, lalu hapus lagi**
- [ ] 15 link produk masih benar semua, tidak ada yang ketuker
- [ ] `overflow-wrap` masih terpasang di `body`, `.card__judul`, `.badge`
      (kata "kebanksentralan" bikin scroll horizontal di layar 320px —
      ini sudah pernah kejadian, jangan sampai balik lagi)
- [ ] Fokus keyboard kelihatan di semua elemen interaktif. Pakai
      `:focus-visible` dengan outline 2px `--accent` + offset 2px
- [ ] `prefers-reduced-motion: reduce` mematikan semua transisi
- [ ] `index.html` di bawah 30 KB
- [ ] Tiga tes yang belum pernah jalan di v2 section 8, kerjakan sekarang:
      font size HP terbesar, tab keyboard sampai habis, zoom browser 200%

**Catatan lingkungan:** Claude tidak bisa membuka halaman di browser di
sandbox. Verifikasi struktural dilakukan programatik (parsing HTML, hitung
kontras, eval JS pakai DOM stub). Verifikasi visual harus dari screenshot
Deus. Jangan mengklaim "sudah dicek visual" kalau yang dilakukan cuma
parsing.

---

## 10. YANG TIDAK BOLEH DIKERJAKAN DI SESI INI

- Regenerate aset Instagram. `ig_feeds_riso_pop.py` masih Riso Pop dan
  **nasibnya belum diputuskan Deus**. Web dulu, IG belakangan.
- Ganti copy di luar yang disebut section 11. Copy v2 hasil feedback
  langsung Deus.
- Ganti struktur data, tambah kategori, tambah listing.
- Pasang custom domain.
- Bikin halaman kedua, search bar, atau email capture. Masih backlog v2.

---

## 11. KEPUTUSAN TERBUKA — MILIK DEUS, BUKAN COWORK

Jangan dieksekusi tanpa jawaban. Kalau tidak ada jawaban, pakai kolom
"default aman" dan tulis di handoff berikutnya bahwa itu default.

| # | Pertanyaan | Default aman |
|---|---|---|
| 1 | Emoji 👌 di hero masih dipakai? Di tema institusional dia jadi satu-satunya elemen yang main-main | **Dibuang.** Brand voice kasual dijaga lewat kata-katanya, bukan emoji |
| 2 | Hero sub "langsung baca" muncul dua kali (masalah terbuka #2 v2) | Rapikan jadi `file PDF, download langsung baca. anywhere, anytime.` |
| 3 | Tombol "checkout!" menghilangkan sinyal pindah domain (masalah terbuka #3 v2) | Ubah jadi `checkout di lynk ↗`. Rombak tema = momen paling murah buat ini |
| 4 | Riso Pop dipensiunin, atau jadi lapisan sosial terpisah? | Tidak diputuskan di sini. Web navy, IG dibiarkan |
| 5 | Judul "Bank Soal TOEFL ITP" di Lynk | Di luar lingkup web. Tabrakan merek dagang ETS, keputusan Deus |
| 6 | Handle `@bukanordalpcpmbi` spesifik PCPM, sementara payungnya overtimeboo | Dibiarkan. Belum blocker |

---

## 12. RISIKO YANG SUDAH DIKETAHUI

**Kemiripan visual dengan Bank Indonesia.** Biru tua tidak bisa diklaim
siapa pun, jadi warnanya sendiri aman. Yang bikin risiko itu sinyal yang
menumpuk: navy institusional + tipografi resmi + lambang/segel + produk
bernama PCPM. Gabungan itu bisa bikin kandidat mengira ini kanal resmi.

Karena itu, keras:
- **nol** segel, perisai, garuda, koin, atau motif mirip lambang instansi
- **nol** logo instansi mana pun
- disclaimer non-afiliasi tetap terbaca di footer, jangan dikecilkan demi
  estetika
- persona Overtimeboo tetap nongol di suatu tempat. Maskot itu pembeda
  paling kuat antara "toko independen" dan "kanal resmi"

**Regresi filter.** Rombak tampilan yang menyentuh nama kelas berisiko
memutus JS yang menargetkan kelas itu. Sebelum mulai, catat semua selektor
yang dipakai JS. Kalau nama kelas diganti, ganti juga di JS-nya, lalu tes
ulang filternya.

---

## 13. LANGKAH BERIKUTNYA SETELAH TEMA JADI

Urutannya penting.

1. **Pasang URL di bio Threads, IG, X sekarang juga — jangan tunggu tema
   selesai.** URL-nya tidak berubah, tema baru cuma nimpa. Menunda link
   bio sampai tema kelar berarti membuang minggu terakhir sebelum window
   September. Ini yang bikin metrik Cloudflare sekarang nyaris nol.
2. Perbaiki bio Lynk yang masih bahasa Inggris.
3. Konten kampanye PCPM window September tetap **deep-link langsung ke
   listing Lynk**, bukan lewat halaman ini. Nambah hop pas traffic puncak
   = kebocoran konversi. Aturan ini dari v2, masih berlaku.
4. Ganti `status:"segera"` jadi `status:"siap"` di array `KATEGORI` untuk
   CPNS SKD dan GAT BUMN kalau produknya sudah dilisting.
