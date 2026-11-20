# Alur Pengguna Lintas Peran (User Flows)
## Sistem Manajemen Operasional, Layanan Drum, dan Keuangan PT Pulo Kencana Drum Industri (PKDI)

**Versi Dokumen:** 1.0 (Fase 1)
**Sumber Acuan:** [PRD_PKDI_Sistem_Operasional_PT.md](file:///c:/sistem_pkdi/docs/PRD_PKDI_Sistem_Operasional_PT.md) dan [requirements.md](file:///c:/sistem_pkdi/docs/requirements.md)

---

## 1. Gambaran Umum Alur Operasional End-to-End

Setiap pesanan layanan drum bergerak melalui tahapan berurutan yang melibatkan lima peran pengguna (**Client**, **Satpam**, **Kepala Gudang**, **Keuangan**, dan **Owner**). Tidak ada tahapan wajib yang dapat dilompati.

```mermaid
sequenceDiagram
    autonumber
    actor CLI as Client (PT Pelanggan)
    participant SYS as Sistem PKDI
    actor SAT as Satpam (Pos Gerbang)
    actor WH as Kepala Gudang
    actor FIN as Keuangan
    actor OWN as Owner

    CLI->>SYS: Registrasi akun & lengkapi profil PT
    CLI->>SYS: Buat Pesanan Jasa (kategori, muatan, jasa, nopol, driver opsional)
    SYS-->>SAT: Masuk antrean Kendaraan Akan Datang (WAITING_GATE_IN)
    SYS-->>WH: Informasi awal pesanan baru
    SYS-->>OWN: Tercatat pada ringkasan pesanan baru

    alt Data kendaraan di gerbang masuk sesuai
        SAT->>SYS: Verifikasi nopol/kendaraan/driver & klik "Setujui Masuk"
        SYS-->>WH: Masuk antrean Pemeriksaan Muatan (WAITING_WAREHOUSE)
        SYS-->>CLI: Update timeline: Kendaraan Masuk Area Pabrik
    else Data kendaraan tidak sesuai
        SAT->>SYS: Klik "Tahan Masuk" + isi alasan wajib (GATE_IN_HELD)
        SYS-->>CLI: Notifikasi penahanan kendaraan di gerbang
    end

    WH->>SYS: Periksa fisik drum & catat kuantitas/kondisi aktual (WAREHOUSE_REVIEW)
    alt Muatan disetujui untuk dikerjakan
        WH->>SYS: Klik "Setujui dan Proses" (IN_PROGRESS)
        WH->>SYS: Catat perkembangan pengerjaan & hasil akhir aktual (SERVICE_COMPLETED)
        SYS-->>FIN: Masuk antrean Perhitungan Tagihan (WAITING_FINANCE)
        SYS-->>CLI: Notifikasi pekerjaan selesai, menunggu tagihan
    else Perlu konfirmasi / Ditolak
        WH->>SYS: Set "Minta Klarifikasi" (NEEDS_CLARIFICATION) atau "Tolak" + alasan
        SYS-->>CLI: Notifikasi klarifikasi / penolakan muatan
    end

    FIN->>SYS: Hitung biaya dari hasil aktual gudang & simpan Draft Invoice
    FIN->>SYS: Terbitkan Invoice PDF (INVOICE_ISSUED)
    SYS-->>CLI: Notifikasi tagihan terbit & siap diunduh

    CLI->>SYS: Lihat tagihan, lakukan pembayaran & unggah bukti bayar
    FIN->>SYS: Periksa bukti bayar & verifikasi nominal
    alt Pembayaran penuh terverifikasi
        FIN->>SYS: Tandai "Lunas" (PAID) -> otomatis catat Kas Masuk
        SYS-->>WH: Masuk antrean Persiapan Surat Jalan (WAITING_EXIT_DOCUMENT)
        SYS-->>CLI: Notifikasi pembayaran lunas
    else Pembayaran sebagian / bukti ditolak
        FIN->>SYS: Set "Pembayaran Sebagian" (PAYMENT_PARTIAL) atau tolak bukti + alasan
        SYS-->>CLI: Barang belum diizinkan keluar
    end

    WH->>SYS: Buat & terbitkan Surat Jalan bernomor unik (WAITING_GATE_OUT)
    SYS-->>SAT: Surat Jalan tampil di antrean Pemeriksaan Keluar

    alt Surat Jalan, nopol, muatan & status PAID valid
        SAT->>SYS: Verifikasi fisik & klik "Setujui Keluar" (COMPLETED)
        SYS-->>CLI: Timeline selesai (Kendaraan Keluar)
        SYS-->>OWN: Transaksi operasional & kas terbarui penuh
    else Ada ketidaksesuaian saat keluar
        SAT->>SYS: Klik "Tahan Keluar" + isi alasan wajib (GATE_OUT_HELD)
    end
```

---

## 2. Alur Rinci per Peran Pengguna

### 2.1 Alur Client (Perusahaan Pelanggan)

#### A. Registrasi dan Pengelolaan Profil Perusahaan
1. Client membuka halaman registrasi publik dan mengisi nama perusahaan (PT), alamat perusahaan, nama penanggung jawab (PIC), alamat email, nomor telepon/kontak, serta kata sandi.
2. Sistem memvalidasi input, membuat data `client_companies` dan akun `users` dengan role `client`.
   - *Catatan Validasi:* Apakah akun langsung aktif atau memerlukan verifikasi internal terlebih dahulu didokumentasikan pada [validation-needed.md](file:///c:/sistem_pkdi/docs/validation-needed.md).
3. Setelah masuk ke portal Client, pengguna dapat memperbarui detail profil perusahaan dan kontak kapan saja melalui menu Profil Perusahaan.

#### B. Pengajuan Pesanan Layanan Drum
1. Client memilih menu **Buat Pesanan**.
2. Client mengisi formulir pengajuan pesanan:
   - Kategori layanan drum (dipilih dari master data).
   - Jenis muatan (dipilih dari master data).
   - Jenis layanan yang diminta: **Diperbarui** atau **Diperbaiki** (atau jenis layanan lain pada master data).
   - Jenis kendaraan pengangkut (misalnya Truk Engkel, Fuso, Tronton, Pick Up).
   - Nomor polisi kendaraan (wajib diisi untuk pemeriksaan pos gerbang).
   - Nama perusahaan/PT asal (otomatis terisi dari profil PT Client, dapat disesuaikan bila mewakili sub-unit yang diizinkan).
   - Nama driver (opsional).
   - Catatan tambahan dan lampiran dokumen pengantar (opsional).
3. Client meninjau ringkasan isian dan menekan tombol **Kirim Pesanan**.
4. Sistem menyimpan pesanan dengan nomor pesanan unik (contoh: `selasa-17/11/26-1`), mencatat waktu pengajuan (WIB), menetapkan status `WAITING_GATE_IN`, dan menampilkan halaman detail pesanan beserta timeline status.

#### C. Pemantauan Progres, Tagihan, dan Pembayaran
1. Dari Dashboard Client atau menu **Pesanan Saya**, Client memantau status terkini setiap pesanan.
2. Saat Kepala Gudang selesai mencatat pemeriksaan muatan dan hasil pengerjaan, Client dapat melihat ringkasan hasil aktual pada halaman detail pesanan.
3. Saat Keuangan menerbitkan tagihan (`INVOICE_ISSUED`), Client menerima notifikasi, dapat melihat rincian biaya pada menu **Tagihan & Pembayaran**, serta mengunduh dokumen Invoice dalam format PDF.
4. Client melakukan pembayaran sesuai instruksi pada invoice, lalu mengunggah bukti pembayaran (nominal yang dibayar, tanggal bayar, metode/bank pengirim, nomor referensi, dan berkas bukti).
5. Setelah Keuangan memverifikasi pelunasan (`PAID`), Kepala Gudang menerbitkan Surat Jalan, dan Satpam menyetujui kendaraan keluar, status pesanan pada portal Client berubah menjadi **Selesai (Kendaraan Keluar)**.

---

### 2.2 Alur Satpam (Pos Gerbang Masuk & Keluar)

```mermaid
flowchart TD
    A["Kendaraan tiba di Pos Gerbang PKDI"] --> B{"Jenis Pemeriksaan?"}
    
    B -->|"Kendaraan Masuk"| C["Cari Pesanan: Nopol / No. Pesanan / Nama PT / Driver"]
    C --> D["Bandingkan Fisik vs Data Pesanan:<br/>1. Nomor Polisi<br/>2. Jenis Kendaraan<br/>3. Nama Driver (catat aktual jika kosong)"]
    D --> E{"Apakah Data Sesuai?"}
    E -->|"Sesuai"| F["Klik 'Setujui Masuk'<br/>Catat waktu masuk & akun Satpam"]
    F --> G["Status: WAITING_WAREHOUSE<br/>Masuk ke antrean Kepala Gudang"]
    E -->|"Tidak Sesuai"| H["Klik 'Tahan Masuk'<br/>Wajib isi alasan penahanan"]
    H --> I["Status: GATE_IN_HELD<br/>Notifikasi ke Gudang & Client"]

    B -->|"Kendaraan Keluar"| J["Cari Surat Jalan: No. Surat Jalan / Nopol / No. Pesanan"]
    J --> K["Sistem memeriksa guard wajib:<br/>1. Status Tagihan == PAID<br/>2. Surat Jalan == ISSUED (belum terpakai)"]
    K --> L{"Syarat Sistem & Fisik Muatan Sesuai?"}
    L -->|"Sesuai"| M["Klik 'Setujui Keluar'<br/>Catat waktu keluar & akun Satpam"]
    M --> N["Status Pesanan: COMPLETED<br/>Status Surat Jalan: USED_AT_GATE_OUT"]
    L -->|"Tidak Sesuai"| O["Klik 'Tahan Keluar'<br/>Wajib isi alasan penahanan"]
    O --> P["Status: GATE_OUT_HELD<br/>Kendaraan dilarang keluar"]
```

#### A. Pemeriksaan Kendaraan Masuk
1. Satpam membuka Dashboard Pos Gerbang yang langsung menampilkan kolom pencarian cepat dan tab **Antrean Masuk**.
2. Ketika kendaraan tiba, Satpam mencari berdasarkan nomor polisi atau nomor pesanan.
3. Satpam membuka kartu pemeriksaan masuk dan memeriksa kecocokan nomor polisi, jenis kendaraan, perusahaan pengirim, serta mencatat nama driver aktual (terutama apabila Client mengosongkan kolom nama driver saat pengajuan).
4. **Jalur Setuju:** Jika cocok, Satpam menekan tombol **Setujui Masuk**. Sistem mencatat timestamp masuk, ID akun Satpam, dan memindahkan pesanan ke antrean Kepala Gudang (`WAITING_WAREHOUSE`).
5. **Jalur Tahan/Tolak:** Jika nomor polisi berbeda atau dokumen pengantar bermasalah, Satpam menekan **Tahan Masuk**, mengisi alasan wajib pada dialog konfirmasi, dan sistem mengubah status ke `GATE_IN_HELD`.

#### B. Pemeriksaan Kendaraan Keluar
1. Ketika kendaraan hendak meninggalkan pabrik, Satpam membuka tab **Menunggu Keluar** dan mencari nomor Surat Jalan atau nomor polisi kendaraan.
2. Sistem menampilkan rincian Surat Jalan beserta indikator status pembayaran (**Lunas**) dan status keabsahan Surat Jalan (**Berlaku / Belum Digunakan**).
3. Apabila tagihan belum `PAID` atau Surat Jalan belum terbit/sudah pernah digunakan, tombol **Setujui Keluar** dikunci oleh server dan layar menampilkan peringatan blokir yang jelas.
4. Satpam mencocokkan fisik kendaraan, nomor polisi, driver, serta jenis dan jumlah drum yang diangkut dengan rincian pada Surat Jalan.
5. **Jalur Setuju:** Jika seluruh pemeriksaan sesuai, Satpam menekan **Setujui Keluar**. Sistem menyimpan waktu keluar, akun Satpam, menandai Surat Jalan sebagai `USED_AT_GATE_OUT`, dan mengubah status pesanan menjadi `COMPLETED`.
6. **Jalur Tahan:** Jika jumlah drum di atas truk tidak sesuai dengan Surat Jalan atau kendaraan berbeda tanpa otorisasi, Satpam menekan **Tahan Keluar**, wajib mengisi alasan, dan status menjadi `GATE_OUT_HELD`.

---

### 2.3 Alur Kepala Gudang

```mermaid
flowchart TD
    A["Pesanan berstatus WAITING_WAREHOUSE<br/>(Kendaraan sudah disetujui masuk oleh Satpam)"] --> B["Kepala Gudang mulai pemeriksaan muatan<br/>(Status: WAREHOUSE_REVIEW)"]
    B --> C["Input Data Pemeriksaan Aktual:<br/>- Jenis muatan aktual<br/>- Kuantitas & satuan aktual<br/>- Kondisi fisik drum & selisih pesanan"]
    C --> D{"Keputusan Pemeriksaan Gudang"}
    
    D -->|"Setujui dan Proses"| E["Status: IN_PROGRESS<br/>Pengerjaan jasa dimulai"]
    D -->|"Minta Klarifikasi"| F["Status: NEEDS_CLARIFICATION<br/>Wajib isi catatan klarifikasi ke Client/Internal"]
    F --> C
    D -->|"Tolak Pekerjaan"| G["Status: CANCELLED / Ditolak<br/>Wajib isi alasan penolakan"]

    E --> H["Perbarui catatan perkembangan pengerjaan"]
    H --> I["Input Hasil Akhir Pekerjaan:<br/>- Jenis layanan aktual<br/>- Kuantitas selesai (layak)<br/>- Kuantitas tidak selesai/tidak layak<br/>- Catatan hasil"]
    I --> J["Konfirmasi Pekerjaan Selesai<br/>(Status: SERVICE_COMPLETED -> WAITING_FINANCE)"]
    J --> K["Menunggu proses tagihan & pelunasan di Keuangan"]
    K --> L["Tagihan terverifikasi PAID<br/>(Status: WAITING_EXIT_DOCUMENT)"]
    L --> M["Kepala Gudang buat & terbitkan Surat Jalan<br/>(Status: WAITING_GATE_OUT)"]
```

1. **Pemeriksaan Muatan Saat Tiba:**
   - Kepala Gudang membuka daftar antrean **Menunggu Pemeriksaan** (hanya pesanan yang kendaraannya telah disetujui masuk oleh Satpam).
   - Kepala Gudang memeriksa muatan drum di lapangan dan mengisi formulir pemeriksaan aktual: jenis muatan aktual, jumlah aktual, satuan, kondisi barang, dan catatan perbedaan terhadap pengajuan awal Client. Data pengajuan awal Client tetap tersimpan utuh sebagai pembanding.
   - Kepala Gudang memilih tindakan: **Setujui dan Proses**, **Minta Klarifikasi**, atau **Tolak**.
2. **Pengerjaan Layanan & Pencatatan Hasil Akhir:**
   - Selama pengerjaan (`IN_PROGRESS`), Kepala Gudang dapat menambahkan catatan progres pengerjaan.
   - Setelah pengerjaan drum selesai, Kepala Gudang mengisi formulir **Hasil Akhir Pekerjaan** yang merinci kuantitas drum yang berhasil diperbarui/diperbaiki, kuantitas drum yang tidak dapat diproses/rusak berat (bila ada), serta catatan teknis yang memengaruhi perhitungan biaya.
   - Setelah dikonfirmasi, pesanan berpindah ke antrean Keuangan (`WAITING_FINANCE`).
3. **Penerbitan Surat Jalan:**
   - Setelah Keuangan memverifikasi pelunasan penuh (`PAID`), pesanan muncul pada tab **Siap Surat Jalan** (`WAITING_EXIT_DOCUMENT`).
   - Kepala Gudang memeriksa kesiapan barang yang akan dimuat ke kendaraan, memeriksa data kendaraan/driver keluar, lalu menerbitkan **Surat Jalan**. Sistem memberikan nomor Surat Jalan unik dan mengirimkannya ke antrean keluar Satpam (`WAITING_GATE_OUT`).

---

### 2.4 Alur Keuangan

```mermaid
flowchart TD
    A["Antrean Hasil Kerja Gudang<br/>(Status: WAITING_FINANCE)"] --> B["Tinjau kuantitas hasil aktual Kepala Gudang<br/>& pilih tarif dari Master Tarif"]
    B --> C["Susun rincian perhitungan biaya, penyesuaian/pajak<br/>Simpan sebagai Draft Invoice (DRAFT)"]
    C --> D["Periksa & klik 'Terbitkan Tagihan'<br/>(Status Pesanan: INVOICE_ISSUED)"]
    D --> E["Invoice PDF tampil di Portal Client"]
    E --> F["Terima bukti bayar dari Client / input pembayaran diterima"]
    F --> G{"Verifikasi Bukti & Nominal"}
    G -->|"Bukti Tidak Valid"| H["Tolak bukti bayar + isi alasan<br/>Tagihan tetap menunggu pembayaran"]
    G -->|"Valid tetapi Belum Penuh"| I["Verifikasi Pembayaran Sebagian<br/>(Status: PAYMENT_PARTIAL)<br/>Otomatis catat Kas Masuk sebesar nominal diterima"]
    I --> F
    G -->|"Valid & Memenuhi Total Tagihan"| J["Verifikasi Lunas (Status: PAID)<br/>Otomatis catat Kas Masuk ke Buku Keuangan"]
    J --> K["Pesanan otomatis masuk ke WAITING_EXIT_DOCUMENT<br/>untuk penerbitan Surat Jalan oleh Kepala Gudang"]
```

1. **Perhitungan Jasa dan Penerbitan Tagihan:**
   - Keuangan membuka antrean **Menunggu Perhitungan Tagihan**.
   - Keuangan melihat rincian kuantitas aktual hasil kerja Kepala Gudang, memilih tarif jasa yang sesuai dari Master Tarif, dan memeriksa subtotal, penyesuaian (bila ada), serta total tagihan.
   - Keuangan dapat menyimpan perhitungan sebagai `DRAFT` terlebih dahulu, kemudian menekan **Terbitkan Tagihan**. Sistem membuat nomor invoice unik, menghasilkan dokumen PDF, mengubah status pesanan menjadi `INVOICE_ISSUED`, dan mengirimkan notifikasi ke Client.
2. **Verifikasi Pembayaran & Buku Kas Masuk:**
   - Ketika Client mengunggah bukti pembayaran atau menyerahkan bukti transfer, Keuangan memeriksa kesesuaian nominal, tanggal, metode, dan referensi mutasi.
   - Jika valid, Keuangan menekan **Verifikasi Pembayaran**. Sistem otomatis mencatat transaksi pemasukan (`IN`) pada tabel `cashbook_entries`.
   - Jika total pembayaran terverifikasi sudah sama dengan total tagihan, status berubah menjadi `PAID` dan sistem membuka akses pembuatan Surat Jalan bagi Kepala Gudang. Jika baru sebagian, status menjadi `PAYMENT_PARTIAL` dan barang belum dapat dibuatkan Surat Jalan.
3. **Pencatatan Biaya Operasional & Buku Kas Keluar:**
   - Keuangan membuka menu **Biaya Operasional** dan menginput pengeluaran perusahaan (kategori seperti bahan baku, perlengkapan, gaji karyawan, utilitas, transportasi; tanggal; uraian; nominal; vendor/penerima; metode bayar; dan unggahan foto/PDF nota).
   - Setiap biaya operasional yang disimpan otomatis tercatat sebagai pengeluaran (`OUT`) pada **Buku Keuangan**.
4. **Revisi dan Pelaporan:**
   - Apabila terdapat kesalahan pada invoice yang sudah diterbitkan atau pencatatan biaya, Keuangan melakukan aksi **Revisi/Koreksi** dengan mengisi alasan wajib. Sistem menyimpan riwayat perubahan tanpa menghapus rekaman lama.
   - Keuangan dapat menyaring Buku Keuangan, daftar tagihan, dan biaya operasional berdasarkan periode/kategori lalu mengekspornya ke PDF atau Excel.

---

### 2.5 Alur Owner

1. Owner masuk ke sistem dan langsung melihat **Dashboard Eksekutif PKDI** yang merangkum kondisi operasional dan keuangan lintas bagian secara *real-time* dari data transaksi nyata.
2. Owner memilih filter periode (hari ini, minggu ini, bulan ini, atau rentang tanggal khusus) untuk meninjau:
   - Distribusi status pesanan (menunggu gerbang, di gudang, menunggu bayar, selesai).
   - Aktivitas kendaraan di gerbang masuk dan keluar.
   - Total tagihan diterbitkan, pembayaran terverifikasi (kas masuk), piutang belum terbayar, biaya operasional per kategori (kas keluar), dan saldo arus kas bersih.
3. Owner dapat melakukan *drill-down* (membuka detail) pada setiap pesanan, invoice, pembayaran, Surat Jalan, maupun biaya operasional, serta membuka tab **Jejak Audit (Audit Log)** untuk memeriksa kronologi tindakan setiap petugas.
4. Owner dapat mengunduh laporan rekapitulasi operasional dan keuangan dalam format PDF atau Excel.
