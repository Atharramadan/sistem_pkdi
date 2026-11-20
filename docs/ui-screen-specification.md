# Spesifikasi Layar per Halaman (UI Screen Specification)
## Sistem Manajemen Operasional PKDI

Setiap halaman di bawah ini dirancang sesuai [DESIGN.md](file:///c:/sistem_pkdi/DESIGN.md) dan [ui-sitemap.md](file:///c:/sistem_pkdi/docs/ui-sitemap.md), dilengkapi state wajib (`ready`, `loading`, `empty`, `filtered`, `error`, `disabled`, `validation error`, `confirmation`) dan perilaku responsif 3 rentang layar (**Ponsel `<640px`**, **Tablet/Laptop Kecil `640px-1023px`**, **Desktop `>=1024px`**).

---

## 1. Public, Autentikasi, dan Pengaturan Akun

| Layar | Tujuan Utama | Elemen & Data Utama | Aksi & Interaksi | State & Perilaku Responsif |
|---|---|---|---|---|
| **Login** (`/login`) | Masuk ke sistem bagi ke-5 peran. | Logo bulat `PKDI`, nama PT, subjudul aksen, field Email & Kata Sandi berlabel, pengingat sesi, serta panel **Pratinjau Peran**. | Validasi form, toggle tampil/sembunyi password, dan tombol pilih peran. | Error per field saat kosong/salah. Ponsel & tablet: 1 kolom form terpusat; Desktop (`lg:`): 2 kolom dengan panel Charcoal PKDI di kanan. |
| **Registrasi Client** (`/register`) | Pendaftaran akun baru khusus perusahaan pelanggan. | Nama PT, Nama PIC, Kontak, Alamat, Email, Kata Sandi & Konfirmasi. | Validasi format email, panjang sandi min 8 karakter, feedback error jelas. | State validasi inline. Lebar maksimal `max-w-xl` terpusat, 1 kolom di ponsel dan 2 kolom pada baris PIC/Kontak di tablet/desktop. |
| **Pengaturan Akun** (`/settings`) | Mengubah data pribadi akun aktif (tersedia di semua 5 peran). | Form Profil (Nama Lengkap, Email, role terkunci) dan Form Ganti Kata Sandi. | Submit dengan validasi dan toast konfirmasi sukses. | State disabled pada field Peran. 1 kolom di ponsel/tablet (`<1024px`), 2 kartu sejajar di desktop (`lg:grid-cols-2`). |

---

## 2. Portal Client (`/client/*`)

| Layar | Tujuan Utama | Elemen & Data Utama | Aksi & Interaksi | State & Perilaku Responsif |
|---|---|---|---|---|
| **Dashboard Client** | Memantau pesanan aktif & tagihan PT sendiri. | CTA `Buat Pesanan Baru`, 4 kartu ringkasan pesanan/tagihan, tabel pesanan terbaru, dan panel tagihan belum lunas. | Klik ke detail pesanan, detail tagihan, atau langsung buat pesanan. | Ponsel: kartu 1 kolom + tabel `overflow-x-auto`; Tablet: kartu `sm:grid-cols-2`; Desktop: `2fr : 1fr` untuk tabel & panel tagihan. |
| **Daftar Pesanan** | Mencari dan meninjau riwayat pesanan PT sendiri. | Filter kata kunci (No. Pesanan/Nopol), filter status, tabel pesanan, dan paginasi. | Pencarian langsung, filter status, atur ulang filter, ke detail. | Dilengkapi 4 state lengkap (`ready`, `loading` skeleton, `empty` + CTA, `filtered` + reset, `error` + muat ulang). |
| **Buat Pesanan Baru** | Mengajukan permintaan layanan drum. | Input teks ketik langsung (tanpa dropdown): **Jenis Kategori**, **Jenis Muatan**, **Jenis Layanan**, **Jenis Kendaraan**; plus Nopol wajib, Asal PT, Jumlah & Satuan, **Nama Driver (Opsional)**, Catatan, & Lampiran Opsional. | Validasi field wajib vs opsional, pratinjau file `<x-ui.file-preview>`, konfirmasi kirim. | Ponsel & tablet: 1 kolom stack dengan tombol penuh di bawah; Desktop: form 2fr + kartu ringkasan 1fr di kanan. |
| **Detail Pesanan & Timeline** | Melacak posisi pesanan di pabrik secara kronologis. | Data Pengajuan Awal vs Pemeriksaan/Hasil Gudang, badge status per jalur, dan `<x-ui.timeline>` WIB. | Salin nomor pesanan, tautan ke detail tagihan bila sudah terbit. | Alert bahaya muncul jika kendaraan ditahan Satpam. Ponsel/tablet: timeline di bawah; Desktop: timeline menempel di kolom kanan (`1fr`). |
| **Daftar & Detail Tagihan** | Melihat rincian biaya, unduh Invoice, & unggah bukti bayar. | Daftar tagihan milik PT sendiri; detail menampilkan tabel rincian perhitungan (`qty x tarif`), status bayar, riwayat verifikasi, & form unggah bukti. | Modal pratinjau cetak PDF Invoice, unggah bukti transfer dengan pratinjau lokal. | Tombol unggah terkunci/diganti alert sukses saat tagihan sudah `Lunas`. |
| **Profil Perusahaan** | Memperbarui identitas PT dan kontak penanggung jawab. | Nama PT, Nama PIC, Telepon, Email perusahaan, dan Alamat lengkap. | Validasi form dan simpan perubahan profil. | 1 kolom di ponsel, 2 kolom pada PIC & Telepon di tablet/desktop. |

---

## 3. Pos Gerbang Satpam (`/gate/*`)

| Layar | Tujuan Utama | Elemen & Data Utama | Aksi & Interaksi | State & Perilaku Responsif |
|---|---|---|---|---|
| **Dashboard Pos Gerbang** | Mempercepat petugas memeriksa kendaraan masuk & keluar. | Bilah pencarian cepat menonjol (Nopol / No. Pesanan / Surat Jalan) dan 3 Tab (`Menunggu Masuk`, `Di Area Pabrik`, `Pemeriksaan Keluar`). | Klik langsung ke layar verifikasi Masuk atau Keluar; target sentuh tombol `min-h-11` (`44px`). | Dirancang tanpa stat card/grafik pengganggu. Ponsel & tablet: kartu antrean 1-2 kolom selebar jari; Desktop: 2 kolom kartu + tabel area pabrik. |
| **Daftar & Detail Periksa Masuk** | Mencocokkan kendaraan masuk terhadap data pesanan. | Kartu acuan pesanan (PT, Nopol, Jenis Kendaraan, Driver/Kosong) + form pencatatan fisik aktual di gerbang. | Tombol **Setujui Masuk** (`btn-success btn-lg`) atau **Tahan Masuk** (modal wajib alasan). | Alert bahaya bila pernah ditahan sebelumnya; kolom driver aktual wajib diisi petugas jika Client mengosongkannya. |
| **Daftar & Detail Periksa Keluar** | Memverifikasi Surat Jalan & pelunasan sebelum keluar. | Detail Surat Jalan, indikator besar status pembayaran (`Lunas` vs `Belum Lunas`), muatan keluar, nopol & driver. | Tombol **Setujui Keluar** (`disabled` otomatis bila belum lunas/Surat Jalan tidak sah) dan **Tahan Keluar** (modal wajib alasan). | State `disabled` jelas dengan alert blokir merah saat belum lunas. Ponsel: tombol aksi penuh selebar layar. |
| **Riwayat Gerbang** | Audit aktivitas kendaraan masuk & keluar per petugas. | Filter jenis gerbang (`Masuk`/`Keluar`), pencarian kata kunci, dan tabel log dengan nama petugas Satpam. | Pencarian, filter jenis, dan paginasi. | Tabel responsif `overflow-x-auto` agar kolom waktu WIB dan nopol tidak terpotong. |

---

## 4. Kepala Gudang (`/warehouse/*`)

| Layar | Tujuan Utama | Elemen & Data Utama | Aksi & Interaksi | State & Perilaku Responsif |
|---|---|---|---|---|
| **Dashboard Antrean Gudang** | Mengatur prioritas pemeriksaan muatan, proses, dan Surat Jalan. | 4 Tab antrean (`Menunggu Periksa`, `Sedang Diproses`, `Siap Surat Jalan`, `Selesai / Ke Keuangan`) + tabel antrean. | Navigasi tab keyboard & klik langsung ke form kerja terkait. | Ponsel/tablet: tab dapat digulir horizontal (`overflow-x-auto`) tanpa meluber keluar layar. |
| **Pemeriksaan Muatan & Proses Jasa** | Memeriksa fisik drum vs permintaan awal & mencatat hasil kerja. | Perbandingan 2 kartu (*Permintaan Awal Client Read-Only* vs *Form Pemeriksaan Aktual*), log progres pengerjaan, dan form Hasil Akhir Pekerjaan. | **Setujui & Proses**, modal **Minta Klarifikasi** (wajib alasan), modal **Tolak** (wajib alasan), tambah progres, dan **Kirim ke Keuangan**. | Data awal Client tidak pernah tertimpa. Ponsel/tablet: 1 kolom berurutan; Desktop: 2 kolom sejajar. |
| **Penerbitan & Pratinjau Surat Jalan** | Menerbitkan izin keluar hanya untuk pesanan berstatus `PAID`. | Form Surat Jalan (rincian barang, nopol keluar, driver) dan kartu pratinjau dokumen cetak bernomor unik. | Validasi blokir otomatis (`disabled` + alert bahaya) jika tagihan belum lunas. | Ponsel/tablet: pratinjau cetak berada di bawah form; Desktop: berdampingan 2 kolom. |

---

## 5. Keuangan (`/finance/*`)

| Layar | Tujuan Utama | Elemen & Data Utama | Aksi & Interaksi | State & Perilaku Responsif |
|---|---|---|---|---|
| **Dashboard Keuangan** | Memantau antrean tagihan, verifikasi bayar, & ringkasan kas. | 4 kartu metrik (`[DATA DEMO]`), tabel *Antrean Hasil Kerja Belum Ditagih*, dan tabel *Pembayaran Menunggu Verifikasi*. | Klik langsung ke perhitungan invoice atau verifikasi pembayaran. | Ponsel: 1 kolom; Tablet: metrik `sm:grid-cols-2`; Desktop: metrik 4 kolom + 2 antrean sejajar. |
| **Hitung Jasa & Buat Invoice** | Menyusun invoice dari hasil kerja aktual Kepala Gudang. | Perbandingan hasil akhir gudang, pemilihan referensi tarif, input penyesuaian/pajak, dan estimasi total. | **Simpan Sebagai Draft** (belum terlihat Client) dan **Terbitkan Tagihan**. | Validasi angka minimal 0 dan kuantitas wajib >= 1. |
| **Daftar Invoice & Verifikasi Bayar** | Mengelola status penagihan, bukti bayar Client, dan revisi. | Filter status invoice, detail bukti bayar Client, tombol **Verifikasi Sah**, **Tolak Bukti** (wajib alasan), catat bayar langsung, dan **Revisi Tagihan** (wajib alasan). | Dialog konfirmasi verifikasi sah dan modal alasan penolakan/revisi. | Ponsel/tablet: panel catat pembayaran menumpuk di bawah riwayat bayar; Desktop: 2fr : 1fr. |
| **Referensi Tarif Jasa** | Mengelola daftar harga acuan perhitungan jasa. | Tabel tarif dan modal tambah tarif (input teks kategori & jenis jasa). | Tambah tarif baru lewat modal native `<dialog>`. | Tabel horizontal scroll aman di layar sempit. |
| **Biaya Operasional** | Mencatat pengeluaran pabrik (bahan, gaji, utilitas, dll.). | Tabel pengeluaran dan modal catat pengeluaran baru (input teks **Kategori Biaya**, nominal, vendor, `<x-ui.file-preview>` nota). | Pencarian kata kunci dan tambah pengeluaran baru dengan validasi file maks 5 MB. | Modal responsif `max-h-[calc(100dvh-2rem)]` dan scroll internal. |
| **Buku Transaksi Keuangan** | Memantau arus kas masuk & keluar terpadu tanpa duplikasi. | 3 kartu ringkasan (Masuk, Keluar, Selisih) dan tabel mutasi kas dengan filter `Masuk`/`Keluar`. | Pencarian keterangan/referensi dan filter arus kas. | Kolom nominal rata kanan dengan `tabular-nums` dan warna + label (`Kas Masuk` / `Kas Keluar`). |
| **Laporan & Kontrol Ekspor** | Pratinjau laporan periodik dan kontrol ekspor PDF/Excel. | Rentang tanggal (`flatpickr`), pilihan jenis laporan, pratinjau tabel rekap, dan tombol Ekspor PDF/Excel berlabel `(Aktif Fase 6)`. | Preset 7 hari / 30 hari, validasi rentang tanggal terbalik, pratinjau filter. | Alert penjelas jujur bahwa ekspor berkas nyata diaktifkan pada Fase 6. |

---

## 6. Owner (`/owner/*`)

| Layar | Tujuan Utama | Elemen & Data Utama | Aksi & Interaksi | State & Perilaku Responsif |
|---|---|---|---|---|
| **Ringkasan Eksekutif Owner** | Memantau kesehatan operasional & kas seluruh PT PKDI (*read-only*). | 4 kartu ringkasan bertanda `(Demo)`, grafik batang SVG *Perbandingan Pemasukan Jasa vs Biaya Operasional (Contoh 6 Bulan)*, feed aktivitas lintas bagian, dan tabel pantauan pesanan. | Telusur transaksi, buka detail audit log, atau ke halaman Pendapatan & Biaya. | Grafik responsif dengan `ResizeObserver` tanpa overflow. Ponsel: 1 kolom; Tablet: 2 kolom; Desktop: `4fr : 3fr`. |
| **Pendapatan & Biaya** | Menganalisis rincian tagihan, pengeluaran, & buku kas (*read-only*). | 4 kartu ringkasan dan 3 Tab (`Tagihan & Pemasukan`, `Biaya Operasional`, `Buku Keuangan`). | Pindah tab (klik & panah keyboard) tanpa aksi ubah/hapus. | Tab wrap rapi di tablet/ponsel tanpa merusak lebar halaman. |
| **Transaksi Lintas Bagian** | Menelusuri status setiap pesanan dari hulu ke hilir. | Tabel komprehensif semua jalur (Gerbang, Gudang, Pembayaran, Utama) + halaman detail terpadu *read-only*. | Pencarian dan drill-down lengkap ke 3 kartu metadata + timeline. | Label eksplisit `Akses Pemantauan Owner (Read-Only)` pada detail. |
| **Jejak Audit Sistem** | Memeriksa akuntabilitas setiap aksi pengguna. | Tabel *append-only* Waktu WIB, Pengguna, Peran, Tindakan, Referensi, Detail & Alasan. | Pencarian teks dan filter berdasarkan peran pengguna. | State `filtered` dengan tombol reset bila tidak ditemukan. |

---

## 7. Daftar Keputusan Visual yang Membutuhkan Persetujuan

1. **Palet Identitas PKDI:** Marun (`#881337` terang / `#BE123C` & `#FB7185` gelap) dipadukan dengan Charcoal (`#18181B`) dan kanvas hangat (`#F6F5F4`).
2. **Logo Mark Sementara:** Teks **`PKDI`** di dalam lingkaran Marun (`rounded-full`) selama aset gambar logo resmi belum tersedia.
3. **Subjudul Aksen Brand:** Menggunakan teks **`Sistem Operasional Layanan Drum`** berwarna aksen Marun (tanpa mengarang slogan pemasaran).
4. **Pemisahan Tata Letak per Peran:** Layar Satpam difokuskan pada pencarian cepat dan tombol verifikasi besar tanpa stat card; layar Gudang berfokus pada perbandingan *Permintaan Awal* vs *Aktual*; layar Keuangan & Owner menampilkan ringkasan kas serta grafik perbandingan.
5. **Pengaturan Akun Universal:** Menyediakan rute `/settings` untuk seluruh 5 peran guna mengganti nama, email, dan kata sandi.

---

## 8. Self-Review Checklist Antislop (Temuan & Perbaikan Fase 2)

| Kelompok Checklist | Temuan Saat Review Dokumen & Desain | Perbaikan yang Diterapkan |
|---|---|---|
| **`antislop-ui`** | 1. Komponen `stat-card` bawaan `JWB-Dashboard-V2` mewajibkan `delta` persentase tren yang berisiko memaksa angka fiktif (`R-17`).<br>2. Komponen `detail-list` dan `select` bawaan memuat karakter em dash (`—`) yang dilarang `R-02`. | 1. Mengubah props `delta` dan `direction` pada `stat-card` menjadi opsional dan menandai seluruh angka pratinjau sebagai `[DATA DEMO]`.<br>2. Mengganti seluruh em dash pada dokumen desain dan komponen Blade menjadi `-` atau tanda baca standar. |
| **`antislop-human`** | 1. Token `--border` awal (`#E4E4E7` di atas `#FFFFFF`) hanya memiliki kontras `1.27:1` yang gagal memenuhi WCAG 1.4.11 (`3:1`) apabila dipakai sebagai garis batas input form dan tombol outline.<br>2. Beberapa tautan/tombol berisiko tidak memiliki kursor pointer seragam setelah reset Tailwind. | 1. Menambahkan token khusus `--control-border` (`#8E8E95` terang = `3.25:1`, `#71717A` gelap = `3.67:1`, terverifikasi via `contrast-check.py`) pada `DESIGN.md` dan `app.css` untuk `.input`, `.textarea`, `.select`, dan `.btn-outline`.<br>2. Menambahkan aturan base CSS `cursor: pointer` pada seluruh elemen interaktif dan `cursor: not-allowed` pada `:disabled` (`R-39`). |
| **`antislop-layoutmobile`** | 1. Perilaku layar menengah (`640px-1023px`, tablet/laptop kecil) pada tabel operasional dan sidebar perlu ditegaskan agar tidak terjepit.<br>2. Target sentuh tombol gerbang Satpam di ponsel harus >= `44px`. | 1. Menetapkan sidebar tetap sebagai drawer overlay di bawah `1024px`, grid kartu menggunakan `sm:grid-cols-2` di tablet, dan seluruh tabel dibungkus `overflow-x-auto`.<br>2. Mengatur `.btn-lg` (`44px`) untuk aksi utama gerbang Satpam dan `min-h-10` untuk kontrol standar di layar ponsel (`R-03`). |
