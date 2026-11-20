# Daftar Keputusan Bisnis dan Teknis yang Perlu Divalidasi (Validation Needed)
## Sistem Manajemen Operasional, Layanan Drum, dan Keuangan PT Pulo Kencana Drum Industri (PKDI)

**Versi Dokumen:** 1.0 (Fase 1)
**Sumber Acuan:** [PRD_PKDI_Sistem_Operasional_PT.md](file:///c:/sistem_pkdi/docs/PRD_PKDI_Sistem_Operasional_PT.md) Bagian 18, [Roadmap_Prompt_Antigravity_PKDI.md](file:///c:/sistem_pkdi/docs/Roadmap_Prompt_Antigravity_PKDI.md) Bagian 8

Sesuai aturan proyek dan `antislop`, poin-poin di bawah ini **tidak dikarang atau dikunci secara sepihak oleh AI**. Untuk setiap poin, disediakan **Rancangan Fallback Aman (Default Sementara)** berbasis struktur master data fleksibel agar Fase 2 dan Fase 3 (desain & prototipe UI) dapat berjalan tanpa menghambat alur, sambil menunggu konfirmasi resmi dari Anda / pihak PKDI.

---

## A. Akun, Registrasi, dan Kewenangan Pengguna

| Kode | Pertanyaan / Keputusan yang Perlu Dikonfirmasi | Rancangan Fallback Aman (Sementara) | Status |
|---|---|---|---|
| `VAL-AUTH-01` | **Pembuatan & pengelolaan akun pengguna:** Bagaimana setiap user mengelola akunnya? | **DIKONFIRMASI:** Akun internal disiapkan untuk masing-masing role, dan di setiap akun user (Client, Satpam, Kepala Gudang, Keuangan, Owner) tersedia menu **Pengaturan Akun (Settings)** untuk mengganti nama, email, dan password masing-masing. | `CONFIRMED` |
| `VAL-AUTH-02` | **Jumlah personel Satpam:** Apakah di lapangan hanya ada 1 akun Satpam atau beberapa petugas shift Satpam yang masing-masing membutuhkan akun individual ber-role `satpam`? | Sistem mendukung pembuatan beberapa akun user individual dengan role `satpam` agar setiap petugas shift tercatat namanya sendiri pada `vehicle_gate_logs`. | `NEEDS VALIDATION` |
| `VAL-AUTH-03` | **Verifikasi registrasi Client baru:** Apakah akun Client yang baru mendaftar langsung aktif (`ACTIVE`) dan bisa membuat pesanan pertama, atau harus diverifikasi terlebih dahulu oleh Keuangan/Owner? | Akun Client langsung aktif setelah registrasi untuk kemudahan alur MVP, dengan kolom `verification_status` yang siap diaktifkan bila diperlukan tahap persetujuan. | `NEEDS VALIDATION` |
| `VAL-AUTH-04` | **Batas tindakan Owner:** Apakah Owner murni bersifat pemantau (*read-only*) atas seluruh transaksi dan laporan, atau ada tindakan persetujuan khusus (misalnya persetujuan revisi tagihan bernominal besar atau pengecualian pengeluaran barang)? | Sesuai PRD Bagian 5.5, akses Owner ditetapkan *read-only* pada MVP. | `NEEDS VALIDATION` |

---

## B. Master Data Operasional, Kendaraan, dan Pesanan

| Kode | Pertanyaan / Keputusan yang Perlu Dikonfirmasi | Rancangan Fallback Aman (Sementara) | Status |
|---|---|---|---|
| `VAL-OPS-01` | **Input Kategori Drum & Jenis Muatan:** Apakah menggunakan dropdown atau diketik manual oleh user? | **DIKONFIRMASI:** Semua kolom jenis/kategori (Jenis Kategori, Jenis Muatan, Jenis Layanan, Jenis Kendaraan, Kategori Biaya) **tidak menggunakan dropdown**, melainkan **diketik langsung (input teks) oleh user** agar pencatatan data lebih akurat sesuai kondisi aktual. | `CONFIRMED` |
| `VAL-OPS-02` | **Input Jenis Jasa:** Bagaimana cara pengisian jenis layanan (misalnya diperbarui / diperbaiki)? | **DIKONFIRMASI:** Diketik langsung sebagai input teks oleh user saat pengajuan pesanan maupun saat pencatatan hasil kerja aktual oleh Kepala Gudang. | `CONFIRMED` |
| `VAL-OPS-03` | **Satuan Kuantitas Muatan & Hasil Kerja:** Apakah satuan perhitungan selalu menggunakan `Drum` / `Unit`, atau terdapat satuan berat/volume (`Kg`, `Ton`, `Liter`)? | Default menggunakan satuan `Drum` / `Unit`, namun kolom `unit` disimpan secara eksplisit pada pesanan, inspeksi, hasil kerja, dan tarif. | `NEEDS VALIDATION` |
| `VAL-OPS-04` | **Relasi Pesanan vs Kendaraan:** Apakah 1 nomor pesanan selalu diangkut oleh 1 kendaraan, atau 1 pesanan dapat dikirim bertahap menggunakan beberapa kendaraan berbeda? | Sesuai struktur form PRD Bagian 7.1, 1 pesanan terkait dengan 1 pengiriman kendaraan masuk dan 1 Surat Jalan keluar. | `NEEDS VALIDATION` |
| `VAL-OPS-05` | **Perubahan Kendaraan saat Masuk vs Keluar:** Apakah kendaraan yang menjemput barang keluar (`GATE_OUT`) boleh berbeda nomor polisi/jenisnya dengan kendaraan yang mengantar barang masuk (`GATE_IN`)? | Data kendaraan disimpan terpisah pada `service_orders` (pengajuan), `vehicle_gate_logs` (masuk & keluar), dan `delivery_orders` (Surat Jalan keluar) sehingga jika truk penjemput berbeda, Kepala Gudang dapat mencatat nomor polisi penjemput yang sah pada Surat Jalan disertai catatan. | `NEEDS VALIDATION` |
| `VAL-OPS-06` | **Perlakuan Ketidaksesuaian di Gerbang Masuk:** Jika nomor polisi atau driver berbeda saat tiba di gerbang, apakah Satpam wajib menahan (`GATE_IN_HELD`) hingga pesanan diperbarui, atau Satpam boleh menyetujui masuk sambil mencatat nomor polisi aktual? | Satpam disediakan tombol **Tahan Masuk** (wajib alasan) bila ketidaksesuaian meragukan, serta kolom pencatatan data aktual pada log pemeriksaan. | `NEEDS VALIDATION` |
| `VAL-OPS-07` | **Kebijakan Pembatalan Pesanan oleh Client:** Sampai status apa Client masih diizinkan membatalkan pesanannya sendiri? | Default: Client hanya dapat membatalkan pesanan selama kendaraan belum masuk area pabrik (`SUBMITTED` / `WAITING_GATE_IN`), wajib disertai alasan. Setelah kendaraan masuk, pembatalan hanya dapat dilakukan oleh pihak internal berwenang. | `NEEDS VALIDATION` |

---

## C. Pemeriksaan Gudang, Hasil Pekerjaan, dan Surat Jalan

| Kode | Pertanyaan / Keputusan yang Perlu Dikonfirmasi | Rancangan Fallback Aman (Sementara) | Status |
|---|---|---|---|
| `VAL-WH-01` | **Foto Dokumentasi Kondisi Drum:** Apakah Kepala Gudang perlu mengunggah foto kondisi drum saat pemeriksaan awal atau saat selesai dikerjakan? | Form pemeriksaan dan hasil akhir menyediakan dukungan unggahan lampiran foto opsional. | `NEEDS VALIDATION` |
| `VAL-WH-02` | **Penanganan Drum Tidak Layak / Tidak Selesai (`unprocessed_or_rejected_quantity`):** Jika dari 100 drum yang masuk terdapat 5 drum rusak berat yang tidak dapat diperbaiki, apakah 5 drum tersebut tetap ikut diangkut kembali oleh Client pada Surat Jalan (tanpa ditagih biaya jasa), atau bagaimana perlakuannya? | `service_results` memisahkan `completed_quantity` (yang ditagihkan jasanya) dan `unprocessed_or_rejected_quantity` (tidak selesai/rusak), serta Surat Jalan menampilkan rincian keduanya secara transparan agar jumlah fisik drum yang keluar di gerbang tetap cocok ketika dihitung Satpam. | `NEEDS VALIDATION` |
| `VAL-WH-03` | **Akses Unduh Surat Jalan bagi Client & Fitur QR Code:** Apakah Client dapat mengunduh PDF Surat Jalan dari portalnya, atau Surat Jalan hanya untuk pegangan internal/driver? Apakah diperlukan QR Code pada cetak Surat Jalan? | Surat Jalan ditampilkan status dan nomornya di portal Client; opsi unduh PDF dan kode referensi siap diaktifkan sesuai keputusan PKDI. | `NEEDS VALIDATION` |

---

## D. Tarif, Tagihan (Invoice), Pembayaran, dan Keuangan

| Kode | Pertanyaan / Keputusan yang Perlu Dikonfirmasi | Rancangan Fallback Aman (Sementara) | Status |
|---|---|---|---|
| `VAL-FIN-01` | **Rumus Perhitungan Tarif Jasa:** Apakah harga dihitung murni `kuantitas selesai x tarif satuan` berdasarkan kombinasi (Kategori + Jenis Muatan + Jenis Jasa), atau ada biaya tambahan/komponen khusus? | `invoice_items` mendukung beberapa baris item perhitungan (`kuantitas x harga satuan`) yang diambil dari Master Tarif ditambah kolom `adjustment_amount` dan `tax_amount` pada header invoice. | `NEEDS VALIDATION` |
| `VAL-FIN-02` | **Pajak (PPN) dan Diskon:** Apakah tagihan PKDI mengenakan PPN (misalnya 11%) secara otomatis atau bersifat opsional per invoice? | Disediakan opsi pengisian pajak/penyesuaian pada form pembuatan draft invoice oleh Keuangan (default `0` jika tidak dipilih). | `NEEDS VALIDATION` |
| `VAL-FIN-03` | **Metode Pembayaran & Bukti Bayar:** Apa saja metode pembayaran resmi yang diterima PKDI (Transfer Bank tertentu, Tunai, Giro), dan apakah unggahan bukti bayar oleh Client bersifat wajib sebelum Keuangan dapat memverifikasi? | Mendukung unggahan bukti bayar oleh Client melalui portal maupun pencatatan/unggah bukti langsung oleh Keuangan apabila bukti dikirimkan di luar aplikasi. | `NEEDS VALIDATION` |
| `VAL-FIN-04` | **Kebijakan Pelunasan 100% vs Pengeluaran Barang:** Apakah aturan "barang tidak boleh keluar sebelum lunas 100% (`PAID`)" bersifat mutlak tanpa pengecualian, atau ada skenario piutang berjangka (TOP) untuk Client tertentu? | Sesuai PRD Bagian 3, 4, dan 12 (Aturan #7), sistem **mengunci mutlak** penerbitan Surat Jalan dan persetujuan keluar Satpam hanya untuk pesanan dengan status tagihan `PAID`. | `NEEDS VALIDATION` |
| `VAL-FIN-05` | **Format Resmi Dokumen Cetak Invoice & Surat Jalan:** Bagaimana format penomoran surat, kop surat, alamat pabrik, rekening bank, dan kolom tanda tangan resmi PT Pulo Kencana Drum Industri? | Template cetak PDF disiapkan menggunakan struktur variabel terpusat di konfigurasi sehingga nomor format, alamat, rekening, dan logo dapat langsung disesuaikan begitu contoh dokumen fisik PKDI diberikan. | `NEEDS VALIDATION` |
| `VAL-FIN-06` | **Kategori Biaya Operasional & Saldo Awal Kas:** Apa saja daftar kategori biaya operasional resmi, dan apakah Buku Keuangan cukup menampilkan arus kas masuk/keluar per periode atau membutuhkan input saldo kas awal? | Kategori awal mengikuti PRD Bagian 7.9 (Pembelian Bahan, Perlengkapan, Gaji Karyawan, Transportasi, Pemeliharaan, Utilitas, Lainnya) dan Buku Keuangan menyajikan total kas masuk, total kas keluar, serta selisih kas bersih per periode berdasarkan tanggal transaksi. | `NEEDS VALIDATION` |

---

## E. Identitas Visual, Brand, dan Fondasi Teknis (Untuk Fase 2 & Fase 3)

| Kode | Pertanyaan / Keputusan yang Perlu Dikonfirmasi | Rancangan Fallback Aman (Sementara) | Status |
|---|---|---|---|
| `VAL-DES-01` | **Logo & Identitas Visual PKDI:** Bagaimana tampilan logo jika aset gambar belum ada? | **DIKONFIRMASI:** Menggunakan teks **PKDI** di dalam elemen berlatar belakang bulat (*circular badge/mark*) dengan palet warna identitas Marun dan Hitam/Charcoal yang lolos uji kontras WCAG AA. | `CONFIRMED` |
| `VAL-DES-02` | **Tagline Perusahaan:** Apakah ada tagline khusus? | **DIKONFIRMASI:** Belum ada tagline khusus; cukup menampilkan identitas teks **PKDI / PT Pulo Kencana Drum Industri** dengan aksen warna brand yang jelas. | `CONFIRMED` |
| `VAL-TEC-01` | **Strategi Penempatan Proyek Laravel 13 vs `docs/JWB-Dashboard-V2`:** Apakah aplikasi utama akan diinisialisasi di root `c:\sistem_pkdi` dengan mengadaptasi komponen Blade/Tailwind dari `docs/JWB-Dashboard-V2` (sehingga folder `docs/JWB-Dashboard-V2` tetap utuh sebagai referensi), atau memindahkan isi `JWB-Dashboard-V2` langsung ke root? | Direkomendasikan membangun aplikasi di root `c:\sistem_pkdi` dengan menyalin/mengadaptasi komponen `<x-ui.*>` dan `<x-layouts.*>` dari `docs/JWB-Dashboard-V2` agar arsitektur bersih dan sesuai target Laravel 13. | `NEEDS VALIDATION` |
