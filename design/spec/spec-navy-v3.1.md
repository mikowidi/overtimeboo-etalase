# SPEC v3.1 — ETALASE OVERTIMEBOO, ARAH "LEMBAR SKOR"

Tanggal: 13 Agustus 2026
Menemani: `handoff-web-etalase-v3-tema-navy.md` (v3)
Mockup arah baru: `Etalase Navy v3.1.dc.html`
Status: siap implementasi ke `index.html`

Dokumen ini **bukan pengganti handoff v3**. Token warna, tipografi, radius,
dan daftar "tidak boleh disentuh" di v3 dipakai persis. Yang ditambah di
sini: komposisi, hierarki, dan tiga komponen baru yang bikin halaman ini
berhenti terasa adem — plus catatan deviasi dari §6 v3 supaya tidak ada yang
diam-diam.

Semua CSS di bawah bisa langsung ditempel. Nama kelas v2/v3 **tidak ada yang
diganti**, hanya ditambah — jadi JS filter, status bar, dan badge kategori
tidak perlu disentuh sama sekali.

---

## 0. DIAGNOSIS: KENAPA VERSI NAVY MASIH ADEM

Palet dan font sudah benar. Yang bikin adem itu tiga hal struktural:

1. **Tidak ada satu pun elemen berskala besar.** Hero 4.6rem lalu semua
   turun ke 1.02rem. Halaman jadi datar setelah 500px pertama.
2. **Angka disembunyikan.** Halaman ini isinya angka (15, 4, 40, 200, 166)
   tapi semuanya diset .74rem. Angka adalah satu-satunya "gambar" yang
   dimiliki produk ini.
3. **Ritme satu warna.** Navy hanya muncul di atas dan di bawah. Di antara
   keduanya 2000px kertas tanpa jeda.

Karena halaman ini tanpa fotografi, "wah"-nya harus datang dari
**tipografi berskala + angka tabular + garis**. Itu tiga bahan yang sudah
dikunci v3, cuma belum dipakai dalam ukuran yang berani.

Tiga langkah eksekusi:

| Langkah | Efek |
|---|---|
| Hero dapat **rail angka** — 15 / 4 / 1 diset clamp(1.9rem, 4.6vw, 3.4rem) tabular, dipisah garis rambut vertikal | angka jadi grafik, tanpa aset |
| Ada **satu blok navy-deep penuh lebar di tengah halaman** (cara pakai, 4 langkah bernomor) | ritme navy → kertas → navy → kertas → navy |
| Kategori jadi **ledger bergaris** dengan angka besar, bukan 4 kotak kecil | hierarki, dan angka besar kedua di halaman |

---

## 1. TOKEN — TEMPEL UTUH, GANTI SELURUH `:root` LAMA

Nol warna baru dibanding v3. Yang ditambah cuma variabel geometri supaya
angka radius tidak ditulis manual di 30 tempat.

```css
:root{
  /* ground */
  --navy:      #0E2A55;
  --navy-deep: #071A36;
  --ink:       #0A1D3A;

  /* paper */
  --paper:     #ECF0F6;
  --card:      #FFFFFF;
  --line:      #D3DCE8;
  --line-dark: #23406E;

  /* text */
  --muted:     #56637A;
  --on-navy:   #FFFFFF;
  --on-navy-2: #B9C6DC;

  /* accent */
  --accent:    #C0392B;

  /* geometri */
  --wrap:      1080px;
  --pad:       22px;
  --r-card:    10px;
  --r-hero:    18px;
  --r-pill:    999px;
  --gap:       14px;
  --sect:      clamp(52px, 8vw, 84px);   /* jarak antar section */
  --tick-w:    2px;
  --tick-h:    11px;
}
```

### 1.1 Kontras — dihitung ulang, termasuk pasangan baru

| Pasangan | Rasio | Status |
|---|---|---|
| `--ink` di `--paper` | 14.7:1 | AAA |
| `--ink` di `--card` | 16.8:1 | AAA |
| putih di `--navy` | 14.2:1 | AAA |
| putih di `--navy-deep` | 17.3:1 | AAA |
| `--on-navy-2` di `--navy` | 8.2:1 | AAA |
| **`--on-navy-2` di `--navy-deep`** (baru: blok cara pakai) | **10.1:1** | AAA |
| **`--navy` di putih** (baru: pill putih di atas navy) | **14.2:1** | AAA |
| `--muted` di `--paper` | 5.3:1 | AA |
| `--muted` di `--card` | 6.1:1 | AA |
| `--accent` di `--paper` | 4.8:1 | AA |
| `--accent` di `--card` | 5.4:1 | AA |
| `--accent` di `--navy` | 2.6:1 | **GAGAL — bentuk saja** |

### 1.2 Anggaran merah per layar — aturan v3 dibuat operasional

Aturan v3: merah maksimal tiga tempat per layar. Di mockup v3 badge kategori
tiap kartu berwarna merah — dengan 15 kartu, itu 15 merah dalam satu layar.
**Perbaikan:** badge kategori di kartu jadi `--muted`. Merah disimpan untuk:

| Layar | Merah yang boleh muncul |
|---|---|
| hero | 1 tick di rail angka (di atas kolom "preview gratis") |
| katalog | tick garis ambang batas + badge `gratis` + harga `gratis` = 3 |
| cara pakai (navy-deep) | 1 tick di atas garis langkah 01 |
| kategori | tick garis ambang batas = 1 |
| faq | garis kiri 3px pada item yang terbuka = 1 |
| footer | 1 tick di garis pemisah + tick di monogram = 2 |

---

## 2. TIPOGRAFI — SKALA v3 + SATU PERAN BARU

Skala §4 v3 dipakai persis. Satu peran ditambah, karena blok tengah butuh
bobot yang sama dengan hero tapi tidak boleh menyamainya:

| Peran | Font | Ukuran |
|---|---|---|
| Hero | Newsreader 400 | `clamp(2.4rem, 7vw, 4.6rem)` / 1.05 / `-.02em` |
| **Poster (judul blok navy-deep)** | **Newsreader 400** | **`clamp(1.9rem, 5vw, 3.1rem)` / 1.1 / `-.02em`** |
| Judul section | Newsreader 500 | `clamp(1.5rem, 3.6vw, 2.1rem)` / 1.18 |
| **Angka rail (hero)** | **Archivo 600** | **`clamp(1.9rem, 4.6vw, 3.4rem)` / 1 / `-.02em` tabular** |
| **Angka ledger (kategori)** | **Archivo 600** | **`clamp(1.6rem, 3vw, 2.2rem)` / 1 tabular** |
| Judul kartu | Archivo 600 | `1.02rem` / 1.35 |
| Body | Archivo 400 | `1.0625rem` / 1.65 |
| Meta / label | Archivo 500 | `.78rem`, `.09em`, HURUF BESAR |
| Harga | Archivo 600 | `1.12rem` tabular |

`font-variant-numeric: tabular-nums` wajib di: `.harga`, `.stat__n`,
`.kat__n-angka`, `.langkah__no`, `.badge b`, dan angka di status bar.

---

## 3. KOMPONEN BARU — CSS SIAP TEMPEL

### 3.1 Rail angka di hero

Pengganti `.hero__meta` yang lama (stat inline .74rem). Tiga sel selebar
sama, dipisah garis rambut vertikal — grid yang kelihatan, bukan grid yang
diniatkan.

```css
.hero{ background:var(--navy); color:var(--on-navy);
  border-radius:0 0 var(--r-hero) var(--r-hero);
  padding:clamp(46px,7vw,80px) 0 0; }
.hero h1{ font:400 clamp(2.4rem,7vw,4.6rem)/1.05 Newsreader,serif;
  letter-spacing:-.02em; margin:0 0 clamp(26px,4vw,40px);
  max-width:16ch; text-wrap:pretty; }
.hero__lead{ display:flex; flex-wrap:wrap; align-items:flex-end;
  justify-content:space-between; gap:22px 40px; }
.hero__lead p{ color:var(--on-navy-2); margin:0; max-width:46ch; }

.rail{ margin-top:clamp(34px,5vw,58px); border-top:1px solid var(--line-dark);
  display:grid; grid-template-columns:repeat(3,minmax(0,1fr)); }
.rail__c{ position:relative; padding:18px 14px clamp(30px,4vw,44px) 18px;
  border-left:1px solid var(--line-dark); }
.rail__c:first-child{ border-left:0; padding-left:0; }
.rail__n{ font:600 clamp(1.9rem,4.6vw,3.4rem)/1 Archivo,sans-serif;
  font-variant-numeric:tabular-nums; letter-spacing:-.02em; }
.rail__l{ margin-top:9px; font:500 .74rem/1.3 Archivo,sans-serif;
  letter-spacing:.1em; text-transform:uppercase; color:var(--on-navy-2); }
/* tick merah = penanda pintu masuk gratis. bentuk, bukan teks. */
.rail__c--mark::before{ content:""; position:absolute; left:18px; top:-6px;
  width:var(--tick-w); height:var(--tick-h); background:var(--accent); }
```

Isi rail diambil dari array, bukan diketik: `PRODUK.length`,
`KATEGORI.length`, dan jumlah item berharga 0. **Jangan pernah menulis
angka ini sebagai teks statis** — itu kesalahan `og.png` v2 yang terulang.

### 3.2 Blok cara pakai (navy-deep, penuh lebar)

Ini jeda ritme di tengah halaman, dan isinya kalimat yang sudah lolos
review Deus (diambil dari jawaban FAQ #1 v2 — lihat §5 soal duplikasinya).

```css
.cara{ background:var(--navy-deep); color:var(--on-navy);
  padding:clamp(48px,7vw,84px) 0; }
.cara h2{ font:400 clamp(1.9rem,5vw,3.1rem)/1.1 Newsreader,serif;
  letter-spacing:-.02em; margin:0 0 clamp(28px,4vw,44px);
  max-width:22ch; text-wrap:pretty; }
.langkah{ display:grid; grid-template-columns:repeat(auto-fit,minmax(300px,1fr));
  gap:0 clamp(24px,4vw,52px); }
.langkah__i{ position:relative; display:grid;
  grid-template-columns:56px minmax(0,1fr); gap:16px; align-items:baseline;
  padding:18px 0; border-top:1px solid var(--line-dark); }
.langkah__i--last{ border-bottom:1px solid var(--line-dark); }
.langkah__no{ font:600 1.4rem/1 Archivo,sans-serif;
  font-variant-numeric:tabular-nums; color:var(--on-navy-2); }
.langkah__i--first::before{ content:""; position:absolute; left:0; top:-6px;
  width:var(--tick-w); height:var(--tick-h); background:var(--accent); }
```

Blok ini **keluar dari `.wrap`** (lihat struktur HTML §4): full-bleed, dan
`.wrap` dipakai lagi di dalamnya.

### 3.3 Kategori sebagai ledger

Empat kotak kecil identik selalu terbaca sebagai pengisi tempat. Ledger
bergaris memberi hierarki dan mengulang bahasa angka dari hero.

```css
.kat__row{ display:grid; grid-template-columns:minmax(0,96px) minmax(0,1fr) auto;
  gap:12px 18px; align-items:baseline; padding:22px 8px 22px 0;
  border-bottom:1px solid var(--line); color:var(--ink);
  text-decoration:none; transition:background .14s, padding-left .14s; }
.kat__row:hover{ background:var(--card); padding-left:10px; }
.kat__row--soon{ opacity:.55; }
.kat__row--soon:hover{ background:none; padding-left:0; }   /* mati, belum bisa diklik */
.kat__angka{ font:600 clamp(1.6rem,3vw,2.2rem)/1 Archivo,sans-serif;
  font-variant-numeric:tabular-nums; letter-spacing:-.02em; }
.kat__n{ font:500 clamp(1.1rem,2.2vw,1.35rem)/1.25 Newsreader,serif; }
.badge{ font:500 .68rem/1 Archivo,sans-serif; letter-spacing:.1em;
  text-transform:uppercase; color:var(--muted); overflow-wrap:break-word; }
.badge--soon{ border:1px solid var(--line); border-radius:var(--r-pill);
  padding:6px 12px; }
```

**Hook JS tidak berubah:** angka tetap ditulis JS ke elemen `.badge b` /
`.kat__angka` sesuai yang sekarang. Kalau JS sekarang menulis
`innerHTML = "<b>13</b> produk"`, biarkan — tempatkan `.badge` di kolom
kanan dan pindahkan angka besar ke `.kat__angka` dengan variabel yang sama.
Jangan menghitung ulang di dua tempat.

### 3.4 Kartu bundle lebar (feature)

Satu kartu navy penuh lebar di atas grid. Ini satu-satunya kartu yang boleh
beda, dan alasannya komersial: bundle margin terbaik.

```css
.card--wide{ background:var(--navy); color:var(--on-navy);
  border:0; border-radius:var(--r-card); padding:clamp(24px,4vw,36px);
  margin-bottom:var(--gap); display:grid;
  grid-template-columns:repeat(auto-fit,minmax(260px,1fr));
  gap:clamp(20px,3vw,36px); }
.card--wide h3{ font:500 clamp(1.45rem,3vw,2rem)/1.2 Newsreader,serif;
  letter-spacing:-.015em; margin:0 0 14px; max-width:22ch;
  overflow-wrap:break-word; }
.card--wide .harga{ font:600 clamp(1.7rem,3.4vw,2.3rem)/1 Archivo,sans-serif;
  letter-spacing:-.02em; color:var(--on-navy); }   /* WAJIB putih, bukan accent */
.kicker{ display:inline-flex; align-items:center; gap:9px;
  font:500 .68rem/1 Archivo,sans-serif; letter-spacing:.11em;
  text-transform:uppercase; color:var(--on-navy); margin-bottom:14px; }
.kicker::before{ content:""; width:var(--tick-w); height:var(--tick-h);
  background:var(--accent); }
/* pill di atas navy dibalik: dasar putih, teks navy. 14.2:1 */
.btn--invert{ background:var(--card); color:var(--navy); border-color:var(--card); }
.btn--invert:hover{ background:var(--paper); border-color:var(--paper); }
```

Jebakan yang gampang kejadian: harga di kartu ini refleks dibikin merah.
`--accent` di `--navy` = 2.6:1, gagal. **Harga putih, tick merah.**

### 3.5 Empty state filter

Belum ada di v2/v3, tapi wajib: kategori `segera` dan chip yang belum punya
produk sekarang menghasilkan grid kosong tanpa penjelasan.

```css
.kosong{ border:1px dashed var(--line); border-radius:var(--r-card);
  padding:26px 22px; background:var(--card); }
.kosong__t{ font:600 1.02rem/1.35 Archivo,sans-serif; margin:0 0 6px; }
.kosong__b{ margin:0; color:var(--muted); font-size:.94rem; }
```

### 3.6 Monogram persona

§12 v3 minta persona Overtimeboo tetap nongol. Tanpa aset, tanpa
ilustrasi — monogram tipografis yang sama dengan spesifikasi `favicon.png`
§8, jadi favicon dan header akhirnya satu bahasa. **Nol segel, nol perisai.**

```css
.mono{ position:relative; width:26px; height:26px; border-radius:var(--r-card);
  background:var(--card); color:var(--navy); display:grid; place-items:center;
  font:500 1.05rem/1 Newsreader,serif; }
.mono::after{ content:""; position:absolute; left:7px; bottom:5px;
  width:12px; height:var(--tick-w); background:var(--accent); }
.brand{ display:inline-flex; align-items:center; gap:10px;
  font:500 1.16rem/1 Newsreader,serif; letter-spacing:-.01em; }
```

### 3.7 Komponen v3 yang dipakai apa adanya

`.batas`, `.eyebrow`, `.chip`, `.card`, `.btn`, `.btn--ghost`, `details`,
`footer` — CSS-nya persis seperti `mockup-tema-navy-v3.html`, dengan dua
penyesuaian kecil:

```css
/* 1. badge kategori di kartu: merah -> muted (lihat anggaran merah §1.2) */
.card__badge{ color:var(--muted); }
.card__badge--gratis{ color:var(--accent); }

/* 2. baris atas kartu: badge kiri + nomor paket kanan (tabular) */
.card__top{ display:flex; align-items:baseline; justify-content:space-between;
  gap:12px; margin-bottom:12px; }
.card__no{ font:600 .74rem/1 Archivo,sans-serif;
  font-variant-numeric:tabular-nums; letter-spacing:.06em; color:var(--muted); }

/* 3. garis ambang batas di atas navy */
.batas--navy{ background:var(--line-dark); }
.batas--navy .batas__label{ background:var(--navy); color:var(--on-navy-2); }
```

### 3.8 Label garis ambang batas yang dipakai (semua angka jujur)

| Posisi | Label |
|---|---|
| sebelum katalog | `40 SOAL / PAKET` |
| sebelum blok cara pakai | `200 SOAL / BUNDLE` |
| sebelum kategori | `166 PASSING GRADE` |
| sebelum FAQ | tanpa label (garis polos) |
| footer, di atas disclaimer | tanpa label, `.batas--navy`, tick merah |

Kalau nanti bundle isinya bukan 200 soal, ganti labelnya. Jangan
dipertahankan karena enak dilihat.

---

## 4. STRUKTUR HTML — URUTAN FINAL

Perubahan struktural cuma satu: `.wrap` tunggal yang membungkus semuanya
dipecah, supaya blok `.cara` bisa full-bleed. Sisanya penambahan.

```html
<header class="top">
  <div class="wrap top__in">
    <span class="brand"><span class="mono">o</span>overtimeboo<b>.</b></span>
    <nav class="top__nav">
      <a href="#katalog">katalog</a>
      <a href="#cara">cara pakai</a>
      <a href="#kategori">kategori</a>
      <a href="#faq">faq</a>
      <a class="top__link" href="[URL LYNK]">lynk.id &#8599;</a>
    </nav>
  </div>
</header>

<!-- strip: hanya dirender kalau STRIP != "" (lihat §6) -->
<div class="tick" id="strip"></div>

<section class="hero">
  <div class="wrap">
    <h1>bank soal yang kunci sama pembahasannya lengkap.</h1>
    <div class="hero__lead">
      <p>soal, kunci, dan pembahasan tiap nomor. file PDF, download langsung baca. anywhere, anytime.</p>
      <a class="btn btn--invert" href="#katalog">lihat katalog &#8595;</a>
    </div>
    <div class="rail">
      <div class="rail__c"><div class="rail__n" id="n-produk">15</div><div class="rail__l">produk</div></div>
      <div class="rail__c"><div class="rail__n" id="n-kategori">4</div><div class="rail__l">kategori</div></div>
      <div class="rail__c rail__c--mark"><div class="rail__n" id="n-gratis">1</div><div class="rail__l">preview gratis</div></div>
    </div>
  </div>
</section>

<div class="wrap" id="katalog">
  <div class="batas"><span class="batas__label">40 SOAL / PAKET</span></div>
  <div class="eyebrow"><span>katalog</span><span id="status">15 produk</span></div>
  <h2>pilih materi yang lagi kamu kejar</h2>
  <div class="chips"><!-- chip: TIDAK BERUBAH, hanya border-radius --></div>
  <article class="card card--wide"><!-- bundle --></article>
  <div class="grid"><!-- .card lainnya, dirender dari array PRODUK --></div>
  <div class="kosong" hidden><!-- empty state --></div>
</div>

<div class="wrap">
  <div class="batas"><span class="batas__label">200 SOAL / BUNDLE</span></div>
</div>

<section class="cara" id="cara">          <!-- full-bleed, di luar .wrap -->
  <div class="wrap">
    <div class="eyebrow eyebrow--navy"><span>cara pakai</span><span>4 langkah</span></div>
    <h2>yang dilatih polanya, bukan soalnya.</h2>
    <div class="langkah">
      <div>
        <div class="langkah__i langkah__i--first"><span class="langkah__no">01</span><span>kerjain dulu tanpa lihat kunci</span></div>
        <div class="langkah__i"><span class="langkah__no">02</span><span>salin pembahasan soal yang salah ke ChatGPT atau Claude</span></div>
      </div>
      <div>
        <div class="langkah__i"><span class="langkah__no">03</span><span>minta 3 soal berpola sama</span></div>
        <div class="langkah__i langkah__i--last"><span class="langkah__no">04</span><span>ulang besoknya</span></div>
      </div>
    </div>
  </div>
</section>

<div class="wrap" id="kategori">
  <div class="batas"><span class="batas__label">166 PASSING GRADE</span></div>
  <div class="eyebrow"><span>kategori</span><span>4 lini</span></div>
  <div class="kat"><!-- .kat__row x4, dirender dari array KATEGORI --></div>

  <div class="batas" id="faq"></div>
  <div class="eyebrow"><span>faq</span><span>2 pertanyaan</span></div>
  <details>…</details>
</div>

<footer>
  <div class="wrap">
    <div class="foot__top">
      <div><span class="brand">…</span><div>bank soal digital &#183; dikirim otomatis lewat lynk.id</div></div>
      <nav class="foot__nav">…</nav>
    </div>
    <div class="batas batas--navy"></div>
    <p class="disc">soal 100% original. ini BUKAN bocoran soal seleksi mana pun. tidak berafiliasi dengan BKN, KemenPANRB, FHCI, Bank Indonesia, maupun BUMN mana pun.</p>
  </div>
</footer>
```

Catatan urutan: blok `cara` sengaja **setelah** katalog. Orang datang untuk
lihat barang; metode itu argumen penutup sebelum kategori dan FAQ, bukan
pembuka.

---

## 5. MEDIA QUERY — CUKUP DUA

Sisanya sudah ditangani `clamp()` dan `auto-fit`.

```css
@media (max-width:560px){
  .rail{ grid-template-columns:1fr; }
  .rail__c{ border-left:0; padding-left:0;
    border-top:1px solid var(--line-dark); }
  .rail__c:first-child{ border-top:0; }
  .rail__c--mark::before{ left:0; }
  .kat__row{ grid-template-columns:minmax(0,64px) minmax(0,1fr);
    row-gap:6px; }
  .kat__row .badge{ grid-column:2; }
}
@media (max-width:420px){
  .top__nav{ gap:14px; font-size:.72rem; }
}
```

---

## 6. STRIP JADWAL — SATU KONSTANTA, SATU GERBANG

Persis mandat §7 v3, ditulis lengkap supaya tidak ada tafsir:

```js
// GANTI/KOSONGKAN SETELAH SEPTEMBER 2026
const STRIP = "TPD ~1 SEP · TES TEKNIS 2–20 SEP · PCPM BI ANGKATAN 41";

const stripEl = document.getElementById("strip");
if (STRIP.trim()) { stripEl.textContent = STRIP; }
else { stripEl.remove(); }        // dihapus, bukan dikosongkan
```

Tanpa animasi. Teks statis di tengah. `text-overflow:ellipsis` sudah ada di
`.tick` untuk layar 320px.

---

## 7. DEVIASI DARI §6 HANDOFF v3 — DIDAFTAR, TIDAK DIAM-DIAM

| # | §6 v3 bilang | v3.1 melakukan | Alasan |
|---|---|---|---|
| 1 | kartu kategori = sama seperti kartu produk | ledger bergaris + angka besar | 4 kartu kecil identik = pengisi tempat; ledger memberi hierarki dan mengulang bahasa angka |
| 2 | semua kartu produk seragam | bundle jadi kartu navy penuh lebar | margin terbaik pantas dapat berat visual; jadi jangkar di atas grid |
| 3 | badge kartu warna `--accent` | `--muted`, kecuali badge `gratis` | 15 kartu × badge merah melanggar aturan "maksimal 3 merah per layar" milik v3 sendiri |
| 4 | `.hero__meta` stat inline .74rem | rail 3 sel, angka sampai 3.4rem | satu-satunya sumber "wah" tanpa fotografi |
| 5 | (tidak ada) | blok `cara pakai` navy-deep full-bleed | ritme warna; halaman butuh jeda di tengah |
| 6 | (tidak ada) | nav anchor di topbar + footer | halaman jadi ~2.5× lebih panjang; tanpa anchor, kategori dan FAQ tidak pernah ditemukan |
| 7 | (tidak ada) | empty state filter | `segera` dan chip kosong sekarang menghasilkan grid kosong tanpa keterangan |
| 8 | 1 peran tipe untuk judul section | +1 peran `poster` untuk judul blok navy | judul di blok gelap butuh bobot hero, tapi tidak boleh menyamai h1 |

**Yang TIDAK dideviasi:** token warna, keluarga font, skala hero/body/meta,
tiga nilai radius, `.batas` beserta ukuran ticknya, `<details>` native,
struktur array, hook JS, tiga URL hardcode.

**Yang sengaja tetap ditolak** walau muncul di semua referensi: dasar krem,
aksen terakota, serif kontras tinggi, fotografi gaya hidup. Referensi dipakai
untuk tata letak saja — sesuai §2 v3.

---

## 8. KEPUTUSAN §11 YANG DIPAKAI DI MOCKUP INI (SEMUA "DEFAULT AMAN")

Belum ada jawaban Deus, jadi semuanya default. Kalau salah satu berubah,
yang berubah cuma copy.

| # | Diputuskan sebagai | Di mana kelihatan |
|---|---|---|
| 1 | emoji 👌 dibuang | hero |
| 2 | sub hero jadi satu kalimat: `file PDF, download langsung baca. anywhere, anytime.` | hero |
| 3 | tombol jadi `checkout di lynk ↗` | semua kartu berbayar |
| 4 | Riso Pop tidak disebut; IG dibiarkan | — |
| 5–6 | tidak disentuh | — |

### 8.1 Satu copy baru, satu copy dipindah — perlu keputusan

- **Baru:** judul blok cara pakai `yang dilatih polanya, bukan soalnya.`
  Ini pertanyaan FAQ #2 v2 yang dijadikan pernyataan. Kalau tidak disetujui,
  pakai `cara pakai yang paling ngefek.` (juga kalimat v2).
- **Dipindah:** empat langkah di blok itu diambil verbatim dari jawaban FAQ
  #1 v2. Supaya tidak muncul dua kali dalam satu halaman, jawaban FAQ #1
  dipotong jadi kalimat watermark-nya saja — yang justru menjawab
  pertanyaannya ("file-nya bisa dishare ke temen?"). **Ini menghapus copy
  hasil review; kalau tidak setuju, blok cara pakai butuh 4 kalimat baru.**

---

## 9. ANGGARAN UKURAN

| Bagian | Perkiraan |
|---|---|
| `index.html` v2 sekarang | 22.5 KB |
| CSS komponen baru (§3.1–3.6) | +1.9 KB |
| markup blok cara pakai + rail + empty state | +1.1 KB |
| ledger kategori (mengganti markup kartu) | +0.2 KB |
| CSS window/retro yang dicabut | −1.4 KB |
| **perkiraan v3.1** | **±24.3 KB** |

Batas 30 KB aman. Kalau tembus, yang dibuang pertama: nav anchor footer,
lalu media query 420px. Aksesibilitas tidak masuk daftar buangan.

---

## 10. KRITERIA TERIMA — TAMBAHAN DI ATAS §9 HANDOFF v3

Semua checklist §9 v3 tetap berlaku. Yang ditambah:

- [ ] `--accent` nol kemunculan sebagai `color` di dalam blok bergrouond
      navy/navy-deep. Cek dengan grep pada `.cara`, `.card--wide`, `.hero`,
      `footer`
- [ ] harga di `.card--wide` putih, bukan merah
- [ ] hitung merah per layar sesuai tabel §1.2 — di layar katalog maksimal 3
- [ ] angka rail hero dan angka ledger dibaca dari array, nol angka statis
      di markup
- [ ] `.langkah`, `.rail`, `.kat__row` tidak dipakai JS mana pun — kalau
      dipakai, catat di handoff berikutnya
- [ ] tab keyboard: nav anchor topbar masuk urutan sebelum chip
- [ ] 320px: ledger kategori tidak bikin scroll horizontal (kolom angka
      64px di breakpoint 560px)
- [ ] blok `.cara` full-bleed tidak menghasilkan scroll horizontal di iOS
      (cek `overflow-x` pada `body`)

---

## 11. og.png — TAMBAHAN KECIL

§8 v3 tetap. Satu tambahan: pakai rail angka sebagai tata letaknya —
`bank soal · kunci · pembahasan` sebagai baris teks, lalu garis ambang batas
tipis merah, tanpa angka apa pun. Nol angka di gambar berarti nol
kemungkinan basi di cache scraper. Itu akar masalah terbuka #4 v2.
