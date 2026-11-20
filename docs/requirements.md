# Spesifikasi Kebutuhan Sistem (Requirements)
## Sistem Manajemen Operasional, Layanan Drum, dan Keuangan PT Pulo Kencana Drum Industri (PKDI)

**Versi Dokumen:** 1.0 (Fase 1)
**Sumber Acuan:** [PRD_PKDI_Sistem_Operasional_PT.md](file:///c:/sistem_pkdi/docs/PRD_PKDI_Sistem_Operasional_PT.md) dan [Roadmap_Prompt_Antigravity_PKDI.md](file:///c:/sistem_pkdi/docs/Roadmap_Prompt_Antigravity_PKDI.md)

---

## 1. Ruang Lingkup Sistem

### 1.1 Termasuk dalam Ruang Lingkup (MVP)
1. Autentikasi pengguna untuk 5 kategori peran: **Client**, **Satpam**, **Kepala Gudang**, **Keuangan**, dan **Owner**.
2. Registrasi akun mandiri dan pengelolaan profil perusahaan (PT) untuk Client.
3. Pengajuan pesanan jasa drum (diperbarui atau diperbaiki) oleh Client.
4. Antrean kerja dan dashboard spesifik untuk masing-masing dari 5 peran pengguna.
5. Pencatatan pemeriksaan kendaraan masuk dan keluar di pos gerbang oleh Satpam.
6. Pencatatan pemeriksaan muatan aktual, persetujuan pekerjaan, progres pengerjaan, dan hasil akhir jasa oleh Kepala Gudang.
7. Pengelolaan tarif jasa, perhitungan biaya berdasarkan hasil aktual gudang, dan penerbitan tagihan (Invoice PDF) oleh Keuangan.
8. Portal Client untuk memantau timeline status pesanan, melihat rincian tagihan, mengunduh invoice, dan mengunggah bukti pembayaran manual.
9. Verifikasi pembayaran oleh Keuangan serta pencatatan otomatis pemasukan ke buku keuangan (buku kas).
10. Penerbitan Surat Jalan (*Delivery Order*) oleh Kepala Gudang setelah sistem memvalidasi status tagihan lunas (`PAID`).
11. Pencatatan biaya operasional perusahaan (bahan, perlengkapan, gaji, utilitas, dll.) beserta lampiran bukti oleh Keuangan.
12. Dashboard pemantauan lintas bagian dan arus kas untuk Owner (*read-only* pada MVP).
13. Notifikasi perubahan status di dalam aplikasi, pencarian, penyaringan periode/status, jejak audit (*audit trail*), serta ekspor laporan ke PDF dan Excel.

### 1.2 Di Luar Ruang Lingkup Awal (Out of Scope MVP)
1. Integrasi *payment gateway* atau rekonsiliasi mutasi rekening bank otomatis.
2. Sistem HRIS/penggajian otomatis penuh (absensi, PPh 21, slip gaji otomatis); pada MVP gaji dicatat sebagai item biaya operasional.
3. Manajemen inventaris gudang bahan baku penuh (*stock opname* bahan kimia/cat/suku cadang).
4. Akuntansi standar PSAK lengkap (jurnal umum berpasangan, buku besar, neraca, laporan pajak resmi).
5. Aplikasi mobile native Android/iOS (sistem dibangun sebagai web responsif).
6. Integrasi perangkat keras gerbang otomatis atau jembatan timbang digital (IoT).

---

## 2. Kebutuhan Fungsional per Peran

### 2.1 Autentikasi dan Manajemen Akses (`FR-AUTH`)

| Kode | Deskripsi Kebutuhan | Peran Terkait | Prioritas | Catatan Validasi |
|---|---|---|---|---|
| `FR-AUTH-01` | Menyediakan halaman login dan fungsi logout untuk seluruh peran pengguna. | Semua Peran | Must | - |
| `FR-AUTH-02` | Menyediakan halaman registrasi akun mandiri khusus untuk Client beserta pengisian data awal perusahaan (PT). | Client | Must | Status aktif langsung atau menunggu verifikasi dicatat pada `validation-needed.md`. |
| `FR-AUTH-03` | Membatasi akses kueri dan tampilan data Client agar hanya memuat data milik `client_company_id` pengguna yang sedang login. | Client | Must | Wajib diuji terhadap celah IDOR. |
| `FR-AUTH-04` | Menyediakan mekanisme pembuatan, penonaktifan, dan pengaturan akun internal tanpa menambah role Admin baru di luar keputusan tertulis. | Pihak Berwenang | Must | `NEEDS VALIDATION`: mekanisme kelola akun internal. |
| `FR-AUTH-05` | Menyediakan pemisahan akun untuk 5 kategori peran pengguna (`client`, `satpam`, `kepala_gudang`, `keuangan`, `owner`) dimana tiap petugas memiliki akun individual. | Semua Peran | Must | Akun tidak boleh dipakai bersama agar audit log akurat. |
| `FR-AUTH-06` | Memvalidasi izin akses pada setiap route, controller, dan tindakan menggunakan middleware serta policy di sisi server (*default deny*). | Semua Peran | Must | - |
| `FR-AUTH-07` | Menyediakan fitur pengaturan ulang kata sandi (*reset password*). | Semua Peran | Should | - |
| `FR-AUTH-08` | Mencatat waktu login terakhir dan aktivitas penting pengguna ke dalam log audit. | Semua Peran | Should | - |

### 2.2 Portal Client (`FR-CLI`)

| Kode | Deskripsi Kebutuhan | Prioritas | Catatan Implementasi / Validasi |
|---|---|---|---|
| `FR-CLI-01` | Client dapat melihat dan memperbarui profil perusahaannya (nama PT, alamat, nama penanggung jawab, email, nomor telepon). | Must | - |
| `FR-CLI-02` | Client dapat mengajukan pesanan layanan baru dengan mengisi: kategori layanan, jenis muatan, jenis jasa (diperbarui/diperbaiki), jenis kendaraan, nomor polisi kendaraan, asal PT, nama driver (opsional), catatan tambahan (opsional), dan lampiran pendukung (opsional). | Must | Seluruh kolom jenis/kategori diketik langsung oleh user (input teks, bukan dropdown) agar akurat sesuai kondisi. |
| `FR-CLI-03` | Sistem membangkitkan nomor pesanan unik secara otomatis saat pesanan dikirim dan menetapkan status awal `WAITING_GATE_IN`. | Must | - |
| `FR-CLI-04` | Client dapat melihat daftar pesanan milik perusahaannya dengan fitur pencarian, filter status, dan paginasi. | Must | - |
| `FR-CLI-05` | Client dapat melihat halaman detail pesanan yang menampilkan data pengajuan, hasil pemeriksaan yang dibagikan, serta timeline riwayat perubahan status secara kronologis. | Must | Perubahan status bersumber dari database server. |
| `FR-CLI-06` | Client menerima notifikasi di dalam aplikasi setiap kali terjadi perubahan status penting atau penerbitan tagihan. | Must | - |
| `FR-CLI-07` | Client dapat melihat daftar tagihan (invoice), membuka rincian perhitungan biaya jasa, dan mengunduh dokumen invoice resmi dalam format PDF. | Must | Hanya untuk invoice berstatus telah diterbitkan (`ISSUED` ke atas), bukan `DRAFT`. |
| `FR-CLI-08` | Client dapat melihat status pembayaran (`Menunggu Pembayaran`, `Pembayaran Sebagian`, `Lunas`) beserta riwayat verifikasi pembayarannya. | Must | - |
| `FR-CLI-09` | Client dapat mengunggah bukti pembayaran (foto/PDF bukti transfer) beserta nominal, tanggal bayar, dan nomor referensi untuk diverifikasi Keuangan. | Should | Unggahan bukti tidak otomatis mengubah status tagihan menjadi lunas. |
| `FR-CLI-10` | Client dapat mengajukan permohonan pembatalan pesanan disertai alasan sebelum batas tahapan yang ditetapkan PKDI. | Could | `NEEDS VALIDATION`: batas status pembatalan yang diizinkan. |

### 2.3 Pos Gerbang / Satpam (`FR-GATE`)

| Kode | Deskripsi Kebutuhan | Prioritas | Catatan Implementasi / Validasi |
|---|---|---|---|
| `FR-GATE-01` | Dashboard Satpam menampilkan antrean kendaraan yang diperkirakan masuk (`WAITING_GATE_IN` / `GATE_IN_HELD`), daftar kendaraan yang sedang berada di area pabrik, dan antrean Surat Jalan yang menunggu pemeriksaan keluar (`WAITING_GATE_OUT` / `GATE_OUT_HELD`). | Must | Antarmuka dirancang ringkas untuk perangkat di pos gerbang. |
| `FR-GATE-02` | Satpam dapat mencari pesanan atau Surat Jalan dengan cepat menggunakan nomor pesanan, nomor Surat Jalan, nomor polisi kendaraan, nama PT Client, atau nama driver. | Must | - |
| `FR-GATE-03` | Pada pemeriksaan masuk, Satpam membandingkan nomor polisi, jenis kendaraan, dan nama driver aktual terhadap data pesanan, lalu menekan `Setujui Masuk` bila sesuai. | Must | Menyimpan waktu masuk, ID akun Satpam, dan mengubah status pesanan ke `WAITING_WAREHOUSE`. |
| `FR-GATE-04` | Apabila data kendaraan masuk tidak sesuai, Satpam dapat menekan `Tahan Masuk` (`GATE_IN_HELD`) dengan kewajiban mengisi alasan penahanan/penolakan. | Must | Mengirim notifikasi ke bagian internal dan Client. |
| `FR-GATE-05` | Pada pemeriksaan keluar, Satpam dapat membuka detail Surat Jalan untuk memeriksa nomor dokumen, pesanan, nama PT, nomor polisi, jenis kendaraan, driver, rincian kuantitas muatan keluar, dan indikator lunas. | Must | - |
| `FR-GATE-06` | Sistem memblokir secara mutlak tindakan `Setujui Keluar` di sisi server apabila tagihan terkait belum berstatus `PAID` atau Surat Jalan belum diterbitkan/sudah pernah dipakai/dibatalkan. | Must | Aturan keamanan bisnis kritis. |
| `FR-GATE-07` | Satpam dapat menyetujui kendaraan keluar (`COMPLETED`) atau menahan kendaraan keluar (`GATE_OUT_HELD` dengan alasan wajib) apabila fisik muatan/kendaraan tidak cocok dengan Surat Jalan. | Must | Mencatat waktu keluar, akun Satpam, dan menandai Surat Jalan telah digunakan (`USED_AT_GATE_OUT`). |
| `FR-GATE-08` | Menyediakan halaman riwayat log kendaraan masuk dan keluar dengan filter rentang tanggal, jenis pemeriksaan (masuk/keluar), nomor polisi, dan petugas Satpam. | Must | - |
| `FR-GATE-09` | Setiap catatan gerbang menyimpan referensi akun Satpam yang melakukan aksi secara permanen. | Must | - |
| `FR-GATE-10` | Mendukung pencarian langsung berdasarkan kode Surat Jalan (dan pemindaian QR apabila kebijakan QR diaktifkan). | Could | `NEEDS VALIDATION`: penggunaan QR pada Surat Jalan. |

### 2.4 Kepala Gudang (`FR-WH`)

| Kode | Deskripsi Kebutuhan | Prioritas | Catatan Implementasi / Validasi |
|---|---|---|---|
| `FR-WH-01` | Dashboard Kepala Gudang menampilkan antrean pesanan berdasarkan tahap kerja: menunggu pemeriksaan muatan (`WAITING_WAREHOUSE`), sedang diperiksa/klarifikasi (`WAREHOUSE_REVIEW` / `NEEDS_CLARIFICATION`), sedang diproses (`IN_PROGRESS`), selesai diproses (`SERVICE_COMPLETED`), dan pesanan lunas yang menunggu Surat Jalan (`WAITING_EXIT_DOCUMENT`). | Must | - |
| `FR-WH-02` | Kepala Gudang dapat melihat detail permintaan awal Client serta catatan pemeriksaan gerbang masuk dari Satpam. | Must | Sistem menolak dimulainya pemeriksaan gudang jika kendaraan belum disetujui masuk oleh Satpam. |
| `FR-WH-03` | Kepala Gudang dapat menginput hasil pemeriksaan muatan aktual yang mencakup: jenis muatan aktual, jumlah/kuantitas aktual beserta satuannya, kondisi fisik drum saat diterima, serta catatan ketidaksesuaian terhadap pesanan awal. | Must | Data pemeriksaan aktual disimpan pada entitas terpisah tanpa menimpa data pengajuan awal Client. |
| `FR-WH-04` | Kepala Gudang dapat mengambil keputusan atas hasil pemeriksaan: `Setujui dan Proses` (`IN_PROGRESS`), `Minta Klarifikasi` (`NEEDS_CLARIFICATION`), atau `Tolak Pekerjaan` (`CANCELLED` / ditolak dengan alasan wajib). | Must | - |
| `FR-WH-05` | Kepala Gudang dapat mencatat pembaruan progres pengerjaan selama status `IN_PROGRESS` yang dapat dilihat pada timeline pesanan. | Must | - |
| `FR-WH-06` | Kepala Gudang dapat mencatat hasil akhir pengerjaan jasa yang memuat: jenis layanan aktual yang dikerjakan, kuantitas selesai (layak), kuantitas tidak selesai/tidak layak (bila ada), dan catatan hasil pengerjaan. | Must | Menjadi dasar perhitungan tagihan oleh Keuangan. |
| `FR-WH-07` | Saat Kepala Gudang mengonfirmasi penyelesaian pengerjaan, sistem otomatis mengubah status ke `WAITING_FINANCE` dan meneruskan data hasil kerja ke antrean Keuangan. | Must | - |
| `FR-WH-08` | Kepala Gudang dapat membuat dan menerbitkan Surat Jalan setelah sistem memvalidasi bahwa tagihan pesanan tersebut telah berstatus `PAID`. | Must | - |
| `FR-WH-09` | Surat Jalan mendapatkan nomor dokumen unik otomatis dan memuat referensi pesanan, PT Client, rincian barang/kuantitas keluar, nomor polisi, jenis kendaraan, driver, waktu terbit, dan akun Kepala Gudang penerbit. | Must | Status pesanan berubah menjadi `WAITING_GATE_OUT`. |
| `FR-WH-10` | Menyediakan daftar riwayat pemeriksaan gudang, hasil pengerjaan, dan Surat Jalan yang telah diterbitkan. | Must | - |

### 2.5 Keuangan (`FR-FIN`)

| Kode | Deskripsi Kebutuhan | Prioritas | Catatan Implementasi / Validasi |
|---|---|---|---|
| `FR-FIN-01` | Dashboard Keuangan menampilkan antrean pekerjaan selesai yang belum dibuatkan tagihan (`WAITING_FINANCE`), draft tagihan, tagihan menunggu pembayaran, pembayaran yang menunggu verifikasi, serta ringkasan kas periode berjalan. | Must | - |
| `FR-FIN-02` | Keuangan dapat meninjau perbandingan antara permintaan awal Client, hasil pemeriksaan gudang, dan kuantitas hasil akhir yang diselesaikan Kepala Gudang. | Must | - |
| `FR-FIN-03` | Keuangan dapat melihat dan mengelola daftar master tarif jasa (nama tarif, kategori, jenis layanan, satuan, harga satuan, masa berlaku, status aktif). | Must | `NEEDS VALIDATION`: otorisasi perubahan master tarif. |
| `FR-FIN-04` | Keuangan dapat menyusun rincian perhitungan tagihan berdasarkan kuantitas hasil kerja aktual dan tarif, menambahkan penyesuaian/potongan atau pajak (bila ditetapkan), serta menyimpannya sebagai `DRAFT`. | Must | - |
| `FR-FIN-05` | Keuangan dapat menerbitkan tagihan (`INVOICE_ISSUED`) dengan nomor invoice unik otomatis sehingga langsung tampil di portal Client terkait. | Must | - |
| `FR-FIN-06` | Sistem menghasilkan dokumen Invoice dalam format PDF siap cetak/unduh dengan identitas resmi PKDI. | Must | - |
| `FR-FIN-07` | Keuangan dapat mencatat pembayaran masuk atau memeriksa bukti pembayaran yang diunggah Client (tanggal bayar, nominal, metode pembayaran, nomor referensi, lampiran bukti). | Must | - |
| `FR-FIN-08` | Sistem menghitung akumulasi pembayaran terverifikasi terhadap total tagihan dan menentukan status tagihan (`INVOICE_ISSUED` / belum bayar, `PAYMENT_PARTIAL`, atau `PAID`). | Must | Saat mencapai `PAID`, status pesanan otomatis siap untuk pembuatan Surat Jalan (`WAITING_EXIT_DOCUMENT`). |
| `FR-FIN-09` | Setiap pembayaran yang diverifikasi sah oleh Keuangan secara otomatis menghasilkan satu entri pemasukan (`IN`) pada buku keuangan (`cashbook_entries`) yang terhubung ke ID pembayaran tersebut. | Must | Mencegah pencatatan pemasukan ganda. |
| `FR-FIN-10` | Keuangan dapat mencatat transaksi biaya operasional perusahaan (tanggal, kategori biaya, uraian, nominal, penerima/vendor, metode bayar, dan unggahan bukti nota/kuitansi). | Must | Otomatis menghasilkan satu entri pengeluaran (`OUT`) pada buku keuangan. |
| `FR-FIN-11` | Keuangan dapat mengelompokkan biaya operasional berdasarkan kategori (misalnya pembelian bahan, perlengkapan, gaji karyawan, transportasi, pemeliharaan, utilitas, lainnya). | Must | - |
| `FR-FIN-12` | Menyediakan halaman Buku Keuangan (kas masuk dan kas keluar) dengan fitur pencarian, filter rentang tanggal/periode, dan filter kategori/sumber transaksi. | Must | - |
| `FR-FIN-13` | Keuangan dapat mengekspor laporan tagihan, pembayaran, biaya operasional, dan buku keuangan ke format PDF dan Excel sesuai filter yang sedang aktif. | Must | - |
| `FR-FIN-14` | Setiap koreksi atau revisi atas tagihan yang sudah terbit maupun pembatalan catatan pembayaran/biaya wajib menyertakan alasan tertulis, menyimpan nilai sebelum dan sesudah di riwayat revisi/audit log, dan tidak menghapus baris transaksi secara permanen. | Must | - |
| `FR-FIN-15` | Menampilkan ringkasan metrik keuangan berdasarkan transaksi nyata yang tersimpan di database: total tagihan diterbitkan, total piutang belum dibayar, total pembayaran terverifikasi, total biaya operasional, dan selisih arus kas bersih pada periode terpilih. | Must | Tanpa angka atau persentase tren rekaan. |

### 2.6 Owner (`FR-OWN`)

| Kode | Deskripsi Kebutuhan | Prioritas | Catatan Implementasi / Validasi |
|---|---|---|---|
| `FR-OWN-01` | Owner dapat melihat ringkasan jumlah pesanan dan distribusinya berdasarkan tahapan status operasional. | Must | - |
| `FR-OWN-02` | Owner dapat memantau aktivitas kendaraan masuk, kendaraan yang masih berada di area pabrik, dan kendaraan keluar. | Must | - |
| `FR-OWN-03` | Owner dapat memantau beban kerja gudang (menunggu pemeriksaan, sedang dikerjakan, selesai). | Must | - |
| `FR-OWN-04` | Owner dapat memantau total pendapatan jasa (tagihan terbit), pembayaran yang telah diterima, dan daftar piutang tagihan yang belum lunas. | Must | - |
| `FR-OWN-05` | Owner dapat melihat daftar dan rekapitulasi biaya operasional perusahaan per kategori beserta lampiran buktinya. | Must | - |
| `FR-OWN-06` | Owner dapat melihat ringkasan arus kas masuk, arus kas keluar, dan arus kas bersih pada periode harian, bulanan, atau rentang tanggal tertentu. | Must | - |
| `FR-OWN-07` | Owner dapat memfilter seluruh tampilan ringkasan dan tabel transaksi berdasarkan rentang tanggal, perusahaan Client, kategori biaya, dan status pesanan. | Must | - |
| `FR-OWN-08` | Owner dapat membuka detail setiap pesanan, pemeriksaan gerbang, hasil gudang, invoice, pembayaran, biaya operasional, serta menelusuri jejak audit (*audit trail*) lengkap siapa melakukan apa dan kapan. | Must | Akses bersifat *read-only* pada MVP. |
| `FR-OWN-09` | Owner dapat mengunduh/mengekspor laporan operasional dan keuangan dalam format PDF maupun Excel. | Should | - |

### 2.7 Notifikasi, Pencarian, Pembaruan Status, dan Audit (`FR-SYS`)

| Kode | Deskripsi Kebutuhan | Prioritas | Catatan Implementasi |
|---|---|---|---|
| `FR-SYS-01` | Sistem membuat notifikasi otomatis untuk peristiwa bisnis utama (pesanan baru, kendaraan masuk/ditahan, hasil pemeriksaan gudang, pekerjaan selesai, invoice terbit, pembayaran diverifikasi/ditolak, Surat Jalan terbit, kendaraan keluar). | Must | - |
| `FR-SYS-02` | Client menerima notifikasi yang relevan dengan pesanannya di portal Client. | Must | - |
| `FR-SYS-03` | Peran internal menerima notifikasi ketika ada item kerja baru yang masuk ke antrean tugasnya. | Must | - |
| `FR-SYS-04` | Dashboard dan halaman detail pesanan memperbarui informasi status dari server secara otomatis (melalui mekanisme *broadcasting* atau *polling* terukur setiap 10 detik) tanpa mengharuskan pengguna melakukan muat ulang halaman manual. | Must | Dijelaskan secara jujur pada dokumentasi arsitektur. |
| `FR-SYS-05` | Setiap tabel daftar memiliki fitur pencarian kata kunci, penyaringan (*filter*), pengurutan kolom (*sorting*), dan paginasi. | Must | - |
| `FR-SYS-06` | Sistem mencatat setiap perubahan status, persetujuan, penahanan, penerbitan dokumen, verifikasi pembayaran, dan revisi data ke tabel `audit_logs` (menyimpan ID pengguna, peran, aksi, entitas, ID data, nilai lama, nilai baru, alasan, alamat IP, dan waktu). | Must | - |
| `FR-SYS-07` | Seluruh pencatatan dan penampilan waktu menggunakan zona waktu operasional Indonesia Barat (`Asia/Jakarta` / WIB) secara konsisten. | Must | - |

---

## 3. Kebutuhan Non-Fungsional (Non-Functional Requirements)

### 3.1 Keamanan (Security)
1. **Otorisasi Sisi Server (*Default Deny*):** Seluruh pembatasan akses per peran dan kepemilikan data diverifikasi di backend Laravel melalui *Middleware*, *FormRequest*, dan *Policy*. Menyembunyikan tombol di Blade bukan pengganti validasi server.
2. **Isolasi Data Client (Anti-IDOR):** Setiap akses Client ke pesanan, timeline, invoice, bukti bayar, dan dokumen wajib difilter dengan `WHERE client_company_id = auth()->user()->client_company_id`. Percobaan mengakses ID milik perusahaan lain menghasilkan respons `403 Forbidden` atau `404 Not Found`.
3. **Keamanan Kredensial & Sesi:** Kata sandi di-hash menggunakan algoritma `bcrypt`/`argon2id` bawaan Laravel. Seluruh form state-changing wajib menggunakan token `@csrf`, proteksi *mass-assignment* (`$fillable` eksplisit), dan regenerasi sesi saat login.
4. **Penyimpanan Berkas Privat:** Bukti pembayaran, lampiran nota biaya operasional, serta file PDF Invoice dan Surat Jalan disimpan pada disk privat (bukan folder `public/` bebas akses) dan hanya dapat diunduh melalui controller yang memeriksa otorisasi pengguna.
5. **Validasi Unggahan Berkas:** Hanya mengizinkan ekstensi dan MIME type yang sah (misalnya `pdf`, `jpg`, `jpeg`, `png`) dengan batas ukuran berkas terukur (misalnya maksimal 5 MB per berkas).
6. **Integritas Jejak Audit:** Tabel `audit_logs` dan `order_status_histories` bersifat *append-only* (hanya tambah, tidak memiliki fitur edit atau hapus bagi pengguna mana pun).

### 3.2 Kinerja dan Skalabilitas (Performance)
1. **Paginasi Wajib:** Seluruh daftar pesanan, log gerbang, invoice, biaya operasional, buku kas, dan audit log menggunakan paginasi server-side agar waktu muat tetap stabil seiring bertambahnya transaksi.
2. **Efisiensi Kueri Dashboard:** Ringkasan angka pada dashboard dihitung menggunakan kueri agregasi database (`COUNT`, `SUM` dengan indeks pada kolom status, tanggal, dan `client_company_id`), tidak memuat ribuan model ke memori PHP.
3. **Target Waktu Muat:** Halaman operasional utama ditargetkan tampil dalam waktu <= 3 detik pada jaringan lokal/normal. Aset CSS dan JavaScript dikompilasi melalui Vite dan tidak bergantung pada CDN eksternal yang memblokir render.

### 3.3 Kemudahan Penggunaan dan Aksesibilitas (Usability & Human Standard)
1. **Desain Responsif Penuh:** Tata letak wajib beradaptasi dengan baik pada layar ponsel (320px-480px), tablet/layar menengah (600px-1024px), hingga desktop lebar (1280px+) tanpa *horizontal overflow* yang merusak halaman (`R-03`).
2. **Ergonomi Pos Gerbang Satpam:** Tampilan Satpam menonjolkan kolom pencarian cepat dan tombol tindakan dengan area sentuh minimal 44x44 px (`R-03`) agar mudah dioperasikan pada layar ponsel atau tablet di pos gerbang.
3. **Kontras & Navigasi Keyboard:** Seluruh teks memenuhi standar kontras WCAG AA (minimal 4.5:1 untuk teks normal, 3:1 untuk teks besar dan batas komponen interaktif, `R-25`), seluruh kontrol memiliki indikator `:focus-visible` yang jelas (`R-32`), serta modal/drawer dapat ditutup dengan tombol `Escape` (`R-26`).
4. **Kelengkapan State UI:** Setiap tampilan data wajib menyediakan state `loading`, `empty` (kosong awal), `filtered` (pencarian tidak ditemukan beserta tombol reset), dan `error` (`R-27`).
5. **Kejelasan Status:** Warna badge status selalu dipasangkan dengan label teks Bahasa Indonesia yang jelas dan konsisten (tidak mengandalkan warna saja, `antislop-human`).

### 3.4 Lokalisasi, Bahasa, dan Format Data
1. **Bahasa Antarmuka:** Seluruh label, tombol, pesan validasi, status, dan laporan menggunakan Bahasa Indonesia yang lugas dan alami (`antislop-copywriting`).
2. **Format Waktu:** Zona waktu `Asia/Jakarta` (WIB), ditampilkan dengan format tanggal Indonesia (contoh: `17 Nov 2026, 10.30 WIB`).
3. **Format Mata Uang & Angka:** Nominal uang diformat dalam Rupiah Indonesia (`Rp 1.250.000`) dengan kelas tipografi `tabular-nums` agar rata kanan dan mudah dibandingkan pada tabel keuangan.
