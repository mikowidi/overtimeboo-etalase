# SPEC v3.2 — REVISI TIPOGRAFI & GRAMMAR LAYOUT

Tanggal: 13 Agustus 2026
Menemani: `spec-etalase-navy-v3.1.md` (masih berlaku) + handoff v3
Mockup: `Etalase Navy v3.2.dc.html` (v3.1 disimpan sebagai pembanding)

Dokumen ini **hanya mengganti dua lapisan** dari v3.1: **tipografi** dan
**grammar komponen**. Yang tidak disebut di sini, ikuti v3.1.

Yang **tidak berubah sama sekali**: 10 token warna §1 v3.1, tabel kontras,
aturan `--accent` tidak boleh jadi teks di atas navy, anggaran merah per
layar, garis ambang batas beserta ukuran ticknya, `<details>` native,
struktur array, hook JS, tiga URL hardcode.

---

## 0. KENAPA REVISI

Dua masalah, dua sebab:

**"Fontnya jadul."** Newsreader itu serif editorial-cetak. Di halaman
komersial 2026 dia terbaca seperti koran, bukan seperti produk. Referensi
yang dikasih **nol serif** — Qorry, Q Industrial, dan Alex Romanov semuanya
grotesk lebar berbobot tebal. Newsreader **dicabut**.

**"Layoutnya belum sama vibes-nya."** v3.1 sudah benar strukturnya (grid
kelihatan, angka besar, garis rambut) tapi masih memakai bahasa komponen
generik: tombol pill teks polos, kartu radius kecil, judul serif. Referensi
punya lima ciri yang sangat spesifik dan tidak satu pun ada di v3.1:

| Ciri referensi | Dari |
|---|---|
| CTA = pill dengan **badge lingkaran panah** di ujung kanan | Femmefit, Alex Romanov |
| Satu frasa di dalam headline dibungkus **pill outline** | Femmefit, Alex Romanov |
| **Nomor superscript kecil** (`00 01 02`) sebelum judul baris | Q Industrial, Qorry |
| Blok besar **radius gemuk**, bukan sudut hampir siku | Alex Romanov, Qorry |
| Display **grotesk lebar berat**, huruf kecil, tracking negatif | Qorry, Lautsprecher |

v3.2 memasang kelimanya.

---

## 1. TIPOGRAFI — SATU KELUARGA, AXIS LEBAR

Newsreader dicabut. Archivo dipakai untuk **semuanya**, dengan axis lebar
(`wdth`) sebagai pembeda display vs UI. Satu keluarga, dua peran — lebih
ketat dari aturan "dua keluarga" v3, bukan lebih longgar.

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Archivo:wdth,wght@62..125,100..900&display=swap" rel="stylesheet">
```

Satu request variable font menggantikan dua request statis v3 — lebih ringan,
bukan lebih berat. `font-stretch` di bawah memetakan ke axis `wdth`; kalau
variable font gagal dimuat, semuanya jatuh ke Archivo lebar normal, tidak ke
serif sistem.

```css
:root{
  --font: Archivo, system-ui, sans-serif;
  /* peran display: berat + lebar + tracking negatif */
  --disp-wd: 115%;   /* hero, poster, angka rail, wordmark footer */
  --sub-wd:  110%;   /* judul section, judul ledger */
  --card-wd: 108%;   /* judul kartu, judul FAQ, langkah */
}
```

| Peran | Deklarasi |
|---|---|
| Hero | `800 clamp(2.5rem,6.6vw,5.2rem)/.96`, `font-stretch:115%`, `letter-spacing:-.035em` |
| Poster (judul blok navy) | `800 clamp(2rem,5.2vw,3.6rem)/1.02`, `115%`, `-.035em` |
| Wordmark footer | `800 clamp(2.2rem,7vw,4.6rem)/.94`, `115%`, `-.04em` |
| Angka rail hero | `800 clamp(2rem,5vw,3.6rem)/1`, `115%`, `-.04em`, tabular |
| Judul section | `700 clamp(1.7rem,3.6vw,2.5rem)/1.06`, `112%`, `-.03em` |
| Judul ledger kategori | `700 clamp(1.4rem,3vw,2.2rem)/1.1`, `110%`, `-.03em` |
| Langkah cara pakai | `600 clamp(1.15rem,2.4vw,1.75rem)/1.2`, `108%`, `-.02em` |
| Judul kartu / FAQ | `700 1.18rem/1.24`, `108%`, `-.02em` |
| Harga | `800 1.3rem/1`, `108%`, `-.03em`, **tabular** |
| Body | `400 1.0625rem/1.6`, lebar normal |
| Label / meta | `600 .72rem/1`, `letter-spacing:.14em`, HURUF BESAR |
| Nomor superscript | `600 .78rem/1`, `letter-spacing:.1em`, tabular |

Aturan tracking: **semakin besar, semakin negatif.** Display di bawah
`-.03em` terlihat lepas; body jangan pernah diberi tracking negatif.

Label naik dari `.09em` ke `.14em`–`.16em`. Itu satu-satunya tempat tracking
positif, dan yang bikin label kecil terbaca sengaja, bukan kekecilan.

---

## 2. RADIUS — DIPETAKAN ULANG, NOL NILAI BARU

Tiga nilai v3 tetap tiga nilai. Yang berubah cuma pembagiannya:

| Elemen | v3.1 | v3.2 |
|---|---|---|
| hero, blok besar, footer | 18px | **18px** (tetap, tapi hero sekarang membulat 4 sudut, bukan 2) |
| kartu produk, kartu kategori, FAQ, empty state | 10px | **18px** |
| badge lingkaran, monogram, pill, chip | 999px | **999px** |
| ~~10px~~ | dipakai | **tidak dipakai lagi** |

Sudut gemuk yang seragam itu yang bikin referensi terasa "sekarang". Radius
10px pada kartu 300px terbaca ragu-ragu — bukan siku, bukan bulat.

Hero jadi blok inset (`margin: 24px`, radius 18 di empat sudut) bukan
full-bleed. Halaman jadi terbaca seperti lembar yang diletakkan di atas
meja — persis Alex Romanov dan Qorry.

---

## 3. KOMPONEN BARU

### 3.1 CTA pill + badge lingkaran panah

Ini komponen paling menentukan. Label kiri, lingkaran di ujung kanan.
Padding kanan sengaja kecil (`8px`) supaya lingkaran menempel di tepi pill.

```css
.btn{ display:inline-flex; align-items:center; gap:12px;
  border-radius:var(--r-pill); padding:7px 8px 7px 20px;
  font:600 .82rem/1 var(--font); white-space:nowrap;
  background:var(--navy); color:var(--on-navy);
  border:1.5px solid var(--navy); transition:background .14s; }
.btn:hover{ background:var(--navy-deep); border-color:var(--navy-deep); }
.btn__ico{ width:30px; height:30px; border-radius:var(--r-pill);
  background:var(--card); color:var(--navy); display:grid;
  place-items:center; font:400 .84rem/1 var(--font); }

/* di atas navy: pill putih, lingkaran navy */
.btn--invert{ background:var(--card); color:var(--navy); border-color:var(--card); }
.btn--invert:hover{ background:var(--paper); border-color:var(--paper); }
.btn--invert .btn__ico{ background:var(--navy); color:var(--on-navy); }

/* gratis: pill outline, lingkaran tetap terisi */
.btn--ghost{ background:transparent; color:var(--navy);
  border:1.5px solid var(--navy); padding:5px 6px 5px 16px; }
.btn--ghost:hover{ background:var(--navy); color:var(--on-navy); }

/* di dalam kartu: satu ukuran lebih kecil */
.btn--sm{ padding:6px 7px 6px 16px; font-size:.8rem; }
.btn--sm .btn__ico{ width:26px; height:26px; font-size:.78rem; }
```

```html
<a class="btn btn--sm" href="[URL LYNK]">checkout di lynk<span class="btn__ico">&#8599;</span></a>
```

Panah `↗` tetap di semua tombol yang pindah domain — sinyal §11 #3 tidak
hilang, malah jadi lebih kelihatan karena sekarang punya lingkaran sendiri.

### 3.2 Pill outline di dalam headline

Satu frasa saja per halaman. Merah di sini adalah **border**, bukan teks —
tulisannya tetap putih 14.2:1, jadi aturan keras §3 v3 aman.

```css
.hl{ display:inline-block; border:2px solid var(--accent);
  border-radius:var(--r-pill); padding:0 .3em .03em; line-height:.92; }
/* dan h1 wajib line-height >= 1.06, lihat catatan di bawah */
```

```html
<h1>bank soal yang kunci sama pembahasannya <span class="hl">lengkap.</span></h1>
```

**Jebakan yang wajib dihitung, bukan ditebak:** kotak pill = `line-height`
+ padding + 2×2px border. Kalau jumlahnya lebih tinggi dari line box `h1`,
pill menggantung dan border merahnya memotong descender baris di atasnya.
Karena itu `h1` di v3.2 pakai `line-height:1.06`, bukan `.96` seperti draf
pertama, dan pill-nya `.92`. Rumusnya: `(.92em + .03em) + 4px < 1.06em`.
Kalau kamu menaikkan bobot atau ukuran hero, hitung ulang pertidaksamaan ini.

Pakai `display:inline-block` dan pilih frasa **satu kata** (atau dua kata
tanpa spasi pemisah baris). Frasa panjang akan terpotong di ganti baris dan
pill-nya pecah jadi dua.

### 3.3 Badge lingkaran (hairline)

Dipakai empat tempat: pojok hero (`↓`), ujung tiap baris cara pakai (`↗`),
hitungan produk di ledger kategori, dan tanda buka/tutup FAQ.

```css
.circ{ flex:none; width:34px; height:34px; border-radius:var(--r-pill);
  border:1px solid var(--line); color:var(--muted);
  display:grid; place-items:center; font:400 .82rem/1 var(--font); }
.circ--navy{ border-color:var(--line-dark); color:var(--on-navy-2); }
.circ--num{ font:700 .82rem/1 var(--font); font-variant-numeric:tabular-nums;
  color:var(--ink); }
```

FAQ memakai `.circ` dengan `+`/`−` dari selektor state — bukan JS:

```css
details.faq .pm::after{ content:"+"; }
details.faq[open] .pm::after{ content:"\2212"; }
details.faq{ border-color:var(--line); }
details.faq[open]{ border-color:var(--navy); }
```

Perhatikan: penanda FAQ terbuka **berpindah** dari garis kiri merah 3px ke
border navy penuh + tanda `−`. Merah di layar FAQ jadi nol, dan itu
memberi ruang satu merah lagi di layar lain kalau nanti dibutuhkan.

### 3.4 Nomor superscript

Ganti nomor besar di kolom kiri (v3.1) jadi nomor kecil di depan judul.
Angka besar pindah ke badge lingkaran di kanan, jadi baris punya dua jangkar
angka, bukan satu.

```css
.sup{ font:600 .78rem/1 var(--font); letter-spacing:.1em;
  font-variant-numeric:tabular-nums; color:var(--muted); }
.sup--live{ color:var(--accent); }   /* baris pertama / aktif. di atas kertas: 4.8:1 */
```

Baris kategori jadi: `.sup` 56px → judul → badge angka. Baris cara pakai
jadi: `.sup` 64px → langkah → `.circ--navy`.

`.sup--live` cuma boleh di atas kertas. Di atas navy, nomor pertama pakai
`--on-navy-2` dan penandanya tick merah 2×11px di garis atas baris.

### 3.5 Topbar terang

Topbar navy dicabut. Sekarang: dasar kertas, garis rambut bawah, monogram
lingkaran navy dengan titik merah, nav huruf besar kecil, dan satu CTA pill.

```css
.top{ background:var(--paper); border-bottom:1px solid var(--line); }
.mono{ position:relative; width:28px; height:28px; border-radius:var(--r-pill);
  background:var(--navy); color:var(--on-navy); display:grid; place-items:center;
  font:700 .95rem/1 var(--font); font-stretch:115%; }
.mono::after{ content:""; position:absolute; right:-1px; bottom:3px;
  width:9px; height:9px; border-radius:var(--r-pill); background:var(--accent); }
```

Alasannya bukan selera: kalau topbar, strip, dan hero semuanya biru gelap,
hero kehilangan pukulannya. Referensi selalu menaruh header terang di atas
blok warna. Strip jadwal juga naik ke **paling atas**, di atas header —
persis Femmefit.

Monogram lingkaran + titik merah ini juga jadi spesifikasi `favicon.png`
yang baru: kotak navy sudut membulat, huruf `o` Archivo 800 putih, titik
merah kanan bawah. Ganti spesifikasi `favicon.png` §8 handoff v3 dengan ini
(huruf `o` Newsreader tidak ada lagi).

### 3.6 Wordmark footer sebagai grafik

Tanpa fotografi, footer adalah tempat paling murah untuk satu elemen besar.
Wordmark diset `clamp(2.2rem,7vw,4.6rem)` — jadi penutup halaman, bukan
catatan kaki.

Titik setelah wordmark **bukan karakter `.` berwarna merah** (itu teks merah
di atas navy, 2.6:1, dilarang). Titiknya elemen kotak bulat:

```css
.wm-dot{ display:inline-block; width:.16em; height:.16em;
  border-radius:var(--r-pill); background:var(--accent);
  vertical-align:baseline; }
```

Disclaimer tetap `.92rem`, `--on-navy-2` di `--navy` = 8.2:1. Tidak dikecilkan.

---

## 4. YANG DIHAPUS DARI v3.1

| Dihapus | Alasan |
|---|---|
| Newsreader (semua peran) | serif = sumber kesan jadul |
| `--r-card: 10px` | radius kartu naik ke 18px |
| topbar navy | header terang bikin hero lebih keras |
| angka besar di kolom kiri ledger | diganti nomor superscript + badge angka di kanan |
| garis kiri merah 3px pada FAQ terbuka | diganti border navy + tanda `−` |
| tombol pill teks polos | diganti pill + badge lingkaran panah |

`--wrap` naik `1080px` → `1200px`, padding `22px` → `24px`. Referensi semuanya
lebih lebar dan lebih padat; 1080 memaksa grid kartu jadi 3 kolom sempit.

---

## 5. ANGGARAN MERAH — DIHITUNG ULANG UNTUK v3.2

| Layar | Merah | Jumlah |
|---|---|---|
| hero | titik monogram + border pill headline + tick rail | 3 |
| katalog | tick garis ambang + `00` superscript + badge `gratis` + harga `gratis` | 4 &#8594; **buang `00` merah di eyebrow kalau layar ini dinilai penuh** |
| cara pakai | tick di garis baris 01 | 1 |

Baris cara pakai tidak punya badge panah: dia bukan link. Badge lingkaran
dipakai hanya di `.btn`, pill lynk.id, dan baris kategori yang bisa diklik.
| kategori | tick garis ambang + `00` superscript baris pertama | 2 |
| faq | nol | 0 |
| footer | tick garis + titik wordmark | 2 |

Satu keputusan buat kamu: di layar katalog merah jadi 4. Aturan v3 bilang
maksimal 3. Yang paling gampang dibuang: warna merah pada `00` di eyebrow
katalog (jadikan `--muted`). Aku biarkan merah di mockup supaya kamu bisa
lihat dulu efeknya sebelum memutuskan.

---

## 6. ANGGARAN UKURAN

| Bagian | Perkiraan |
|---|---|
| baseline v3.1 | ±24.3 KB |
| dua request font statis &#8594; satu variable | −0.2 KB |
| CSS `.btn__ico`, `.circ`, `.hl`, `.sup`, `.mono` | +0.9 KB |
| markup badge lingkaran (±20 tempat) | +0.8 KB |
| CSS Newsreader yang dicabut | −0.3 KB |
| **perkiraan v3.2** | **±25.5 KB** |

Batas 30 KB masih aman.

---

## 7. KRITERIA TERIMA TAMBAHAN

- [ ] `Newsreader` nol hasil di seluruh berkas — termasuk di `og.png` dan
      `favicon.png` yang harus diregenerate dengan Archivo 800
- [ ] `10px` nol hasil sebagai nilai `border-radius`
- [ ] `font-stretch` hanya muncul pada peran display di tabel §1, tidak pada
      body atau meta
- [ ] pill outline headline tidak pecah di 320px, 375px, dan pada font size
      HP terbesar
- [ ] setiap `.btn` punya tepat satu `.btn__ico`; nol tombol tanpa badge
- [ ] tanda `+`/`−` FAQ datang dari `details[open]`, nol JS
- [ ] titik wordmark footer bukan karakter berwarna (cek: nol `color:#C0392B`
      di dalam blok `footer`)
- [ ] chip filter `white-space:nowrap` — tanpa itu label pecah di tengah kata
      pada kolom sempit
- [ ] tab keyboard: badge lingkaran di dalam `.btn` tidak dapat fokus
      terpisah (dia `<span>`, bukan `<button>`)
