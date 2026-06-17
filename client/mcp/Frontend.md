# Frontend Architecture: Roastery & Coffee Shop ERP

Repositori ini menggunakan pendekatan *Feature-Driven* untuk mempermudah navigasi basis kode berskala besar. Konfigurasi utamanya berjalan di atas **Vite** untuk *build* yang cepat dan menggunakan **Chakra UI v3** untuk sistem desain komponennya.

## Struktur Folder Direktori

```text
roastery-erp-frontend/
├── public/                     # Aset statis yang tidak diproses oleh Vite (favicon, dll)
├── src/                        # Source code utama aplikasi React
│   ├── assets/                 # Aset statis yang di-import ke komponen (gambar, logo roastery)
│   ├── components/             # Reusable UI Components (Komponen murni tanpa logika bisnis rumit)
│   │   ├── common/             # Tombol kustom, Input, Modal, Table standar Chakra UI
│   │   └── layouts/            # Kerangka halaman utama (Sidebar, Navbar, Dashboard Layout)
│   ├── features/               # Kumpulan modul bisnis berdasarkan SOP ERP
│   │   ├── o2c/                # Logika & UI spesifik Order to Cash
│   │   │   ├── components/     # Contoh: FormCreateOrder, InvoiceTable
│   │   │   ├── hooks/          # Contoh: useFulfillment, useAcceptPayment
│   │   │   └── utils/          # Helper spesifik O2C
│   │   ├── p2p/                # Logika & UI spesifik Procure to Pay
│   │   │   ├── components/     # Contoh: PurchaseOrderGrid, ReceiveItemModal
│   │   │   └── hooks/          # Contoh: useThreeWayMatchValidation
│   │   └── inventory/          # Logika & UI spesifik Item Management
│   │       ├── components/     # Contoh: StockAdjustmentForm, TransferLocationGrid
│   │       └── hooks/          # Contoh: useYieldLossCalculator
│   ├── hooks/                  # Custom hooks global (bisa diakses lintas fitur)
│   │   └── useAuth.ts          # Hook untuk manajemen sesi login & validasi role
│   ├── pages/                  # Halaman level teratas yang dihubungkan dengan Router
│   │   ├── admin/              # Dashboard Superuser (Owner)
│   │   ├── sales/              # Halaman khusus role Sales (Fokus pada SO)
│   │   ├── inventory/          # Halaman khusus role Inventory (Fokus pada PO & Stok)
│   │   ├── monetary/           # Halaman khusus role Monetary (Fokus pada Invoice & Bills)
│   │   └── auth/               # Halaman Login
│   ├── routes/                 # Konfigurasi navigasi & proteksi akses halaman
│   │   └── ProtectedRoute.tsx  # Komponen pembungkus untuk membatasi akses berdasarkan Role
│   ├── services/               # Lapisan komunikasi API dengan Backend (Express.js)
│   │   ├── apiClient.ts        # Konfigurasi dasar Axios/Fetch (Base URL & Interceptors)
│   │   ├── o2cApi.ts           # Definisi endpoint untuk O2C
│   │   └── p2pApi.ts           # Definisi endpoint untuk P2P
│   ├── store/                  # Global State Management (sangat disarankan menggunakan Zustand)
│   │   └── authStore.ts        # Menyimpan data user yang sedang login dan tokennya
│   ├── theme/                  # Konfigurasi tema global Chakra UI v3
│   │   └── index.ts            # Kustomisasi palet warna (misal: warna brand roastery), tipografi
│   ├── types/                  # Antarmuka (Interfaces) TypeScript
│   │   └── api.d.ts            # Tipe data kembalian (DTO) dari backend agar tersinkronisasi
│   ├── utils/                  # Fungsi pembantu global
│   │   ├── currency.ts         # Formatter Rupiah (IDR) untuk Nominal / Invoice
│   │   └── date.ts             # Formatter tanggal untuk tenggat waktu (due date) tagihan
│   ├── App.tsx                 # Root component: Membungkus Router & ChakraProvider
│   └── main.tsx                # Entry point aplikasi (menghubungkan React ke DOM HTML)
├── .env                        # Variabel environment (misal: VITE_API_BASE_URL)
├── .eslintrc.cjs               # Aturan linter untuk menjaga konsistensi gaya kode
├── index.html                  # Template HTML utama yang di-serve ke browser
├── package.json                # Daftar dependency & scripts aplikasi
├── tsconfig.json               # Konfigurasi ketat TypeScript untuk frontend
└── vite.config.ts              # Konfigurasi bundler Vite