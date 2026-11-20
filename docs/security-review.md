# Laporan Audit Keamanan (Security Review)
## Sistem Manajemen Operasional, Layanan Drum, dan Keuangan PT Pulo Kencana Drum Industri (PKDI)

**Tanggal Audit:** 17 November 2026
**Fase:** Fase 7 (Security Review & Hardening)

---

## 1. Metode Audit & Urutan Temuan Berdasarkan Severity

Audit dilakukan terhadap seluruh controller, service domain, model Eloquent, migration, middleware, policy, konfigurasi `.env.example`, dan template Blade. Temuan diurutkan berdasarkan tingkat keparahan (*High*, *Medium*, *Low*), diperbaiki langsung apabila berada dalam lingkup teknis aplikasi, serta dicatat apabila memerlukan keputusan kebijakan bisnis PKDI.

### Daftar Temuan yang Diperbaiki pada Fase 7

| ID | Severity | Kategori | Temuan Saat Audit | Perbaikan yang Diterapkan & Verifikasi |
|---|---|---|---|---|
| `SEC-FIX-01` | **HIGH** | Validasi Batas Atas & Overpayment Keuangan | Input nominal pada form pembuatan invoice, revisi invoice, pembayaran, dan biaya operasional memeriksa `min:1` namun belum membatasi batas atas (`max:`) serta belum menolak pembayaran yang melebihi `balance_due` (*overpayment*). Nilai ekstrem (`10^13+`) berisiko memicu overflow `DECIMAL(15,2)`. | Menambahkan aturan `max:999999999999` (dan `max:999999999` pada `unit_price`) di `FinancePreviewController`, `ClientPreviewController`, dan `BillingAndExitWorkflowService`, serta memvalidasi `$amount <= (float) $lockedInvoice->balance_due`. Diverifikasi pada `Phase7EndToEndQaAndSecurityTest`. |
| `SEC-FIX-02` | **MEDIUM** | Resolusi Kode Dokumen Tidak Dikenal (`404` vs Fallback) | Ketika kode pesanan/invoice yang tidak ada di database dikirim ke endpoint detail internal (`/gate/check-in/{code}`, `/warehouse/orders/{code}`, `/finance/invoices/{code}`, `/owner/transactions/{code}`), helper `PkdiDemoData::findOrder()` sebelumnya mengembalikan elemen pertama array demo alih-alih `404 Not Found`. | Menambahkan `PkdiDemoData::findOrderOrNull()` dan memperbarui seluruh controller agar memanggil `abort(404)` apabila kode tidak ditemukan di database maupun daftar demo yang sah. Diverifikasi pada `Phase7EndToEndQaAndSecurityTest`. |
| `SEC-FIX-03` | **LOW** | Atribut Simulasi Tersisa pada Komponen Konfirmasi | Komponen `resources/views/components/ui/confirm-action.blade.php` masih memuat atribut `data-demo-form` dari Fase 3. | Mengganti `data-demo-form` menjadi `data-prevent-double-submit` dan memperbarui teks footer pada `config/pkdi.php`. |

---

## 2. Evaluasi Kontrol Keamanan Aplikasi

### 2.1 Autentikasi & Keamanan Sesi
- **Hashing Kata Sandi:** Seluruh password disimpan menggunakan algoritma Bcrypt/Argon melalui cast `'password' => 'hashed'` dan `Hash::make()`. Tidak ada penyimpanan atau pencetakan password plain text di log maupun response.
- **Proteksi Brute-Force:** Endpoint `POST /login` menerapkan `RateLimiter` maksimal **5 percobaan gagal per menit** berdasarkan kombinasi `email|ip`.
- **Regenerasi Sesi & CSRF:** Login dan registrasi memanggil `$request->session()->regenerate()`, sedangkan logout memanggil `invalidate()` dan `regenerateToken()`. Seluruh form `POST`/`PUT` menggunakan token `@csrf`.
- **Penonaktifan Akun Seketika:** Middleware `EnsureUserIsActive` (`active.user`) memeriksa `user()->is_active` pada setiap request terautentikasi dan langsung menghentikan sesi jika akun dinonaktifkan.
- **Pembatasan Switcher Demo:** Route `/preview/role/{role}` dikunci dengan `abort_unless(app()->environment(['local', 'testing']), 403)` sehingga otomatis tertutup pada environment `production`.

### 2.2 Otorisasi Berlapis (Defense in Depth) & Anti-IDOR
- **Lapis 1 (Server-Side Route Middleware):** Grup route `/client/*`, `/gate/*`, `/warehouse/*`, `/finance/*`, dan `/owner/*` masing-masing dilindungi middleware `['auth', 'active.user', 'role:<nama_role>']`.
- **Lapis 2 (Resource Policies):**
  - `ServiceOrderPolicy`: Memastikan `client` hanya dapat melihat pesanan dengan `client_company_id` miliknya sendiri, serta mengunci aksi gerbang pada `satpam`, aksi gudang pada `kepala_gudang`, dan aksi penagihan pada `keuangan`.
  - `InvoicePolicy`: Memastikan `client` hanya dapat melihat, mengunggah bukti bayar, dan mengunduh PDF untuk invoice milik `client_company_id`-nya sendiri (`status !== DRAFT`), serta memblokir `satpam` dan `kepala_gudang` dari membuka detail nominal invoice.
  - `ClientCompanyPolicy`: Memastikan `client` hanya dapat mengubah profil PT miliknya sendiri.
  - Seluruh policy menetapkan `delete()` = `false` (larangan hapus permanen transaksi).
- **Lapis 3 (State Machine & Database Locking):** Seluruh mutasi status di `OrderWorkflowService` dan `BillingAndExitWorkflowService` berjalan di dalam `DB::transaction()` dengan `lockForUpdate()`, menolak lompatan status (seperti menerbitkan Surat Jalan sebelum `PAID` atau menyetujui kendaraan keluar tanpa Surat Jalan berstatus `ISSUED`).

### 2.3 Pencegahan Mass Assignment & Manipulasi Input
- Pada model `User`, atribut `role` dan `is_active` **dikecualikan dari `$fillable`**. Penetapan role dilakukan secara eksplisit di server (`$user->role = User::ROLE_CLIENT` saat registrasi mandiri).
- Perhitungan subtotal, total tagihan, akumulasi pembayaran terverifikasi (`paid_amount`), dan sisa tagihan (`balance_due`) dihitung ulang di server dari kuantitas, harga satuan, dan tabel `payments` berstatus `VERIFIED`, tidak pernah mempercayai total yang dikirim dari browser.

### 2.4 Keamanan Penyimpanan Berkas & Jejak Audit
- Lampiran pesanan, bukti transfer pembayaran, dan nota biaya operasional disimpan pada disk privat `local` (`storage/app/private/*`) dengan validasi tipe MIME (`mimes:pdf,jpg,jpeg,png`) dan ukuran maksimum `5 MB` (`max:5120`).
- Unduhan PDF Invoice (`GET /invoices/{code}/download-pdf`) dan ekspor laporan (`GET /finance/reports/export`, `GET /owner/reports/export`) melewati pemeriksaan autentikasi dan otorisasi server sebelum dokumen dialirkan.
- Setiap tindakan kritis (`CLIENT_REGISTERED`, `USER_LOGIN`, `ORDER_CREATED`, `GATE_IN_APPROVED`, `GATE_IN_HELD`, `WAREHOUSE_INSPECTION_APPROVED`, `SERVICE_COMPLETED`, `INVOICE_ISSUED`, `INVOICE_REVISED`, `UPLOAD_PAYMENT_PROOF`, `PAYMENT_VERIFIED`, `PAYMENT_REJECTED`, `OPERATIONAL_EXPENSE_RECORDED`, `DELIVERY_ORDER_ISSUED`, `GATE_OUT_APPROVED`, `GATE_OUT_HELD`) dicatat secara *append-only* pada tabel `audit_logs` beserta ID aktor, role, IP address, user agent, nilai sebelum/sesudah, dan alasan.

---

## 3. Risiko & Keputusan Kebijakan yang Memerlukan Konfirmasi PKDI

1. **Verifikasi Akun Perusahaan Client Baru (`VAL-AUTH-03`):** Saat ini akun Client baru langsung berstatus `ACTIVE` setelah registrasi mandiri agar dapat diuji end-to-end. Jika manajemen PKDI menginginkan verifikasi dokumen legalitas PT (NPWP/NIB) oleh Keuangan/Owner sebelum pesanan pertama dapat dibuat, kolom `client_companies.verification_status` (`PENDING_VERIFICATION` -> `ACTIVE`) sudah tersedia di database untuk diaktifkan.
2. **Batas Otorisasi Nominal Revisi Invoice (`VAL-FIN-04`):** Saat ini divisi Keuangan dapat menyimpan revisi invoice disertai alasan wajib yang tercatat di Audit Log Owner. Apabila koreksi di atas nilai tertentu membutuhkan persetujuan eksplisit dari Owner, aturan batas nominal tersebut perlu ditetapkan oleh direksi PKDI.
