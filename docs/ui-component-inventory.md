# Inventaris Komponen UI (Adaptasi JWB-Dashboard-V2)
## Sistem Manajemen Operasional PKDI

Seluruh antarmuka Fase 3 dan Fase selanjutnya dibangun di atas komponen Blade dari `docs/JWB-Dashboard-V2` agar konsisten, mudah dipelihara, dan bebas duplikasi.

| Komponen `JWB-Dashboard-V2` | Lokasi File Target | Penggunaan Spesifik di Sistem PKDI | Penyesuaian yang Dilakukan |
|---|---|---|---|
| `<x-layouts.admin>` | `resources/views/components/layouts/admin.blade.php` | Kerangka seluruh halaman terautentikasi 5 role. | Menampilkan sidebar dinamis berdasarkan role aktif, tombol switcher pratinjau role (khusus Fase 3), lonceng notifikasi per role, dan toggle tema terang/gelap. |
| `<x-layouts.guest>` | `resources/views/components/layouts/guest.blade.php` | Kerangka halaman `/login` dan `/register`. | Identitas visual Marun & Charcoal PKDI, logo bulat `PKDI`, penghapusan tombol SSO/Google yang tidak ada di PRD (`R-26`). |
| `<x-ui.page-header>` | `resources/views/components/ui/page-header.blade.php` | Judul `<h1>` dan deskripsi singkat di setiap halaman. | Dipasangkan dengan tombol aksi kontekstual di sebelah kanan (misalnya `Buat Pesanan`, `Catat Biaya`, `Kembali`). |
| `<x-ui.card>` | `resources/views/components/ui/card.blade.php` | Kontainer utama tabel, form, detail, dan panel aksi. | Menggunakan token border & background PKDI (`bg-card`, `rounded-xl`). |
| `<x-ui.stat-card>` | `resources/views/components/ui/stat-card.blade.php` | Kartu ringkasan angka operasional & keuangan pada dashboard Client, Keuangan, dan Owner. | **Dimodifikasi:** props `delta` dan `direction` dibuat opsional agar tidak memaksa menampilkan persentase perbandingan bila belum ada seri pembanding nyata (`R-17`). |
| `<x-ui.filter-bar>` | `resources/views/components/ui/filter-bar.blade.php` | Pencarian dan penyaringan tabel (pesanan, invoice, biaya operasional, buku kas, audit log). | Mendukung `method="GET"` serta tampilan chip filter aktif. |
| `<x-ui.table-state>` | `resources/views/components/ui/table-state.blade.php` | State `loading`, `empty`, `filtered`, dan `error` untuk setiap tabel operasional. | Disediakan tombol toggle/demo state pada Fase 3 agar seluruh state dapat diuji langsung (`R-27`). |
| `<x-ui.badge>` | `resources/views/components/ui/badge.blade.php` | Label status 17 siklus pesanan, status gerbang, status bayar, dan Surat Jalan. | Ditambahkan helper pemetaan warna semantik (`OrderStatusHelper` / config mapping) agar konsisten di semua role. |
| `<x-ui.detail-list>` | `resources/views/components/ui/detail-list.blade.php` | Ringkasan metadata pesanan, data kendaraan, dan rincian tagihan. | Dipakai berpasangan pada halaman Gudang (Permintaan Awal vs Pemeriksaan Aktual). |
| `<x-ui.timeline>` | `resources/views/components/ui/timeline.blade.php` | Timeline progres pesanan di Portal Client, Detail Gudang, dan Audit Owner. | Menampilkan jam WIB, aktor peran, dan catatan/alasan setiap transisi. |
| `<x-ui.tabs>` | `resources/views/components/ui/tabs.blade.php` | Pemisah antrean pada Dashboard Satpam (Masuk / Dalam Area / Keluar) dan tab detail Keuangan/Gudang. | Mendukung navigasi keyboard Panah Kiri/Kanan (`R-32`). |
| `<x-ui.field>`, `<x-ui.input>`, `<x-ui.textarea>` | `resources/views/components/ui/*.blade.php` | Seluruh formulir (Pesanan, Pemeriksaan Gerbang, Inspeksi Gudang, Biaya, Pengaturan Akun). | **Aturan input:** Kategori Layanan, Jenis Muatan, Jenis Layanan, Jenis Kendaraan, dan Kategori Biaya menggunakan `<x-ui.input>` teks langsung (tanpa dropdown). |
| `<x-ui.file-preview>` | `resources/views/components/ui/file-preview.blade.php` | Unggah lampiran pesanan Client, bukti bayar, dan nota biaya operasional. | Pratinjau nama/ukuran file lokal sebelum kirim. |
| `<x-ui.date-range>` | `resources/views/components/ui/date-range.blade.php` | Filter rentang tanggal pada Riwayat Gerbang, Buku Kas, Laporan Keuangan, dan Dashboard Owner. | Berbasis `flatpickr`. |
| `<x-ui.modal>`, `<x-ui.confirm-action>`, `<x-ui.drawer>` | `resources/views/components/ui/*.blade.php` | Dialog konfirmasi persetujuan, form modal wajib alasan penahanan/penolakan, dan pratinjau dokumen cetak. | Dapat ditutup dengan `Escape`, memiliki fokus trap native `<dialog>`. |
| `<x-ui.chart>` | `resources/views/components/ui/chart.blade.php` | Grafik tren Pemasukan Jasa vs Biaya Operasional pada Dashboard Owner & Keuangan. | Hanya dipakai pada halaman Keuangan & Owner dengan keterangan jelas bahwa data adalah `[DATA DEMO]`. |

### Ekstensi Ikon Lucide SVG Lokal (`public/icons.svg`)
Selain ikon bawaan `JWB-Dashboard-V2`, ditambahkan ikon Lucide SVG berstandar `viewBox="0 0 24 24" stroke-width="2"` untuk kebutuhan operasional pabrik:
- `clipboard-check` (Pemeriksaan Muatan/Gerbang)
- `printer` (Cetak Invoice / Surat Jalan)
- `receipt` (Tagihan / Biaya Operasional)
- `wallet` (Buku Kas / Pembayaran)
- `building` (Profil Perusahaan PT)
- `warehouse` / `box` (Gudang)
- `history` (Riwayat & Jejak Audit)
- `ban` (Tahan / Tolak dengan Alasan)
- `file-spreadsheet` (Ekspor Excel)
