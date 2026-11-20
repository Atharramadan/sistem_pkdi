# CHANGELOG

Seluruh perubahan penting revisi **Sistem Manajemen Operasional dan Keuangan PT Pulo Kencana Drum Industri (PKDI)** menuju **PRD PKDI v2.0** dan **PRD PKDI v2.1 (Revisi Alur Pengangkutan Limbah & Pemisahan Tracker)** dicatat pada dokumen ini secara kronologis.

## [2.1.0] - 2026-11-19

### Penyesuaian PRD PKDI v2.1 & Pemisahan Tracker Operasional
- **Perubahan Layanan Utama (`Pengangkutan Limbah` & `Rekondisi Drum`):**
  - Menyesuaikan layanan kedua dari `Penjualan Drum` menjadi **`Pengangkutan Limbah` (`WASTE_TRANSPORT`)** berdampingan dengan **`Rekondisi Drum` (`RECONDITIONING`)**, sambil mempertahankan alias kompatibilitas `ServiceOrder::TYPE_DRUM_SALE = 'WASTE_TRANSPORT'` agar query dan filter lama maupun baru berjalan tanpa putus.
  - Menambahkan migrasi non-destruktif `2026_11_19_000002_add_prd_v2_1_waste_transport_columns_to_service_orders.php` untuk kolom koordinasi pengangkutan internal (`transporter_name`, `driver_phone`, `scheduled_pickup_date`, `in_transit_at`, `transport_notes`) serta konversi non-destruktif baris transaksi historis ke `WASTE_TRANSPORT`.
- **Tahap Koordinasi Pengangkutan & Perjalanan Internal PKDI (`TRANSPORT_SCHEDULED` -> `IN_TRANSIT`):**
  - Menambahkan metode `OrderWorkflowService::scheduleOrUpdateTransport()` dan route `POST /warehouse/orders/{code}/transport` (`warehouse.orders.transport.store`) sehingga Kepala Gudang dapat mencatat jadwal penjemputan, nama transporter/armada, jenis kendaraan, nomor polisi, nama sopir, kontak sopir, dan catatan perjalanan tanpa mengembalikan peran Satpam.
- **Pemisahan Tracker Alur Proses Rekondisi Drum vs Pengangkutan Limbah (`OrderPresenter.php`, `order-stepper.blade.php`, `warehouse/orders/show.blade.php`):**
  - **Alur Rekondisi Drum (`RECONDITIONING`):**
    - **Tracker Client (`Tracker Rekondisi Drum (5 Tahap)`):** `1. Diajukan & Ditinjau` -> `2. Disetujui & Dalam Perjalanan` -> `3. Diterima di PKDI` -> `4. Dalam Penanganan Rekondisi` -> `5. Rekondisi Selesai & Siap Serah`.
    - **Tracker Operasional Internal Gudang (`Alur Operasional Gudang: Rekondisi Drum (6 Tahap)`):** `1. Tinjau Permintaan` -> `2. Jadwal & Perjalanan` -> `3. Penerimaan Fisik Drum` -> `4. Penyortiran Drum` -> `5. Proses Rekondisi` -> `6. Hasil Akhir & Penyerahan`.
  - **Alur Pengangkutan Limbah (`WASTE_TRANSPORT`):**
    - **Tracker Client (`Tracker Pengangkutan Limbah (4 Tahap)`):** `1. Diajukan & Ditinjau` -> `2. Disetujui & Dalam Perjalanan` -> `3. Diterima di PKDI` -> `4. Layanan Selesai`.
    - **Tracker Operasional Internal Gudang (`Alur Operasional Gudang: Pengangkutan Limbah (4 Tahap)`):** `1. Tinjau Permintaan` -> `2. Jadwal & Perjalanan` -> `3. Penerimaan Fisik Limbah` -> `4. Konfirmasi Hasil & Siap Tagih` (melewati tahap penyortiran dan rekondisi drum sehingga setelah limbah diterima fisik di PKDI, Kepala Gudang langsung mengonfirmasi hasil penerimaan untuk masuk antrean penagihan Keuangan).

## [2.0.0] - 2026-11-19

### Phase 0: Audit Proyek
- Melakukan audit penuh terhadap struktur direktori, migrasi database, 17 model Eloquent, 10 controller, 3 service, 5 policy, route aktif, dan 33 file yang sebelumnya memiliki ketergantungan terhadap role Satpam / Pos Gerbang.
- Mendokumentasikan hasil temuan, klasifikasi fitur, serta 7 poin `PERLU_VALIDASI_PKDI` beserta baseline alur operasional sementara pada `PROJECT_AUDIT.md`.

### Phase 1: Perbaikan Struktur Role, Route, dan Database (Non-Destruktif)
- **Skema Database Non-Destruktif (`2026_11_19_000001_upgrade_pkdi_schema_for_prd_v2_warehouse_and_billing.php`):**
  - Menambahkan tabel baru: `drum_products`, `reconditioning_services`, `warehouse_receipts`, `warehouse_sortings`, dan `billable_transactions`.
  - Memperluas kolom pada `client_companies` (`nib_number`, `nib_file_path`, `documents_status`), `service_orders` (`transaction_type`, `drum_product_id`, `reconditioning_service_id`, `po_number`, `po_attachment_path`, `needed_date`, `specification_notes`, `billing_status`, `delivery_method`, `delivered_quantity`, `delivered_at`, `delivery_notes`), `warehouse_inspections`, `service_processes`, `service_results`, dan `invoice_items`.
  - Mengubah kolom kendaraan lama (`vehicle_type`, `license_plate`) serta field teks bebas lama menjadi nullable; menetapkan default `gate_status = NOT_REQUIRED`. Tabel historis `vehicle_gate_logs` dan `delivery_orders` tetap dipertahankan tanpa penghapusan data lama.
- **Master Data (`MasterDataSeeder.php` & `DatabaseSeeder.php`):**
  - Menyediakan dropdown master `drum_products`: `Drum besi`, `Poligen 150 liter`, `Poligen 120 liter`, dan `Jerigen`.
  - Menyediakan dropdown tunggal `reconditioning_services`: `Laundry`, `Laundry + Cat`, dan `Laundry + Cat + Sablon`.
- **Role, Route & Keamanan:**
  - Menetapkan 4 role aktif (`client`, `kepala_gudang`, `keuangan`, `owner`) dan mengeluarkan `satpam` dari alur aktif, menu navigasi, `ServiceOrderPolicy`, `DeliveryOrderPolicy`, `SetupInternalAccountsCommand`, dan login (`EnsureUserIsActive` + `AuthenticatedSessionController`) tanpa menghapus data akun lama.
  - Menghapus seluruh route `/gate/*` dari `routes/web.php` dan menambahkan endpoint terproteksi Policy `GET /orders/{code}/attachments/{type}`.
- **Portal Client & Aset Visual:**
  - Memperbarui form pemesanan Client (`client/orders/create.blade.php`) dengan pemilih kartu `Jasa Rekondisi Drum` vs `Penjualan Drum`, dropdown master produk, dropdown paket rekondisi, upload PO/lampiran, dan tanpa mewajibkan plat nomor.
  - Memperbarui profil perusahaan Client (`client/company.blade.php`) dengan NPWP, NIB, unggah berkas NIB, dan indikator kelengkapan dokumen.
  - Memperbarui `DESIGN.md` dan menambahkan dukungan `prefers-reduced-motion: reduce` pada `resources/css/app.css`.

### Phase 2: Implementasi Penuh Modul Gudang (PRD 2.0)
- **Service Operasional (`OrderWorkflowService.php`):**
  - Menambahkan `reviewOrder()` (Terima -> `WAITING_RECEIPT` untuk Rekondisi / `PREPARING` untuk Penjualan Drum; Minta Klarifikasi dengan alasan wajib; Tolak dengan alasan wajib).
  - Menambahkan `recordReceipt()` untuk pencatatan penerimaan fisik drum (`RCV-YYYYMMDD-NNN`, foto opsional, selisih tanpa menimpa data awal Client).
  - Menambahkan `recordSortingAndInspection()` dengan validasi keseimbangan matematis (`qty_passed + qty_special_repair + qty_rejected == jumlah fisik diterima`) dan alasan wajib bila `qty_rejected > 0`.
  - Memperluas `recordProcess()` dengan unggah foto opsional dan kontrol `is_visible_to_client` agar catatan internal Gudang tidak tampil pada akun Client.
  - Memperluas `completeService()` agar atomik dalam `DB::transaction` membentuk satu `BillableTransaction` unik (`BTX-YYYYMMDD-NNN`, `billing_status = READY_TO_BILL`) saat hasil akhir dikonfirmasi.
  - Menambahkan `recordDelivery()` untuk penyerahan/pengiriman (`DIANTAR_PKDI` / `DIAMBIL_CLIENT`, status `IN_DELIVERY` -> `COMPLETED`, info armada internal opsional).
- **Controller, UI & Laporan Gudang:**
  - Merancang ulang `warehouse/dashboard.blade.php`, `warehouse/orders/show.blade.php` (tata letak 2 kolom asimetris referensi `JWB-Dashboard-V2`, form kontekstual sesuai tahap aktif, dan sidebar 3 jalur status terpisah), `warehouse/inspections/index.blade.php`, dan `warehouse/processes/index.blade.php`.
  - Menambahkan halaman `warehouse/reports/index.blade.php` serta ekspor PDF (`%PDF-1.4`) dan Excel OpenXML (`.xlsx`) Laporan Operasional Gudang pada `ReportExportService.php`.
  - Menambahkan kalkulator keseimbangan sortir real-time (`initWarehouseSortingBalance`) pada `resources/js/admin/interactions.js`.

### Phase 3: Modul Keuangan PRD 2.0, Portal Client & Monitoring Owner
- **Invoice Gabungan Multi-Transaksi & Siklus Penagihan (`BillingAndExitWorkflowService.php`):**
  - Menambahkan `createCombinedInvoice()` untuk menggabungkan beberapa `BillableTransaction` milik Client yang sama ke satu invoice (`DRAFT` atau `ISSUED`), menolak lintas Client, dan mencegah penagihan ganda.
  - Menambahkan `publishDraftInvoice()` dan `voidInvoice()` (pembatalan resmi dengan alasan wajib yang melepaskan kembali transaksi ke antrean `READY_TO_BILL` selama belum ada pembayaran terverifikasi).
  - Memperbarui `submitClientPaymentProof()`, `verifyOrRecordPayment()`, dan `reviseInvoice()` agar menyinkronkan `payment_status` ke seluruh pesanan dalam invoice gabungan tanpa mengubah status operasional `order_status`, mengirim notifikasi alasan penolakan bukti bayar ke Client, dan menjaga pencatatan satu kali pada `cashbook_entries`.
  - Menambahkan endpoint terproteksi Policy `GET /payments/{payment}/proof` dan memperbarui PDF faktur pada `ReportExportService::exportInvoicePdf()` untuk mendukung invoice gabungan dan instruksi transfer bank manual PKDI.
- **Antarmuka Keuangan, Client & Owner:**
  - Merombak `finance/billable/index.blade.php` dengan filter Client, jenis transaksi, status, dan periode serta kalkulator invoice gabungan real-time (`initCombinedBillingForm`).
  - Memperbarui `finance/dashboard.blade.php`, `finance/invoices/index.blade.php`, `finance/invoices/show.blade.php`, `client/invoices/index.blade.php`, dan `client/invoices/show.blade.php` agar mendukung invoice satuan maupun gabungan, alasan penolakan bukti transfer, dan unduhan bukti aman.
  - Memperbarui `owner/dashboard.blade.php`, `owner/transactions/index.blade.php`, dan `owner/transactions/show.blade.php` dengan ringkasan Penjualan Drum vs Jasa Rekondisi, pekerjaan Gudang aktif/selesai, antrean siap ditagihkan, filter Client & jenis transaksi, dan 3 jalur status terpisah.

### Phase 4: Pengujian Akhir, Audit Anti-Slop & Dokumentasi
- Menambahkan suite pengujian akhir end-to-end dan keamanan lintas 4 role (`Phase4FinalPrdV2AcceptanceTest.php`).
- Membersihkan sisa teks gerbang/Satpam pada pratinjau Surat Jalan (`warehouse/delivery-orders/show.blade.php`).
- Melakukan audit kontras WCAG 2.1 (23 pasangan warna light & dark), audit integritas route aktif (66 route, 0 broken route), audit kode Laravel Pint, dan menyusun `CHANGELOG.md` serta `IMPLEMENTATION_REPORT.md`.
