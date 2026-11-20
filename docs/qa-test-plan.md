# Rencana Pengujian QA (QA Test Plan)
## Sistem Manajemen Operasional, Layanan Drum, dan Keuangan PT Pulo Kencana Drum Industri (PKDI)

**Versi Dokumen:** 1.0 (Fase 7)
**Lingkup:** Pengujian End-to-End 11 Tahap, Matriks Otorisasi 5 Peran, Validasi Data & Rekonsiliasi Keuangan, Responsivitas Multi-Breakpoint, Aksesibilitas WCAG 2.2 AA, dan Antislop Delivery Gate.

---

## 1. Strategi dan Lingkungan Pengujian

1. **Lingkungan Eksekusi:**
   - **Runtime:** PHP `8.4.27` (CLI & Apache XAMPP) dengan Laravel `v13.42.0`.
   - **Database Development:** MySQL / MariaDB `10.11.16` (XAMPP, database `sistem_pkdi`).
   - **Database Test Otomatis:** PHPUnit Feature & Unit Test (`RefreshDatabase`) + verifikasi migrasi dan seeding langsung pada MySQL XAMPP (`sistem_pkdi`).
   - **Frontend Build:** Vite `7.3.8` + Tailwind CSS v4 + `@fontsource/inter`.
2. **Urutan Audit & Perbaikan Berdasarkan Severity:**
   - Sebelum pengujian akhir dikunci, dilakukan audit kode menyeluruh untuk menemukan potensi bug berdasarkan tingkat keparahan (*High*, *Medium*, *Low*), memperbaiki temuan yang jelas di dalam lingkup MVP, dan mendokumentasikan keputusan bisnis yang masih menunggu validasi PKDI.

---

## 2. Daftar Skenario Uji

### A. Skenario End-to-End Lintas 5 Peran (`TC-E2E-01` s.d. `TC-E2E-11`)

| Kode Uji | Peran | Tahapan yang Diuji | Hasil yang Diharapkan |
|---|---|---|---|
| `TC-E2E-01` | `client` (Publik) | Registrasi mandiri akun perusahaan baru (`POST /register`) | Record `client_companies` & `users` (`role = client`) tersimpan, password di-hash, audit log `CLIENT_REGISTERED` tercatat, diarahkan ke `/client/dashboard`. |
| `TC-E2E-02` | `client` | Pengajuan pesanan jasa drum (`POST /client/orders`) | Nomor pesanan unik `hari-DD/MM/YY-N` terbentuk, status `WAITING_GATE_IN` / `EXPECTED_IN`, tampil di antrean Satpam & Owner. |
| `TC-E2E-03` | `satpam` | Pemeriksaan kendaraan masuk (`POST /gate/check-in/{code}`) | Menolak `APPROVED` bila nopol berbeda tanpa catatan verifikasi; mewajibkan alasan saat `HELD`; memindahkan ke `WAITING_WAREHOUSE` (`CHECKED_IN_APPROVED`) saat disetujui. |
| `TC-E2E-04` | `kepala_gudang` | Pemeriksaan muatan, progres, & hasil akhir (`POST /warehouse/orders/{code}/inspect`, `/processes`, `/result`) | Menolak aksi sebelum `CHECKED_IN_APPROVED`; menyimpan data aktual terpisah dari permintaan awal Client; memindahkan ke `WAITING_FINANCE` setelah selesai. |
| `TC-E2E-05` | `client` | Pemantauan timeline & status dari database (`GET /client/orders/{code}` & `/status-heartbeat`) | Menampilkan kuantitas aktual gudang, progres pengerjaan, dan status terbaru dari database. |
| `TC-E2E-06` | `keuangan` | Perhitungan biaya dari hasil kerja gudang & penerbitan Invoice (`POST /finance/billable-orders/{code}/invoices`) | Menolak pembuatan invoice jika pekerjaan gudang belum selesai; mendukung simpan `DRAFT` dan terbit `ISSUED` (`INVOICE_ISSUED`). |
| `TC-E2E-07` | `client` | Melihat & mengunduh PDF tagihan serta mengunggah bukti bayar (`GET /invoices/{code}/download-pdf`, `POST /client/invoices/{code}/payments`) | PDF terunduh (`%PDF-1.4`); bukti pembayaran tersimpan sebagai `PENDING_VERIFICATION` tanpa langsung mengubah status menjadi `PAID`. |
| `TC-E2E-08` | `keuangan` | Verifikasi pembayaran (`POST /finance/invoices/{code}/payments`) | Mengubah status pembayaran menjadi `VERIFIED`, membuat tepat 1 entri `IN` di `cashbook_entries`, dan mengubah pesanan ke `PAID` / `WAITING_EXIT_DOCUMENT`. |
| `TC-E2E-09` | `kepala_gudang` | Penerbitan Surat Jalan (`POST /warehouse/delivery-orders/{code}`) | Ditolak bila belum `PAID`; menerbitkan `SJ/PKDI/...` (`ISSUED`) dan mengubah status ke `WAITING_GATE_OUT`. |
| `TC-E2E-10` | `satpam` | Pemeriksaan Surat Jalan & persetujuan keluar (`POST /gate/check-out/{code}`) | Ditolak bila belum `PAID` atau Surat Jalan tidak valid/sudah terpakai; menandai Surat Jalan `USED_AT_GATE_OUT` dan pesanan `COMPLETED`. |
| `TC-E2E-11` | `owner` | Pemantauan transaksi lintas bagian, laporan keuangan, & jejak audit (`GET /owner/*`) | Menampilkan rekap pendapatan, pembayaran, biaya operasional, dan audit trail yang konsisten secara *read-only*. |

---

### B. Skenario Uji Otorisasi & Keamanan (`TC-SEC-01` s.d. `TC-SEC-06`)
- **`TC-SEC-01` (Matriks Role-Route):** Setiap role (`client`, `satpam`, `kepala_gudang`, `keuangan`, `owner`) mencoba mengakses route GET dan POST milik role lain -> wajib ditolak (`403 Forbidden`).
- **`TC-SEC-02` (Isolasi Tenant / Anti-IDOR):** Client A mencoba mengakses detail pesanan, detail invoice, unggah bukti bayar, unduh PDF invoice, dan heartbeat milik Client B melalui manipulasi slug maupun ID numerik -> wajib ditolak (`403 Forbidden`).
- **`TC-SEC-03` (Eskalasi Hak Akses / Mass Assignment):** Mengirim parameter `role=owner` atau `is_active=0` pada form registrasi dan update profil -> diabaikan oleh server.
- **`TC-SEC-04` (Perlindungan Dokumen Draft & Ekspor):** Client mencoba membuka/mengunduh invoice berstatus `DRAFT` atau mengakses `/finance/reports/export` -> ditolak (`403 Forbidden`).
- **`TC-SEC-05` (Penanganan Resource Tidak Ditemukan):** Mengakses kode pesanan atau invoice fiktif (`/gate/check-in/tidak-ada-999`, `/finance/invoices/INV-TIDAK-ADA-999`) -> mengembalikan `404 Not Found`.
- **`TC-SEC-06` (Rate Limiting & Akun Nonaktif):** Percobaan login salah lebih dari 5 kali per menit dibatasi oleh `RateLimiter`, dan akun `is_active = false` otomatis dikeluarkan oleh middleware `EnsureUserIsActive`.

---

### C. Skenario Uji Data, Edge Cases, & Rekonsiliasi Keuangan (`TC-FIN-01` s.d. `TC-FIN-05`)
- **`TC-FIN-01`:** Input kosong dan format email/tanggal tidak valid ditolak dengan pesan Bahasa Indonesia yang spesifik.
- **`TC-FIN-02`:** Nilai negatif (`-5`, `-50000`), kuantitas nol (`0`), dan nominal ekstrem (`9999999999999`) ditolak dengan `422` tanpa menyebabkan error SQL overflow.
- **`TC-FIN-03`:** Pembayaran melebihi sisa tagihan (*overpayment* > `balance_due`) ditolak dengan pesan validasi.
- **`TC-FIN-04`:** Pengiriman bukti bayar ganda dengan nomor referensi yang sama atau verifikasi ulang pada pembayaran yang sudah `VERIFIED` ditolak; constraint `UNIQUE(payment_id)` dan `UNIQUE(operational_expense_id)` mencegah duplikasi baris Buku Kas.
- **`TC-FIN-05`:** Total Kas Masuk (`IN`) pada `cashbook_entries` sama persis dengan jumlah `payments` berstatus `VERIFIED`, dan Total Kas Keluar (`OUT`) sama persis dengan jumlah `operational_expenses` berstatus `RECORDED`.

---

### D. Skenario Uji Responsivitas, Aksesibilitas, & Antislop Delivery Gate (`TC-UX-01` s.d. `TC-UX-05`)
- **`TC-UX-01`:** Pemeriksaan 5 rentang viewport: Ponsel Kecil (`360px`), Ponsel Besar (`412px`), Tablet (`768px`), Laptop (`1024px`), dan Desktop (`1440px`).
- **`TC-UX-02`:** Navigasi keyboard (`Tab`, `Shift+Tab`, `Enter`, `Escape` pada `<dialog>`), skip-link `#page-root`, dan indikator `:focus-visible`.
- **`TC-UX-03`:** Rasio kontras warna Mode Terang & Mode Gelap menggunakan `contrast-check.py` (WCAG AA `>= 4.5:1` teks normal, `>= 3.0:1` batas kontrol interaktif).
- **`TC-UX-04`:** Ketersediaan state `loading`, `empty`, `filtered`, `error`, `disabled`, serta dukungan `@media (prefers-reduced-motion: reduce)`.
- **`TC-UX-05`:** Pemeriksaan penuh 4 blok **Antislop Delivery Gate** (Hard Gate, Purpose-Gate, Liveliness, Craftsmanship & Quality Locks).
