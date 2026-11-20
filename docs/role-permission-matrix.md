# Matriks Peran dan Hak Akses (Role-Permission Matrix)
## Sistem Manajemen Operasional, Layanan Drum, dan Keuangan PT Pulo Kencana Drum Industri (PKDI)

**Versi Dokumen:** 1.0 (Fase 1)
**Sumber Acuan:** [PRD_PKDI_Sistem_Operasional_PT.md](file:///c:/sistem_pkdi/docs/PRD_PKDI_Sistem_Operasional_PT.md) Bagian 5 & 12, [Roadmap_Prompt_Antigravity_PKDI.md](file:///c:/sistem_pkdi/docs/Roadmap_Prompt_Antigravity_PKDI.md) Bagian 1.1 & 2

---

## 1. Prinsip Dasar Otorisasi

1. **Lima Kategori Peran Resmi:** Sistem hanya mengenal 5 role berikut:
   - `client` (Perusahaan pelanggan eksternal)
   - `satpam` (Petugas pemeriksaan pos gerbang masuk dan keluar)
   - `kepala_gudang` (Pengelola pemeriksaan muatan, proses pengerjaan drum, hasil jasa, dan Surat Jalan)
   - `keuangan` (Pengelola tarif, tagihan/invoice, verifikasi pembayaran, buku kas, biaya operasional, dan laporan)
   - `owner` (Pemilik perusahaan, pemantauan menyeluruh lintas bagian)
2. **Tanpa Role Admin Tambahan:** Sesuai instruksi proyek, tidak ada penambahan role `admin` atau `superadmin` di luar kelima kategori di atas tanpa keputusan tertulis dari PKDI.
3. **Prinsip *Default Deny*:** Setiap route dan aksi controller ditolak secara default kecuali role pengguna secara eksplisit diberi izin pada matriks ini dan lolos pemeriksaan Policy di sisi server.
4. **Isolasi Tenant Client (Multi-Company Isolation):** Untuk role `client`, izin **Lihat**, **Tambah**, **Ubah**, dan **Cetak/Unduh** hanya berlaku untuk baris data yang memiliki `client_company_id` yang sama dengan akun Client tersebut.
5. **Larangan Hapus Permanen (*No Hard Delete*):** Transaksi operasional dan keuangan (pesanan, log gerbang, pemeriksaan gudang, invoice, pembayaran, surat jalan, biaya operasional, buku kas, dan audit log) **tidak boleh dihapus permanen** oleh role mana pun. Pembatalan atau koreksi dilakukan melalui perubahan status (`CANCELLED` / `VOIDED` / `REVISED`) disertai alasan wajib.

---

## 2. Keterangan Kode Izin pada Matriks

- **L (Lihat / Read):** Membuka daftar dan melihat detail data.
- **T (Tambah / Create):** Membuat entri atau pengajuan baru.
- **U (Ubah / Update / Revisi):** Memperbarui data draft atau melakukan revisi tercatat sesuai tahapan yang diizinkan.
- **S (Setujui / Tahan / Tolak):** Mengambil keputusan operasional (persetujuan gerbang, persetujuan muatan gudang, penahanan dengan alasan).
- **V (Verifikasi):** Memverifikasi keabsahan pembayaran atau syarat pelepasan transaksi.
- **C (Cetak / Terbitkan Dokumen):** Menerbitkan dokumen resmi bernomor unik atau mengunduh PDF dokumen (Invoice / Surat Jalan).
- **E (Ekspor Laporan):** Mengekspor rekapitulasi data tabel ke berkas PDF atau Excel.
- **H (Hapus / Nonaktifkan):** Menghapus permanen atau menonaktifkan entitas (dibatasi ketat hanya untuk penonaktifan master data/akun, bukan penghapusan transaksi).
- **`-` (Tidak Diizinkan):** Ditolak oleh server (`403 Forbidden`).

---

## 3. Tabel Matriks Izin per Modul dan Entitas

| Modul / Sumber Daya | `client` | `satpam` | `kepala_gudang` | `keuangan` | `owner` | Aturan Pembatasan & Guard Server |
|---|---|---|---|---|---|---|
| **1. Registrasi & Profil Perusahaan Client** | | | | | | |
| Registrasi akun Client baru | **T** (Publik) | - | - | - | - | Hanya untuk pendaftaran akun `client`. |
| Profil PT Client (`client_companies`) | **L, U** (Milik sendiri) | **L** (Nama PT pada pesanan) | **L** (Nama PT & kontak operasional) | **L** (Detail penagihan PT) | **L** (Semua PT) | Client hanya dapat melihat dan mengubah profil PT miliknya sendiri. |
| **2. Manajemen Akun Pengguna (`users`)** | | | | | | |
| Profil & kata sandi akun sendiri | **L, U** | **L, U** | **L, U** | **L, U** | **L, U** | Pengguna tidak dapat mengubah kolom `role` miliknya sendiri. |
| Pengelolaan akun internal & status akun | - | - | - | - | **L** (`NEEDS VALIDATION` untuk **T, U, H**) | Mekanisme pembuatan/penonaktifan akun internal menunggu konfirmasi PKDI (`validation-needed.md`). |
| **3. Master Data Operasional & Tarif** | | | | | | |
| Master Kategori, Jenis Muatan, Jenis Jasa | **L** (Saat buat pesanan) | **L** | **L** (`NEEDS VALIDATION` untuk **T, U**) | **L** | **L** | Pengelola master kategori/muatan dikonfirmasi pada `validation-needed.md`. |
| Master Tarif Jasa (`service_tariffs`) | - | - | - | **L, T, U** | **L** | Satpam, Kepala Gudang, dan Client tidak memiliki akses ke tabel master tarif internal. |
| **4. Pesanan Layanan Drum (`service_orders`)** | | | | | | |
| Pengajuan pesanan baru | **T** (Milik sendiri) | - | - | - | - | Menghasilkan nomor pesanan unik & status `WAITING_GATE_IN`. |
| Daftar & detail pesanan | **L** (Milik sendiri) | **L** (Data gerbang & kendaraan) | **L** (Data muatan & proses jasa) | **L** (Data hasil jasa & tagihan) | **L, E** (Semua pesanan) | Satpam hanya melihat informasi identitas pesanan, PT, kendaraan, driver, dan status (tanpa rincian nominal uang). |
| Timeline riwayat status pesanan | **L** (Milik sendiri) | **L** (Terkait gerbang) | **L** | **L** | **L** | Client melihat tahapan status resmi dan catatan yang dibagikan untuk eksternal. |
| Permohonan pembatalan pesanan | **U** (`Could`, milik sendiri) | - | **S** (Tolak saat inspeksi) | - | **L** | Pembatalan wajib mencantumkan alasan dan hanya sebelum batas tahapan yang disepakati. |
| **5. Pemeriksaan Pos Gerbang (`vehicle_gate_logs`)** | | | | | | |
| Pemeriksaan kendaraan masuk (`Gate In`) | **L** (Status di timeline sendiri) | **L, T, S** (Setujui / Tahan Masuk) | **L** | **L** (Status) | **L, E** | Hanya `satpam` yang dapat mencatat dan menyetujui/menahan kendaraan masuk. Alasan wajib diisi bila ditahan. |
| Pemeriksaan kendaraan keluar (`Gate Out`) | **L** (Status di timeline sendiri) | **L, T, S** (Setujui / Tahan Keluar) | **L** | **L** (Status) | **L, E** | Server memblokir aksi Setujui Keluar oleh `satpam` jika tagihan belum `PAID` atau Surat Jalan belum `ISSUED`/valid. |
| **6. Pemeriksaan & Pekerjaan Gudang** | | | | | | |
| Pemeriksaan muatan aktual (`warehouse_inspections`) | **L** (Ringkasan hasil milik sendiri) | - | **L, T, U, S** (Setujui / Klarifikasi / Tolak) | **L** (Acuan tagihan) | **L, E** | Hanya dapat diinput `kepala_gudang` setelah kendaraan disetujui masuk oleh `satpam`. Tidak menimpa data awal pesanan Client. |
| Progres pengerjaan (`service_processes`) | **L** (Timeline sendiri) | - | **L, T, U** | **L** | **L** | Hanya dapat diperbarui oleh `kepala_gudang` saat pesanan berstatus `IN_PROGRESS`. |
| Hasil akhir pekerjaan (`service_results`) | **L** (Milik sendiri) | - | **L, T, U** (Kirim ke Keuangan) | **L** (Dasar perhitungan) | **L, E** | Setelah dikirim ke Keuangan (`WAITING_FINANCE`), perubahan oleh `kepala_gudang` dikunci kecuali melalui alur revisi. |
| **7. Tagihan & Pembayaran (`invoices`, `payments`)** | | | | | | |
| Pembuatan draft & perhitungan tagihan | - | - | - | **L, T, U** | **L** | Hanya dari pesanan yang telah memiliki `service_results` selesai dari Kepala Gudang. |
| Penerbitan & revisi Invoice | **L, C** (Unduh PDF milik sendiri) | - | **L** (Hanya indikator status `PAID` / belum) | **L, T, U, C, E** (Terbitkan, Revisi + alasan, Cetak PDF) | **L, C, E** | `kepala_gudang` dan `satpam` hanya melihat status pelunasan (`PAID` atau belum), tidak mengelola nominal tagihan. |
| Unggah bukti pembayaran | **T, L** (Milik sendiri) | - | - | **L, T** | **L** | Unggahan bukti oleh Client berstatus `PENDING_VERIFICATION` dan belum mengubah tagihan menjadi lunas. |
| Verifikasi pembayaran & status lunas | **L** (Status milik sendiri) | **L** (Indikator lunas pada Surat Jalan) | **L** (Indikator lunas untuk syarat Surat Jalan) | **L, V, U** (Verifikasi penuh/sebagian/tolak + alasan) | **L, E** | Hanya `keuangan` yang berhak memverifikasi pembayaran dan mengubah status tagihan menjadi `PAID`. |
| **8. Surat Jalan (`delivery_orders`)** | | | | | | |
| Pembuatan & penerbitan Surat Jalan | **L** (`C` unduh PDF sesuai kebijakan PKDI) | **L, V** (Verifikasi saat keluar) | **L, T, C** (Buat & terbitkan Surat Jalan) | **L** | **L, C, E** | Server menolak pembuatan Surat Jalan oleh `kepala_gudang` apabila tagihan pesanan belum berstatus `PAID`. |
| **9. Buku Keuangan & Biaya Operasional** | | | | | | |
| Biaya operasional (`operational_expenses`) | - | - | - | **L, T, U, E** (Catat biaya + unggah bukti, koreksi + alasan) | **L, E** | Sama sekali tidak dapat diakses oleh `client`, `satpam`, dan `kepala_gudang`. |
| Buku kas masuk & keluar (`cashbook_entries`) | - | - | - | **L, E** (Dibuat otomatis dari pembayaran & biaya) | **L, E** | Entri kas masuk dibuat otomatis saat pembayaran diverifikasi; kas keluar dibuat otomatis saat biaya operasional dicatat. |
| Laporan keuangan & operasional (PDF/Excel) | - (Hanya unduh Invoice sendiri) | - | - | **L, C, E** (Laporan keuangan) | **L, C, E** (Laporan operasional & keuangan) | Ekspor mengikuti filter aktif dan memvalidasi role pengguna di server. |
| **10. Audit Log & Notifikasi** | | | | | | |
| Notifikasi aplikasi (`notifications`) | **L, U** (Tandai baca milik sendiri) | **L, U** (Milik sendiri) | **L, U** (Milik sendiri) | **L, U** (Milik sendiri) | **L, U** (Milik sendiri) | Setiap pengguna hanya melihat notifikasi yang ditujukan untuk akun/perannya. |
| Jejak audit (`audit_logs`) | - | - | - | **L** (Riwayat revisi transaksi keuangan) | **L, E** (Seluruh jejak audit sistem) | Bersifat *append-only*; tidak ada role (termasuk `owner`) yang dapat mengubah atau menghapus jejak audit. |

---

## 4. Ringkasan Pemisahan Kewenangan Kritis (Segregation of Duties)

Untuk mencegah kecurangan atau kesalahan operasional lintas bagian, sistem menegakkan lima pemisahan kewenangan berikut di tingkat database dan aplikasi:

1. **Client vs Internal:** Client hanya dapat membuat pesanan awal dan mengunggah bukti bayar untuk perusahaannya sendiri; Client tidak dapat menyetujui pemeriksaan, mengubah kuantitas hasil kerja, atau menandai tagihan lunas.
2. **Satpam vs Gudang & Keuangan:** Satpam adalah penjaga gerbang fisik (masuk dan keluar). Satpam tidak dapat mengubah data muatan gudang, tidak dapat membuat Surat Jalan, dan tidak dapat memverifikasi pembayaran.
3. **Kepala Gudang vs Keuangan:** Kepala Gudang menentukan kuantitas hasil kerja aktual dan menerbitkan Surat Jalan, tetapi tidak dapat menentukan tarif harga, tidak dapat menandai tagihan menjadi `PAID`, dan tidak dapat menerbitkan Surat Jalan sebelum Keuangan memverifikasi status `PAID`.
4. **Keuangan vs Gudang & Gerbang:** Keuangan mengelola tarif, tagihan, verifikasi pembayaran, dan biaya operasional, tetapi tidak dapat mengubah kuantitas hasil pemeriksaan Kepala Gudang dan tidak dapat menyetujui kendaraan masuk/keluar maupun menerbitkan Surat Jalan.
5. **Owner sebagai Pemantau (*Read-Only Oversight*):** Pada lingkup MVP, Owner dapat melihat seluruh data, dokumen, laporan, dan jejak audit dari ke-4 bagian lainnya tanpa mengintervensi langsung status operasional harian dan tanpa hak menghapus jejak transaksi.
