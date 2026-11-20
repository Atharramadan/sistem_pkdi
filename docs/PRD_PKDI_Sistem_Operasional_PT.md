# PRODUCT REQUIREMENTS DOCUMENT (PRD)

# Sistem Manajemen Operasional, Layanan Drum, dan Keuangan PT Pulo Kencana Drum Industri (PKDI)

| Informasi | Keterangan |
|---|---|
| Nama Produk | Sistem Manajemen Operasional PKDI |
| Jenis Produk | Aplikasi web internal perusahaan dengan portal client |
| Organisasi | PT Pulo Kencana Drum Industri (PKDI) |
| Versi Dokumen | 1.0 — Draft kebutuhan awal |
| Status | Untuk validasi proses bisnis sebelum desain UI/UX dan development |
| Pengguna Internal | Satpam, Kepala Gudang, Keuangan, dan Owner (masing-masing memiliki akun sendiri) |
| Pengguna Eksternal | Client / perusahaan pelanggan (dapat mendaftar dan membuat akun sendiri) |

---

## 1. Ringkasan Produk

Sistem Manajemen Operasional PKDI adalah aplikasi web terpusat untuk mengelola permintaan layanan drum dari client, pemeriksaan kendaraan di gerbang, pemeriksaan dan pengerjaan barang oleh kepala gudang, perhitungan biaya jasa, penagihan, pencatatan pembayaran, pengeluaran barang, serta pencatatan biaya operasional perusahaan.

Client dapat membuat akun sendiri, mengajukan pesanan layanan, melihat perkembangan proses secara real-time, serta melihat dan mengunduh tagihan. Di sisi internal, setiap bagian memiliki dashboard dan daftar pekerjaan sesuai tanggung jawabnya. Satpam mencatat kendaraan masuk dan keluar, Kepala Gudang mengelola pemeriksaan serta hasil pengerjaan barang, Keuangan mengelola tagihan, pembayaran, buku keuangan, dan biaya operasional, sedangkan Owner dapat memantau keseluruhan aktivitas operasional dan kondisi keuangan perusahaan.

Sistem menggunakan satu sumber data terpusat agar status pesanan, aktivitas kendaraan, hasil pekerjaan, tagihan, pembayaran, surat jalan, dan biaya operasional saling terhubung serta memiliki riwayat perubahan.

## 2. Latar Belakang dan Masalah

Proses layanan dan administrasi perusahaan membutuhkan koordinasi lintas bagian. Tanpa pencatatan terpusat, informasi pesanan, posisi barang, hasil pengerjaan, status pembayaran, dan kendaraan yang keluar berisiko tersebar di beberapa catatan atau komunikasi terpisah.

Permasalahan yang ingin diselesaikan:

1. Client belum memiliki satu tempat untuk mengajukan pesanan dan memantau status layanannya.
2. Informasi kendaraan masuk dan keluar perlu dicatat serta dapat ditelusuri oleh petugas Satpam.
3. Kepala Gudang membutuhkan daftar pesanan, rincian muatan, jenis layanan yang diminta, pemeriksaan, dan hasil pengerjaan.
4. Keuangan membutuhkan data hasil jasa sebagai dasar perhitungan tagihan, pencatatan pembayaran, dan rekap keuangan.
5. Pengeluaran barang harus melalui verifikasi pembayaran dan surat jalan sebelum disetujui Satpam.
6. Owner membutuhkan ringkasan operasional, pendapatan jasa, pembayaran, dan biaya operasional dalam satu dashboard.
7. Setiap perubahan status dan transaksi perlu memiliki waktu, pengguna yang melakukan tindakan, serta riwayat yang dapat diaudit.

## 3. Tujuan Produk

- Mendigitalisasi alur layanan dari pengajuan client hingga barang keluar dari area perusahaan.
- Menghubungkan proses gerbang, gudang, dan keuangan dalam satu alur kerja.
- Memberikan informasi status layanan yang mudah dipantau oleh client dan bagian internal.
- Memastikan barang tidak dapat keluar sebelum pembayaran dinyatakan lunas dan surat jalan diterbitkan.
- Menyediakan pencatatan pendapatan jasa serta biaya operasional perusahaan.
- Menyediakan laporan yang dapat difilter dan diekspor ke PDF maupun Excel.
- Memberikan Owner gambaran menyeluruh atas transaksi dan aktivitas perusahaan.

## 4. Indikator Keberhasilan

1. Setiap pesanan client memiliki nomor pesanan unik dan dapat dilacak sampai selesai.
2. Setiap kendaraan yang masuk atau keluar tercatat dengan waktu dan akun Satpam yang melakukan pemeriksaan.
3. Setiap pekerjaan gudang memiliki catatan pemeriksaan, status proses, dan hasil akhir.
4. Setiap tagihan berasal dari hasil jasa yang dicatat Kepala Gudang dan memiliki rincian perhitungan.
5. Sistem menolak proses persetujuan keluar apabila pembayaran belum lunas atau surat jalan belum valid.
6. Pendapatan, pembayaran, dan biaya operasional dapat direkap berdasarkan periode.
7. Client hanya dapat mengakses pesanan dan dokumen milik perusahaannya sendiri.
8. Owner dapat melihat ringkasan lintas bagian tanpa harus meminta rekap manual dari setiap bagian.

## 5. Pengguna, Peran, dan Hak Akses

Sistem memiliki lima jenis peran pengguna (total 5 jenis akun) dengan akun masing-masing sesuai tanggung jawabnya: 4 akun internal (Satpam, Kepala Gudang, Keuangan, dan Owner) serta akun eksternal Client yang dapat mendaftar sendiri. Setiap akun bersifat individual dan tidak boleh dipakai bersama agar riwayat tindakan dapat ditelusuri.

### 5.1 Client

Client adalah perusahaan/pelanggan yang menggunakan layanan PKDI.

Hak akses:
- Membuat akun client sendiri.
- Melengkapi profil perusahaan dan informasi kontak.
- Membuat permintaan layanan baru.
- Melihat daftar pesanan milik perusahaannya sendiri.
- Melihat status dan riwayat proses pesanan secara real-time.
- Melihat rincian hasil jasa dan tagihan.
- Mengunduh invoice/tagihan dan dokumen yang memang dibagikan kepada client.
- Melihat status pembayaran.
- Mengirim bukti pembayaran, apabila metode pembayaran manual ini disetujui sebagai proses bisnis PKDI.
- Melihat notifikasi perubahan status pesanan dan tagihan.

Client tidak dapat mengubah hasil pemeriksaan internal, nominal tagihan yang telah diterbitkan, status pembayaran terverifikasi, surat jalan, atau catatan transaksi internal.

### 5.2 Satpam

Satpam bertanggung jawab atas pemeriksaan kendaraan pada saat masuk dan keluar area perusahaan.

Hak akses:
- Melihat daftar kendaraan/pesanan yang diperkirakan masuk.
- Mencari pesanan berdasarkan nomor pesanan, nomor polisi, nama perusahaan, atau nama driver.
- Memeriksa nomor polisi, jenis kendaraan, dan nama driver terhadap data pesanan.
- Menyetujui kendaraan masuk apabila data sesuai.
- Menolak atau menahan proses masuk apabila data tidak sesuai, disertai alasan.
- Melihat surat jalan/izin keluar yang diterbitkan Kepala Gudang.
- Memeriksa kendaraan, driver, dan muatan terhadap surat jalan.
- Menyetujui kendaraan keluar apabila seluruh persyaratan valid.
- Mencatat ketidaksesuaian atau penolakan keluar.
- Melihat riwayat kendaraan masuk dan keluar dengan filter tanggal, kendaraan, pesanan, dan petugas.

Satpam tidak dapat mengubah nominal tagihan, memverifikasi pembayaran, mengubah hasil pekerjaan gudang, atau menerbitkan surat jalan.

### 5.3 Kepala Gudang

Kepala Gudang bertanggung jawab atas pemeriksaan muatan, persetujuan pekerjaan, proses layanan, hasil pekerjaan, dan penerbitan surat jalan.

Hak akses:
- Melihat pesanan client dan informasi layanan yang diminta.
- Melihat status kendaraan setelah disetujui masuk oleh Satpam.
- Memeriksa jenis dan jumlah muatan drum serta kondisi barang saat diterima.
- Mencatat hasil pemeriksaan dan perbedaan antara pesanan dengan kondisi aktual.
- Menyetujui pekerjaan untuk diproses, meminta klarifikasi, atau menolak dengan alasan.
- Mengubah status pekerjaan menjadi sedang diproses.
- Mencatat perkembangan pekerjaan dan hasil akhir jasa.
- Mencatat kuantitas hasil, barang yang tidak dapat diproses, atau catatan tambahan yang memengaruhi perhitungan jasa.
- Mengirim hasil pekerjaan ke antrean Keuangan.
- Membuat dan menerbitkan surat jalan/izin keluar setelah persyaratan pembayaran dipenuhi.
- Melihat riwayat pekerjaan dan transaksi gudang.

Kepala Gudang tidak dapat menyatakan pembayaran lunas atau mengubah catatan pembayaran yang menjadi kewenangan Keuangan.

### 5.4 Keuangan

Keuangan bertanggung jawab atas perhitungan biaya jasa, penerbitan tagihan, pencatatan pembayaran, buku keuangan, dan biaya operasional.

Hak akses:
- Melihat hasil pekerjaan yang sudah diselesaikan Kepala Gudang.
- Memeriksa rincian jasa, kuantitas, tarif, penyesuaian, dan total tagihan.
- Membuat, menerbitkan, dan mengirim tagihan melalui portal client.
- Mengubah tagihan yang masih berstatus draft.
- Membuat revisi/koreksi tagihan dengan alasan dan riwayat yang tercatat.
- Mencatat pembayaran yang diterima dan memverifikasi bukti pembayaran.
- Memperbarui status pembayaran menjadi lunas hanya setelah jumlah yang diterima memenuhi total tagihan.
- Mencatat transaksi pemasukan jasa ke buku keuangan.
- Mencatat biaya operasional, misalnya pembelian bahan, perlengkapan, gaji karyawan, dan pengeluaran lain.
- Melampirkan bukti transaksi, seperti nota atau kuitansi.
- Melihat dan mengekspor laporan keuangan ke PDF dan Excel.
- Melihat rekap seluruh transaksi yang dibutuhkan untuk pekerjaan Keuangan.

Keuangan tidak dapat menyetujui kendaraan masuk/keluar atau mengubah hasil pemeriksaan gudang.

### 5.5 Owner

Owner memiliki akses pemantauan menyeluruh terhadap aktivitas perusahaan.

Hak akses:
- Melihat ringkasan seluruh pesanan dan status layanan.
- Melihat aktivitas kendaraan masuk dan keluar.
- Melihat jumlah dan status pekerjaan gudang.
- Melihat pendapatan jasa, tagihan belum dibayar, pembayaran yang diterima, dan riwayat transaksi.
- Melihat seluruh biaya operasional perusahaan.
- Melihat ringkasan arus kas masuk dan keluar serta laporan berdasarkan periode.
- Melihat detail transaksi dan riwayat aktivitas pengguna.
- Mengekspor laporan yang diizinkan.

**Prinsip akses awal:** Owner bersifat pemantauan (read-only) terhadap transaksi operasional dan keuangan. Perubahan atau persetujuan khusus oleh Owner hanya ditambahkan apabila PKDI menetapkan kebutuhan tersebut. Owner tidak boleh menghapus jejak transaksi.

## 6. Ruang Lingkup Produk

### 6.1 Termasuk dalam ruang lingkup

- Registrasi dan login Client.
- Pengelolaan akun internal dan role-based access control.
- Profil perusahaan Client.
- Pengajuan dan pengelolaan pesanan jasa drum.
- Dashboard dan antrean pekerjaan sesuai peran.
- Pemeriksaan kendaraan masuk dan keluar oleh Satpam.
- Pemeriksaan muatan dan pengelolaan pekerjaan oleh Kepala Gudang.
- Pencatatan hasil pekerjaan.
- Perhitungan tarif dan penerbitan tagihan oleh Keuangan.
- Portal Client untuk melihat tagihan dan status pembayaran.
- Pencatatan dan verifikasi pembayaran.
- Pembuatan surat jalan/izin keluar oleh Kepala Gudang.
- Pencatatan kendaraan keluar setelah verifikasi Satpam.
- Buku keuangan/pencatatan transaksi masuk dan keluar.
- Pencatatan biaya operasional perusahaan.
- Dashboard Owner.
- Notifikasi perubahan status.
- Pencarian, filter, riwayat, audit log, dan ekspor PDF/Excel.

### 6.2 Tidak termasuk dalam ruang lingkup awal (MVP)

- Integrasi payment gateway atau pembayaran otomatis melalui bank.
- Integrasi langsung dengan sistem perbankan atau mutasi rekening.
- Sistem penggajian lengkap, perhitungan pajak karyawan, absensi, dan HRIS.
- Manajemen stok bahan baku dan inventaris secara penuh.
- Akuntansi lengkap seperti jurnal umum, neraca, laba rugi sesuai standar akuntansi, dan pelaporan pajak resmi.
- Aplikasi mobile native Android/iOS.
- Integrasi timbangan digital, perangkat IoT, atau akses gerbang otomatis.
- Multi-cabang/multi-perusahaan, kecuali kemudian ditetapkan sebagai kebutuhan.

Fitur di luar MVP dapat dipertimbangkan sebagai pengembangan lanjutan setelah alur operasional utama digunakan dan divalidasi.

## 7. Alur Bisnis Utama

### 7.1 Registrasi dan pengajuan pesanan oleh Client

1. Client membuka halaman registrasi dan membuat akun.
2. Client mengisi profil perusahaan, nama penanggung jawab, email/nomor kontak, dan kata sandi.
3. Setelah berhasil masuk, Client memilih menu **Buat Pesanan**.
4. Client mengisi informasi pesanan:
   - Jenis kategori layanan.
   - Jenis muatan.
   - Jenis layanan: diperbarui atau diperbaiki.
   - Jenis kendaraan.
   - Nomor polisi kendaraan.
   - Asal perusahaan/PT.
   - Nama driver (opsional, sesuai kebutuhan operasional).
   - Catatan tambahan (opsional).
   - Lampiran dokumen pendukung (opsional, bila diaktifkan).
5. Client meninjau dan mengirim pesanan.
6. Sistem membuat nomor pesanan unik, menyimpan waktu pengajuan, dan memberi status awal **Menunggu Kedatangan/Pemeriksaan Satpam**.
7. Pesanan muncul pada dashboard internal secara otomatis dengan antrean yang relevan:
   - Satpam: daftar kendaraan/pesanan yang akan datang.
   - Kepala Gudang: daftar pesanan yang perlu dipersiapkan untuk pemeriksaan gudang.
   - Keuangan: ringkasan pesanan baru sebagai informasi awal; belum menjadi tagihan.
   - Owner: ringkasan pesanan baru dan status keseluruhan.
8. Client dapat melihat nomor pesanan dan timeline status dari portal Client.

### 7.2 Pemeriksaan kendaraan masuk oleh Satpam

1. Kendaraan tiba di gerbang PKDI.
2. Satpam mencari pesanan menggunakan nomor pesanan, nomor polisi, atau identitas lain yang tersedia.
3. Satpam memeriksa kesesuaian:
   - Nomor polisi kendaraan.
   - Jenis kendaraan.
   - Nama driver, apabila diisi oleh Client.
   - Nomor pesanan dan perusahaan asal.
4. Jika data sesuai, Satpam menekan tombol **Setujui Masuk**.
5. Sistem menyimpan waktu masuk, akun Satpam, data pemeriksaan, dan status kendaraan.
6. Status pesanan berubah menjadi **Kendaraan Masuk — Menunggu Pemeriksaan Gudang**.
7. Jika data tidak sesuai, Satpam memilih **Tahan/Tolak Masuk**, wajib mengisi alasan, dan sistem mengirimkan pemberitahuan kepada bagian terkait serta Client.
8. Riwayat kendaraan masuk tampil pada dashboard Satpam dan ringkasan Owner.

### 7.3 Pemeriksaan muatan dan persetujuan pekerjaan oleh Kepala Gudang

1. Setelah kendaraan disetujui masuk, pesanan muncul pada antrean Kepala Gudang.
2. Kepala Gudang membuka detail pesanan untuk melihat jenis kategori, jenis muatan, dan layanan yang diminta Client.
3. Kepala Gudang memeriksa muatan drum secara aktual.
4. Kepala Gudang mencatat hasil pemeriksaan, minimal:
   - Jenis muatan aktual.
   - Jumlah/kuantitas aktual dengan satuan yang disepakati.
   - Kondisi atau catatan pemeriksaan.
   - Ketidaksesuaian terhadap pesanan, jika ada.
5. Kepala Gudang memilih salah satu tindakan:
   - **Setujui dan Proses**: barang diterima untuk dikerjakan.
   - **Minta Klarifikasi**: data atau kondisi perlu dikonfirmasi.
   - **Tolak**: pekerjaan tidak dapat diterima, disertai alasan.
6. Jika disetujui, status menjadi **Sedang Diproses** dan Client mendapat notifikasi.
7. Kepala Gudang dapat memperbarui perkembangan pekerjaan yang dapat dilihat Client sesuai informasi yang dibagikan.

### 7.4 Penyelesaian pekerjaan oleh Kepala Gudang

1. Setelah jasa selesai, Kepala Gudang membuka pesanan dan mengisi hasil akhir pekerjaan.
2. Data hasil akhir minimal meliputi jenis layanan yang benar-benar dilakukan, kuantitas selesai, kuantitas tidak selesai/tidak layak (jika ada), dan catatan hasil.
3. Kepala Gudang mengonfirmasi pekerjaan selesai.
4. Sistem menyimpan waktu penyelesaian dan akun Kepala Gudang.
5. Status berubah menjadi **Selesai Diproses — Menunggu Perhitungan Keuangan**.
6. Hasil pekerjaan dan kuantitas aktual otomatis masuk ke antrean Keuangan.
7. Client menerima notifikasi bahwa pekerjaan selesai dan sedang dihitung untuk penagihan.

### 7.5 Perhitungan jasa dan penerbitan tagihan oleh Keuangan

1. Keuangan membuka daftar pekerjaan yang telah selesai.
2. Keuangan meninjau hasil pekerjaan dari Kepala Gudang.
3. Keuangan menghitung biaya berdasarkan tarif jasa, jenis layanan, kuantitas, serta komponen biaya lain yang telah disepakati PKDI.
4. Sistem menampilkan rincian perhitungan, subtotal, potongan atau penyesuaian (bila diizinkan), pajak (jika berlaku dan sudah ditetapkan), serta total tagihan.
5. Keuangan menyimpan perhitungan sebagai draft untuk diperiksa.
6. Setelah benar, Keuangan menerbitkan tagihan/invoice dengan nomor dokumen unik.
7. Tagihan diterbitkan ke akun Client sehingga dapat dilihat dan diunduh oleh Client.
8. Status pesanan menjadi **Menunggu Pembayaran**.
9. Sistem mengirim notifikasi kepada Client dan menampilkan tagihan pada dashboard Keuangan.

### 7.6 Pembayaran dan verifikasi oleh Keuangan

1. Client membuka tagihan melalui akun Client.
2. Client melihat nominal, rincian jasa, tanggal terbit, tanggal jatuh tempo (jika digunakan), dan instruksi pembayaran.
3. Pembayaran dilakukan melalui metode yang ditetapkan PKDI.
4. Untuk proses manual pada MVP, Client dapat mengirim bukti pembayaran atau Keuangan mencatat pembayaran yang diterima berdasarkan bukti perusahaan.
5. Keuangan memeriksa nominal, tanggal, metode, referensi transaksi, dan bukti pembayaran.
6. Jika valid, Keuangan mencatat pembayaran dan sistem memperbarui sisa tagihan.
7. Status menjadi **Lunas** hanya jika total pembayaran terverifikasi telah memenuhi total tagihan.
8. Jika belum lunas, status tetap **Menunggu Pembayaran** atau **Pembayaran Sebagian** dan barang belum boleh keluar.
9. Setiap pembayaran yang diverifikasi otomatis tercatat sebagai pemasukan pada buku keuangan.
10. Client menerima notifikasi atas pembayaran yang berhasil diverifikasi.

### 7.7 Penerbitan surat jalan oleh Kepala Gudang

1. Setelah tagihan berstatus lunas, pesanan masuk ke antrean persiapan barang keluar.
2. Kepala Gudang memeriksa barang yang akan dikeluarkan dan memastikan jumlah/jenisnya sesuai hasil pekerjaan serta dokumen.
3. Kepala Gudang membuat surat jalan/izin keluar yang memiliki nomor dokumen unik.
4. Surat jalan memuat referensi pesanan, Client, rincian barang/kuantitas, kendaraan, nomor polisi, nama driver bila tersedia, tanggal penerbitan, dan petugas penerbit.
5. Sistem hanya mengizinkan penerbitan surat jalan apabila seluruh tagihan terkait telah lunas dan tidak ada penahanan transaksi.
6. Surat jalan tersedia untuk Satpam dan dapat dibagikan/diunduh oleh Client sesuai kebijakan PKDI.
7. Status berubah menjadi **Menunggu Pemeriksaan Keluar Satpam**.

### 7.8 Pemeriksaan kendaraan keluar oleh Satpam

1. Kendaraan datang ke gerbang keluar dengan membawa surat jalan/izin keluar.
2. Satpam mencari atau memindai nomor surat jalan/QR (jika fitur QR diaktifkan).
3. Satpam memeriksa:
   - Surat jalan masih berlaku dan belum pernah digunakan untuk transaksi keluar.
   - Nomor pesanan dan Client.
   - Nomor polisi serta jenis kendaraan.
   - Nama driver, apabila tercatat.
   - Jenis dan jumlah barang yang dibawa.
   - Status pembayaran lunas.
4. Jika seluruh data sesuai, Satpam menekan **Setujui Keluar**.
5. Sistem menyimpan tanggal dan waktu keluar, akun Satpam, referensi surat jalan, dan catatan pemeriksaan.
6. Status pesanan menjadi **Selesai — Kendaraan Keluar**.
7. Client, Kepala Gudang, Keuangan, dan Owner dapat melihat bahwa proses telah selesai.
8. Jika terdapat ketidaksesuaian, Satpam menahan kendaraan dan mengisi alasan. Sistem tidak mengubah status menjadi selesai.

### 7.9 Pencatatan biaya operasional oleh Keuangan

1. Keuangan membuka menu **Biaya Operasional**.
2. Keuangan menambahkan transaksi pengeluaran dengan mengisi kategori, tanggal, uraian, nominal, penerima/vendor (jika ada), metode pembayaran, dan lampiran bukti.
3. Contoh kategori: pembelian bahan, perlengkapan, gaji karyawan, transportasi, pemeliharaan, utilitas, dan biaya lainnya.
4. Sistem menyimpan transaksi sebagai pengeluaran perusahaan dan memasukkannya ke buku keuangan.
5. Owner dapat melihat data dan ringkasannya.
6. Data dapat difilter per kategori, tanggal, dan periode serta diekspor ke PDF/Excel.

## 8. Status Pesanan dan Aturan Perubahan Status

Status berikut merupakan rancangan status standar. Nama label dapat disesuaikan pada tahap UI, tetapi transisi dan kontrolnya harus dipertahankan.

| Kode Status | Label yang Ditampilkan | Pemicu Perubahan | Pihak Utama |
|---|---|---|---|
| `SUBMITTED` | Pesanan Diajukan | Client mengirim pesanan | Client |
| `WAITING_GATE_IN` | Menunggu Kedatangan/Pemeriksaan Satpam | Pesanan berhasil dibuat | Sistem |
| `GATE_IN_HELD` | Pemeriksaan Masuk Ditahan | Data kendaraan tidak cocok | Satpam |
| `WAITING_WAREHOUSE` | Kendaraan Masuk — Menunggu Pemeriksaan Gudang | Satpam menyetujui masuk | Satpam |
| `WAREHOUSE_REVIEW` | Sedang Diperiksa Gudang | Kepala Gudang mulai pemeriksaan | Kepala Gudang |
| `NEEDS_CLARIFICATION` | Menunggu Klarifikasi | Ada data/muatan yang perlu dikonfirmasi | Kepala Gudang |
| `IN_PROGRESS` | Sedang Diproses | Kepala Gudang menyetujui pekerjaan | Kepala Gudang |
| `SERVICE_COMPLETED` | Selesai Diproses | Kepala Gudang mengisi hasil akhir | Kepala Gudang |
| `WAITING_FINANCE` | Menunggu Perhitungan Keuangan | Hasil pekerjaan dikirim ke Keuangan | Sistem |
| `INVOICE_ISSUED` | Tagihan Diterbitkan | Keuangan menerbitkan invoice | Keuangan |
| `PAYMENT_PARTIAL` | Pembayaran Sebagian | Pembayaran terverifikasi belum penuh | Keuangan |
| `PAID` | Lunas | Pembayaran terverifikasi memenuhi total tagihan | Keuangan |
| `WAITING_EXIT_DOCUMENT` | Menunggu Surat Jalan | Pembayaran sudah lunas | Sistem |
| `WAITING_GATE_OUT` | Menunggu Pemeriksaan Keluar Satpam | Kepala Gudang menerbitkan surat jalan | Kepala Gudang |
| `GATE_OUT_HELD` | Pemeriksaan Keluar Ditahan | Dokumen/kendaraan/muatan tidak sesuai | Satpam |
| `COMPLETED` | Selesai — Kendaraan Keluar | Satpam menyetujui kendaraan keluar | Satpam |
| `CANCELLED` | Dibatalkan | Pembatalan sesuai kebijakan dan alasan tercatat | Pihak berwenang |

Aturan penting:
- Status tidak boleh meloncat melewati tahapan wajib.
- Satpam hanya dapat mengubah hasil pemeriksaan gerbang, bukan status pekerjaan atau pembayaran.
- Kepala Gudang hanya dapat mengubah status pemeriksaan, proses, hasil jasa, dan dokumen keluar.
- Keuangan hanya dapat mengubah status tagihan dan pembayaran sesuai kewenangannya.
- Sistem wajib memvalidasi status pembayaran sebelum surat jalan diterbitkan.
- Sistem wajib memvalidasi surat jalan dan status pembayaran sebelum kendaraan disetujui keluar.
- Setiap perubahan status menyimpan status sebelumnya, status baru, waktu, pengguna, dan catatan.
- Pesanan yang sudah selesai tidak boleh diedit langsung. Koreksi harus melalui mekanisme revisi yang menyimpan alasan dan riwayat.

## 9. Kebutuhan Fungsional

### 9.1 Autentikasi dan manajemen pengguna

| ID | Kebutuhan | Prioritas |
|---|---|---|
| FR-AUTH-01 | Sistem menyediakan login dan logout untuk seluruh peran. | Must |
| FR-AUTH-02 | Client dapat melakukan registrasi akun sendiri. | Must |
| FR-AUTH-03 | Sistem membatasi data Client berdasarkan perusahaan pemilik akun. | Must |
| FR-AUTH-04 | Owner/pengelola yang diberi kewenangan dapat membuat, menonaktifkan, dan mengatur akun internal. | Must |
| FR-AUTH-05 | Sistem menyediakan akun masing-masing untuk 5 peran pengguna (Client, Satpam, Kepala Gudang, Keuangan, dan Owner). | Must |
| FR-AUTH-06 | Setiap halaman dan tindakan dibatasi berdasarkan role dan izin. | Must |
| FR-AUTH-07 | Sistem menyediakan reset kata sandi dengan mekanisme aman. | Should |
| FR-AUTH-08 | Sistem mencatat waktu login dan aktivitas penting pengguna. | Should |

### 9.2 Portal Client dan pesanan

| ID | Kebutuhan | Prioritas |
|---|---|---|
| FR-CLI-01 | Client dapat membuat dan memperbarui profil perusahaannya. | Must |
| FR-CLI-02 | Client dapat membuat pesanan dengan jenis kategori, jenis muatan, jenis layanan, jenis kendaraan, nomor polisi, asal PT, dan nama driver opsional. | Must |
| FR-CLI-03 | Sistem menghasilkan nomor pesanan unik secara otomatis. | Must |
| FR-CLI-04 | Client dapat melihat daftar pesanan milik perusahaannya. | Must |
| FR-CLI-05 | Client dapat membuka detail pesanan dan timeline status. | Must |
| FR-CLI-06 | Client menerima notifikasi ketika status berubah atau tagihan diterbitkan. | Must |
| FR-CLI-07 | Client dapat melihat dan mengunduh tagihan yang diterbitkan untuk perusahaannya. | Must |
| FR-CLI-08 | Client dapat melihat status pembayaran dan riwayat pembayaran. | Must |
| FR-CLI-09 | Client dapat mengunggah bukti pembayaran jika proses manual ini disetujui. | Should |
| FR-CLI-10 | Client dapat meminta pembatalan sebelum tahap yang ditetapkan PKDI, dengan alasan. | Could |

### 9.3 Dashboard dan pemeriksaan Satpam

| ID | Kebutuhan | Prioritas |
|---|---|---|
| FR-GATE-01 | Dashboard Satpam menampilkan antrean kendaraan yang diperkirakan masuk dan surat jalan yang menunggu pemeriksaan keluar. | Must |
| FR-GATE-02 | Satpam dapat mencari pesanan berdasarkan nomor pesanan, nomor polisi, Client, dan nama driver. | Must |
| FR-GATE-03 | Satpam dapat mencatat hasil pemeriksaan masuk dan menyetujui kendaraan masuk. | Must |
| FR-GATE-04 | Satpam dapat menahan/menolak kendaraan masuk dengan alasan wajib. | Must |
| FR-GATE-05 | Satpam dapat membuka detail surat jalan untuk pemeriksaan keluar. | Must |
| FR-GATE-06 | Sistem memblokir persetujuan keluar jika pembayaran belum lunas atau surat jalan tidak valid. | Must |
| FR-GATE-07 | Satpam dapat mencatat persetujuan atau penahanan kendaraan keluar. | Must |
| FR-GATE-08 | Dashboard menyediakan riwayat kendaraan masuk dan keluar serta filter. | Must |
| FR-GATE-09 | Sistem menyimpan akun Satpam yang melakukan setiap pemeriksaan. | Must |
| FR-GATE-10 | Sistem mendukung pencarian/pemindaian QR surat jalan jika QR diaktifkan. | Could |

### 9.4 Dashboard dan pekerjaan Kepala Gudang

| ID | Kebutuhan | Prioritas |
|---|---|---|
| FR-WH-01 | Dashboard menampilkan pesanan yang menunggu pemeriksaan, sedang diproses, dan selesai. | Must |
| FR-WH-02 | Kepala Gudang dapat melihat rincian permintaan layanan dari Client. | Must |
| FR-WH-03 | Kepala Gudang dapat mencatat pemeriksaan jenis, jumlah, kondisi, dan ketidaksesuaian muatan. | Must |
| FR-WH-04 | Kepala Gudang dapat menyetujui, meminta klarifikasi, atau menolak pekerjaan dengan alasan. | Must |
| FR-WH-05 | Kepala Gudang dapat memperbarui status pekerjaan menjadi sedang diproses. | Must |
| FR-WH-06 | Kepala Gudang dapat memasukkan hasil akhir dan kuantitas pekerjaan. | Must |
| FR-WH-07 | Sistem mengirim hasil pekerjaan yang diselesaikan ke antrean Keuangan. | Must |
| FR-WH-08 | Kepala Gudang dapat membuat surat jalan setelah sistem memastikan tagihan lunas. | Must |
| FR-WH-09 | Surat jalan memiliki nomor unik dan terhubung ke pesanan. | Must |
| FR-WH-10 | Dashboard menyediakan riwayat pemeriksaan, pekerjaan, dan surat jalan. | Must |

### 9.5 Dashboard dan modul Keuangan

| ID | Kebutuhan | Prioritas |
|---|---|---|
| FR-FIN-01 | Dashboard menampilkan pekerjaan selesai yang menunggu perhitungan tagihan. | Must |
| FR-FIN-02 | Keuangan dapat melihat hasil pekerjaan yang dicatat Kepala Gudang. | Must |
| FR-FIN-03 | Keuangan dapat mengelola daftar tarif jasa yang digunakan untuk perhitungan. | Must |
| FR-FIN-04 | Keuangan dapat membuat rincian dan draft tagihan. | Must |
| FR-FIN-05 | Keuangan dapat menerbitkan tagihan ke portal Client. | Must |
| FR-FIN-06 | Tagihan dapat dicetak/diunduh sebagai PDF. | Must |
| FR-FIN-07 | Keuangan dapat mencatat pembayaran beserta tanggal, nominal, metode, referensi, dan bukti. | Must |
| FR-FIN-08 | Sistem menghitung sisa pembayaran dan menentukan status belum lunas, sebagian, atau lunas. | Must |
| FR-FIN-09 | Pembayaran terverifikasi otomatis menjadi pemasukan pada buku keuangan. | Must |
| FR-FIN-10 | Keuangan dapat mencatat biaya operasional dan lampiran bukti transaksi. | Must |
| FR-FIN-11 | Keuangan dapat mengelompokkan biaya berdasarkan kategori. | Must |
| FR-FIN-12 | Sistem menyediakan buku pemasukan dan pengeluaran dengan filter periode dan kategori. | Must |
| FR-FIN-13 | Laporan keuangan dapat diekspor ke PDF dan Excel. | Must |
| FR-FIN-14 | Koreksi tagihan/pembayaran harus menyertakan alasan dan riwayat; transaksi tidak dihapus permanen. | Must |
| FR-FIN-15 | Sistem dapat menampilkan ringkasan tagihan terbit, belum dibayar, pembayaran masuk, biaya operasional, dan saldo bersih kas berdasarkan transaksi tercatat. | Must |

### 9.6 Dashboard Owner

| ID | Kebutuhan | Prioritas |
|---|---|---|
| FR-OWN-01 | Owner dapat melihat ringkasan jumlah pesanan berdasarkan status. | Must |
| FR-OWN-02 | Owner dapat melihat aktivitas kendaraan masuk dan keluar. | Must |
| FR-OWN-03 | Owner dapat melihat jumlah pekerjaan gudang yang menunggu, berjalan, dan selesai. | Must |
| FR-OWN-04 | Owner dapat melihat pendapatan jasa, tagihan belum dibayar, dan pembayaran diterima. | Must |
| FR-OWN-05 | Owner dapat melihat rincian biaya operasional perusahaan. | Must |
| FR-OWN-06 | Owner dapat melihat ringkasan pemasukan, pengeluaran, dan saldo bersih kas untuk periode tertentu. | Must |
| FR-OWN-07 | Owner dapat memfilter data berdasarkan periode, Client, kategori, dan status yang relevan. | Must |
| FR-OWN-08 | Owner dapat melihat detail transaksi dan audit trail. | Must |
| FR-OWN-09 | Owner dapat mengunduh laporan ringkasan sesuai hak akses. | Should |

### 9.7 Notifikasi, pencarian, dan audit

| ID | Kebutuhan | Prioritas |
|---|---|---|
| FR-SYS-01 | Sistem membuat notifikasi untuk pesanan baru dan perubahan status penting. | Must |
| FR-SYS-02 | Client menerima notifikasi untuk pemeriksaan, proses, pekerjaan selesai, tagihan, dan pembayaran. | Must |
| FR-SYS-03 | Bagian internal menerima notifikasi ketika ada pekerjaan baru pada antreannya. | Must |
| FR-SYS-04 | Dashboard memperbarui status tanpa pengguna harus keluar dan masuk kembali. | Must |
| FR-SYS-05 | Sistem menyediakan pencarian dan filter sesuai modul. | Must |
| FR-SYS-06 | Sistem menyimpan audit log untuk perubahan data penting dan tindakan persetujuan. | Must |
| FR-SYS-07 | Sistem mencatat waktu dalam zona waktu operasional Indonesia (WIB) dan menyimpan waktu secara konsisten. | Must |

## 10. Rancangan Isi Dashboard

### 10.1 Dashboard Client

- Ringkasan pesanan aktif.
- Ringkasan pesanan menunggu pemeriksaan, sedang diproses, selesai diproses, dan selesai seluruhnya.
- Tagihan belum dibayar dan total tagihan.
- Riwayat pesanan.
- Timeline status setiap pesanan.
- Notifikasi terbaru.
- Tombol **Buat Pesanan**.

### 10.2 Dashboard Satpam

- Kendaraan/pesanan menunggu masuk.
- Kendaraan yang sedang berada di area perusahaan (berdasarkan catatan masuk/keluar).
- Surat jalan yang menunggu pemeriksaan keluar.
- Kendaraan masuk hari ini.
- Kendaraan keluar hari ini.
- Riwayat dan pencarian transaksi gerbang.

### 10.3 Dashboard Kepala Gudang

- Pesanan menunggu pemeriksaan muatan.
- Pesanan menunggu persetujuan.
- Pekerjaan sedang diproses.
- Pekerjaan selesai dan menunggu Keuangan.
- Pesanan lunas yang menunggu persiapan/surat jalan.
- Riwayat pemeriksaan, hasil kerja, dan surat jalan.

### 10.4 Dashboard Keuangan

- Pekerjaan selesai yang belum dihitung.
- Draft tagihan.
- Tagihan terbit dan belum dibayar.
- Pembayaran menunggu verifikasi.
- Pembayaran sebagian dan lunas.
- Total pemasukan jasa pada periode terpilih.
- Total biaya operasional pada periode terpilih.
- Ringkasan saldo bersih kas.
- Daftar transaksi terbaru.

### 10.5 Dashboard Owner

- Total pesanan dan distribusi status layanan.
- Aktivitas kendaraan masuk/keluar.
- Pekerjaan gudang yang berjalan dan selesai.
- Total tagihan diterbitkan.
- Total pembayaran diterima.
- Total tagihan yang belum dibayar.
- Total biaya operasional.
- Ringkasan arus kas masuk, arus kas keluar, dan saldo bersih kas.
- Grafik tren layanan dan keuangan per hari/bulan.
- Aktivitas terbaru dari seluruh bagian.

## 11. Data yang Dikelola

Bagian ini merupakan rancangan entitas konseptual, bukan keputusan struktur tabel database final.

| Entitas | Data Utama | Keterangan |
|---|---|---|
| User | Nama, email, kata sandi terenkripsi, role, status akun | Akun internal dan client |
| Client Company | Nama PT, alamat, kontak, penanggung jawab, status verifikasi | Profil perusahaan client |
| Service Order | Nomor pesanan, client, kategori, jenis muatan, jenis layanan, kendaraan, nomor polisi, driver opsional, status, tanggal | Permintaan layanan client |
| Vehicle Gate Log | Pesanan, jenis pemeriksaan (masuk/keluar), kendaraan, driver, hasil, alasan, waktu, petugas | Riwayat aktivitas Satpam |
| Warehouse Inspection | Pesanan, muatan aktual, kuantitas, satuan, kondisi, catatan, pemeriksa, waktu | Pemeriksaan saat barang tiba |
| Service Process | Pesanan, status pekerjaan, catatan proses, penanggung jawab, waktu | Riwayat pengerjaan gudang |
| Service Result | Pesanan, jenis jasa aktual, kuantitas selesai/tidak selesai, catatan hasil | Dasar perhitungan tagihan |
| Service Tariff | Nama tarif, jenis layanan/kategori, satuan, harga, masa berlaku, status | Master tarif untuk Keuangan |
| Invoice | Nomor tagihan, pesanan, tanggal, rincian, subtotal, penyesuaian, pajak bila berlaku, total, status | Dokumen penagihan |
| Payment | Tagihan, tanggal bayar, nominal, metode, referensi, bukti, status verifikasi, pemeriksa | Catatan pembayaran |
| Delivery Order | Nomor surat jalan, pesanan, rincian barang, kendaraan, driver, penerbit, tanggal, status | Izin barang keluar |
| Operational Expense | Tanggal, kategori, uraian, nominal, penerima/vendor, metode, bukti, pencatat | Biaya operasional perusahaan |
| Cashbook Entry | Jenis masuk/keluar, sumber transaksi, tanggal, nominal, referensi | Buku keuangan; terhubung ke pembayaran/biaya |
| Notification | Penerima, pesan, referensi transaksi, waktu, status baca | Notifikasi pengguna |
| Audit Log | Pengguna, tindakan, jenis data, ID data, nilai sebelum/sesudah, waktu, alasan | Jejak audit |

## 12. Aturan Bisnis Penting

1. **Satu pesanan memiliki satu nomor unik.** Nomor pesanan menjadi penghubung proses Client, Satpam, Kepala Gudang, Keuangan, surat jalan, dan Owner.
2. **Data Client terisolasi.** Client hanya dapat melihat pesanan, tagihan, pembayaran, dan dokumen milik perusahaannya.
3. **Pemeriksaan masuk wajib.** Pekerjaan gudang tidak boleh dimulai sebelum kendaraan tercatat masuk oleh Satpam, kecuali PKDI menetapkan prosedur khusus.
4. **Pemeriksaan gudang wajib.** Kuantitas aktual dan kondisi barang perlu dicatat sebelum pekerjaan disetujui.
5. **Hasil kerja menjadi dasar tagihan.** Keuangan menggunakan hasil aktual yang diserahkan Kepala Gudang, bukan hanya kuantitas permintaan awal Client.
6. **Pembayaran harus terverifikasi.** Unggahan bukti oleh Client tidak otomatis berarti pembayaran lunas.
7. **Barang tidak boleh keluar sebelum lunas.** Surat jalan tidak dapat diterbitkan dan Satpam tidak dapat menyetujui keluar apabila masih ada sisa tagihan.
8. **Surat jalan hanya untuk satu transaksi keluar.** Surat jalan yang sudah digunakan tidak boleh digunakan kembali.
9. **Kesesuaian gerbang wajib dicatat.** Perbedaan kendaraan, driver, surat jalan, atau kuantitas harus menyebabkan penahanan dan dicatat alasannya.
10. **Transaksi keuangan tidak dihapus permanen.** Kesalahan diperbaiki melalui pembatalan/koreksi dengan alasan dan audit trail.
11. **Pemasukan tidak boleh dihitung ganda.** Pembayaran terverifikasi menjadi satu catatan pemasukan yang terhubung ke tagihan asal.
12. **Biaya operasional terpisah dari biaya jasa client.** Keduanya ditampilkan terpisah dan dapat digabung dalam ringkasan arus kas Owner.
13. **Status real-time harus konsisten.** Perubahan status di satu bagian harus terlihat pada dashboard bagian lain dan portal Client sesuai hak akses.
14. **Nama driver bersifat opsional pada pengajuan.** Jika kosong, Satpam perlu mengikuti prosedur pemeriksaan identitas aktual dan mencatat hasilnya sebelum persetujuan masuk/keluar.
15. **Koreksi jumlah atau jenis muatan harus tercatat.** Perubahan dari data permintaan awal wajib disimpan sebagai data pemeriksaan aktual, tidak menimpa informasi awal.

## 13. Laporan dan Ekspor

### Laporan Operasional
- Daftar pesanan per periode.
- Status seluruh pesanan.
- Riwayat kendaraan masuk dan keluar.
- Rekap pemeriksaan muatan.
- Rekap pekerjaan jasa dan hasil pekerjaan.
- Daftar surat jalan yang diterbitkan dan digunakan.

### Laporan Keuangan
- Rekap tagihan per Client dan periode.
- Rekap pembayaran diterima.
- Daftar tagihan belum lunas dan pembayaran sebagian.
- Rekap pemasukan jasa.
- Rekap biaya operasional berdasarkan kategori.
- Buku kas masuk dan kas keluar.
- Ringkasan arus kas per hari, bulan, dan rentang tanggal.

### Format dan kontrol ekspor
- PDF digunakan untuk dokumen tagihan dan laporan yang siap dicetak.
- Excel digunakan untuk rekap dan pengolahan data lanjutan.
- Ekspor mengikuti filter periode, Client, kategori, dan status yang sedang dipilih.
- Setiap dokumen resmi memiliki nomor dokumen, tanggal, dan identitas perusahaan sesuai format yang disepakati.
- Data pribadi atau transaksi hanya dapat diekspor oleh role yang memiliki izin.

## 14. Notifikasi dan Pembaruan Real-Time

Peristiwa yang memicu notifikasi:
- Pesanan baru berhasil diajukan.
- Kendaraan disetujui masuk atau ditahan Satpam.
- Pemeriksaan gudang selesai dan pekerjaan disetujui/ditolak/perlu klarifikasi.
- Status pekerjaan berubah menjadi sedang diproses.
- Pekerjaan selesai dan dikirim ke Keuangan.
- Tagihan diterbitkan atau direvisi.
- Bukti pembayaran diterima, pembayaran diverifikasi, atau pembayaran ditolak.
- Tagihan menjadi lunas.
- Surat jalan diterbitkan.
- Kendaraan disetujui keluar atau ditahan Satpam.

Pembaruan status harus terlihat di dashboard tanpa pengguna perlu memuat ulang halaman secara manual. Target awal pembaruan yang disarankan adalah maksimal 10 detik pada kondisi koneksi normal. Mekanisme teknis dapat menggunakan broadcasting/WebSocket atau polling terjadwal sesuai arsitektur implementasi.

## 15. Kebutuhan Non-Fungsional

### 15.1 Keamanan
- Autentikasi untuk seluruh akses sistem.
- Hak akses berbasis role dan pemeriksaan izin di sisi server, bukan hanya menyembunyikan tombol di UI.
- Password disimpan menggunakan algoritma hashing yang aman.
- Validasi dan sanitasi input pada semua form.
- Proteksi terhadap akses langsung ke data milik Client lain (IDOR/cross-tenant access).
- File upload dibatasi berdasarkan tipe, ukuran, dan pemeriksaan keamanan.
- Dokumen tagihan dan bukti transaksi tidak boleh dapat diakses publik tanpa otorisasi.
- Audit log untuk persetujuan masuk/keluar, perubahan status, penerbitan tagihan, verifikasi pembayaran, dan koreksi transaksi.

### 15.2 Kinerja
- Halaman daftar menggunakan pagination.
- Pencarian dan filter tetap responsif pada data transaksi yang bertambah.
- Dashboard menggunakan agregasi yang efisien dan tidak memuat seluruh transaksi sekaligus.
- Target awal: halaman operasional utama tampil dalam sekitar 3 detik pada kondisi server dan jaringan normal; target akhir ditetapkan setelah pengujian data nyata.

### 15.3 Ketersediaan dan pencadangan
- Database memiliki jadwal backup rutin.
- File dokumen dan bukti transaksi ikut dicadangkan.
- Prosedur pemulihan backup harus diuji.
- Sistem menampilkan pesan yang jelas ketika koneksi atau penyimpanan gagal.

### 15.4 Kemudahan penggunaan
- Tampilan responsif untuk desktop, tablet, dan ponsel.
- Tampilan Satpam harus mudah digunakan pada perangkat di pos gerbang.
- Tombol tindakan utama seperti Setujui Masuk, Tahan, Terbitkan Tagihan, Terbitkan Surat Jalan, dan Setujui Keluar harus jelas.
- Form menampilkan validasi yang spesifik dan pesan kesalahan yang mudah dipahami.
- Status menggunakan label dan warna yang konsisten.

### 15.5 Bahasa dan waktu
- Bahasa antarmuka utama: Bahasa Indonesia.
- Zona waktu operasional yang ditampilkan: WIB (Asia/Jakarta).
- Format tanggal dan nominal mengikuti format Indonesia.

## 16. Kriteria Penerimaan (Acceptance Criteria)

### Skenario A — Client membuat pesanan
- Client dapat mendaftar dan masuk.
- Client dapat mengirim pesanan dengan seluruh data wajib.
- Sistem membuat nomor pesanan unik.
- Pesanan muncul pada antrean internal yang sesuai.
- Client dapat melihat status awal pesanan.

### Skenario B — Satpam memeriksa kendaraan masuk
- Satpam dapat menemukan pesanan.
- Data kendaraan dan driver dapat dibandingkan dengan data pesanan.
- Persetujuan masuk menyimpan waktu dan akun Satpam.
- Ketidaksesuaian dapat ditahan dengan alasan.
- Status dan dashboard bagian terkait diperbarui.

### Skenario C — Kepala Gudang memeriksa dan menyelesaikan pekerjaan
- Kepala Gudang dapat melihat informasi permintaan Client.
- Kepala Gudang dapat mencatat muatan dan kuantitas aktual.
- Kepala Gudang dapat menyetujui, meminta klarifikasi, atau menolak.
- Kepala Gudang dapat mencatat proses dan hasil akhir.
- Hasil akhir otomatis tersedia untuk Keuangan.

### Skenario D — Keuangan menerbitkan tagihan
- Keuangan hanya dapat membuat tagihan dari hasil pekerjaan yang telah diselesaikan.
- Perhitungan menampilkan rincian dan total.
- Tagihan yang diterbitkan muncul pada akun Client yang benar.
- Client dapat membuka dan mengunduh tagihan.

### Skenario E — Pembayaran dan barang keluar
- Bukti pembayaran tidak otomatis mengubah status menjadi lunas.
- Keuangan dapat memverifikasi pembayaran.
- Apabila pembayaran belum lunas, sistem memblokir penerbitan surat jalan dan persetujuan keluar.
- Setelah lunas, Kepala Gudang dapat menerbitkan surat jalan.
- Satpam hanya dapat menyetujui keluar jika surat jalan dan data kendaraan/muatan valid.
- Sistem menyimpan transaksi keluar dan menyelesaikan pesanan.

### Skenario F — Biaya operasional dan laporan
- Keuangan dapat mencatat biaya operasional dengan kategori dan nominal.
- Transaksi tercatat di buku keuangan.
- Owner dapat melihat pemasukan jasa, biaya operasional, tagihan belum dibayar, dan ringkasan kas.
- Laporan dapat difilter dan diekspor ke PDF/Excel.

### Skenario G — Pembatasan hak akses
- Client tidak dapat melihat data Client lain.
- Satpam tidak dapat memverifikasi pembayaran atau mengubah hasil gudang.
- Kepala Gudang tidak dapat menyatakan tagihan lunas.
- Keuangan tidak dapat menyetujui kendaraan masuk/keluar.
- Owner dapat melihat lintas bagian sesuai kewenangan pemantauan.
- Tindakan penting memiliki jejak audit.

## 17. Prioritas Pengembangan MVP

### Tahap 1 — Fondasi dan autentikasi
- Struktur aplikasi, database, login, role, permission.
- Registrasi dan profil Client.
- Manajemen akun pengguna untuk kelima peran (Client, Satpam, Kepala Gudang, Keuangan, dan Owner).
- Layout dashboard berdasarkan role.

### Tahap 2 — Pesanan dan gerbang masuk
- Form pengajuan pesanan Client.
- Nomor pesanan otomatis.
- Dashboard antrean internal.
- Pemeriksaan dan pencatatan kendaraan masuk oleh Satpam.

### Tahap 3 — Gudang dan hasil layanan
- Pemeriksaan muatan.
- Persetujuan pekerjaan.
- Pembaruan proses.
- Pencatatan hasil akhir.

### Tahap 4 — Keuangan dan tagihan
- Master tarif.
- Perhitungan jasa.
- Penerbitan tagihan PDF.
- Portal tagihan Client.
- Pencatatan dan verifikasi pembayaran.

### Tahap 5 — Surat jalan dan gerbang keluar
- Pembuatan surat jalan oleh Kepala Gudang.
- Validasi lunas otomatis.
- Pemeriksaan kendaraan keluar oleh Satpam.
- Penyelesaian pesanan.

### Tahap 6 — Buku keuangan dan Owner dashboard
- Pencatatan biaya operasional.
- Buku kas masuk dan keluar.
- Laporan PDF/Excel.
- Dashboard Owner dan rekap lintas bagian.

### Tahap 7 — Penguatan sistem
- Audit log menyeluruh.
- Pengujian keamanan dan pembatasan data Client.
- Pengujian alur ujung-ke-ujung.
- Backup dan pemulihan.
- Uji coba bersama perwakilan setiap bagian PKDI.

## 18. Hal yang Perlu Divalidasi dengan PKDI

Keputusan berikut belum ditentukan secara lengkap dari kebutuhan awal. Jangan mengunci aturan atau membuat asumsi permanen sebelum dikonfirmasi kepada pihak PKDI.

| No. | Hal yang Perlu Dikonfirmasi | Rekomendasi Awal |
|---|---|---|
| 1 | Apakah akun Client langsung aktif setelah registrasi atau perlu diverifikasi? Siapa yang memverifikasi? | Client dapat mendaftar sendiri; aktivasi/verifikasi perusahaan dilakukan sebelum pesanan pertama bila dibutuhkan. Tentukan apakah Owner atau Keuangan yang bertugas. |
| 2 | Daftar pilihan jenis kategori dan jenis muatan drum. | Buat master data yang dapat dikelola pihak berwenang, bukan pilihan hard-coded. |
| 3 | Apakah layanan hanya “diperbarui” dan “diperbaiki”, atau ada jenis jasa lain? | Gunakan master jenis layanan yang dapat ditambah. |
| 4 | Apakah nomor polisi dan jenis kendaraan wajib saat Client membuat pesanan? | Nomor polisi disarankan wajib; driver tetap opsional sesuai brief. Konfirmasi apakah kendaraan dapat berubah. |
| 5 | Apakah kendaraan masuk dan keluar selalu kendaraan yang sama? | Simpan data kendaraan masuk dan keluar sebagai catatan terpisah; perubahan harus disertai alasan. |
| 6 | Satuan kuantitas muatan dan hasil kerja. | Tentukan apakah per drum, unit, kilogram, ton, atau satuan lain. |
| 7 | Bagaimana tarif jasa dihitung? | Tetapkan tarif berdasarkan jenis jasa, kategori, kuantitas, atau kombinasi; tarif memiliki tanggal berlaku. |
| 8 | Apakah ada pajak, biaya tambahan, diskon, uang muka, atau pembayaran sebagian? | Sediakan struktur perhitungan fleksibel, tetapi aktifkan komponen setelah aturan resmi dikonfirmasi. |
| 9 | Apakah Client melakukan pembayaran melalui payment gateway atau transfer/manual? | MVP menggunakan pencatatan dan verifikasi manual oleh Keuangan; payment gateway tahap lanjutan. |
| 10 | Apakah barang harus menunggu pelunasan penuh sebelum keluar tanpa pengecualian? | Default sistem: wajib lunas penuh. Pengecualian hanya jika ada kebijakan dan approval resmi. |
| 11 | Siapa yang boleh membuat/merevisi tarif dan menyetujui koreksi tagihan? | Default: Keuangan mengelola draft; Owner memantau. Tentukan apakah koreksi nominal besar perlu persetujuan Owner. |
| 12 | Apakah Client perlu mengunduh surat jalan atau hanya melihat status? | Default: surat jalan tersedia untuk internal; akses unduh Client ditentukan kebijakan PKDI. |
| 13 | Format resmi invoice dan surat jalan PKDI. | Minta contoh dokumen perusahaan untuk dijadikan acuan desain PDF. |
| 14 | Kategori biaya operasional dan apakah gaji dicatat per karyawan atau sebagai total pengeluaran. | Mulai dengan pencatatan biaya sederhana; detail gaji dibatasi untuk Keuangan dan Owner. |
| 15 | Apakah Owner hanya melihat data atau juga perlu menyetujui transaksi tertentu? | MVP read-only; daftar persetujuan Owner ditentukan setelah proses bisnis dikonfirmasi. |
| 16 | Kanal notifikasi yang digunakan. | Notifikasi di dalam aplikasi sebagai minimum; email/WhatsApp dapat menjadi integrasi lanjutan. |
| 17 | Apakah ada lebih dari satu lokasi gudang/gerbang atau satu lokasi operasional? | MVP satu lokasi; struktur dapat disiapkan agar tidak menghambat pengembangan multi-lokasi. |
| 18 | Kebijakan pembatalan pesanan dan penanganan barang yang ditolak/tidak selesai. | Tentukan batas tahapan pembatalan, biaya yang mungkin timbul, dan pihak yang menyetujui. |
| 19 | Apakah dokumen perlu QR code atau tanda tangan digital? | QR surat jalan dapat ditambahkan setelah format dan perangkat di gerbang dikonfirmasi. |
| 20 | Apakah buku keuangan cukup berupa kas masuk/keluar atau harus mengikuti format akuntansi tertentu? | MVP berupa buku kas dan rekap operasional; kebutuhan akuntansi resmi dibahas terpisah. |

## 19. Catatan Implementasi dan Prinsip Desain

- Bangun sistem berdasarkan alur bisnis dan hak akses, bukan hanya kumpulan halaman dashboard.
- Gunakan satu sumber data pesanan agar semua bagian membaca status transaksi yang sama.
- Pisahkan data permintaan awal Client, data pemeriksaan aktual, hasil pekerjaan, tagihan, pembayaran, dan data barang keluar. Jangan menimpa data awal ketika ada koreksi.
- Gunakan master data untuk kategori, jenis muatan, jenis layanan, tarif, dan kategori biaya operasional.
- Buat setiap tindakan penting melalui validasi backend dan transaksi database agar tidak terjadi status yang saling bertentangan.
- Gunakan soft delete atau mekanisme arsip untuk data operasional; transaksi keuangan dan audit log tidak boleh dihapus permanen oleh pengguna biasa.
- Desain UI harus berorientasi pada pekerjaan masing-masing bagian: antrean, status, detail, dan tindakan utama. Hindari dashboard yang hanya berisi banyak kartu statistik tanpa membantu pekerjaan harian.
- Prioritaskan antarmuka Satpam yang sederhana, cepat dibaca, dan nyaman digunakan pada layar kecil di pos gerbang.
- Tampilan Client harus berfokus pada pembuatan pesanan, timeline, tagihan, dan status pembayaran.
- Dashboard Owner harus menyajikan ringkasan lintas bagian dengan kemampuan membuka detail sumber data.

## 20. Definisi Selesai (Definition of Done)

Sebuah modul dianggap selesai apabila:

1. Fitur berjalan sesuai kebutuhan fungsional dan hak akses role.
2. Validasi input tersedia pada frontend dan backend.
3. Perubahan status tersimpan konsisten dan terlihat oleh role terkait.
4. Tindakan penting tercatat pada audit log.
5. Tidak ditemukan akses silang data Client pada pengujian.
6. Alur sukses dan alur gagal/ditahan sudah diuji.
7. Tidak ada tombol atau menu yang hanya menjadi dekorasi tanpa fungsi.
8. Tampilan responsif dan telah diuji pada ukuran desktop serta ponsel.
9. Dokumentasi singkat penggunaan dan perubahan modul tersedia.
10. Pengujian integrasi lintas bagian lulus, terutama alur pesanan sampai kendaraan keluar.

---

**Akhir dokumen PRD — Versi 1.0 Draft.**

Dokumen ini menjadi dasar diskusi dan validasi kebutuhan dengan Satpam, Kepala Gudang, Keuangan, Owner, serta perwakilan Client sebelum dibuatkan desain UI/UX, rancangan database final, dan tahapan development.
