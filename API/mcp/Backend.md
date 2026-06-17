# Backend Architecture: Roastery & Coffee Shop ERP

Repositori ini menggunakan arsitektur *layered* untuk memisahkan *routing*, *HTTP transport* (request/response), dan *business logic*. 

## Struktur Folder Direktori

```text
roastery-erp-backend/
├── prisma/                     # Konfigurasi Database & ORM
│   ├── schema.prisma           # Definisi tabel (PO, Bills, Items, dll) dan relasinya
│   ├── seed.ts                 # Skrip untuk mengisi data awal (misal: default roles & admin)
│   └── migrations/             # Folder otomatis buatan Prisma untuk tracking perubahan skema
├── src/                        # Source code utama aplikasi
│   ├── config/                 # Konfigurasi environment & koneksi eksternal
│   │   └── env.ts              # Validasi dan export variabel dari .env
│   ├── controllers/            # Layer presentasi: Menerima request & mengembalikan response
│   │   ├── o2c.controller.ts   # Menangani endpoint Order to Cash (Sales Order, dsb)
│   │   ├── p2p.controller.ts   # Menangani endpoint Procure to Pay (PO, Receive, Bills)
│   │   └── item.controller.ts  # Menangani endpoint Item Management & Adjustments
│   ├── middlewares/            # Fungsi penengah sebelum request mencapai controller
│   │   ├── auth.middleware.ts  # Verifikasi JWT dan autentikasi user
│   │   ├── role.middleware.ts  # Otorisasi role (Admin, Sales, Inventory, Monetary)
│   │   └── error.middleware.ts # Global error handler untuk menangkap exception
│   ├── routes/                 # Layer routing: Mendefinisikan URL endpoint aplikasi
│   │   ├── index.ts            # Main router yang menggabungkan semua sub-router
│   │   ├── o2c.routes.ts       # Endpoint spesifik O2C
│   │   ├── p2p.routes.ts       # Endpoint spesifik P2P
│   │   └── item.routes.ts      # Endpoint spesifik Inventory
│   ├── services/               # Layer bisnis: Tempat logika utama aplikasi berada
│   │   ├── o2c.service.ts      # Logika pembuatan invoice, validasi fulfillment
│   │   ├── p2p.service.ts      # Logika krusial Three-Way Matching & unlocking Bills
│   │   └── item.service.ts     # Logika kalkulasi yield loss & pergerakan stok
│   ├── types/                  # Definisi antarmuka (interface) TypeScript kustom
│   │   └── express.d.ts        # Modifikasi global type Express (misal: inject req.user)
│   ├── utils/                  # Fungsi bantuan (helper) yang bisa dipakai berulang
│   │   ├── logger.ts           # Konfigurasi logging (opsional)
│   │   └── response.ts         # Format standardisasi response API (JSON formatter)
│   ├── app.ts                  # Inisialisasi Express, pemasangan middleware & routes global
│   └── server.ts               # Entry point aplikasi: Menjalankan server pada port tertentu
├── .env                        # Variabel rahasia (DATABASE_URL, JWT_SECRET, PORT)
├── .eslintrc.js                # Konfigurasi linter untuk standardisasi kode
├── .prettierrc                 # Konfigurasi formating kode
├── package.json                # Daftar dependency & scripts (dijalankan via Bun)
└── tsconfig.json               # Konfigurasi kompilasi TypeScript