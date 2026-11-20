# Laporan Review Responsivitas dan Aksesibilitas
## Sistem Manajemen Operasional, Layanan Drum, dan Keuangan PT Pulo Kencana Drum Industri (PKDI)

**Tanggal Review:** 17 November 2026
**Fase:** Fase 7 (Responsive Layout & Human Accessibility Review)
**Acuan Skill:** `antislop-ui`, `antislop-layoutmobile`, `antislop-human`, `antislop-copywriting`, dan [DESIGN.md](file:///c:/sistem_pkdi/DESIGN.md)

---

## 1. Evaluasi Responsivitas Multi-Breakpoint (`antislop-layoutmobile`)

Sistem diuji dan ditinjau pada 5 rentang lebar layar (bukan hanya dua ekstrem ponsel dan desktop):

| Rentang Layar | Lebar Uji | Perilaku Layout, Navigasi, Tabel, dan Form | Status |
|---|---|---|---|
| **Ponsel Kecil** | `360px` | Sidebar otomatis tersembunyi sebagai drawer off-canvas (`data-sidebar="closed"`) dengan overlay gelap saat dibuka; kartu metrik 1 kolom (`grid-cols-1`); tombol aksi utama (`btn-lg`) memiliki tinggi `44px` dan lebar penuh (`w-full`) di form pos gerbang dan pesanan; tabel lebar dibungkus `.overflow-x-auto` sehingga tidak menimbulkan horizontal scroll pada `body`. | **PASS** |
| **Ponsel Besar** | `412px` | Header menampilkan tombol toggle navigasi, switcher/notifikasi, dan toggle tema dengan jarak sentuh aman (`gap-2`, ukuran ikon tombol `40px x 40px`); modal konfirmasi menyesuaikan lebar `w-[calc(100%-2rem)]`. | **PASS** |
| **Tablet (Portrait & Landscape)** | `768px` | Grid kartu ringkasan dan form input berpasangan beralih ke 2 kolom (`sm:grid-cols-2`), sementara sidebar tetap berupa drawer overlay hingga `< 1024px` agar tabel operasional (yang membutuhkan ruang horizontal) tidak terjepit di layar tablet `768px`. | **PASS** |
| **Laptop** | `1024px` | Sidebar kiri terbuka permanen (`w-64`) dan mendukung mode ringkas (`sbmode="mini"`, `w-16`); halaman detail pesanan, pemeriksaan gerbang, gudang, dan invoice terbagi menjadi 2 kolom proporsional (`lg:grid-cols-2` atau `lg:grid-cols-[minmax(0,2fr)_minmax(18rem,1fr)]`). | **PASS** |
| **Desktop Lebar** | `1280px - 1440px` | Kontainer utama dibatasi `max-w-7xl` agar baris teks tidak terlalu panjang; kartu metrik atas tersusun 4 kolom (`xl:grid-cols-4`) dengan nominal Rupiah berkelas `whitespace-nowrap tabular-nums xl:text-[1.2rem]` sehingga angka puluhan juta Rupiah tetap satu baris dan tidak terpotong. | **PASS** |

### Pencegahan Overflow & Pemotongan Teks
- Seluruh nomor pesanan (`hari-DD/MM/YY-N`, contoh: `senin-16/11/26-3`) dan nomor polisi kendaraan diberi kelas `tabular-nums whitespace-nowrap` pada sel tabel agar tidak pernah patah ke baris kedua.
- Kontainer flex dan grid yang memuat teks panjang atau grafik SVG dilengkapi `min-w-0` untuk mencegah elemen mendorong batas kolom keluar layar.

---

## 2. Verifikasi Kontras Warna WCAG 2.2 AA (`contrast-check.py`)

Seluruh pasangan warna pada [DESIGN.md](file:///c:/sistem_pkdi/DESIGN.md) dan `resources/css/app.css` diuji menggunakan utilitas `contrast-check.py`:

### A. Mode Terang (Default Operasional & Halaman Login/Register)

| Elemen | Warna Depan | Warna Latar | Rasio Terukur | Ambang WCAG | Hasil |
|---|---|---|---|---|---|
| Teks Utama pada Kartu/Tabel | `#18181B` | `#FFFFFF` | `17.72:1` | `>= 4.5:1` | **PASS** |
| Teks Utama pada Kanvas | `#18181B` | `#F6F5F4` | `16.27:1` | `>= 4.5:1` | **PASS** |
| Teks Sekunder / Timestamp | `#52525B` | `#FFFFFF` | `7.73:1` | `>= 4.5:1` | **PASS** |
| Teks Sekunder pada Kanvas | `#52525B` | `#F6F5F4` | `7.10:1` | `>= 4.5:1` | **PASS** |
| Batas Kontrol Form (`.input`, `.select`, `.btn-outline`) | `#8E8E95` | `#FFFFFF` | `3.25:1` | `>= 3.0:1` (UI 1.4.11) | **PASS** |
| Tombol Utama Marun PKDI | `#FFFFFF` | `#881337` | `9.57:1` | `>= 4.5:1` | **PASS** |
| Teks Aksen Brand (`Sistem Operasional Layanan Drum`) | `#881337` | `#FFFFFF` | `9.57:1` | `>= 4.5:1` | **PASS** |
| Badge Soft / Navigasi Aktif | `#881337` | `#FFF1F2` | `8.71:1` | `>= 4.5:1` | **PASS** |
| Badge Status `success` | `#15803D` | `#DCFCE7` | `4.57:1` | `>= 4.5:1` | **PASS** |
| Badge Status `warning` | `#92400E` | `#FEF3C7` | `6.37:1` | `>= 4.5:1` | **PASS** |
| Badge Status `danger` | `#B91C1C` | `#FEE2E2` | `5.30:1` | `>= 4.5:1` | **PASS** |
| Badge Status `info` | `#075985` | `#E0F2FE` | `6.59:1` | `>= 4.5:1` | **PASS** |

### B. Mode Gelap (Dashboard Internal)

| Elemen | Warna Depan | Warna Latar | Rasio Terukur | Ambang WCAG | Hasil |
|---|---|---|---|---|---|
| Teks Utama pada Kartu | `#F4F4F5` | `#18181B` | `16.12:1` | `>= 4.5:1` | **PASS** |
| Teks Sekunder / Timestamp | `#A1A1AA` | `#18181B` | `6.91:1` | `>= 4.5:1` | **PASS** |
| Batas Kontrol Form (`.input`, `.select`, `.btn-outline`) | `#71717A` | `#18181B` | `3.67:1` | `>= 3.0:1` (UI 1.4.11) | **PASS** |
| Tombol Utama Mode Gelap | `#FFFFFF` | `#BE123C` | `6.29:1` | `>= 4.5:1` | **PASS** |
| Teks Aksen Brand Mode Gelap | `#FB7185` | `#18181B` | `6.58:1` | `>= 4.5:1` | **PASS** |
| Badge Status `success` | `#86EFAC` | `#143322` | `9.80:1` | `>= 4.5:1` | **PASS** |
| Badge Status `warning` | `#FCD34D` | `#3A270B` | `9.87:1` | `>= 4.5:1` | **PASS** |
| Badge Status `danger` | `#FCA5A5` | `#3B1219` | `8.60:1` | `>= 4.5:1` | **PASS** |
| Badge Status `info` | `#7DD3FC` | `#0F2A3F` | `8.85:1` | `>= 4.5:1` | **PASS** |

---

## 3. Evaluasi Aksesibilitas & Interaksi Manusia (`antislop-human`)

1. **Struktur Dokumen & Landmark (`R-39`):**
   - Setiap halaman memiliki `<html lang="id">`, `<title>` kontekstual, tautan *skip-to-content* (`Langsung ke konten utama` menuju `#page-root`), `<aside aria-label="Navigasi utama">`, `<header>`, `<main id="page-root" tabindex="-1">`, dan `<footer>`.
   - Tidak ada atribut `user-scalable=no` atau `maximum-scale=1` pada `<meta name="viewport">`, sehingga pembesaran teks 200% berfungsi penuh.
2. **Navigasi Keyboard & Fokus Terlihat (`R-32`):**
   - Seluruh elemen interaktif menggunakan tag HTML semantik (`<a href>`, `<button type="button|submit">`, `<input>`, `<select>`, `<textarea>`, `<dialog>`).
   - Cincin fokus `:focus-visible` (`outline: 2px solid hsl(var(--ring)); outline-offset: 2px`) aktif pada seluruh tombol, tautan, input, dan header kolom sortir tabel (`th[data-sort]` dengan `tabindex="0"` dan dukungan tombol `Enter`).
   - Komponen tab (`role="tablist"`, `role="tab"`) mendukung navigasi panah kiri/kanan (`ArrowLeft` / `ArrowRight`), dan modal native `<dialog>` mendukung penutupan dengan tombol `Escape`.
3. **Label Formulir, Penanda Wajib, & Pesan Kesalahan:**
   - Setiap kolom input dibungkus `<x-ui.field>` yang merender `<label class="label" for="...">` secara eksplisit di atas input (tidak pernah mengandalkan placeholder sebagai pengganti label).
   - Tanda bintang wajib `*` memiliki `aria-hidden="true"` dan input memiliki atribut `required`.
   - Saat validasi gagal, pesan kesalahan spesifik ditampilkan di dalam `<p class="field-error" role="alert">` disertai kelas `.input-invalid` dan ringkasan `<x-ui.alert variant="danger">` di bagian atas halaman.
4. **Indikator Status Non-Warna & 4 State Data (`R-27`):**
   - Setiap `<x-ui.badge>` selalu menampilkan teks Bahasa Indonesia yang jelas (`Menunggu Pemeriksaan Satpam`, `Pemeriksaan Masuk Ditahan`, `Sedang Diproses Gudang`, `Lunas`, `Diblokir Keluar`) sehingga pengguna dengan gangguan penglihatan warna tidak bergantung pada warna semata.
   - Komponen `<x-ui.table-state>` dan `<x-ui.empty>` menyediakan state `loading` (skeleton + `role="status"`), `empty` (data kosong awal), `filtered` (kosong hasil pencarian + tombol `Atur Ulang Filter`), dan `error` (`role="alert"` + tombol muat ulang).
   - Tombol aksi yang belum memenuhi syarat bisnis (seperti `Setujui Keluar` pada pesanan belum lunas atau `Terbitkan Surat Jalan` sebelum `PAID`) diberi atribut `disabled` (`cursor: not-allowed; opacity: 0.55`) disertai kotak peringatan tertulis yang menjelaskan syarat yang belum terpenuhi.
5. **Motion & `prefers-reduced-motion` (`R-19`, `R-39`):**
   - Di dalam `resources/css/app.css`, aturan `@media (prefers-reduced-motion: reduce)` mematikan seluruh durasi animasi dan transisi (`animation-duration: 0.01ms !important; transition-duration: 0.01ms !important`).
