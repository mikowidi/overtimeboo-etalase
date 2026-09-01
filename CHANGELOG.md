# Riwayat versi

Repo ini dibuat setelah situsnya sudah jalan, jadi commit sebelum
15 Agustus 2026 **diimpor secara retroaktif** dari arsip berkas. Tanggal
pada commit mencerminkan kapan versi itu benar-benar dibuat, tapi
commit-nya sendiri baru dicatat belakangan. Ini disebutkan supaya
riwayatnya tidak dibaca sebagai jejak kerja harian.

---

## v3.5 — 15 Agustus 2026 (live sekarang)
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
