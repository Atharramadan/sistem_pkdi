# PRODUCT REQUIREMENTS DOCUMENT (PRD)

# Sistem Manajemen Operasional dan Keuangan PT Pulo Kencana Drum Industri (PKDI)

| Informasi | Keterangan |
|---|---|
| Nama Produk | Sistem Manajemen Operasional dan Keuangan PKDI |
| Organisasi | PT Pulo Kencana Drum Industri (PKDI) |
| Versi | 2.0 — Draft berdasarkan riset perusahaan |
| Tanggal dasar riset | 18 November 2026 dan tindak lanjut diskusi |
| Status | Draft untuk validasi proses bisnis |
| Jenis aplikasi | Aplikasi web dengan portal Client dan aplikasi internal PKDI |
| Pengguna tahap awal | Client, Kepala Gudang, Keuangan, Owner |
| Teknologi yang direncanakan | PHP Laravel 13, MySQL; XAMPP untuk lingkungan pengembangan lokal |
| Dokumen acuan | PRD 1.0, catatan riset, dan transkrip diskusi PKDI yang disampaikan pengguna |

> **Catatan penting:** Dokumen ini menggambarkan target sistem perusahaan secara keseluruhan. Bagian khusus tugas akhir mahasiswa dibatasi pada Modul Gudang dan dijelaskan terpisah pada Bab 21. Kebutuhan yang belum dikonfirmasi ditandai **Perlu Validasi PKDI** dan tidak boleh dianggap sebagai keputusan final.

---

## Riwayat dan Arah Perubahan Dokumen

PRD 2.0 menggantikan alur utama PRD 1.0 yang sebelumnya berpusat pada pemeriksaan kendaraan oleh Satpam, proses gerbang masuk/keluar, dan persetujuan surat jalan. Berdasarkan riset lanjutan, sistem diarahkan untuk mendukung dua jenis transaksi utama PKDI: **penjualan drum** dan **jasa rekondisi drum**.

Perubahan utama dari PRD 1.0:

1. Menghapus Satpam dari role dan alur digital tahap awal.
2. Mempertahankan Kepala Gudang sebagai pengguna yang menangani penerimaan fisik, pemeriksaan, persiapan produk, dan pekerjaan rekondisi.
3. Memisahkan tanggung jawab operasional Gudang dari tanggung jawab Keuangan. Pesanan tidak harus melewati Keuangan sebelum pekerjaan Gudang dimulai.
4. Mengubah penagihan satu pesanan-satu invoice menjadi mekanisme antrean transaksi yang memungkinkan beberapa transaksi milik Client yang sama digabung ke satu invoice.
5. Menggunakan transfer bank manual dengan bukti pembayaran wajib diunggah Client dan diverifikasi Keuangan.
6. Memisahkan status operasional pesanan, status penagihan, dan status pembayaran.
7. Menunda Super Admin dan antarmuka pengelolaan seluruh akun/konfigurasi aplikasi ke tahap pengembangan lanjutan.
8. Menambahkan dua jenis transaksi, master produk drum, paket layanan rekondisi, administrasi Client, lampiran dokumen/foto, dan kebutuhan Customer Service Live Chat.
9. Memisahkan target sistem perusahaan dari ruang lingkup tugas akhir yang hanya mengerjakan Modul Gudang.

## Status Keputusan dalam PRD

- **Disepakati:** dinyatakan secara jelas oleh pengguna berdasarkan hasil riset atau keputusan lanjutan.
- **Rancangan:** usulan proses/fitur yang logis untuk mengoperasikan kebutuhan, tetapi masih perlu persetujuan PKDI.
- **Perlu Validasi PKDI:** informasi belum cukup atau tanggung jawab/prosedurnya belum dikonfirmasi.
- **Ditunda:** sengaja tidak dimasukkan ke tahap awal.

---

# 1. Ringkasan Produk

Sistem Manajemen Operasional dan Keuangan PKDI adalah aplikasi web terintegrasi untuk mendukung proses bisnis penjualan drum dan jasa rekondisi drum, mulai dari pengajuan oleh Client, pemeriksaan serta pelaksanaan pekerjaan oleh Kepala Gudang, penagihan oleh Keuangan, pembayaran, hingga pemantauan oleh Owner.

PKDI sebelumnya belum memiliki sistem digital terpusat untuk proses yang dibahas dalam riset; pencatatan masih dilakukan secara manual. Sistem baru diharapkan menjadi sumber data bersama agar informasi pelanggan, pesanan, produk, penerimaan drum, pekerjaan rekondisi, hasil pekerjaan, invoice, pembayaran, pemasukan, dan pengeluaran dapat ditelusuri.

Sistem memiliki dua sisi penggunaan:

- **Portal Client:** registrasi, pengisian administrasi perusahaan, permintaan/penawaran, pemesanan, komunikasi, pemantauan status, melihat invoice, dan mengunggah bukti pembayaran.
- **Portal Internal PKDI:** Kepala Gudang mengelola aktivitas barang dan pekerjaan; Keuangan mengelola penagihan serta transaksi keuangan; Owner memantau laporan.

Sistem tidak menjadikan Satpam sebagai perantara antara Client dan Gudang. Setelah Client mengajukan pesanan, pesanan masuk ke antrean operasional untuk ditinjau oleh pihak Gudang sesuai kewenangan yang disepakati. Keuangan menangani transaksi pada tahap penagihan dan pembayaran, bukan sebagai gerbang persetujuan pekerjaan Gudang.

## 1.1 Dua Jenis Transaksi Utama

### A. Penjualan Drum

Client memesan produk drum yang dijual PKDI. Kepala Gudang menerima informasi pesanan, memeriksa ketersediaan/kesiapan produk sesuai proses perusahaan, mencatat persiapan dan penyerahan/pengiriman, kemudian transaksi tersedia untuk ditagihkan.

### B. Jasa Rekondisi Drum

Client mengajukan drum untuk dikerjakan oleh PKDI. Kepala Gudang mencatat penerimaan fisik, jumlah dan kondisi drum, memproses pekerjaan sesuai paket layanan, mencatat hasil aktual, dan menandai pekerjaan siap diserahkan/dikirim. Data transaksi yang memenuhi ketentuan penagihan masuk ke antrean Keuangan.

## 1.2 Paket Layanan Rekondisi

Jenis layanan rekondisi yang digunakan pada dropdown Client adalah:

1. **Laundry**
2. **Laundry + Cat**
3. **Laundry + Cat + Sablon**

Pilihan tersebut disimpan sebagai master data layanan yang dikelola secara teknis pada tahap awal. Client memilih satu paket layanan dari dropdown; Client tidak mengetik nama layanan secara bebas. Harga paket ditentukan PKDI dan dikelola oleh Keuangan.

> Daftar ini mengikuti catatan riset pengguna. Nama, ruang lingkup, kombinasi, dan tarif setiap paket tetap perlu dicocokkan dengan daftar layanan resmi PKDI sebelum produksi aplikasi.

---

# 2. Latar Belakang dan Permasalahan

Berdasarkan hasil riset, PKDI membutuhkan sistem digital untuk mengurangi ketergantungan pada pencatatan manual dan komunikasi yang tersebar. Perusahaan membutuhkan pencatatan terpusat yang dapat menghubungkan pesanan, pekerjaan Gudang, penagihan, dan pembayaran.

Permasalahan/kebutuhan yang menjadi dasar pengembangan:

1. Client membutuhkan portal untuk mendaftar, melengkapi data perusahaan, menyampaikan kebutuhan, mengajukan pesanan, dan memantau transaksi.
2. PKDI membutuhkan data Client dan dokumen administrasi, termasuk NPWP dan NIB.
3. Jenis produk drum dan paket layanan perlu dipilih melalui dropdown untuk mengurangi variasi penulisan.
4. Kepala Gudang membutuhkan antrean pekerjaan yang berasal dari pesanan Client, serta kemampuan mencatat penerimaan drum dan kondisi aktual.
5. Jasa rekondisi membutuhkan pencatatan tahapan dan hasil pengerjaan agar jumlah aktual dapat menjadi dasar penagihan.
6. Keuangan membutuhkan antrean transaksi yang belum ditagihkan, dapat difilter berdasarkan Client dan periode, serta dapat dipilih beberapa transaksi untuk diterbitkan menjadi satu invoice.
7. Client membutuhkan akses ke invoice dan status pembayaran.
8. Pembayaran manual perlu memiliki bukti yang diunggah Client dan diverifikasi oleh Keuangan.
9. Keuangan membutuhkan pencatatan pemasukan dan pengeluaran serta rekapitulasi.
10. Owner membutuhkan ringkasan kegiatan operasional dan keuangan.
11. Informasi operasional dan keuangan harus memiliki jejak perubahan untuk mengurangi sengketa dan kesalahan pencatatan.

---

# 3. Tujuan Produk

1. Mendigitalisasi proses penjualan drum dan jasa rekondisi PKDI.
2. Menyediakan portal Client sebagai tempat pengajuan pesanan, administrasi, komunikasi, dan pemantauan transaksi.
3. Menyediakan antrean pekerjaan bagi Kepala Gudang tanpa mewajibkan proses persetujuan Satpam.
4. Mencatat penerimaan fisik, pemeriksaan kondisi, pekerjaan, dan hasil aktual drum.
5. Menghubungkan hasil/penyelesaian transaksi dengan antrean penagihan Keuangan.
6. Mendukung penggabungan beberapa transaksi milik Client yang sama ke dalam satu invoice.
7. Mendigitalisasi transfer manual melalui unggah bukti pembayaran dan verifikasi Keuangan.
8. Menyediakan pencatatan pemasukan dan pengeluaran serta laporan ringkas.
9. Menyediakan dashboard Owner untuk memantau operasional dan kondisi keuangan.
10. Menyediakan fondasi aplikasi yang dapat dikembangkan lebih lanjut, tanpa memasukkan Super Admin dan pengelolaan seluruh akun ke tahap awal.

---

# 4. Indikator Keberhasilan

Sistem dianggap mencapai tujuan awal apabila:

1. Client dapat mendaftar dan memiliki profil perusahaan yang terpisah dari Client lain.
2. Client dapat mengajukan pesanan penjualan drum atau jasa rekondisi.
3. Jenis drum dan paket rekondisi dipilih dari pilihan master/dropdown yang tersedia.
4. Setiap pesanan memiliki nomor unik dan riwayat perubahan status.
5. Kepala Gudang dapat melihat antrean pesanan yang menjadi tanggung jawabnya.
6. Kepala Gudang dapat mencatat penerimaan drum, jumlah, kondisi, dan hasil pengerjaan.
7. Data transaksi yang memenuhi kriteria penagihan muncul satu kali pada antrean Keuangan.
8. Keuangan dapat memilih beberapa transaksi belum ditagihkan milik Client yang sama dan menerbitkannya menjadi satu invoice.
9. Invoice yang diterbitkan memiliki nomor unik, rincian transaksi, total tagihan, dan file PDF.
10. Client dapat mengunggah bukti transfer.
11. Bukti transfer tidak otomatis mengubah status menjadi lunas; Keuangan harus memverifikasinya.
12. Pembayaran yang sudah diverifikasi tercatat sebagai pemasukan tanpa pencatatan ganda.
13. Pengeluaran dapat dicatat dengan kategori dan bukti pendukung.
14. Owner dapat melihat ringkasan pesanan, pekerjaan, invoice, pembayaran, pemasukan, dan pengeluaran.
15. Client tidak dapat mengakses data Client lain.

---

# 5. Pengguna, Peran, dan Hak Akses

Tahap awal menggunakan **empat role** berikut. Satpam tidak termasuk dalam alur digital tahap awal. Super Admin dan pengelolaan akun internal melalui antarmuka khusus ditunda.

## 5.1 Client

Client merupakan perusahaan/pelanggan PKDI. Akun Client dapat didaftarkan melalui portal.

Hak akses:

- Mendaftar dan login.
- Melengkapi profil perusahaan dan kontak penanggung jawab.
- Mengisi NPWP dan mengunggah dokumen administrasi yang diminta, termasuk NIB.
- Melihat status kelengkapan/verifikasi dokumen sesuai kebijakan PKDI.
- Melihat master produk dan layanan yang tersedia.
- Mengajukan pesanan penjualan drum.
- Mengajukan jasa rekondisi drum.
- Mengisi kuantitas, tanggal kebutuhan pengiriman/penyerahan, PO, spesifikasi, dan catatan.
- Mengunggah dokumen dan foto pendukung.
- Melihat daftar, detail, dan status pesanan milik perusahaannya.
- Menggunakan fasilitas komunikasi/Live Chat apabila diaktifkan.
- Melihat invoice PDF yang diterbitkan untuk perusahaannya.
- Melakukan transfer manual dan wajib mengunggah bukti pembayaran.
- Melihat status verifikasi dan riwayat pembayaran.
- Menerima notifikasi perubahan status.

Client tidak dapat:

- Mengubah hasil pemeriksaan fisik atau hasil pekerjaan Kepala Gudang.
- Mengubah tarif resmi dan total invoice yang telah diterbitkan.
- Mengubah status pembayaran menjadi lunas.
- Melihat data perusahaan Client lain.
- Mengubah catatan internal PKDI.

## 5.2 Kepala Gudang

Kepala Gudang bertanggung jawab atas penerimaan/persiapan barang, pemeriksaan, pekerjaan rekondisi, dan pencatatan hasil operasional.

Hak akses:

- Melihat antrean pesanan yang diajukan Client.
- Melihat informasi pesanan dan dokumen/spesifikasi yang relevan.
- Meninjau kelayakan operasional dan ketersediaan/kesiapan barang sesuai prosedur PKDI.
- Meminta klarifikasi atau memperbarui hasil peninjauan dengan alasan.
- Untuk jasa rekondisi, mencatat penerimaan fisik drum.
- Mencatat kuantitas aktual, kondisi, dan ketidaksesuaian barang.
- Mencatat hasil pemeriksaan dan proses rekondisi.
- Memperbarui status pekerjaan sesuai tahapan yang ditetapkan.
- Mencatat kuantitas hasil, kuantitas tidak selesai/tidak layak, dan catatan hasil.
- Menandai barang/pesanan siap diserahkan atau dikirim.
- Mengirim hasil transaksi yang memenuhi ketentuan ke antrean penagihan.
- Melihat riwayat pekerjaan dan transaksi Gudang.

Kepala Gudang tidak dapat:

- Mengubah tarif resmi.
- Menerbitkan atau mengubah invoice.
- Memverifikasi pembayaran.
- Mencatat pengeluaran keuangan.
- Menandai invoice lunas.

## 5.3 Keuangan

Keuangan berfokus pada penagihan dan pengelolaan keuangan. Keuangan **bukan perantara yang harus menyetujui pesanan sebelum Kepala Gudang mulai bekerja**.

Hak akses:

- Melihat antrean transaksi yang memenuhi kriteria penagihan.
- Memfilter antrean berdasarkan Client, jenis transaksi, tanggal/periode, dan status.
- Memilih beberapa transaksi milik Client yang sama untuk digabung dalam satu invoice.
- Mengelola tarif penjualan drum dan jasa rekondisi sesuai kewenangan yang diberikan PKDI.
- Menghitung dan memeriksa rincian tagihan.
- Membuat draft, menerbitkan, dan mencetak/mengunduh invoice PDF.
- Melihat riwayat invoice dan statusnya.
- Memeriksa bukti pembayaran yang diunggah Client.
- Menerima/menolak bukti pembayaran disertai alasan.
- Mencatat pembayaran terverifikasi dan sisa tagihan.
- Mencatat pemasukan dan pengeluaran.
- Mengelola kategori biaya operasional.
- Melampirkan bukti pengeluaran.
- Melihat dan mengekspor rekap keuangan sesuai fitur yang tersedia.

Keuangan tidak dapat:

- Mengubah kondisi atau hasil pemeriksaan fisik drum.
- Mengubah hasil pekerjaan Gudang.
- Mengelola pekerjaan produksi.
- Mengubah data awal pesanan tanpa jejak perubahan.

## 5.4 Owner

Owner berperan sebagai pemantau kegiatan perusahaan.

Hak akses:

- Melihat ringkasan pesanan berdasarkan jenis transaksi dan status.
- Melihat ringkasan pekerjaan Gudang.
- Melihat rekap transaksi penjualan drum dan jasa rekondisi.
- Melihat invoice terbit, tagihan belum dibayar, pembayaran diterima, dan piutang.
- Melihat pemasukan dan pengeluaran.
- Memfilter laporan berdasarkan periode dan Client sesuai kebutuhan.
- Melihat detail transaksi dan riwayat aktivitas yang diizinkan.
- Mengunduh laporan yang diizinkan.

Prinsip awal: Owner bersifat **read-only/monitoring**. Hak persetujuan atau perubahan transaksi oleh Owner belum termasuk kecuali kelak ditetapkan PKDI.

## 5.5 Akun Internal dan Super Admin

- Registrasi mandiri hanya tersedia untuk Client.
- Tidak ada role Super Admin pada tahap awal.
- Antarmuka untuk membuat, menghapus, menonaktifkan, dan mengatur seluruh akun internal belum termasuk scope awal.
- Akun internal awal disiapkan melalui proses provisioning teknis yang dikendalikan pengembang/penanggung jawab sistem sebelum aplikasi digunakan. Kredensial awal harus diserahkan secara aman kepada pemilik akun dan tidak ditanam sebagai akun publik di aplikasi.
- Pengelolaan akun terpusat dan role Super Admin masuk roadmap pengembangan lanjutan.

> **Perlu Validasi PKDI:** siapa di perusahaan yang berwenang meminta pembuatan/perubahan akun internal selama tahap awal ketika halaman pengelolaan pengguna belum tersedia.

---

# 6. Ruang Lingkup Produk

## 6.1 Termasuk dalam target sistem perusahaan

- Registrasi dan login Client.
- Profil perusahaan Client.
- Administrasi Client: NPWP, NIB, dan dokumen pendukung.
- Dua jenis transaksi: penjualan drum dan jasa rekondisi drum.
- Master data jenis drum/produk.
- Dropdown paket layanan rekondisi: Laundry; Laundry + Cat; Laundry + Cat + Sablon.
- Form pesanan dan dokumen PO.
- Lampiran file dan foto, termasuk pengambilan foto menggunakan kamera perangkat bila browser/perangkat mendukung.
- Notifikasi pesanan dan perubahan status.
- Customer Service Live Chat, dengan penanggung jawab internal yang perlu ditetapkan.
- Dashboard Kepala Gudang.
- Pencatatan penerimaan dan pemeriksaan drum.
- Pencatatan pekerjaan dan hasil rekondisi.
- Pencatatan persiapan/penyerahan produk penjualan drum.
- Informasi pengiriman/penyerahan.
- Antrean transaksi siap ditagihkan.
- Invoice gabungan untuk beberapa transaksi milik Client yang sama.
- Invoice PDF.
- Transfer bank manual dan unggah bukti pembayaran wajib.
- Verifikasi pembayaran oleh Keuangan.
- Pencatatan pemasukan dan pengeluaran.
- Dashboard Owner dan laporan ringkas.
- Pencarian, filter, riwayat, audit log, dan ekspor yang diperlukan.

## 6.2 Tidak termasuk dalam tahap awal

- Role dan dashboard Satpam.
- Pemeriksaan kendaraan masuk/keluar oleh Satpam.
- Integrasi gerbang/akses kendaraan.
- Super Admin dan UI pengelolaan seluruh akun internal.
- Payment gateway, QRIS dinamis, dan Virtual Account.
- Integrasi mutasi rekening otomatis.
- Aplikasi native Android/iOS.
- Sistem akuntansi penuh (jurnal umum, neraca, laba rugi sesuai standar akuntansi, pelaporan pajak resmi).
- HRIS, payroll, absensi, dan penggajian.
- Manajemen inventaris/stok penuh.
- Multi-cabang/multi-perusahaan operator.
- Integrasi IoT atau perangkat produksi.

## 6.3 Batasan Stok Penjualan Drum

Penjualan drum mengharuskan PKDI memastikan ketersediaan produk. Akan tetapi, kebutuhan stok penuh belum dikonfirmasi.

Rancangan awal yang disarankan:

- Kepala Gudang dapat memberikan status ketersediaan/kesiapan produk untuk pesanan.
- Sistem menyimpan kuantitas yang dipesan dan kuantitas yang disiapkan/diserahkan.
- Sistem tidak mengklaim sebagai sistem inventaris penuh (mutasi lokasi, kartu stok detail, opname, batch/serial) sampai kebutuhan tersebut divalidasi.

**Perlu Validasi PKDI:** apakah pencatatan stok sederhana sudah cukup untuk penjualan drum atau perusahaan membutuhkan modul persediaan lengkap.

---

# 7. Kategori Produk dan Layanan

## 7.1 Master Jenis Produk/Drum

Pilihan yang sudah muncul dalam catatan riset:

- Drum besi.
- Poligen 150 liter.
- Poligen 120 liter.
- Jerigen.

Daftar ini merupakan pilihan awal dan perlu dicocokkan dengan master produk resmi PKDI. Spesifikasi rinci seperti ukuran, tipe tutup, merek, warna, dan kriteria teknis hanya ditambahkan setelah datanya diberikan perusahaan.

## 7.2 Dropdown Jenis Layanan Rekondisi

Form Client menggunakan satu dropdown dengan opsi:

| Nilai pilihan | Keterangan ringkas |
|---|---|
| Laundry | Pencucian drum |
| Laundry + Cat | Pencucian dan pengecatan drum |
| Laundry + Cat + Sablon | Pencucian, pengecatan, dan penyablonan drum |

Ketentuan:

- Client memilih paket dari dropdown; tidak mengetik nama layanan secara bebas.
- Sistem menyimpan ID/identitas master layanan, bukan hanya teks bebas.
- Tarif ditetapkan Keuangan berdasarkan kebijakan PKDI.
- Client tidak dapat mengubah tarif.
- Jika paket/layanan baru dibutuhkan, penambahannya harus dilakukan melalui perubahan master data oleh pihak teknis berwenang pada tahap awal; UI Super Admin belum tersedia.

## 7.3 Perbedaan Jenis Transaksi dan Jenis Layanan

- **Jenis transaksi** menentukan model bisnis: `Penjualan Drum` atau `Jasa Rekondisi`.
- **Jenis produk** menentukan drum/produk yang dipesan atau diterima.
- **Jenis layanan rekondisi** hanya digunakan jika jenis transaksi adalah `Jasa Rekondisi`.

Untuk transaksi penjualan drum, dropdown paket Laundry tidak ditampilkan. Untuk transaksi rekondisi, Client wajib memilih salah satu paket layanan yang tersedia.

---

# 8. Proses Bisnis (To-Be)

## 8.1 Registrasi dan Administrasi Client

1. Client membuka portal PKDI.
2. Client membuat akun menggunakan informasi login yang diperlukan.
3. Client melengkapi profil perusahaan, alamat, penanggung jawab, email, dan nomor kontak.
4. Client mengisi NPWP dan mengunggah dokumen administrasi yang diminta, termasuk NIB.
5. Client dapat melengkapi dokumen pendukung seperti kontrak, SPH, atau dokumen lain apabila diminta PKDI.
6. Sistem menyimpan status kelengkapan dokumen.
7. Client menerima notifikasi bila ada dokumen yang perlu diperbaiki.

**Perlu Validasi PKDI:** apakah verifikasi NPWP/NIB harus diselesaikan sebelum pesanan pertama dapat diajukan, atau Client boleh mengajukan pesanan sambil menunggu verifikasi.

## 8.2 Permintaan Informasi/Penawaran sebelum Pesanan

1. Client dapat menghubungi PKDI untuk menanyakan produk, layanan, harga, warna, spesifikasi, sampel, dan kebutuhan lain.
2. Komunikasi dapat berlangsung melalui Live Chat bila fitur telah diaktifkan.
3. Pembicaraan penawaran belum otomatis menjadi pesanan resmi.
4. Setelah kebutuhan dan kesepakatan awal jelas, Client mengajukan pesanan melalui form.

Pemisahan ini penting agar pertanyaan/penawaran tidak dihitung sebagai pesanan yang sudah harus diproses atau ditagihkan.

**Perlu Validasi PKDI:** siapa petugas internal yang menangani Live Chat dan penawaran. Keuangan tetap berfokus pada penagihan/keuangan; Live Chat tidak otomatis dibebankan ke Keuangan.

## 8.3 Alur Penjualan Drum

1. Client memilih jenis transaksi **Penjualan Drum**.
2. Client memilih jenis drum/produk dari dropdown master produk.
3. Client mengisi kuantitas, satuan jika diperlukan, tanggal kebutuhan pengiriman/penyerahan, PO, dan catatan spesifikasi.
4. Client mengunggah PO dan dokumen pendukung yang diwajibkan.
5. Sistem membuat nomor pesanan unik dan mengirim notifikasi kepada Kepala Gudang.
6. Kepala Gudang meninjau pesanan, ketersediaan/kesiapan produk, kuantitas, dan kebutuhan penyerahan.
7. Jika ada ketidaksesuaian, Kepala Gudang meminta klarifikasi atau memperbarui status dengan alasan.
8. Jika dapat dipenuhi, Kepala Gudang mencatat pesanan diterima untuk persiapan.
9. Kepala Gudang mencatat kuantitas yang disiapkan dan kuantitas yang benar-benar diserahkan/dikirim.
10. Status operasional diperbarui sampai barang diterima Client atau diselesaikan sesuai bukti penyerahan.
11. Transaksi masuk ke antrean penagihan sesuai ketentuan PKDI.
12. Keuangan memilih transaksi tersebut untuk invoice individual atau invoice gabungan.
13. Client menerima invoice, melakukan transfer, dan mengunggah bukti pembayaran.
14. Keuangan memverifikasi pembayaran.

**Perlu Validasi PKDI:** apakah invoice penjualan drum diterbitkan sebelum pengiriman, setelah barang diserahkan, atau mengikuti kesepakatan pelanggan. PRD tidak mengunci salah satu sebelum dikonfirmasi.

## 8.4 Alur Jasa Rekondisi Drum

1. Client memilih jenis transaksi **Jasa Rekondisi Drum**.
2. Client memilih jenis drum dari dropdown.
3. Client memilih satu paket dari dropdown: Laundry; Laundry + Cat; atau Laundry + Cat + Sablon.
4. Client mengisi kuantitas perkiraan, tanggal kebutuhan, spesifikasi, warna/hasil yang diminta bila relevan, PO, dan catatan.
5. Client mengunggah dokumen atau foto pendukung.
6. Sistem membuat nomor pesanan unik dan mengirimkannya ke antrean Kepala Gudang.
7. Kepala Gudang meninjau kebutuhan dan informasi yang diajukan. Jika perlu, Kepala Gudang meminta klarifikasi.
8. Saat drum secara fisik diterima PKDI, Kepala Gudang mencatat tanggal penerimaan, jenis, kuantitas aktual, dan kondisi awal drum.
9. Jika jumlah/kondisi aktual berbeda dengan permintaan awal, sistem menyimpan perbedaan tanpa menimpa data pengajuan Client.
10. Kepala Gudang mencatat proses pengerjaan sesuai paket layanan.
11. Kepala Gudang mencatat hasil akhir, kuantitas selesai, kuantitas tidak selesai/tidak layak (jika ada), dan catatan hasil.
12. Kepala Gudang menandai pekerjaan selesai dan barang siap diserahkan/dikirim.
13. Transaksi masuk ke antrean penagihan berdasarkan hasil aktual dan ketentuan tarif PKDI.
14. Keuangan memasukkan transaksi dalam invoice yang sesuai.
15. Client menerima invoice, melakukan transfer, dan mengunggah bukti pembayaran.
16. Keuangan memverifikasi pembayaran.

**Perlu Validasi PKDI:** apakah barang hasil rekondisi selalu dikirim kembali ke Client, diambil Client, atau dapat menggunakan kedua cara. Informasi kendaraan tidak wajib diinput Client; bila diperlukan, informasi pengiriman dilengkapi oleh pihak internal yang ditunjuk PKDI.

## 8.5 Penerimaan Drum oleh Kepala Gudang

Penerimaan fisik tidak melalui approval Satpam dalam sistem tahap awal.

Data minimal yang dicatat:

- Nomor pesanan.
- Client/perusahaan asal.
- Tanggal dan waktu penerimaan.
- Jenis drum.
- Kuantitas menurut pengajuan.
- Kuantitas aktual diterima.
- Kondisi awal drum.
- Catatan ketidaksesuaian.
- Foto dokumentasi bila diperlukan.
- Pengguna internal yang mencatat.

## 8.6 Pekerjaan dan Hasil Rekondisi

Catatan pekerjaan internal minimal meliputi:

- Paket layanan yang diminta.
- Tanggal mulai dan selesai jika dicatat.
- Status pekerjaan.
- Catatan pekerjaan.
- Kuantitas yang sedang/selesai dikerjakan.
- Kuantitas yang tidak dapat diselesaikan (bila ada).
- Catatan hasil dan dokumentasi.

Status internal dapat lebih rinci daripada status yang ditampilkan ke Client. Client hanya melihat ringkasan progres yang telah ditentukan PKDI.

## 8.7 Antrean Penagihan dan Invoice Gabungan

1. Transaksi penjualan drum atau rekondisi yang telah memenuhi syarat penagihan masuk ke antrean Keuangan.
2. Keuangan dapat memfilter antrean berdasarkan Client, tanggal/periode, jenis transaksi, dan status belum ditagihkan.
3. Keuangan memilih satu atau beberapa transaksi milik Client yang sama.
4. Sistem menghitung subtotal berdasarkan snapshot kuantitas dan tarif transaksi yang berlaku.
5. Keuangan meninjau rincian, melakukan koreksi melalui mekanisme yang tercatat, lalu menerbitkan invoice.
6. Satu invoice dapat memiliki beberapa baris transaksi (`invoice_items`).
7. Setelah invoice diterbitkan, transaksi terkait ditandai sudah ditagihkan dan tidak muncul lagi sebagai transaksi belum ditagihkan.
8. Invoice memiliki nomor unik, tanggal terbit, identitas Client, rincian transaksi, subtotal, komponen pajak/penyesuaian jika telah disepakati, total, dan instruksi pembayaran.
9. Invoice dapat diunduh sebagai PDF oleh Keuangan dan Client yang berhak.

**Disepakati:** invoice dapat menggabungkan beberapa transaksi dari Client yang sama.

**Perlu Validasi PKDI:** apakah transaksi penjualan drum dan jasa rekondisi boleh digabung dalam satu invoice yang sama, atau harus dipisahkan menurut jenis transaksi/ketentuan administrasi perusahaan. Sistem sebaiknya menyimpan jenis transaksi pada setiap baris invoice agar kedua pilihan dapat didukung setelah kebijakan diputuskan.

## 8.8 Pembayaran Manual dan Verifikasi

1. Client membuka invoice yang diterbitkan.
2. Client melihat total, rincian, rekening tujuan, dan petunjuk pembayaran yang disediakan PKDI.
3. Client melakukan transfer bank secara manual.
4. Client wajib mengunggah bukti transfer untuk pembayaran yang dilaporkan melalui portal.
5. Client mengisi informasi pembayaran seperti tanggal transfer dan nominal; referensi transaksi diisi jika tersedia.
6. Sistem membuat status **Menunggu Verifikasi**.
7. Keuangan memeriksa bukti dan kesesuaian nominal dengan rekening perusahaan.
8. Keuangan menyetujui atau menolak bukti. Penolakan harus disertai alasan agar Client dapat memperbaiki/mengirim ulang.
9. Pembayaran hanya dihitung sebagai pembayaran terverifikasi setelah disetujui Keuangan.
10. Status invoice menjadi **Lunas** jika jumlah pembayaran terverifikasi memenuhi total invoice.
11. Pembayaran terverifikasi tercatat satu kali sebagai pemasukan.

**Disepakati:** tahap awal menggunakan transfer bank manual; payment gateway, QRIS, dan Virtual Account tidak dibangun pada MVP. Bukti pembayaran wajib diunggah Client.

**Perlu Validasi PKDI:** apakah pembayaran sebagian diperbolehkan, batas waktu pembayaran, dan apakah bukti pembayaran harus diunggah per transaksi transfer.

## 8.9 Pencatatan Pemasukan dan Pengeluaran

Pemasukan:

- Pembayaran invoice yang telah diverifikasi.
- Sumber pemasukan lain yang secara resmi diizinkan untuk dicatat Keuangan.

Pengeluaran:

- Tanggal transaksi.
- Kategori biaya.
- Deskripsi.
- Nominal.
- Penerima/vendor jika ada.
- Metode pembayaran.
- Lampiran nota/kuitansi jika tersedia.
- Pengguna pencatat.

Sistem menyediakan ringkasan kas masuk dan kas keluar berdasarkan transaksi yang tercatat. Fitur ini bukan pengganti software akuntansi penuh.

## 8.10 Monitoring Owner

Owner melihat ringkasan pesanan, transaksi penjualan, jasa rekondisi, status pekerjaan, nilai invoice, pembayaran, piutang, pemasukan, dan pengeluaran. Owner tidak melakukan perubahan transaksi dalam rancangan awal.

---

# 9. Status, Workflow, dan Timeline

Status operasional, status penagihan, dan status pembayaran harus dipisahkan. Satu field status tidak boleh mewakili ketiga proses tersebut.

## 9.1 Status Operasional Internal

| Kode | Label | Penjelasan | Pengguna/sistem |
|---|---|---|---|
| `SUBMITTED` | Diajukan | Client mengirim pesanan | Client |
| `UNDER_REVIEW` | Ditinjau Gudang | Kepala Gudang memeriksa permintaan | Kepala Gudang |
| `NEEDS_CLARIFICATION` | Perlu Klarifikasi | Data/spesifikasi perlu dilengkapi | Kepala Gudang |
| `ACCEPTED` | Diterima untuk Diproses | Permintaan dapat dipenuhi secara operasional | Kepala Gudang |
| `WAITING_RECEIPT` | Menunggu Penerimaan Drum | Berlaku untuk jasa rekondisi bila drum belum diterima fisik | Sistem/Kepala Gudang |
| `RECEIVED` | Drum Diterima | Penerimaan fisik dicatat | Kepala Gudang |
| `PREPARING` | Persiapan Produk | Berlaku untuk penjualan drum | Kepala Gudang |
| `IN_PROGRESS` | Sedang Diproses | Berlaku untuk pekerjaan rekondisi | Kepala Gudang |
| `READY` | Siap Diserahkan/Dikirim | Barang siap diserahkan/dikirim | Kepala Gudang |
| `IN_DELIVERY` | Dalam Pengiriman | Bila pengiriman dilakukan PKDI dan status digunakan | Pihak internal yang ditunjuk |
| `DELIVERED` | Diterima Client | Penerimaan/penyerahan dikonfirmasi | Client/pihak internal sesuai prosedur |
| `COMPLETED` | Selesai | Tahap operasional selesai | Sistem/pihak berwenang |
| `CANCELLED` | Dibatalkan | Pembatalan dengan alasan dan aturan | Pihak berwenang sesuai kebijakan |

Status yang berlaku tergantung jenis transaksi. Contohnya, `IN_PROGRESS` digunakan untuk rekondisi dan tidak wajib muncul untuk penjualan drum siap jual.

## 9.2 Tampilan Tracker Client

Tracker Client menggunakan ringkasan status, bukan detail seluruh aktivitas internal Gudang.

Rancangan label ringkas:

`Diajukan → Ditinjau → Diproses/Persiapan → Siap Dikirim → Dalam Pengiriman → Diterima → Selesai`

Status invoice dan pembayaran ditampilkan di bagian terpisah:

`Belum Ditagihkan → Invoice Terbit → Menunggu Pembayaran → Menunggu Verifikasi → Lunas`

Client tidak melihat catatan internal, komentar internal, atau rincian proses yang tidak ditujukan untuk pelanggan.

## 9.3 Status Penagihan

| Kode | Label | Arti |
|---|---|---|
| `NOT_READY` | Belum Siap Ditagihkan | Syarat transaksi belum terpenuhi |
| `READY_TO_BILL` | Siap Ditagihkan | Transaksi tersedia di antrean Keuangan |
| `INVOICED` | Sudah Ditagihkan | Transaksi telah masuk invoice |
| `VOIDED` | Dibatalkan/Dikoreksi | Ditangani melalui prosedur pembatalan/koreksi yang tercatat |

## 9.4 Status Invoice dan Pembayaran

| Status | Arti |
|---|---|
| `UNPAID` | Invoice terbit, belum ada pembayaran terverifikasi |
| `PROOF_SUBMITTED` | Client mengirim bukti, menunggu pemeriksaan |
| `PROOF_REJECTED` | Bukti ditolak dengan alasan |
| `PARTIALLY_PAID` | Pembayaran terverifikasi belum memenuhi total invoice, jika pembayaran sebagian diizinkan |
| `PAID` | Total pembayaran terverifikasi telah memenuhi invoice |
| `VOIDED` | Invoice dibatalkan melalui prosedur resmi |

## 9.5 Aturan Perubahan Status

- Status hanya dapat diubah oleh role yang berwenang atau proses sistem yang sah.
- Status operasional tidak otomatis mengubah status pembayaran.
- Invoice terbit tidak otomatis berarti Client telah membayar.
- Bukti transfer yang diunggah tidak otomatis berarti pembayaran lunas.
- Transaksi yang sudah ditagihkan tidak boleh ditagihkan ulang.
- Setiap perubahan penting menyimpan pengguna, waktu, nilai sebelum/sesudah, dan alasan bila diperlukan.
- Status yang sudah selesai tidak boleh diubah diam-diam; koreksi harus meninggalkan riwayat.

---

# 10. Kebutuhan Fungsional

Prioritas: **Must** = wajib untuk alur minimum; **Should** = penting tetapi dapat dijadwalkan setelah alur inti; **Could** = tambahan; **Later** = pengembangan lanjutan.

## 10.1 Autentikasi dan Akun

| ID | Kebutuhan | Prioritas |
|---|---|---|
| FR-AUTH-01 | Sistem menyediakan login dan logout untuk empat role tahap awal. | Must |
| FR-AUTH-02 | Client dapat melakukan registrasi mandiri. | Must |
| FR-AUTH-03 | Client hanya dapat mengakses data perusahaan/pesanannya sendiri. | Must |
| FR-AUTH-04 | Akun internal disediakan melalui provisioning teknis sebelum penggunaan. | Must |
| FR-AUTH-05 | Halaman Super Admin/manajemen seluruh akun tidak tersedia pada tahap awal. | Must |
| FR-AUTH-06 | Backend memeriksa role/permission pada setiap tindakan yang dilindungi. | Must |
| FR-AUTH-07 | Sistem menyediakan mekanisme reset kata sandi yang aman sesuai fasilitas deployment. | Should |
| FR-AUTH-08 | Sistem mencatat aktivitas penting pengguna. | Must |

## 10.2 Portal Client dan Administrasi

| ID | Kebutuhan | Prioritas |
|---|---|---|
| FR-CLI-01 | Client dapat mengelola profil perusahaannya. | Must |
| FR-CLI-02 | Form administrasi menyediakan NPWP dan unggah NIB. | Must |
| FR-CLI-03 | Client dapat mengunggah PO dan dokumen pendukung pesanan. | Must |
| FR-CLI-04 | Client dapat mengambil/mengunggah foto melalui perangkat bila didukung browser. | Should |
| FR-CLI-05 | Sistem menampilkan status kelengkapan dokumen. | Must |
| FR-CLI-06 | Client dapat melihat daftar dan detail pesanannya sendiri. | Must |
| FR-CLI-07 | Client dapat melihat tracker ringkas. | Must |
| FR-CLI-08 | Client menerima notifikasi perubahan pesanan dan invoice. | Must |
| FR-CLI-09 | Client dapat melihat dan mengunduh invoice PDF. | Must |
| FR-CLI-10 | Client dapat mengunggah bukti pembayaran. | Must |

## 10.3 Pemesanan

| ID | Kebutuhan | Prioritas |
|---|---|---|
| FR-ORD-01 | Client dapat memilih jenis transaksi Penjualan Drum atau Jasa Rekondisi Drum. | Must |
| FR-ORD-02 | Sistem menampilkan field sesuai jenis transaksi yang dipilih. | Must |
| FR-ORD-03 | Client memilih jenis produk dari dropdown master. | Must |
| FR-ORD-04 | Untuk rekondisi, Client memilih Laundry, Laundry + Cat, atau Laundry + Cat + Sablon dari dropdown. | Must |
| FR-ORD-05 | Client mengisi kuantitas dan tanggal kebutuhan pengiriman/penyerahan. | Must |
| FR-ORD-06 | Sistem menerima nomor PO dan lampiran PO. | Must |
| FR-ORD-07 | Sistem menerima spesifikasi/catatan tambahan dan lampiran relevan. | Should |
| FR-ORD-08 | Sistem membuat nomor pesanan unik. | Must |
| FR-ORD-09 | Sistem mengirim pesanan ke antrean Kepala Gudang. | Must |
| FR-ORD-10 | Informasi kendaraan/driver tidak wajib diisi Client; bila diperlukan, data dilengkapi internal. | Must |
| FR-ORD-11 | Client dapat mengajukan klarifikasi melalui komunikasi yang tersedia. | Should |

## 10.4 Kepala Gudang dan Operasional

| ID | Kebutuhan | Prioritas |
|---|---|---|
| FR-WH-01 | Dashboard menampilkan antrean pesanan baru, ditinjau, pekerjaan aktif, dan siap diserahkan. | Must |
| FR-WH-02 | Kepala Gudang dapat melihat detail pesanan dan dokumen yang relevan. | Must |
| FR-WH-03 | Kepala Gudang dapat menandai pesanan perlu klarifikasi dengan alasan. | Must |
| FR-WH-04 | Kepala Gudang dapat meninjau kesiapan/ketersediaan produk penjualan drum. | Must |
| FR-WH-05 | Kepala Gudang dapat mencatat persiapan dan jumlah barang yang disiapkan/diserahkan untuk penjualan. | Must |
| FR-WH-06 | Kepala Gudang dapat mencatat penerimaan fisik drum untuk rekondisi. | Must |
| FR-WH-07 | Kepala Gudang dapat mencatat jumlah aktual dan kondisi awal drum. | Must |
| FR-WH-08 | Kepala Gudang dapat mencatat ketidaksesuaian terhadap pengajuan. | Must |
| FR-WH-09 | Kepala Gudang dapat mencatat paket layanan rekondisi dan progres internal. | Must |
| FR-WH-10 | Kepala Gudang dapat mencatat hasil akhir dan kuantitas aktual pekerjaan. | Must |
| FR-WH-11 | Kepala Gudang dapat menambahkan catatan dan dokumentasi foto pekerjaan bila diperlukan. | Should |
| FR-WH-12 | Sistem mengirim transaksi yang memenuhi syarat ke antrean penagihan. | Must |
| FR-WH-13 | Kepala Gudang dapat mencatat status siap diserahkan/dikirim dan data hasil penyerahan sesuai kewenangan. | Must |
| FR-WH-14 | Sistem menyimpan riwayat perubahan dan pengguna yang melakukan pencatatan. | Must |

## 10.5 Keuangan, Antrean Penagihan, dan Invoice

| ID | Kebutuhan | Prioritas |
|---|---|---|
| FR-FIN-01 | Dashboard menampilkan transaksi yang siap ditagihkan dan belum masuk invoice. | Must |
| FR-FIN-02 | Keuangan dapat memfilter antrean berdasarkan Client, periode, jenis transaksi, dan status. | Must |
| FR-FIN-03 | Keuangan dapat memilih beberapa transaksi dari Client yang sama untuk satu invoice. | Must |
| FR-FIN-04 | Sistem mencegah transaksi yang sudah ditagihkan dipilih ulang. | Must |
| FR-FIN-05 | Sistem menghitung subtotal dan total dari rincian transaksi yang dipilih. | Must |
| FR-FIN-06 | Sistem menyimpan snapshot tarif dan kuantitas ketika transaksi dibuat/ditagihkan agar perubahan tarif berikutnya tidak mengubah tagihan lama. | Must |
| FR-FIN-07 | Keuangan dapat menyimpan draft invoice. | Must |
| FR-FIN-08 | Keuangan dapat menerbitkan invoice dengan nomor unik. | Must |
| FR-FIN-09 | Invoice menampilkan identitas PKDI dan Client, nomor, tanggal, rincian transaksi, total, dan instruksi pembayaran. | Must |
| FR-FIN-10 | Invoice dapat diunduh/dicetak sebagai PDF. | Must |
| FR-FIN-11 | Invoice terbit dapat dilihat Client yang terkait. | Must |
| FR-FIN-12 | Koreksi/pembatalan invoice memiliki alasan dan riwayat audit. | Must |
| FR-FIN-13 | Sistem mendukung filter invoice berdasarkan Client, periode, dan status. | Must |
| FR-FIN-14 | Penggabungan jenis transaksi berbeda dapat diatur setelah kebijakan PKDI divalidasi. | Should |

## 10.6 Pembayaran Manual

| ID | Kebutuhan | Prioritas |
|---|---|---|
| FR-PAY-01 | Sistem menampilkan instruksi rekening transfer resmi PKDI pada invoice/halaman pembayaran. | Must |
| FR-PAY-02 | Client wajib mengunggah bukti transfer ketika melaporkan pembayaran. | Must |
| FR-PAY-03 | Client mengisi nominal dan tanggal transfer; referensi diisi bila tersedia. | Must |
| FR-PAY-04 | Bukti yang diunggah masuk antrean verifikasi Keuangan. | Must |
| FR-PAY-05 | Keuangan dapat menerima atau menolak bukti dengan catatan. | Must |
| FR-PAY-06 | Sistem tidak menganggap upload bukti sebagai pembayaran lunas. | Must |
| FR-PAY-07 | Status PAID hanya diberikan setelah jumlah pembayaran terverifikasi memenuhi total invoice. | Must |
| FR-PAY-08 | Pembayaran terverifikasi tercatat satu kali pada pemasukan/cashbook. | Must |
| FR-PAY-09 | Riwayat bukti dan keputusan verifikasi tersimpan. | Must |
| FR-PAY-10 | Payment gateway, QRIS dinamis, dan VA tidak dibangun pada tahap awal. | Must |

## 10.7 Pemasukan, Pengeluaran, dan Rekap Keuangan

| ID | Kebutuhan | Prioritas |
|---|---|---|
| FR-CASH-01 | Keuangan dapat melihat pemasukan dari pembayaran terverifikasi. | Must |
| FR-CASH-02 | Keuangan dapat mencatat pemasukan manual lain apabila diizinkan. | Should |
| FR-CASH-03 | Keuangan dapat mencatat pengeluaran dengan tanggal, kategori, uraian, nominal, dan penerima/vendor bila ada. | Must |
| FR-CASH-04 | Keuangan dapat mengunggah bukti pengeluaran. | Must |
| FR-CASH-05 | Sistem menyediakan filter transaksi berdasarkan periode dan kategori. | Must |
| FR-CASH-06 | Sistem mencegah pembayaran invoice tercatat dua kali sebagai pemasukan. | Must |
| FR-CASH-07 | Sistem menampilkan ringkasan kas masuk dan kas keluar berdasarkan data yang dicatat. | Must |
| FR-CASH-08 | Laporan dapat diekspor ke PDF/Excel sesuai kebutuhan dan permission. | Should |

## 10.8 Dashboard Owner

| ID | Kebutuhan | Prioritas |
|---|---|---|
| FR-OWN-01 | Owner dapat melihat jumlah pesanan berdasarkan jenis dan status. | Must |
| FR-OWN-02 | Owner dapat melihat pekerjaan Gudang yang aktif dan selesai. | Must |
| FR-OWN-03 | Owner dapat melihat ringkasan penjualan drum dan jasa rekondisi. | Must |
| FR-OWN-04 | Owner dapat melihat invoice terbit dan piutang. | Must |
| FR-OWN-05 | Owner dapat melihat pembayaran terverifikasi. | Must |
| FR-OWN-06 | Owner dapat melihat pemasukan dan pengeluaran berdasarkan periode. | Must |
| FR-OWN-07 | Owner dapat membuka detail laporan sesuai hak akses. | Should |

## 10.9 Live Chat, Notifikasi, Pencarian, dan Audit

| ID | Kebutuhan | Prioritas |
|---|---|---|
| FR-COM-01 | Client dapat mengirim pertanyaan kepada PKDI melalui Live Chat. | Should |
| FR-COM-02 | Percakapan dapat dikaitkan dengan Client dan, bila relevan, nomor pesanan. | Should |
| FR-COM-03 | Sistem menyimpan riwayat pesan dan lampiran percakapan. | Should |
| FR-COM-04 | Penanggung jawab internal Live Chat ditetapkan PKDI sebelum fitur diaktifkan. | Must |
| FR-SYS-01 | Sistem membuat notifikasi ketika pesanan diajukan. | Must |
| FR-SYS-02 | Client mendapat notifikasi perubahan status yang boleh dilihatnya. | Must |
| FR-SYS-03 | Kepala Gudang mendapat notifikasi pesanan baru/klarifikasi. | Must |
| FR-SYS-04 | Keuangan mendapat notifikasi transaksi siap ditagihkan dan bukti pembayaran baru. | Must |
| FR-SYS-05 | Client mendapat notifikasi invoice terbit dan hasil verifikasi pembayaran. | Must |
| FR-SYS-06 | Sistem menyediakan pencarian dan filter sesuai modul. | Must |
| FR-SYS-07 | Sistem mencatat audit log untuk tindakan penting. | Must |
| FR-SYS-08 | Waktu ditampilkan dalam zona operasional WIB (Asia/Jakarta). | Must |

---

# 11. Rancangan Dashboard

## 11.1 Dashboard Client

Fokus: transaksi dan informasi yang diperlukan pelanggan.

- Ringkasan pesanan aktif.
- Tombol Buat Pesanan.
- Ringkasan penjualan drum dan jasa rekondisi.
- Tracker ringkas pesanan.
- Invoice belum dibayar.
- Bukti pembayaran yang menunggu verifikasi.
- Riwayat pembayaran.
- Notifikasi.
- Administrasi/dokumen perusahaan.
- Akses Customer Service Live Chat bila diaktifkan.

Client tidak menampilkan antrean detail internal Gudang, catatan internal, daftar pengeluaran perusahaan, atau data Client lain.

## 11.2 Dashboard Kepala Gudang

- Pesanan baru yang perlu ditinjau.
- Pesanan menunggu penerimaan fisik.
- Pekerjaan rekondisi aktif.
- Produk penjualan yang sedang dipersiapkan.
- Pekerjaan selesai.
- Transaksi yang perlu dilengkapi hasil aktualnya.
- Riwayat pemeriksaan dan pekerjaan.

## 11.3 Dashboard Keuangan

- Transaksi siap ditagihkan.
- Transaksi belum ditagihkan.
- Filter Client dan periode.
- Draft invoice.
- Invoice terbit.
- Bukti pembayaran menunggu verifikasi.
- Pembayaran ditolak/perlu perbaikan.
- Invoice lunas dan belum lunas.
- Total pemasukan.
- Total pengeluaran.
- Ringkasan kas masuk dan keluar.

## 11.4 Dashboard Owner

- Total pesanan dan statusnya.
- Ringkasan penjualan drum.
- Ringkasan jasa rekondisi.
- Pekerjaan Gudang aktif/selesai.
- Total transaksi siap ditagihkan.
- Invoice terbit dan piutang.
- Pembayaran diterima.
- Pemasukan dan pengeluaran per periode.
- Tren operasional dan keuangan berdasarkan data yang tersedia.

---

# 12. Entitas Data Konseptual

Bagian ini merupakan rancangan domain data, bukan ERD final atau keputusan nama tabel migration.

| Entitas | Fungsi/data utama |
|---|---|
| User | Akun Client dan empat role; password disimpan dalam bentuk hash |
| ClientCompany | Nama perusahaan, alamat, kontak, penanggung jawab, identitas perusahaan |
| ClientDocument | NPWP, NIB, PO, kontrak, SPH, spesifikasi, dan dokumen pendukung |
| Product | Master produk drum: kategori, tipe, kapasitas, satuan, status |
| ReconditioningService | Master paket Laundry, Laundry + Cat, Laundry + Cat + Sablon |
| Order | Nomor pesanan, Client, jenis transaksi, tanggal, status operasional |
| OrderItem | Produk/layanan, kuantitas, satuan, spesifikasi, snapshot harga jika telah disepakati |
| WarehouseReceipt | Penerimaan fisik drum untuk pekerjaan yang memerlukan penerimaan |
| WarehouseInspection | Jumlah aktual, kondisi awal, ketidaksesuaian, catatan, pemeriksa, waktu |
| WarehouseJob/ProductionJob | Pekerjaan persiapan produk atau rekondisi |
| ProductionResult | Hasil pekerjaan, kuantitas selesai/tidak selesai, catatan dan dokumentasi |
| Shipment/Delivery | Informasi penyerahan/pengiriman, tanggal, kuantitas, kendaraan bila relevan |
| Tariff | Tarif produk/layanan, satuan, masa berlaku, status |
| BillableTransaction | Referensi transaksi operasional yang siap ditagihkan dan belum masuk invoice |
| Invoice | Header invoice: Client, nomor, tanggal, periode, subtotal, total, status |
| InvoiceItem | Baris transaksi yang tergabung dalam invoice |
| Payment | Nominal, tanggal, metode manual, referensi, status verifikasi |
| PaymentProof | File bukti transfer dan riwayat pemeriksaan |
| Expense | Pengeluaran, kategori, nominal, tanggal, penerima/vendor, bukti |
| CashbookEntry | Referensi pemasukan/pengeluaran; harus mencegah duplikasi pencatatan |
| Notification | Penerima, pesan, referensi transaksi, waktu, status baca |
| Conversation | Sesi Live Chat yang terkait dengan Client/transaksi |
| Message | Isi pesan, pengirim, waktu, lampiran |
| AuditLog | Pengguna, tindakan, objek, nilai sebelum/sesudah, waktu, alasan |

Relasi penting:

- Satu Client Company dapat memiliki banyak User Client sesuai kebijakan akun pelanggan; ketentuan satu akun atau banyak akun per perusahaan perlu divalidasi.
- Satu Client Company dapat memiliki banyak Order.
- Satu Order dapat memiliki beberapa OrderItem.
- Satu Order rekondisi dapat memiliki data penerimaan, pemeriksaan, pekerjaan, dan hasil.
- Satu transaksi operasional dapat menjadi satu BillableTransaction.
- Satu Invoice memiliki banyak InvoiceItem.
- Satu Invoice dapat mencakup beberapa BillableTransaction milik Client yang sama.
- Satu Invoice dapat menerima satu atau beberapa catatan pembayaran jika pembayaran sebagian diperbolehkan.

---

# 13. Aturan Bisnis Penting

1. Setiap pesanan memiliki nomor unik.
2. Setiap pesanan terhubung ke satu perusahaan Client.
3. Client hanya dapat membaca/mengubah data yang diizinkan milik perusahaannya sendiri.
4. Jenis transaksi hanya dua pada scope awal: Penjualan Drum dan Jasa Rekondisi Drum.
5. Pilihan layanan rekondisi wajib menggunakan dropdown: Laundry; Laundry + Cat; Laundry + Cat + Sablon.
6. Data pengajuan awal Client tidak boleh ditimpa oleh data aktual hasil pemeriksaan Gudang.
7. Untuk jasa rekondisi, penerimaan fisik dan pemeriksaan drum dicatat oleh Kepala Gudang.
8. Untuk penjualan drum, Kepala Gudang mencatat kesiapan dan kuantitas yang disiapkan/diserahkan sesuai prosedur PKDI.
9. Keuangan tidak menjadi persetujuan wajib sebelum Kepala Gudang memproses pesanan.
10. Transaksi masuk antrean penagihan hanya setelah syarat operasional/penagihan untuk jenis transaksinya terpenuhi.
11. Satu invoice dapat memuat beberapa transaksi milik Client yang sama.
12. Satu transaksi yang sudah masuk invoice tidak boleh masuk invoice lain, kecuali melalui koreksi/pembatalan resmi.
13. Tarif dan kuantitas yang dipakai untuk invoice harus tersimpan sebagai snapshot agar perubahan tarif di masa depan tidak mengubah invoice lama.
14. Bukti pembayaran wajib diunggah Client pada alur pembayaran manual portal.
15. Upload bukti tidak otomatis berarti pembayaran valid/lunas.
16. Hanya Keuangan yang dapat memverifikasi pembayaran.
17. Pembayaran yang diverifikasi hanya dicatat satu kali pada cashbook.
18. Pengeluaran dicatat terpisah dari invoice dan pembayaran Client.
19. Status operasional, penagihan, dan pembayaran disimpan terpisah.
20. Koreksi data transaksi/invoice/pembayaran harus memiliki alasan dan jejak audit.
21. Tidak ada proses persetujuan kendaraan masuk/keluar oleh Satpam dalam alur digital tahap awal.
22. Tidak ada Super Admin atau halaman manajemen seluruh akun pada tahap awal.

---

# 14. Laporan dan Ekspor

## 14.1 Laporan Operasional

- Daftar pesanan per periode.
- Pesanan berdasarkan jenis transaksi.
- Rekap penerimaan drum rekondisi.
- Rekap kondisi dan ketidaksesuaian drum.
- Rekap pekerjaan rekondisi berdasarkan paket layanan.
- Rekap hasil pekerjaan dan kuantitas aktual.
- Rekap penjualan drum dan barang yang diserahkan.
- Daftar transaksi siap ditagihkan.

## 14.2 Laporan Keuangan

- Rekap invoice per Client dan periode.
- Rekap transaksi yang sudah dan belum ditagihkan.
- Daftar invoice belum dibayar/lunas.
- Rekap bukti pembayaran menunggu verifikasi.
- Rekap pembayaran terverifikasi.
- Rekap pemasukan.
- Rekap pengeluaran berdasarkan kategori.
- Buku kas masuk dan keluar.
- Ringkasan arus kas berdasarkan periode.

## 14.3 Format dan Kontrol Ekspor

- Invoice disediakan dalam PDF.
- Laporan dapat disediakan dalam PDF/Excel sesuai prioritas implementasi.
- Ekspor mengikuti filter yang sedang dipilih.
- Dokumen resmi memakai identitas dan format perusahaan setelah PKDI menyediakan contoh/template.
- Hanya role yang berwenang dapat mengekspor data sensitif.

---

# 15. Notifikasi

Peristiwa yang dapat memicu notifikasi:

- Registrasi Client berhasil.
- Dokumen administrasi perlu diperbaiki atau telah diverifikasi.
- Pesanan berhasil diajukan.
- Pesanan ditinjau, diterima untuk diproses, perlu klarifikasi, atau ditolak.
- Drum diterima dan hasil pemeriksaan dicatat.
- Pekerjaan rekondisi dimulai atau selesai.
- Produk siap diserahkan/dikirim.
- Pesanan diterima Client.
- Transaksi masuk antrean penagihan.
- Invoice diterbitkan.
- Bukti pembayaran diunggah.
- Bukti pembayaran diterima/ditolak.
- Pembayaran menjadi lunas.
- Pesan Live Chat baru, jika fitur diaktifkan.

Notifikasi minimal tersedia di dalam aplikasi. Email/WhatsApp merupakan integrasi lanjutan kecuali disepakati berbeda.

---

# 16. Kebutuhan Non-Fungsional

## 16.1 Keamanan

- Autentikasi untuk seluruh akses.
- Pemeriksaan role/permission di backend.
- Password disimpan menggunakan hashing yang aman.
- Proteksi terhadap akses silang data Client (IDOR/cross-tenant access).
- Validasi semua input di frontend dan backend.
- Upload file dibatasi tipe, ukuran, dan aksesnya.
- Dokumen NPWP, NIB, PO, invoice, dan bukti pembayaran tidak boleh dapat diakses publik tanpa otorisasi.
- File tidak boleh dipercaya hanya dari ekstensi; lakukan validasi MIME/ukuran dan penyimpanan yang aman.
- Audit log untuk penerimaan drum, perubahan hasil, penerbitan invoice, verifikasi pembayaran, dan koreksi transaksi.
- Tidak ada kredensial default yang dipublikasikan atau ditanam dalam repositori.

## 16.2 Kinerja

- Halaman daftar menggunakan pagination.
- Pencarian/filter tidak memuat seluruh data tanpa batas.
- Dashboard menggunakan query agregasi yang efisien.
- Target awal waktu muat halaman operasional utama sekitar 3 detik pada kondisi server dan jaringan normal; target perlu diuji dengan data riil.

## 16.3 Ketersediaan dan Pencadangan

- Database dan dokumen transaksi dicadangkan berkala.
- Prosedur pemulihan backup perlu diuji.
- Sistem memberikan pesan yang jelas ketika koneksi atau penyimpanan gagal.

## 16.4 Kemudahan Penggunaan

- Bahasa antarmuka utama Bahasa Indonesia.
- Responsif untuk desktop, tablet, dan ponsel.
- Form menggunakan dropdown untuk pilihan terstandar.
- Pesan validasi menjelaskan kesalahan dan tindakan perbaikan.
- Dashboard berorientasi pada antrean pekerjaan dan tindakan utama, bukan hanya kartu statistik.
- Client hanya melihat informasi yang relevan untuk transaksinya.

## 16.5 Bahasa dan Waktu

- Format tanggal dan angka mengikuti Indonesia.
- Zona waktu yang ditampilkan: WIB (Asia/Jakarta).
- Nominal ditampilkan dalam Rupiah (IDR), kecuali PKDI menetapkan ketentuan lain.

---

# 17. Kriteria Penerimaan (Acceptance Criteria)

## Skenario A — Client mendaftar dan melengkapi profil

- Client dapat mendaftar dan login.
- Client dapat mengisi profil perusahaan.
- NPWP dapat diisi dan NIB/dokumen dapat diunggah.
- Client tidak dapat melihat profil perusahaan lain.

## Skenario B — Client membuat pesanan penjualan drum

- Client memilih Penjualan Drum.
- Sistem menampilkan dropdown produk yang tersedia.
- Client mengisi kuantitas, tanggal kebutuhan, dan dokumen PO sesuai aturan.
- Pesanan mendapatkan nomor unik.
- Pesanan muncul pada antrean Kepala Gudang.

## Skenario C — Client membuat pesanan rekondisi

- Client memilih Jasa Rekondisi Drum.
- Sistem menampilkan dropdown paket: Laundry, Laundry + Cat, Laundry + Cat + Sablon.
- Client mengisi produk/jenis drum, kuantitas, tanggal kebutuhan, dan spesifikasi.
- Pesanan mendapatkan nomor unik dan masuk ke antrean Kepala Gudang.

## Skenario D — Kepala Gudang menerima dan memeriksa drum

- Kepala Gudang dapat membuka detail pesanan.
- Kepala Gudang mencatat tanggal, jenis, jumlah aktual, dan kondisi drum yang diterima.
- Perbedaan jumlah/kondisi dapat dicatat tanpa menghapus data pengajuan.
- Kepala Gudang dapat memperbarui progres dan hasil pekerjaan.
- Hasil pekerjaan dapat menjadi sumber transaksi penagihan.

## Skenario E — Keuangan membuat invoice gabungan

- Keuangan melihat transaksi siap ditagihkan.
- Keuangan memfilter transaksi berdasarkan Client dan periode.
- Keuangan dapat memilih beberapa transaksi milik Client yang sama.
- Sistem mencegah transaksi yang sudah ditagihkan dipilih kembali.
- Sistem menghitung total secara benar.
- Invoice terbit dengan nomor unik dan dapat diunduh PDF.
- Client hanya dapat melihat invoice milik perusahaannya.

## Skenario F — Client membayar dengan transfer manual

- Client dapat melihat instruksi transfer.
- Client wajib mengunggah bukti pembayaran.
- Upload bukti membuat status menunggu verifikasi, bukan lunas.
- Keuangan dapat menyetujui/menolak bukti dengan catatan.
- Pembayaran menjadi lunas hanya setelah jumlah terverifikasi memenuhi total invoice.
- Pembayaran terverifikasi masuk ke cashbook satu kali.

## Skenario G — Keuangan mencatat pemasukan dan pengeluaran

- Pembayaran invoice yang terverifikasi tercatat sebagai pemasukan.
- Keuangan dapat mencatat pengeluaran dengan kategori dan bukti.
- Sistem menampilkan rekap berdasarkan periode.
- Pembayaran tidak terhitung ganda.

## Skenario H — Owner memantau laporan

- Owner dapat melihat ringkasan pesanan dan pekerjaan.
- Owner dapat melihat invoice, pembayaran, pemasukan, dan pengeluaran.
- Owner tidak dapat mengubah transaksi pada mode monitoring awal.

## Skenario I — Pembatasan akses

- Client tidak dapat membuka data Client lain melalui URL atau manipulasi request.
- Kepala Gudang tidak dapat menerbitkan invoice atau memverifikasi pembayaran.
- Keuangan tidak dapat mengubah hasil pemeriksaan/pekerjaan Gudang.
- Owner hanya memperoleh akses monitoring sesuai kebijakan.
- Setiap tindakan penting menyimpan audit log.

---

# 18. Prioritas dan Roadmap Pengembangan Sistem Perusahaan

Urutan berikut menggambarkan target produk secara keseluruhan. Implementasi nyata dapat dilakukan bertahap dan tidak berarti seluruh fitur harus selesai dalam satu periode tugas akhir.

## Phase 1 — Fondasi Aplikasi

- Setup Laravel dan database.
- Login Client dan internal.
- Role/permission empat role awal.
- Registrasi Client.
- Profil Client dan dokumen administrasi.
- Layout dashboard berdasarkan role.
- Provisioning akun internal tanpa UI Super Admin.

## Phase 2 — Master Data dan Pemesanan

- Master produk drum.
- Master paket layanan rekondisi.
- Dropdown jenis transaksi dan produk.
- Form penjualan drum.
- Form jasa rekondisi.
- PO, tanggal kebutuhan, spesifikasi, dan lampiran.
- Nomor pesanan dan notifikasi.

## Phase 3 — Modul Kepala Gudang

- Antrean pesanan.
- Peninjauan pesanan dan klarifikasi.
- Penerimaan drum.
- Pemeriksaan kuantitas dan kondisi.
- Pekerjaan rekondisi.
- Hasil aktual pekerjaan.
- Persiapan/penyerahan penjualan drum.
- Status siap diserahkan/dikirim.

## Phase 4 — Keuangan dan Invoice

- Master tarif.
- Antrean transaksi siap ditagihkan.
- Filter Client dan periode.
- Pemilihan beberapa transaksi.
- Invoice gabungan.
- PDF invoice.
- Riwayat dan koreksi invoice.

## Phase 5 — Pembayaran Manual dan Cashbook

- Instruksi transfer bank.
- Upload bukti pembayaran.
- Verifikasi Keuangan.
- Status pembayaran.
- Pemasukan dan pengeluaran.
- Rekap kas masuk/keluar.

## Phase 6 — Portal Client dan Owner Lengkap

- Tracker Client.
- Notifikasi lengkap.
- Invoice dan riwayat pembayaran Client.
- Dashboard Owner.
- Laporan operasional dan keuangan.
- Ekspor PDF/Excel.

## Phase 7 — Komunikasi dan Penguatan

- Live Chat dan penetapan petugas internal.
- Audit log lanjutan.
- Pengujian keamanan dan akses Client.
- Backup dan pemulihan.
- Pengujian end-to-end bersama PKDI.
- Perbaikan berdasarkan penggunaan awal.

## Pengembangan Lanjutan di Luar Tahap Awal

- Super Admin dan UI pengelolaan akun internal.
- Payment gateway, QRIS dinamis, Virtual Account.
- Integrasi bank.
- Manajemen stok/inventaris penuh bila dibutuhkan.
- Aplikasi mobile native.
- Integrasi sistem eksternal.

---

# 19. Hal yang Perlu Divalidasi dengan PKDI

Daftar ini menjaga agar rancangan tidak menganggap dugaan sebagai fakta.

| No. | Hal yang belum final | Mengapa perlu divalidasi |
|---|---|---|
| 1 | Daftar produk lengkap dan spesifikasi drum | Jenis drum yang disebut belum tentu seluruh produk PKDI |
| 2 | Detail ukuran, tipe tutup, merek, warna, dan kriteria teknis | Catatan menyebut spesifikasi tetapi belum lengkap |
| 3 | Paket Laundry, Laundry + Cat, Laundry + Cat + Sablon | Nama sudah dicatat; detail proses, hasil, dan tarif perlu disahkan |
| 4 | Ketersediaan/stok produk penjualan | Menentukan apakah cukup status siap/tidak tersedia atau perlu modul stok |
| 5 | Siapa yang meninjau/menerima pesanan baru di sisi operasional | Rancangan awal mengarahkan ke Kepala Gudang, tetapi batas kewenangannya perlu dikonfirmasi |
| 6 | Kapan penjualan drum masuk antrean penagihan | Sebelum pengiriman atau setelah diserahkan |
| 7 | Kapan jasa rekondisi masuk antrean penagihan | Setelah pekerjaan selesai atau setelah barang diserahkan kembali |
| 8 | Apakah invoice boleh menggabungkan penjualan drum dan jasa rekondisi sekaligus | Penggabungan beberapa transaksi Client disepakati, tetapi aturan campuran jenis transaksi belum tegas |
| 9 | Periode penagihan | Contoh dua minggu muncul dalam diskusi, tetapi periode baku PKDI perlu ditetapkan |
| 10 | Pembayaran sebagian dan jatuh tempo | Belum dikonfirmasi |
| 11 | Rekening bank dan format bukti pembayaran | Dibutuhkan untuk instruksi pembayaran dan verifikasi |
| 12 | Prosedur pengiriman/penyerahan | Apakah diantar PKDI, diambil Client, atau keduanya |
| 13 | Siapa mengisi jenis kendaraan, nomor polisi, dan data driver | Catatan menyebut diisi pihak PKDI, tetapi role internalnya belum ditentukan |
| 14 | Siapa menangani Live Chat dan permintaan penawaran | Keuangan berfokus finance; petugas komunikasi belum ditetapkan |
| 15 | Persyaratan NPWP/NIB dan siapa yang memverifikasi | NPWP dan NIB dibutuhkan, tetapi alur verifikasi belum lengkap |
| 16 | Jumlah akun Client per satu perusahaan | Perlu kebijakan apakah satu perusahaan boleh memiliki beberapa user |
| 17 | Format resmi invoice, PO, dan dokumen perusahaan | Diperlukan contoh dokumen dari PKDI |
| 18 | Kategori pengeluaran yang digunakan | Contoh awal harus disesuaikan dengan pembukuan PKDI |
| 19 | Apakah Owner hanya memantau | Rancangan awal read-only; hak approval belum ditentukan |
| 20 | Proses pembatalan, barang tidak layak, dan hasil rekondisi sebagian | Perlu kebijakan operasional dan penagihan |

---

# 20. Prinsip Implementasi dan Desain

1. Bangun aplikasi berdasarkan proses kerja nyata, bukan sekadar kumpulan dashboard.
2. Pisahkan dua jenis transaksi: penjualan drum dan jasa rekondisi.
3. Pisahkan status operasional, status penagihan, dan status pembayaran.
4. Jadikan Kepala Gudang penanggung jawab pencatatan fisik dan hasil pekerjaan.
5. Jadikan Keuangan penanggung jawab penagihan, pembayaran, pemasukan, dan pengeluaran.
6. Jangan menempatkan Keuangan sebagai approval wajib sebelum Gudang bekerja.
7. Gunakan satu sumber data pesanan agar Client, Kepala Gudang, Keuangan, dan Owner melihat data yang konsisten sesuai akses.
8. Pisahkan data permintaan Client, pemeriksaan aktual, hasil pekerjaan, transaksi yang dapat ditagihkan, invoice, dan pembayaran.
9. Gunakan master data untuk produk, paket layanan, satuan, dan tarif.
10. Tarif dan rincian pada invoice harus memiliki snapshot.
11. Setiap tindakan penting divalidasi di backend dan menggunakan transaksi database bila melibatkan beberapa perubahan yang harus konsisten.
12. Jangan menghapus permanen transaksi keuangan dan audit log.
13. Client hanya dapat melihat data perusahaannya sendiri.
14. Jangan tampilkan aktivitas internal Gudang secara berlebihan kepada Client; gunakan tracker ringkas.
15. Hindari tombol/fitur dekoratif yang tidak memiliki fungsi.
16. Antarmuka setiap role harus mendukung tugas hariannya: antrean, detail, status, dan tindakan utama.
17. UI harus profesional, konsisten, ringan, responsif, dan tidak menggunakan pola visual generik tanpa alasan.
18. Dokumentasikan setiap perubahan proses dan skema data agar sistem bisa dikembangkan setelah tugas akhir selesai.

---

# 21. Batasan Khusus Tugas Akhir Mahasiswa

> **Penting: Bab ini memisahkan kebutuhan tugas akhir dari target sistem perusahaan.** PKDI dapat membutuhkan sistem yang lebih luas, tetapi implementasi dan laporan tugas akhir mahasiswa mengikuti batasan yang ditetapkan dosen, yaitu berfokus pada bagian Gudang.

## 21.1 Tujuan Implementasi Tugas Akhir

Membangun modul Gudang berbasis web yang mendukung pencatatan penerimaan drum, pemeriksaan kondisi dan kuantitas, pengelolaan pekerjaan rekondisi, pencatatan hasil pekerjaan, dan penyediaan data operasional Gudang yang dapat menjadi sumber bagi proses lain pada pengembangan sistem PKDI.

Judul tugas akhir tetap mengikuti judul yang disetujui dosen dan proposal. PRD ini tidak otomatis mengubah judul akademik.

## 21.2 Fitur yang Masuk Cakupan Tugas Akhir

- Login pengguna Gudang/Kepala Gudang.
- Dashboard pekerjaan Gudang.
- Daftar pesanan yang menjadi input pekerjaan Gudang.
- Detail pesanan dan dokumen pendukung.
- Pencatatan penerimaan drum untuk rekondisi.
- Pencatatan jenis drum, kuantitas aktual, dan kondisi awal.
- Pencatatan ketidaksesuaian jumlah/kondisi.
- Pemilihan/pembacaan paket layanan: Laundry, Laundry + Cat, Laundry + Cat + Sablon.
- Pencatatan progres pekerjaan rekondisi.
- Pencatatan kuantitas hasil dan barang yang tidak selesai/tidak layak bila ada.
- Pencatatan persiapan/penyerahan produk untuk transaksi penjualan drum, hanya sejauh diperlukan oleh proses Gudang dan disetujui dosen.
- Riwayat pekerjaan dan filter pencarian.
- Notifikasi/status pekerjaan yang relevan.
- Ringkasan dan laporan Gudang.
- Hak akses pengguna Gudang dan pengamanan data.

## 21.3 Fitur di Luar Cakupan Tugas Akhir

- Portal Client secara penuh dan proses registrasi pelanggan, kecuali diperlukan sebagai sumber data/input demonstrasi.
- Modul Keuangan.
- Penerbitan invoice dan invoice gabungan.
- Pembayaran dan verifikasi bukti transfer.
- Pencatatan pemasukan/pengeluaran.
- Dashboard Owner lintas perusahaan.
- Live Chat penuh.
- Satpam dan gerbang masuk/keluar.
- Super Admin dan pengelolaan akun tingkat lanjut.
- Payment gateway, QRIS, dan Virtual Account.
- Manajemen stok penuh jika tidak diminta dosen/PKDI.

## 21.4 Batas Integrasi dengan Sistem Perusahaan

Walaupun tugas akhir hanya mengimplementasikan Modul Gudang, desain datanya sebaiknya tidak menghambat integrasi ke sistem perusahaan.

- Modul Gudang menerima referensi pesanan, Client, produk, kuantitas, dan paket layanan.
- Jika portal Client/Modul Pesanan belum tersedia, data pesanan untuk pengujian dapat dimasukkan melalui data uji atau form input terbatas sesuai persetujuan dosen.
- Modul Gudang menyimpan hasil penerimaan, pemeriksaan, pekerjaan, dan kuantitas aktual.
- Modul Gudang menyediakan data hasil pekerjaan dalam struktur yang kelak dapat dibaca Modul Keuangan.
- Tugas akhir tidak perlu mengimplementasikan invoice atau pembayaran; cukup memastikan hasil pekerjaan memiliki data yang diperlukan untuk integrasi penagihan di tahap berikutnya.

## 21.5 Kriteria Keberhasilan Tugas Akhir

1. Pengguna Gudang dapat login.
2. Pengguna dapat melihat daftar pekerjaan/pesanan.
3. Pengguna dapat mencatat penerimaan dan kondisi drum.
4. Pengguna dapat mencatat proses rekondisi berdasarkan pilihan paket.
5. Pengguna dapat mencatat hasil pekerjaan dan kuantitas aktual.
6. Riwayat pekerjaan dapat dicari dan difilter.
7. Laporan Gudang dapat ditampilkan/dicetak sesuai kebutuhan tugas akhir.
8. Validasi input, hak akses, dan pengujian alur utama berfungsi.
9. Data hasil pekerjaan tersimpan konsisten dan siap menjadi dasar integrasi Keuangan.

## 21.6 Prinsip Pemisahan Akademik dan Proyek Riil

- PRD ini adalah rancangan target sistem perusahaan.
- Tugas akhir hanya mengimplementasikan bagian Gudang sesuai arahan dosen.
- Fitur di luar Modul Gudang dapat didokumentasikan sebagai integrasi/roadmap, tetapi tidak boleh diklaim telah dibangun jika belum diimplementasikan.
- Demo tugas akhir harus menunjukkan fitur yang benar-benar dibuat.
- Jika ada perubahan scope dari dosen, batas tugas akhir diperbarui tanpa mengubah tujuan sistem perusahaan secara keseluruhan.

---

# 22. Definition of Done

Sebuah modul dinyatakan selesai apabila:

1. Fitur berjalan sesuai kebutuhan fungsional yang disetujui.
2. Hak akses role diuji di backend, bukan hanya melalui penyembunyian menu.
3. Validasi input tersedia di frontend dan backend.
4. Perubahan data/status tersimpan konsisten.
5. Tindakan penting tercatat pada audit log.
6. Client tidak dapat mengakses data Client lain.
7. Upload dokumen dan bukti pembayaran memiliki validasi serta kontrol akses.
8. Alur berhasil dan alur gagal/klarifikasi telah diuji.
9. Tidak ada tombol yang hanya menjadi dekorasi tanpa fungsi.
10. Tampilan responsif untuk desktop dan ponsel.
11. Dokumentasi singkat penggunaan dan perubahan modul tersedia.
12. Pengujian integrasi sesuai scope modul telah lulus.
13. Data uji tidak menggunakan data pribadi/keuangan perusahaan tanpa izin.
14. Backup dan pemulihan diuji sebelum aplikasi digunakan secara operasional.

---

# 23. Penutup

PRD 2.0 ini menjadi dasar diskusi dan validasi kebutuhan untuk pengembangan Sistem Manajemen Operasional dan Keuangan PT Pulo Kencana Drum Industri. Fokus utama produk adalah integrasi penjualan drum, jasa rekondisi, pekerjaan Gudang, penagihan, pembayaran manual, dan monitoring Owner.

Untuk tahap awal, sistem menggunakan empat role: Client, Kepala Gudang, Keuangan, dan Owner. Satpam tidak dimasukkan dalam integrasi digital tahap awal. Super Admin dan pengelolaan akun internal melalui UI ditunda ke pengembangan lanjutan.

Keputusan proses yang belum dikonfirmasi dicatat pada Bab 19 dan perlu diselesaikan bersama pihak PKDI sebelum implementasi final. Khusus tugas akhir mahasiswa, implementasi dibatasi pada Modul Gudang sebagaimana arahan dosen, dengan tetap memperhatikan kesiapan integrasi ke sistem perusahaan yang lebih luas.

**Akhir dokumen — PRD 2.0 Draft untuk Validasi PKDI.**
