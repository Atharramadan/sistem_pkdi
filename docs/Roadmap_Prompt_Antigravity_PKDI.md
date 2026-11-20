# ROADMAP & PROMPT ANTIGRAVITY
## Sistem Manajemen Operasional PT Pulo Kencana Drum Industri (PKDI)

**Versi dokumen:** 1.0  
**Tujuan:** Panduan pengerjaan bertahap untuk Antigravity berdasarkan PRD PKDI terbaru.  
**Prinsip pengerjaan:** bertahap, dapat diuji, dokumentasi mengikuti kode, dan UI tidak generik.

---

# 1. Konteks dan aturan utama proyek

Bangun sistem operasional internal PT Pulo Kencana Drum Industri (PKDI) yang menghubungkan proses pesanan client, pemeriksaan gerbang, pekerjaan gudang, penagihan, pembayaran, surat jalan, dan laporan owner.

## 1.1 Lima kategori pengguna

Sistem memiliki lima kategori/role utama:

1. **Client** — dapat mendaftar akun sendiri, membuat pesanan jasa, memantau progres pesanan, dan melihat tagihan serta status pembayaran melalui portal client.
2. **Satpam** — memeriksa kendaraan ketika masuk dan keluar pabrik, mencocokkan nomor polisi/jenis kendaraan/nama driver, serta mencatat aktivitas kendaraan masuk dan keluar.
3. **Kepala Gudang** — memeriksa muatan drum, menyetujui pekerjaan, memperbarui proses jasa, mencatat hasil pekerjaan, dan menerbitkan surat jalan setelah syarat pengeluaran terpenuhi.
4. **Keuangan** — menghitung biaya jasa, membuat tagihan, memverifikasi pembayaran, mencatat transaksi keuangan, mencatat biaya operasional, serta membuat laporan PDF/Excel.
5. **Owner** — melihat ringkasan dan detail transaksi dari seluruh bagian, pendapatan jasa, pembayaran, dan biaya operasional perusahaan.

**Penting tentang akun:** angka lima adalah lima kategori pengguna di atas, bukan lima akun Satpam. Setiap pengguna internal yang membutuhkan akses harus memiliki akun individual sesuai role-nya. Client dapat melakukan registrasi sendiri dan jumlah akun client tidak dibatasi menjadi satu atau lima.

Jangan menambahkan role Admin baru atau menganggap Owner otomatis boleh mengubah semua data operasional tanpa keputusan yang terdokumentasi. Cara pembuatan dan pengelolaan akun internal harus ditandai sebagai keputusan yang perlu dikonfirmasi apabila PRD belum menetapkannya.

## 1.2 Alur operasional utama

1. Client mendaftar/login, lalu membuat pesanan jasa.
2. Form pesanan mencatat kategori, jenis muatan, jenis jasa (misalnya diperbarui atau diperbaiki), jenis kendaraan, nomor polisi kendaraan, asal perusahaan/PT, dan nama driver yang bersifat opsional.
3. Pesanan muncul pada dashboard/antrean kerja internal yang relevan: Satpam, Kepala Gudang, Keuangan, dan Owner. Setiap role mendapatkan tampilan dan tindakan sesuai kewenangannya.
4. Satpam memeriksa kendaraan dan identitas pengemudi. Jika sesuai, Satpam menyetujui kendaraan masuk dan mencatat waktu masuk. Jika tidak sesuai, alasan penolakan wajib dicatat.
5. Kepala Gudang memeriksa muatan drum dan detail jasa yang diminta client. Setelah pemeriksaan dan persetujuan, status pekerjaan diperbarui dan dapat dilihat client.
6. Kepala Gudang memperbarui proses pekerjaan sampai hasil jasa selesai. Hasil pekerjaan diteruskan ke Keuangan.
7. Keuangan menghitung biaya jasa, membuat tagihan yang dapat dilihat client melalui akunnya, mencatat pembayaran, dan memverifikasi pelunasan.
8. Setelah pembayaran terverifikasi, Kepala Gudang menerbitkan surat jalan/izin keluar sesuai kewenangan yang disetujui.
9. Satpam memeriksa surat jalan dan kendaraan. Jika sesuai, Satpam mencatat kendaraan keluar. Transaksi selesai.
10. Seluruh aktivitas penting tercatat dalam riwayat sehingga dapat ditelusuri siapa melakukan apa dan kapan.

## 1.3 Teknologi wajib

- **Backend:** PHP dengan **Laravel 13**.
- **Database:** MySQL, dijalankan secara lokal melalui XAMPP selama development.
- **Frontend:** Blade + Tailwind CSS.
- **Build frontend:** Vite sesuai kebutuhan Laravel.
- **JavaScript tambahan:** gunakan seperlunya untuk interaksi kecil; jangan menambahkan framework frontend besar tanpa alasan teknis dan persetujuan.
- **Version control:** Git.

Laravel 13 membutuhkan PHP 8.3 atau lebih tinggi. Periksa versi PHP CLI dan PHP yang digunakan Apache/XAMPP; jangan berasumsi keduanya otomatis sama. Referensi resmi: https://laravel.com/docs/13.x/deployment

Jangan mengganti stack menjadi Node/Express, Next.js, React, Vue, Bootstrap, atau database lain. Jangan meng-upgrade atau menurunkan versi dependency secara sembarangan.

## 1.4 Aturan Antislop wajib

Skill Antislop sudah tersedia di lingkungan Antigravity. **Jangan menginstal ulang, meng-clone ulang, menyalin ulang, atau membuat versi duplikat skill tersebut.** Temukan lokasi skill yang benar pada workspace/konfigurasi Antigravity, lalu baca file `SKILL.md` yang relevan sebelum mengerjakan fase terkait.

Gunakan mode **During** untuk proyek ini karena Antislop harus membimbing pekerjaan sejak awal, bukan hanya mengaudit hasil akhir. Pada awal sesi, ikuti instruksi mode di skill inti dan nyatakan bahwa mode During aktif sesuai aturan skill. Jangan mengulang pengumuman itu berkali-kali dalam satu sesi.

Skill yang tersedia berdasarkan instalasi pengguna:

| Skill | Kapan wajib dipakai |
|---|---|
| `antislop` | Selalu. Baca dan terapkan sebagai filter inti di seluruh fase. |
| `antislop-ui` | Wajib saat menyusun `DESIGN.md`, membangun layout, komponen, dashboard, dan halaman UI. |
| `antislop-copywriting` | Saat menulis label, judul, instruksi form, pesan validasi, empty state, notifikasi, dan microcopy. |
| `antislop-human` | Saat membangun form, navigasi keyboard, fokus, kontras, state error/success/disabled, dan aksesibilitas. |
| `antislop-layoutmobile` | Saat membangun dan menguji layout responsif dari ponsel sampai desktop. |
| `antislop-code` | Saat membersihkan atau meninjau komentar kode. Jangan mengubah logika kode hanya karena skill komentar ini. |

Lokasi skill dapat berbeda sesuai cara instalasi. Cari folder skill yang benar, misalnya folder `.agents/skills/` atau lokasi skill Antigravity yang memang terpasang. Jangan mengarang path. Jika file skill tidak ditemukan, laporkan nama skill yang hilang dan lokasi yang sudah diperiksa; jangan berpura-pura sudah membacanya.

**Antislop adalah filter, bukan pengganti arahan desain.** Karena itu `DESIGN.md` wajib dibuat sebelum UI dianggap siap dipakai. Isi file tersebut dengan arah visual PKDI yang konkret, bukan hanya kalimat “buat modern dan minimalis”.

## 1.5 Aturan visual yang tidak boleh dilanggar

- Jangan gunakan emoji sebagai icon UI.
- Gunakan icon SVG dari **Lucide** dengan ukuran dan stroke konsisten. Jangan mencampur beberapa keluarga icon tanpa alasan.
- Jangan gunakan emoji, karakter Unicode dekoratif, atau icon font sebagai pengganti icon UI.
- Tagline perusahaan harus memiliki warna aksen yang terlihat jelas dan konsisten dengan identitas brand; jangan dibuat abu-abu pucat atau hilang di antara teks biasa. Gunakan tagline resmi bila tersedia. Jangan mengarang tagline perusahaan; bila belum tersedia, tandai sebagai placeholder yang mudah diganti.
- Arah identitas visual PKDI mengacu pada nuansa **marun dan hitam/charcoal** yang pernah ditentukan untuk identitas PKDI. Periksa logo/aset brand yang tersedia sebelum menetapkan kode warna final. Jika belum ada aset, usulkan palet di `DESIGN.md` dan jelaskan alasan serta kontrasnya.
- Gunakan warna status secara semantik: berhasil, menunggu, ditolak, sedang diproses, dan gagal harus mudah dibedakan serta tetap terbaca.
- Hindari tampilan dashboard generik: gradient besar tanpa fungsi, glassmorphism berlebihan, susunan card identik di semua halaman, dekorasi status dot yang tidak bermakna, grafik palsu, angka statistik rekaan, hero section yang tidak relevan, dan animasi berlebihan.
- Gunakan data contoh hanya untuk preview development dan tandai sebagai data dummy. Jangan menyajikan angka dummy sebagai statistik perusahaan sungguhan.
- Animasi harus ringan dan memiliki tujuan. Utamakan transisi halus, feedback interaksi, dan `prefers-reduced-motion`; hindari animasi berat yang memperlambat dashboard.
- Dashboard harus terasa seperti perangkat lunak operasional industri yang benar-benar dipakai karyawan, bukan landing page promosi.

---

# 2. Peta modul dan hak akses

| Modul | Client | Satpam | Kepala Gudang | Keuangan | Owner |
|---|---|---|---|---|---|
| Registrasi akun client | Buat akun sendiri | — | — | — | Melihat data sesuai kebutuhan |
| Membuat pesanan jasa | Buat dan lihat pesanan sendiri | Lihat data yang diperlukan untuk pemeriksaan gerbang | Lihat detail pesanan | Lihat rincian untuk perhitungan | Lihat seluruh ringkasan/detail |
| Pemeriksaan kendaraan masuk | Lihat status | Periksa dan setujui/tolak dengan alasan | Lihat status masuk | Lihat status transaksi | Lihat riwayat |
| Pemeriksaan muatan drum | Lihat hasil/status yang relevan | Lihat informasi kendaraan seperlunya | Periksa dan catat hasil | Lihat data hasil untuk perhitungan | Lihat riwayat |
| Proses pekerjaan gudang | Pantau timeline | Lihat status seperlunya | Kelola proses dan hasil | Terima hasil pekerjaan | Pantau seluruh proses |
| Tagihan dan pembayaran | Lihat tagihan/status dan informasi pembayaran | Tidak perlu akses nominal kecuali ada kebutuhan tervalidasi | Lihat status pembayaran yang diperlukan untuk surat jalan | Buat tagihan dan verifikasi pembayaran | Lihat transaksi dan ringkasan |
| Surat jalan | Lihat status/dokumen sesuai kebijakan | Verifikasi sebelum kendaraan keluar | Terbitkan setelah syarat terpenuhi | Lihat status pelunasan | Lihat riwayat |
| Log kendaraan masuk/keluar | Lihat status pesanan sendiri bila relevan | Catat dan lihat transaksi gerbang | Lihat riwayat terkait pesanan | Lihat status terkait tagihan | Lihat semua transaksi |
| Buku keuangan dan biaya operasional | Tidak boleh | Tidak boleh | Tidak boleh, kecuali data status yang diperlukan | Kelola | Lihat |
| Laporan dan ekspor keuangan | Tagihan milik sendiri bila tersedia | Tidak boleh | Tidak boleh | PDF/Excel | Lihat/unduh sesuai kebijakan |

Prinsip keamanan: **default deny**. Setiap role hanya boleh membuka data dan menjalankan tindakan yang memang menjadi kewenangannya. Client hanya boleh melihat pesanan dan tagihan miliknya sendiri.

---

# 3. Status dan aturan bisnis yang harus dirancang

Jangan menyimpan seluruh alur ke dalam satu kolom status yang terlalu umum. Pisahkan status pesanan/jasa, pemeriksaan gerbang, pembayaran, dan surat jalan bila diperlukan. Sediakan riwayat perubahan status yang menyimpan aktor, waktu, status sebelum/sesudah, dan catatan.

Contoh alur status awal untuk divalidasi dan disempurnakan:

1. Pesanan dikirim client.
2. Menunggu pemeriksaan kendaraan masuk.
3. Kendaraan masuk disetujui atau ditolak dengan alasan.
4. Menunggu pemeriksaan muatan gudang.
5. Pemeriksaan gudang disetujui atau perlu klarifikasi.
6. Pekerjaan sedang diproses.
7. Pekerjaan selesai.
8. Menunggu perhitungan Keuangan.
9. Tagihan diterbitkan.
10. Menunggu pembayaran.
11. Pembayaran diverifikasi/lunas.
12. Menunggu penerbitan surat jalan.
13. Surat jalan diterbitkan.
14. Menunggu pemeriksaan kendaraan keluar.
15. Kendaraan keluar disetujui dan transaksi selesai.

Ketentuan wajib:

- Satpam tidak dapat menyetujui kendaraan keluar hanya karena client mengatakan sudah membayar.
- Status lunas hanya boleh diberikan setelah pembayaran diverifikasi oleh Keuangan atau mekanisme resmi yang ditentukan.
- Surat jalan tidak boleh diterbitkan sebelum prasyarat pembayaran dan persetujuan terkait terpenuhi.
- Satpam harus memeriksa surat jalan yang sah sebelum mencatat kendaraan keluar.
- Setiap penolakan, ketidaksesuaian kendaraan/muatan, perubahan nominal tagihan, verifikasi pembayaran, dan penerbitan surat jalan harus meninggalkan audit trail.
- Perubahan status client harus berasal dari data server/database, bukan hanya perubahan tampilan di browser.
- “Realtime” harus benar-benar menampilkan perubahan status dari server. Tentukan implementasi yang sesuai; gunakan broadcasting/WebSocket bila infrastruktur mendukung, atau polling terukur sebagai fallback yang dijelaskan. Jangan mengklaim realtime jika halaman hanya berubah setelah pengguna refresh manual.

---

# 4. Roadmap pengerjaan bertahap

**Kerjakan satu fase pada satu waktu.** Jangan meminta Antigravity menjalankan semua fase sekaligus. Setelah satu fase selesai, periksa hasil, jalankan pengujian, dan baru kirim prompt fase berikutnya.

| Fase | Fokus | Hasil utama | Database |
|---|---|---|---|
| 0 | Audit workspace, PRD, dan skill Antislop | Laporan kondisi proyek, skill yang berhasil dibaca, rencana teknis | Belum disentuh |
| 1 | Analisis proses dan arsitektur | Flow, role-permission matrix, status lifecycle, data dictionary, daftar keputusan | Desain saja |
| 2 | Arah visual dan UX | `DESIGN.md`, sitemap, wireframe/komponen, panduan UI | Belum dieksekusi |
| 3 | UI frontend dengan data dummy | Layout, login/registrasi, dashboard dan halaman per role | Belum ada migrasi/tabel nyata |
| 4 | Fondasi Laravel dan autentikasi | Laravel 13, koneksi MySQL XAMPP, model, migration, seeder, role/permission | Mulai dibuat |
| 5 | Workflow operasional | Pesanan client, gerbang, gudang, timeline, status realtime | Terhubung |
| 6 | Keuangan dan owner | Tagihan, pembayaran, buku keuangan, biaya operasional, laporan | Terhubung |
| 7 | Integrasi, keamanan, dan QA | Pengujian end-to-end, audit akses, aksesibilitas, responsive, Delivery Gate | Divalidasi |
| 8 | UAT dan persiapan rilis | Panduan pengguna, panduan instalasi, checklist backup dan deployment | Siap diuji pengguna |

---

# 5. Prompt utama untuk Antigravity

Kirim prompt ini di awal project/sesi. Prompt per fase di bagian berikutnya tetap harus dikirim satu per satu.

```text
Kita akan membangun Sistem Manajemen Operasional PT Pulo Kencana Drum Industri (PKDI) berdasarkan PRD PKDI terbaru di workspace.

STACK WAJIB:
- PHP + Laravel 13
- MySQL melalui XAMPP untuk development lokal
- Blade + Tailwind CSS
- Vite untuk asset frontend
- Git untuk version control

Lima kategori pengguna: Client, Satpam, Kepala Gudang, Keuangan, dan Owner. Client dapat mendaftar sendiri dan jumlah akun client tidak dibatasi. Pengguna internal memiliki akun individual sesuai role. Jangan menganggap ada lima akun Satpam dan jangan menambahkan role Admin tanpa keputusan tertulis.

ANTISLOP WAJIB:
1. Temukan skill Antislop yang sudah terpasang di Antigravity. Jangan install ulang, clone ulang, atau membuat salinannya.
2. Baca SKILL.md dari antislop sebagai aturan inti sebelum bekerja.
3. Gunakan mode During untuk development ini sesuai mekanisme Two Usage Modes di skill inti.
4. Sebelum UI, baca antislop-ui. Untuk responsive baca antislop-layoutmobile. Untuk aksesibilitas dan interaksi baca antislop-human. Untuk seluruh microcopy baca antislop-copywriting. Untuk pembersihan komentar kode baca antislop-code.
5. Jangan mengaku menggunakan skill yang belum benar-benar dibaca.
6. Buat DESIGN.md sebelum membangun UI yang dianggap siap digunakan. Antislop merupakan filter, bukan pengganti art direction.

ATURAN UI:
- Dilarang menggunakan emoji sebagai icon.
- Gunakan Lucide SVG icons secara konsisten.
- Tagline perusahaan harus memakai warna aksen brand yang terlihat jelas. Gunakan tagline resmi jika tersedia; jangan mengarang tagline.
- Arah identitas visual PKDI mengikuti marun dan hitam/charcoal, tetapi periksa aset brand sebelum menetapkan HEX final.
- Jangan menghasilkan dashboard AI-slop atau template admin generik.
- Jangan gunakan angka statistik palsu sebagai data perusahaan. Data dummy hanya untuk preview dan harus jelas ditandai.
- Animasi dan transisi halus, ringan, memiliki tujuan, dan menghormati prefers-reduced-motion.
- Semua halaman wajib responsif dari mobile sampai desktop.

ATURAN PENGEMBANGAN:
- Baca PRD, file instruksi, struktur folder, dan dokumentasi dependency yang sudah ada sebelum mengubah kode.
- Jangan mengubah file yang tidak terkait.
- Jangan menjalankan migration, menghapus database, mengubah .env aktif, atau mengganti dependency tanpa fase yang memang mengizinkan dan tanpa menjelaskan dampaknya.
- Jangan tampilkan atau menyalin secret dari .env ke laporan.
- Terapkan validasi server-side, authorization/policy, CSRF protection, mass-assignment protection, dan audit trail.
- Client hanya bisa mengakses data miliknya sendiri.
- Pisahkan status pesanan/jasa, status gerbang, status pembayaran, dan status surat jalan bila diperlukan.
- Semua aktivitas penting harus bisa ditelusuri aktor dan waktunya.
- Tulis kode yang terbaca dan mudah dipelihara. Komentar hanya untuk menjelaskan alasan atau logika yang tidak jelas, bukan mengulang kode.
- Jangan menyelesaikan error dengan mematikan security atau menghapus test.

CARA KERJA:
- Kerjakan fase yang diminta saja.
- Sebelum coding, tulis rencana ringkas dan file yang akan disentuh.
- Implementasikan secara bertahap.
- Setelah implementasi, jalankan test/lint/build yang relevan dan laporkan hasil asli.
- Jika ada requirement yang belum jelas, catat sebagai NEEDS VALIDATION, jangan mengarang keputusan bisnis.
- Akhiri setiap fase dengan daftar file berubah, hasil validasi, risiko/keputusan yang belum terjawab, dan rekomendasi fase selanjutnya.
- Jangan lanjut ke fase berikutnya sebelum saya memberikan prompt fase berikutnya.
```

---

# 6. Prompt per fase

## FASE 0 — Audit project, environment, PRD, dan Antislop

**Tujuan:** memastikan Antigravity memahami workspace dan benar-benar membaca skill yang sudah tersedia. Tidak boleh mengubah kode pada fase ini.

```text
JALANKAN FASE 0 — AUDIT WORKSPACE PKDI.

Jangan coding dan jangan mengubah file apa pun pada fase ini.

1. Baca PRD PKDI terbaru yang ada di workspace. Jika tidak ditemukan, laporkan file yang dicari dan minta saya menempatkannya; jangan menyusun kebutuhan dari asumsi sendiri.
2. Audit struktur folder dan identifikasi apakah project Laravel sudah ada atau masih kosong.
3. Temukan instalasi Antislop yang sudah tersedia pada Antigravity. Baca isi lengkap:
   - antislop/SKILL.md (wajib)
   - antislop-ui/SKILL.md
   - antislop-layoutmobile/SKILL.md
   - antislop-human/SKILL.md
   - antislop-copywriting/SKILL.md
   - antislop-code/SKILL.md
   Baca skill tambahan yang relevan sesuai struktur skill yang ditemukan. Jangan menebak path. Catat path aktual setiap file yang berhasil dibaca.
4. Ikuti pilihan mode di skill inti. Untuk pekerjaan project ini gunakan During. Catat bahwa Antislop aktif dalam mode During sesuai format yang diminta skill.
5. Audit versi PHP CLI, Composer, Node, npm, Laravel (jika ada), serta konfigurasi XAMPP/MySQL yang dapat diperiksa. Jangan membuka atau menyalin secret dari .env.
6. Periksa apakah PHP CLI dan PHP Apache/XAMPP berpotensi berbeda versi. Laravel 13 membutuhkan PHP minimal 8.3.
7. Identifikasi dependency dan struktur frontend yang sudah ada. Jangan mengganti dependency.
8. Buat laporan audit di docs/project-audit.md. Jika folder docs belum ada, buat folder dan file dokumentasi saja.

LAPORAN HARUS MEMUAT:
- Kondisi workspace.
- Status PRD.
- Lokasi dan status pembacaan masing-masing Antislop skill.
- Status mode During.
- Versi tools yang ditemukan.
- Kesesuaian environment dengan Laravel 13.
- Risiko teknis dan hal yang perlu divalidasi.
- Rekomendasi urutan fase selanjutnya.

Jangan membuat migration, model, controller, route, atau UI pada fase ini. Berhenti setelah laporan selesai.
```

**Kriteria selesai Fase 0:** PRD teridentifikasi, skill yang relevan terbaca, versi environment tercatat, tidak ada kode aplikasi yang berubah.

---

## FASE 1 — Analisis kebutuhan, alur, role, dan arsitektur

**Tujuan:** mengubah PRD menjadi rancangan implementasi yang tidak ambigu. Belum membuat database sungguhan atau UI final.

```text
JALANKAN FASE 1 — ANALISIS SISTEM DAN ARSITEKTUR PKDI.

Baca PRD PKDI dan docs/project-audit.md. Terapkan antislop core. Jangan coding aplikasi dan jangan menjalankan migration.

Buat dokumentasi berikut:
1. docs/requirements.md — kebutuhan fungsional dan nonfungsional per role.
2. docs/user-flows.md — alur Client, Satpam, Kepala Gudang, Keuangan, dan Owner.
3. docs/role-permission-matrix.md — matriks izin lihat, tambah, ubah, setujui, verifikasi, cetak, ekspor, dan hapus.
4. docs/status-lifecycle.md — state machine pesanan, pemeriksaan gerbang, pekerjaan gudang, pembayaran, dan surat jalan. Pisahkan status yang memang berbeda.
5. docs/data-dictionary.md — definisi data dan relasi konseptual, belum berupa migration.
6. docs/system-architecture.md — arsitektur Laravel 13, Blade, Tailwind, MySQL, autentikasi, authorization, audit log, dan mekanisme update status.
7. docs/validation-needed.md — keputusan bisnis yang belum didukung PRD.

Pastikan alur mencakup:
- Client dapat registrasi sendiri dan hanya dapat melihat data miliknya.
- Client memasukkan kategori, jenis muatan, jenis jasa (perbarui/perbaiki), jenis kendaraan, nomor polisi, asal PT, serta nama driver opsional.
- Pesanan masuk ke antrean masing-masing role internal sesuai kebutuhan.
- Satpam memeriksa kendaraan masuk dan mencatat kendaraan keluar hanya setelah surat jalan sah diperiksa.
- Kepala Gudang memeriksa muatan, menyetujui pekerjaan, memperbarui proses, mencatat hasil, dan menerbitkan surat jalan setelah prasyarat terpenuhi.
- Keuangan menghitung jasa, menerbitkan tagihan di portal client, memverifikasi pembayaran, mencatat pemasukan/pengeluaran, dan membuat laporan.
- Owner dapat memantau transaksi seluruh bagian, pendapatan jasa, pembayaran, dan biaya operasional.

Jangan menambahkan fitur yang tidak diminta tanpa menandainya sebagai usulan. Jangan menetapkan cara pembayaran, rumus harga, detail kategori muatan, aturan pembatalan, atau pengelolaan akun internal jika belum ada dasar di PRD. Masukkan semuanya ke validation-needed.md.

Buat diagram Mermaid untuk alur utama dan status lifecycle jika membantu pembacaan. Periksa konsistensi antar dokumen. Akhiri dengan ringkasan keputusan yang sudah pasti dan daftar hal yang perlu persetujuan saya.

Jangan membangun UI atau backend pada fase ini.
```

**Kriteria selesai Fase 1:** semua role, alur, izin, status, entitas data, dan hal yang belum pasti terdokumentasi.

---

## FASE 2 — DESIGN.md dan rancangan UI/UX

**Tujuan:** menetapkan arah visual sebelum halaman dibuat. Fase ini wajib menggunakan `antislop-ui`, `antislop-human`, dan `antislop-layoutmobile`; gunakan `antislop-copywriting` untuk teks UI.

```text
JALANKAN FASE 2 — DESIGN SYSTEM DAN UX PKDI.

Baca dan terapkan:
- antislop core
- antislop-ui
- antislop-human
- antislop-layoutmobile
- antislop-copywriting

Baca docs/requirements.md, docs/user-flows.md, docs/role-permission-matrix.md, dan docs/status-lifecycle.md.

Buat DESIGN.md sebagai sumber utama arah visual project. Jangan mulai dari template admin dashboard generik. Rancang identitas produk untuk sistem operasional industri PKDI.

ARAH VISUAL:
- Gunakan identitas marun dan hitam/charcoal sebagai arah awal, lalu periksa logo/aset brand di workspace sebelum menetapkan warna final.
- Tentukan palet utama, warna permukaan, teks, border, warna status, dan aturan kontras.
- Tagline perusahaan harus memakai warna aksen yang jelas dan konsisten. Pakai tagline resmi bila tersedia. Jangan membuat tagline baru atas nama perusahaan.
- Icon UI wajib menggunakan Lucide SVG. Dilarang memakai emoji sebagai icon.
- Tentukan tipografi, hierarki heading, spacing, grid, radius, elevation, tabel, form, modal, badge status, toast, empty state, loading state, dan error state.
- Tentukan motion rules: ringan, singkat, fungsional, serta mendukung prefers-reduced-motion.
- Definisikan pola responsive mobile/tablet/desktop. Jangan hanya membuat dua keadaan ekstrem yang tidak nyaman pada ukuran menengah.

RANCANGAN UX:
- Buat sitemap terpisah untuk Client, Satpam, Kepala Gudang, Keuangan, dan Owner.
- Buat layout navigasi yang sesuai tugas setiap role; jangan memaksakan satu sidebar dan menu yang sama untuk semua role.
- Rancang tabel operasional dengan pencarian, filter, pagination, sorting, dan tindakan yang jelas.
- Rancang timeline status pesanan yang dapat dipahami client.
- Rancang alur pemeriksaan gerbang yang cepat digunakan Satpam.
- Rancang tampilan pemeriksaan muatan dan proses jasa untuk Kepala Gudang.
- Rancang halaman invoice, pencatatan pembayaran, buku keuangan, biaya operasional, dan laporan untuk Keuangan.
- Rancang overview Owner yang menyatukan pendapatan, biaya operasional, transaksi, dan status proses tanpa memberi kesan semua angka adalah data sungguhan.

OUTPUT:
1. DESIGN.md.
2. docs/ui-sitemap.md.
3. docs/ui-component-inventory.md.
4. docs/ui-screen-specification.md — tujuan, data, aksi, state, dan responsive behavior setiap halaman.
5. Daftar keputusan visual yang butuh persetujuan.

Belum perlu membuat backend, migration, atau menghubungkan database. Jangan menggunakan placeholder yang terlihat seperti konten final. Jangan menggunakan grafik dekoratif tanpa tujuan operasional.

Sebelum selesai, lakukan self-review berdasarkan checklist antislop-ui, antislop-human, dan antislop-layoutmobile. Laporkan temuan dan perbaikan pada dokumen.
```

**Kriteria selesai Fase 2:** `DESIGN.md` lengkap, sitemap dan spesifikasi halaman tersedia, penggunaan warna/tagline/icon telah didefinisikan.

---

## FASE 3 — Implementasi UI frontend menggunakan data dummy

**Tujuan:** membangun tampilan dan interaksi frontend terlebih dahulu agar desain dapat diperiksa sebelum database dan backend dieksekusi.

```text
JALANKAN FASE 3 — IMPLEMENTASI UI FRONTEND PKDI DENGAN MOCK DATA.

Baca DESIGN.md dan seluruh dokumen UI. Terapkan antislop core, antislop-ui, antislop-human, antislop-layoutmobile, dan antislop-copywriting.

Bangun UI menggunakan Laravel Blade + Tailwind CSS sesuai stack. Jika project Laravel belum dibuat, siapkan fondasi frontend Laravel 13 yang diperlukan tanpa membuat schema database bisnis. Jangan menjalankan migration bisnis pada fase ini.

HALAMAN YANG DIBANGUN:
A. Public dan Client:
- halaman login.
- registrasi client.
- dashboard client.
- daftar pesanan.
- form pesanan jasa.
- detail pesanan dan timeline status.
- daftar tagihan dan detail tagihan.
- tampilan status pembayaran.

B. Satpam:
- dashboard antrean kendaraan.
- detail pemeriksaan kendaraan masuk.
- form catatan pemeriksaan dan persetujuan/penolakan.
- pemeriksaan surat jalan dan kendaraan keluar.
- riwayat kendaraan masuk/keluar.

C. Kepala Gudang:
- dashboard antrean pekerjaan.
- detail pesanan dan pemeriksaan muatan.
- persetujuan pekerjaan.
- pembaruan proses jasa.
- pencatatan hasil pekerjaan.
- halaman penerbitan/pratinjau surat jalan.

D. Keuangan:
- dashboard keuangan.
- daftar hasil pekerjaan yang menunggu perhitungan.
- form perhitungan jasa dan pembuatan tagihan.
- daftar invoice dan detail pembayaran.
- verifikasi pembayaran.
- buku transaksi keuangan.
- pencatatan biaya operasional.
- laporan dan kontrol ekspor PDF/Excel (boleh berupa UI preview dahulu, jangan mengklaim ekspor sudah bekerja jika belum).

E. Owner:
- dashboard ringkasan seluruh aktivitas.
- ringkasan pendapatan jasa dan pengeluaran operasional.
- daftar transaksi lintas bagian.
- detail transaksi dan riwayat status.

ATURAN IMPLEMENTASI:
- Setiap role memiliki navigasi dan dashboard yang relevan.
- Gunakan fixture/mock data yang realistis tetapi jelas sebagai data demo.
- Buat semua state penting: loading, empty, error, success, disabled, selected, validation error, dan confirmation.
- Form memiliki label yang jelas, bukan placeholder sebagai pengganti label.
- Nama driver bersifat opsional sesuai PRD.
- Nomor polisi kendaraan harus tersedia untuk pemeriksaan Satpam.
- Semua icon UI menggunakan Lucide SVG; tidak boleh ada emoji icon.
- Tagline berwarna aksen sesuai DESIGN.md.
- Semua tombol dan kontrol yang ditampilkan harus memiliki interaksi yang masuk akal. Jangan membuat tombol dekoratif yang tidak melakukan apa-apa.
- Jangan menyamarkan mock data sebagai data perusahaan.
- Pastikan layout berfungsi pada lebar mobile, tablet, laptop, dan desktop.
- Gunakan animasi ringan dan jangan menambahkan library animasi besar tanpa alasan.

Setelah selesai:
1. Jalankan npm run build.
2. Jalankan pemeriksaan yang tersedia.
3. Buka/preview seluruh role dan lakukan smoke test interaksi UI.
4. Buat docs/ui-review-phase-3.md berisi halaman yang sudah jadi, interaksi yang diuji, masalah responsive, dan perbaikan.

Belum membuat migration bisnis, model bisnis, dan integrasi transaksi nyata. Berhenti setelah UI siap direview.
```

**Kriteria selesai Fase 3:** semua halaman utama kelima role tersedia, responsif, interaktif dengan mock data, dan telah direview sebelum backend dibangun.

---

## FASE 4 — Fondasi Laravel 13, MySQL, autentikasi, dan authorization

**Tujuan:** mulai menghubungkan sistem dengan database setelah rancangan UI diterima.

```text
JALANKAN FASE 4 — BACKEND FOUNDATION DAN AUTHENTICATION.

Sebelum coding, baca kembali PRD, docs/system-architecture.md, docs/data-dictionary.md, docs/role-permission-matrix.md, dan DESIGN.md. Terapkan antislop core dan antislop-code bila meninjau komentar kode.

KONFIGURASI:
- Pastikan framework tepat Laravel 13.
- Pastikan PHP minimal 8.3 pada CLI dan environment web XAMPP.
- Gunakan MySQL dari XAMPP.
- Konfigurasi .env secara lokal tanpa menampilkan password/secret pada output.
- Jangan menimpa .env pengguna. Bila perlu, perbarui .env.example dan berikan instruksi manual untuk nilai lokal.
- Pertahankan Blade + Tailwind + Vite.

IMPLEMENTASI:
1. Rancang migration berdasarkan data dictionary yang telah disetujui.
2. Buat model, relasi, factory, dan seeder yang diperlukan.
3. Implementasikan autentikasi.
4. Implementasikan registrasi mandiri khusus Client.
5. Akun internal dibuat melalui mekanisme yang sudah disetujui pada Fase 1. Jika belum ada keputusan, jangan mengarang role Admin; sediakan mekanisme setup awal yang aman dan catat keputusan yang masih diperlukan.
6. Implementasikan role dan authorization menggunakan middleware, policy, atau mekanisme Laravel yang sesuai.
7. Pastikan Client hanya bisa melihat dan mengubah data yang diizinkan miliknya sendiri.
8. Pisahkan menu berdasarkan role pada server, bukan hanya menyembunyikan tombol di frontend.
9. Tambahkan audit log untuk aksi penting sesuai rancangan.
10. Pastikan validasi dilakukan di server.

KEAMANAN:
- Jangan menyimpan password plain text.
- Validasi dan normalisasi input.
- Cegah mass assignment yang tidak sah.
- Terapkan CSRF, session security, authorization per resource, dan pembatasan akses file bila ada.
- Jangan percaya role atau nominal yang dikirim dari browser.

TEST WAJIB:
- Client dapat registrasi dan login.
- Pengguna internal dapat login sesuai akun yang disediakan.
- Role tidak dapat membuka route milik role lain.
- Client A tidak dapat melihat pesanan/tagihan Client B meskipun URL atau ID diganti.
- Seeder hanya membuat data demo yang jelas.
- Migration dapat dijalankan pada database development yang benar.

Jangan mengimplementasikan seluruh workflow bisnis pada fase ini. Fokus pada fondasi, autentikasi, akses, dan schema yang telah disetujui. Laporkan command yang dijalankan dan hasil test sebenarnya.
```

**Kriteria selesai Fase 4:** Laravel 13 berjalan, MySQL XAMPP tersambung, login/registrasi dan role access bekerja serta diuji.

---

## FASE 5 — Workflow pesanan, gerbang, gudang, dan status client

**Tujuan:** membangun alur operasional inti dari pesanan sampai pekerjaan gudang selesai.

```text
JALANKAN FASE 5 — WORKFLOW PESANAN, SATPAM, DAN GUDANG.

Baca docs/status-lifecycle.md dan docs/user-flows.md. Terapkan antislop core serta skill UI/human/layoutmobile/copywriting saat menyentuh halaman terkait.

IMPLEMENTASIKAN SECARA BERURUTAN:
1. Client membuat pesanan dengan kategori, jenis muatan, jenis jasa, jenis kendaraan, nomor polisi, asal PT, dan nama driver opsional.
2. Simpan pesanan menggunakan validasi server-side.
3. Pesanan muncul pada antrean role internal yang relevan.
4. Satpam membuka detail kendaraan, mencocokkan informasi, mencatat hasil pemeriksaan, menyetujui atau menolak kendaraan masuk, serta mencatat waktu dan pelaku.
5. Kepala Gudang melihat pesanan yang kendaraannya telah masuk, memeriksa muatan drum, mencatat hasil pemeriksaan, dan memberikan persetujuan atau catatan ketidaksesuaian.
6. Kepala Gudang memperbarui progres pekerjaan dan mencatat hasil akhir jasa.
7. Client dapat melihat timeline dan status yang berasal dari database.
8. Owner dapat melihat perkembangan keseluruhan tanpa melanggar pembatasan data.

ATURAN:
- Setiap transisi status harus diperiksa di backend.
- Jangan izinkan lompat status yang tidak sah.
- Simpan actor_id, timestamp, status sebelum/sesudah, dan catatan untuk perubahan penting.
- Penolakan harus memiliki alasan.
- Hindari duplicate submission pada tombol aksi dan request berulang.
- Gunakan transaksi database untuk operasi yang perlu konsistensi.
- Bedakan status pesanan, gerbang, pemeriksaan gudang, dan pekerjaan jasa sesuai rancangan.

REALTIME:
Implementasikan update status dari server ke dashboard Client dan internal. Evaluasi broadcasting/WebSocket Laravel yang sesuai dengan environment. Jika memakai polling sebagai tahap awal, interval harus terukur, tidak membebani database, dan disebut sebagai fallback polling—jangan mengklaim push realtime.

TEST:
- Pesanan valid berhasil dibuat.
- Field wajib dan opsional sesuai aturan.
- Satpam tidak bisa menyetujui kendaraan yang datanya tidak cocok tanpa keputusan/catatan yang sesuai.
- Kepala Gudang tidak bisa memproses pesanan sebelum kendaraan masuk disetujui.
- Client melihat perubahan status.
- Client tidak bisa mengakses pesanan client lain.
- Perubahan status tercatat di audit trail.

Jangan implementasikan pembayaran dan surat jalan final pada fase ini kecuali bagian status placeholder yang diperlukan. Buat laporan test dan berhenti.
```

**Kriteria selesai Fase 5:** pesanan, pemeriksaan gerbang masuk, pemeriksaan muatan, proses gudang, hasil pekerjaan, dan timeline client berjalan end-to-end.

---

## FASE 6 — Keuangan, tagihan, pembayaran, surat jalan, dan Owner

**Tujuan:** melengkapi siklus komersial dan izin pengeluaran barang.

```text
JALANKAN FASE 6 — KEUANGAN, TAGIHAN, PEMBAYARAN, SURAT JALAN, DAN OWNER.

Baca PRD, docs/status-lifecycle.md, docs/role-permission-matrix.md, docs/data-dictionary.md, dan keputusan pada docs/validation-needed.md. Jangan mengarang tarif atau metode pembayaran. Gunakan aturan yang telah disetujui; keputusan yang belum ada harus ditandai NEEDS VALIDATION.

KEUANGAN:
1. Tampilkan pekerjaan selesai yang menunggu perhitungan.
2. Buat rincian perhitungan biaya jasa yang dapat ditelusuri.
3. Buat invoice/tagihan dengan nomor unik dan detail jasa.
4. Tampilkan tagihan pada akun client yang benar.
5. Simpan histori perubahan invoice dan nominal sesuai aturan yang disepakati.
6. Sediakan pencatatan pembayaran dan proses verifikasi oleh Keuangan.
7. Hanya Keuangan atau mekanisme resmi yang diberi kewenangan boleh menandai pembayaran lunas.
8. Catat pemasukan dan pengeluaran pada buku transaksi keuangan.
9. Buat modul biaya operasional untuk item seperti pembelian bahan dan gaji karyawan. Untuk MVP, catat gaji sebagai biaya operasional jika belum ada kebutuhan payroll penuh; jangan membangun sistem payroll otomatis tanpa persetujuan.
10. Sediakan filter periode, kategori, jenis transaksi, pencarian, detail transaksi, dan ekspor PDF/Excel.

SURAT JALAN DAN PENGELUARAN:
1. Setelah status lunas terverifikasi, Kepala Gudang dapat menerbitkan surat jalan berdasarkan prasyarat yang sah.
2. Surat jalan memiliki identitas/nomor unik, referensi pesanan, kendaraan, dan detail muatan sesuai data yang telah divalidasi.
3. Satpam dapat melihat status dan memeriksa surat jalan pada proses keluar.
4. Satpam mencocokkan nomor surat jalan, kendaraan, dan data yang diwajibkan.
5. Jika sesuai, Satpam mencatat waktu keluar dan menyelesaikan transaksi.
6. Jika tidak sesuai, kendaraan tidak boleh ditandai keluar dan alasan wajib dicatat.

OWNER:
- Tampilkan pendapatan jasa, tagihan, pembayaran, biaya operasional, dan ringkasan transaksi lintas role.
- Sediakan filter tanggal/periode dan detail drill-down ke sumber transaksi.
- Jangan membuat angka contoh seolah-olah angka riil.
- Owner bersifat pemantauan sesuai permission yang sudah disepakati. Jangan memberi hak edit semua transaksi secara otomatis.

LAPORAN DAN KONSISTENSI:
- Total laporan harus berasal dari transaksi tersimpan, bukan angka hardcode.
- Tentukan apakah laporan memakai tanggal invoice, tanggal pembayaran, atau tanggal transaksi; jelaskan pada UI.
- Cegah pencatatan pembayaran ganda.
- Gunakan format rupiah dan tanggal Indonesia secara konsisten.
- Ekspor PDF/Excel harus mengikuti filter dan data yang sedang dipilih.
- Periksa otorisasi untuk unduhan dokumen; client hanya boleh mengunduh invoice miliknya.

TEST:
- Invoice dibuat dari hasil pekerjaan yang valid.
- Client hanya melihat invoice miliknya.
- Status lunas hanya setelah verifikasi.
- Surat jalan tidak dapat diterbitkan sebelum syarat terpenuhi.
- Satpam tidak dapat menyelesaikan kendaraan keluar tanpa surat jalan yang valid.
- Buku transaksi sesuai dengan invoice/pembayaran/biaya operasional.
- Ekspor PDF/Excel sesuai filter dan total data.
- Owner dapat melihat rekap yang konsisten.

Jangan lanjut ke deployment. Laporkan seluruh hasil dan test.
```

**Kriteria selesai Fase 6:** siklus hasil gudang → invoice → pembayaran terverifikasi → surat jalan → pemeriksaan Satpam → transaksi selesai berjalan; pencatatan dan laporan keuangan konsisten.

---

## FASE 7 — Integrasi, keamanan, responsive, dan Antislop Delivery Gate

**Tujuan:** menguji sistem secara menyeluruh, bukan hanya memastikan halaman terlihat bagus.

```text
JALANKAN FASE 7 — QA END-TO-END, SECURITY REVIEW, DAN ANTISLOP DELIVERY GATE.

Baca ulang Antislop core dan skill yang sesuai: antislop-ui, antislop-human, antislop-layoutmobile, antislop-copywriting, dan antislop-code bila melakukan review komentar kode.

JANGAN langsung mengubah semua temuan. Audit dahulu, urutkan berdasarkan severity, perbaiki bug yang jelas sesuai scope, dan catat risiko yang memerlukan keputusan.

1. TEST END-TO-END:
   - Client registrasi.
   - Client membuat pesanan.
   - Satpam memeriksa kendaraan masuk.
   - Kepala Gudang memeriksa muatan dan mengelola pekerjaan.
   - Client menerima update status.
   - Keuangan menghitung jasa dan menerbitkan invoice.
   - Client melihat tagihan.
   - Keuangan memverifikasi pembayaran.
   - Kepala Gudang menerbitkan surat jalan.
   - Satpam memeriksa surat jalan dan mencatat kendaraan keluar.
   - Owner melihat transaksi dan laporan yang sesuai.

2. TEST OTORISASI:
   - Uji seluruh pasangan role-route.
   - Uji manipulasi ID/URL untuk mengakses data user lain.
   - Uji akses invoice, file, laporan, dan endpoint melalui request langsung.
   - Pastikan frontend hidden button bukan satu-satunya perlindungan.

3. TEST DATA DAN KEUANGAN:
   - Uji input kosong, format salah, nilai negatif, nominal ekstrem, dan data duplikat.
   - Uji pembayaran ganda dan perubahan status yang tidak sah.
   - Cocokkan buku transaksi, invoice, pembayaran, biaya, dan laporan.

4. TEST RESPONSIVE:
   - Uji layar ponsel kecil, ponsel besar, tablet, laptop, dan desktop.
   - Periksa tabel lebar, modal, navigasi, form, filter, tombol aksi, overflow horizontal, dan target sentuh.
   - Pastikan tidak hanya ada layout phone dan desktop; uji breakpoint menengah.

5. TEST HUMAN/ACCESSIBILITY:
   - Navigasi keyboard.
   - Focus visible.
   - Label form dan pesan error.
   - Kontras teks dan status.
   - State loading/empty/error/success/disabled.
   - prefers-reduced-motion.

6. ANTISLOP DELIVERY GATE:
   - Jalankan checklist Delivery Gate dari skill core yang terpasang.
   - Klik dan uji satu per satu seluruh elemen interaktif penting; catat bukti hasilnya.
   - Pastikan icon UI bukan emoji dan konsisten dengan Lucide.
   - Pastikan tagline menggunakan warna aksen sesuai DESIGN.md.
   - Pastikan tidak ada grafik/angka palsu, tombol mati, placeholder yang tertinggal, atau komponen dekoratif tanpa fungsi.
   - Pastikan laporan Delivery Gate menyatakan PASS/FAIL dengan jujur. Jangan mengklaim PASS jika masih ada blocker.

OUTPUT:
- docs/qa-test-plan.md
- docs/qa-test-results.md
- docs/security-review.md
- docs/responsive-accessibility-review.md
- laporan Antislop Delivery Gate

Jalankan test, build, dan pemeriksaan yang relevan. Jangan menghapus test atau menurunkan validasi untuk membuat hasil terlihat hijau.
```

**Kriteria selesai Fase 7:** alur end-to-end diuji, akses data aman, tampilan responsif, aksesibilitas diperiksa, dan Delivery Gate melaporkan PASS/FAIL secara jujur.

---

## FASE 8 — UAT, dokumentasi, dan persiapan rilis

**Tujuan:** menyiapkan sistem agar dapat diuji oleh pihak PKDI dan dipasang kembali dengan prosedur yang jelas.

```text
JALANKAN FASE 8 — USER ACCEPTANCE TEST DAN DOKUMENTASI RILIS PKDI.

Baca PRD, seluruh dokumen docs/, hasil QA, dan laporan Antislop Delivery Gate.

Buat dokumentasi:
1. docs/user-guide-client.md
2. docs/user-guide-satpam.md
3. docs/user-guide-kepala-gudang.md
4. docs/user-guide-keuangan.md
5. docs/user-guide-owner.md
6. docs/local-installation-xampp.md
7. docs/database-backup-restore.md
8. docs/release-checklist.md
9. docs/known-limitations.md

Panduan harus menggunakan langkah yang benar-benar sesuai dengan implementasi. Jangan menulis tombol/menu yang sebenarnya belum ada.

Siapkan UAT checklist per role dengan skenario berhasil dan skenario penolakan/kesalahan. Pisahkan:
- Fitur yang sudah siap diuji.
- Fitur yang belum selesai.
- Keputusan bisnis yang menunggu persetujuan PKDI.
- Risiko teknis yang masih ada.

Untuk persiapan produksi:
- Jelaskan perbedaan environment lokal XAMPP dan server produksi.
- Jangan menyarankan XAMPP sebagai server publik produksi tanpa penilaian keamanan dan operasional.
- Siapkan checklist HTTPS, APP_DEBUG=false, pengelolaan secret, backup, storage permission, scheduler/queue jika digunakan, logging, monitoring, dan prosedur rollback.
- Jangan melakukan deployment atau mengubah server produksi tanpa instruksi eksplisit.

Akhiri dengan ringkasan kesiapan UAT dan daftar persetujuan yang dibutuhkan dari PKDI.
```

**Kriteria selesai Fase 8:** panduan tiap role, panduan instalasi lokal, backup/restore, UAT, limitasi, serta checklist rilis tersedia dan sesuai aplikasi.

---

# 7. Cara menggunakan prompt ini di Antigravity

1. Buka workspace project PKDI yang benar.
2. Letakkan PRD PKDI terbaru di workspace, misalnya pada `docs/PRD_PKDI.md`, agar Antigravity dapat membaca sumber kebutuhan secara langsung.
3. Kirim **Prompt Utama** terlebih dahulu.
4. Kirim **Fase 0** dan tunggu sampai laporan audit selesai.
5. Periksa laporan Fase 0. Pastikan Antigravity benar-benar menyebut path skill dan hasil pembacaannya.
6. Lanjutkan Fase 1, lalu Fase 2, dan seterusnya. Jangan melewati fase hanya karena agent mengatakan sudah selesai.
7. Setelah Fase 3, periksa UI semua role sebelum mengizinkan pembuatan schema database pada Fase 4.
8. Simpan perubahan penting menggunakan Git commit per fase agar mudah rollback.
9. Bila Antigravity membuka sesi baru, ingatkan kembali agar membaca instruksi project dan Antislop core; skill harus dibaca sesuai kebutuhan fase.

## Checklist persetujuan manual

- [ ] PRD terbaru sudah diletakkan di workspace.
- [ ] Audit environment dan Antislop selesai.
- [ ] Role dan permission disetujui.
- [ ] Status lifecycle disetujui.
- [ ] DESIGN.md dan tagline/warna brand disetujui.
- [ ] UI seluruh role disetujui sebelum database dibuat.
- [ ] Alur harga dan perhitungan jasa disetujui.
- [ ] Metode pembayaran dan bukti pembayaran disetujui.
- [ ] Aturan penerbitan surat jalan disetujui.
- [ ] UAT dilakukan dengan perwakilan PKDI.

---

# 8. Keputusan bisnis yang harus divalidasi dengan PKDI

Jangan membiarkan AI menentukan hal-hal berikut secara sepihak:

1. Siapa yang berwenang membuat, menonaktifkan, dan mereset akun internal?
2. Apakah terdapat satu akun Satpam bersama atau beberapa akun individual untuk personel Satpam? Rekomendasi keamanan: satu akun per orang, bukan akun bersama.
3. Apakah satu pesanan hanya terkait satu kendaraan atau dapat melibatkan beberapa kendaraan?
4. Daftar kategori drum, jenis muatan, dan jenis jasa apa saja yang harus tersedia?
5. Bagaimana cara menghitung tarif jasa: per drum, per jenis layanan, per kategori, berdasarkan kondisi, atau kombinasi?
6. Apakah client membayar melalui transfer manual, tunai, virtual account, atau payment gateway?
7. Apakah client perlu mengunggah bukti pembayaran sebelum Keuangan memverifikasi?
8. Apakah pembayaran wajib lunas 100% sebelum barang boleh keluar, atau ada pengecualian dengan otorisasi khusus?
9. Siapa yang membuat dan menandatangani surat jalan? Apakah perlu nomor surat otomatis dan format cetak khusus perusahaan?
10. Data apa yang wajib ada pada pemeriksaan muatan, hasil pengerjaan, dan surat jalan?
11. Apakah biaya gaji dicatat sebagai satu transaksi biaya operasional atau perlu modul payroll terpisah?
12. Apakah Owner hanya melihat data atau juga boleh menyetujui transaksi tertentu?
13. Apakah client boleh membatalkan pesanan dan sampai tahap mana pembatalan masih diperbolehkan?
14. Bagaimana perlakuan jika kendaraan datang tetapi data kendaraan/driver berbeda dari pesanan?
15. Apakah perlu upload foto kondisi drum sebelum/sesudah pengerjaan?
16. Berapa lama data transaksi dan dokumen harus disimpan?
17. Siapa yang boleh mengoreksi invoice atau transaksi yang sudah diterbitkan?
18. Apakah laporan keuangan membutuhkan saldo kas/bank awal dan rekonsiliasi, atau cukup buku pemasukan-pengeluaran?

---

# 9. Definition of Done keseluruhan proyek

Project tidak dianggap selesai hanya karena dashboard bisa dibuka. Sistem baru bisa dinyatakan siap UAT apabila:

- Lima kategori pengguna memiliki pengalaman dan hak akses yang sesuai.
- Client bisa mendaftar sendiri dan hanya melihat data miliknya.
- Alur pesanan → gerbang masuk → gudang → hasil pekerjaan → tagihan → pembayaran → surat jalan → gerbang keluar berjalan konsisten.
- Perubahan status berasal dari server dan terlihat oleh pengguna terkait.
- Keuangan dapat melacak pemasukan jasa dan biaya operasional.
- Owner dapat memantau transaksi serta ringkasan keuangan.
- PDF/Excel berisi data sesuai filter dan hasil perhitungannya konsisten.
- Seluruh tindakan sensitif memiliki otorisasi dan audit trail.
- UI sesuai DESIGN.md, tanpa emoji sebagai icon, tagline berwarna aksen, dan responsif.
- Test inti, security review, responsive review, dan Antislop Delivery Gate selesai dengan laporan jujur.
- Dokumentasi instalasi, backup/restore, panduan pengguna, dan UAT tersedia.

**Catatan akhir:** dokumen ini adalah roadmap dan kumpulan prompt kerja. PRD PKDI tetap menjadi sumber utama kebutuhan bisnis. Bila prompt dan PRD bertentangan, jangan memilih sendiri; catat konflik dan minta keputusan pemilik proyek.

---

# Referensi resmi

- Antislop repository: https://github.com/miqdadbadjuber/anti-slop
- Antislop README dan penggunaan skill: https://github.com/miqdadbadjuber/anti-slop#readme
- Laravel 13 deployment requirements: https://laravel.com/docs/13.x/deployment
- Laravel 13 release notes: https://laravel.com/docs/13.x/releases
