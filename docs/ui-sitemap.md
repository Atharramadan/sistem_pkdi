# Peta Situs dan Navigasi per Peran (UI Sitemap)
## Sistem Manajemen Operasional PKDI

Dokumen ini mendefinisikan struktur halaman dan menu navigasi terpisah untuk masing-masing dari 5 peran pengguna ditambah halaman publik/autentikasi. **Tidak ada menu yang tidak memiliki halaman tujuan (`R-24`).**

---

## 1. Halaman Publik & Autentikasi
- `GET /` : Pengalihan ke `/login` atau halaman portal sesuai peran yang sedang aktif pada sesi pratinjau.
- `GET /login` : Halaman masuk terpusat (mendukung seluruh peran: Client, Satpam, Kepala Gudang, Keuangan, Owner) + panel akses cepat pratinjau peran pada Fase 3.
- `GET /register` : Halaman registrasi akun mandiri khusus **Client** (nama PT, alamat, PIC, email, kontak, password).
- `GET /settings` : Halaman **Pengaturan Akun** yang tersedia untuk **semua 5 peran** (ganti Nama, Email, dan Kata Sandi).

---

## 2. Navigasi & Sitemap per Peran

### A. Portal Client (`/client/*`)
- **Sidebar Navigasi:**
  1. `Ringkasan Portal` (`/client/dashboard`)
  2. `Pesanan Saya` (`/client/orders`)
  3. `Buat Pesanan Baru` (`/client/orders/create`)
  4. `Tagihan & Pembayaran` (`/client/invoices`)
  5. `Profil Perusahaan` (`/client/company`)
  6. `Pengaturan Akun` (`/settings`)
- **Halaman Detail:**
  - `/client/orders/{code}` : Detail Pesanan & Timeline Status Kronologis
  - `/client/invoices/{code}` : Rincian Tagihan, Status Pembayaran, Unduh PDF (Pratinjau), & Form Unggah Bukti Bayar

### B. Pos Gerbang Satpam (`/gate/*`)
- **Sidebar Navigasi:**
  1. `Pos Gerbang (Antrean)` (`/gate/dashboard`)
  2. `Pemeriksaan Masuk` (`/gate/check-in`)
  3. `Pemeriksaan Keluar (Surat Jalan)` (`/gate/check-out`)
  4. `Riwayat Gerbang` (`/gate/history`)
  5. `Pengaturan Akun` (`/settings`)
- **Halaman Detail:**
  - `/gate/check-in/{code}` : Detail Verifikasi Kendaraan Masuk (Setujui / Tahan Masuk)
  - `/gate/check-out/{code}` : Detail Verifikasi Surat Jalan & Kendaraan Keluar (Blokir jika belum Lunas / Setujui / Tahan Keluar)

### C. Kepala Gudang (`/warehouse/*`)
- **Sidebar Navigasi:**
  1. `Antrean Gudang` (`/warehouse/dashboard`)
  2. `Pemeriksaan Muatan` (`/warehouse/inspections`)
  3. `Proses & Hasil Kerja` (`/warehouse/processes`)
  4. `Surat Jalan` (`/warehouse/delivery-orders`)
  5. `Pengaturan Akun` (`/settings`)
- **Halaman Detail:**
  - `/warehouse/orders/{code}` : Detail Pesanan, Form Pemeriksaan Muatan Aktual (Setujui / Klarifikasi / Tolak), Pembaruan Progres, & Form Hasil Akhir Pekerjaan
  - `/warehouse/delivery-orders/{code}` : Penerbitan & Pratinjau Surat Jalan (dengan validasi lunas)

### D. Keuangan (`/finance/*`)
- **Sidebar Navigasi:**
  1. `Ringkasan Keuangan` (`/finance/dashboard`)
  2. `Antrean Hitung Tagihan` (`/finance/billable-orders`)
  3. `Daftar Invoice & Verifikasi Bayar` (`/finance/invoices`)
  4. `Master Tarif Jasa` (`/finance/tariffs`)
  5. `Biaya Operasional` (`/finance/expenses`)
  6. `Buku Transaksi Keuangan` (`/finance/cashbook`)
  7. `Laporan & Ekspor` (`/finance/reports`)
  8. `Pengaturan Akun` (`/settings`)
- **Halaman Detail:**
  - `/finance/billable-orders/{code}` : Form Perhitungan Biaya Jasa & Simpan Draft / Terbitkan Tagihan
  - `/finance/invoices/{code}` : Detail Invoice, Revisi Tagihan, & Verifikasi Bukti Pembayaran

### E. Owner (`/owner/*`)
- **Sidebar Navigasi:**
  1. `Ringkasan Eksekutif` (`/owner/dashboard`)
  2. `Pendapatan & Biaya` (`/owner/financial-summary`)
  3. `Transaksi Lintas Bagian` (`/owner/transactions`)
  4. `Jejak Audit Sistem` (`/owner/audit-logs`)
  5. `Pengaturan Akun` (`/settings`)
- **Halaman Detail:**
  - `/owner/transactions/{code}` : Detail Lengkap Pesanan, Log Gerbang, Hasil Gudang, Invoice, Pembayaran, Surat Jalan, & Riwayat Status (*read-only*)
