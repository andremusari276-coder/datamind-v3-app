# DataMind - Intelligent Auto EDA Platform

**DataMind** adalah platform analisis data eksploratori (Exploratory Data Analysis) berbasis web yang ditenagai oleh AI. Pengguna cukup mengunggah dataset dan DataMind secara otomatis akan memproses, memvisualisasikan, dan menghasilkan insight statistik yang mendalam, tanpa perlu menulis satu baris kode pun.

---

## Daftar Isi

- [Tentang Proyek](#tentang-proyek)
- [Fitur Utama](#fitur-utama)
- [Teknologi yang Digunakan](#teknologi-yang-digunakan)
- [Struktur Proyek](#struktur-proyek)
- [Instalasi dan Menjalankan](#instalasi-dan-menjalankan)
- [Konfigurasi Lingkungan](#konfigurasi-lingkungan)
- [Panduan Penggunaan](#panduan-penggunaan)
- [Titik Akhir API](#titik-akhir-api)
- [Laporan Ekspor](#laporan-ekspor)
- [Sistem Autentikasi](#sistem-autentikasi)
- [Kontributor](#kontributor)

---

## Tentang Proyek

DataMind V2 dirancang sebagai solusi EDA (Exploratory Data Analysis) otomatis yang memudahkan siapa saja, mulai dari pelajar, analis data, hingga peneliti, untuk memahami dataset mereka dengan cepat dan mendalam. Proses yang biasanya membutuhkan waktu berjam-jam direduksi menjadi hanya beberapa menit.

DataMind dikembangkan sebagai proyek akhir dengan pengawasan akademis dari:

**Dosen Pembimbing:** Bakti Siregar, M.Sc., CDS

---

## Fitur Utama

### Upload dan Deteksi Data Otomatis
- Mendukung format file CSV, XLSX, XLS, dan TXT dengan ukuran maksimal 50 MB
- Deteksi encoding karakter secara otomatis menggunakan library `chardet` (UTF-8, Latin-1, dan lainnya)
- Deteksi separator CSV secara otomatis (koma, titik koma, tab, pipe)
- Validasi kolom duplikat dan konsistensi data sebelum diproses
- Data disimpan sepenuhnya di memori (in-memory) untuk menjaga tipe data tetap akurat

### Deteksi Skala Pengukuran Otomatis
Setiap kolom dianalisis dan diklasifikasikan ke dalam salah satu skala berikut:
- **Nominal** - data kategorikal tanpa urutan
- **Ordinal** - data kategorikal dengan urutan (level, rating, rank, grade, dll.)
- **Interval** - data numerik tanpa titik nol sejati (tahun, suhu Celsius)
- **Ratio** - data numerik dengan titik nol sejati
- **Datetime** - kolom tanggal dan waktu
- **Boolean** - kolom ya/tidak, true/false, 1/0

### Statistik Deskriptif Lengkap
- Mean, median, standar deviasi, varians, min, maks
- Kuartil Q1 dan Q3, Interquartile Range (IQR)
- Koefisien variasi (CV), skewness, dan kurtosis
- Uji normalitas (distribusi normal atau tidak normal)
- Deteksi jumlah dan persentase missing values per kolom
- Statistik kategorikal: frekuensi, persentase, nilai paling sering muncul (modus)

### Visualisasi Interaktif Otomatis
Semua chart dirender menggunakan Plotly dengan standar visualisasi internasional:
- **Univariat Numerik:** Histogram, Boxplot, Density Plot, QQ-Plot, Violin Plot
- **Univariat Kategorikal:** Bar Chart, Pie Chart (maks 5 kategori + "Others"), Count Plot, Pareto Chart
- **Bivariat:** Scatter Plot, Regression Plot, Bubble Chart, Boxplot by Category, Violin by Category
- **Multivariat:** Correlation Heatmap, Pair Plot Matrix, Grouped Bar Chart
- **Time Series:** Deteksi otomatis kolom datetime dan ringkasan tren waktu

### Data Quality Score
- Skor kesehatan data (Data Quality Health Score) dalam persentase
- Klasifikasi kondisi data: Clean (< 5%), Warning (5-20%), Critical (> 20%)
- Analisis pola missing values dengan heatmap
- Deteksi dan penanganan baris duplikat
- Deteksi outlier menggunakan metode IQR
- Tinjauan per baris dengan missing values dan outlier

### Data Preprocessing Interaktif
- Penanganan missing values: hapus baris, isi dengan mean/median/modus, isi nilai konstan
- Penghapusan baris duplikat
- Penanganan outlier: hapus, ganti dengan batas IQR, atau isi dengan median
- Fitur undo untuk membatalkan operasi preprocessing terakhir

### AI Chat Assistant
- Chatbot berbasis Gemini 2.5 Flash (Google AI) yang memahami konteks dataset yang sedang dianalisis
- Mampu menjawab pertanyaan tentang statistik, distribusi, missing values, dan outlier secara spesifik
- Fallback otomatis ke analisis berbasis aturan jika API Gemini tidak tersedia
- Dapat menjawab pertanyaan umum di luar konteks dataset

### Insight Generator
- Narasi insight otomatis dalam Bahasa Indonesia menggunakan Gemini API
- Deskripsi kualitatif tentang distribusi, korelasi, anomali, dan rekomendasi
- Fallback ke insight berbasis rules jika API tidak tersedia

### Sistem Riwayat Dataset
- Setiap dataset yang diunggah oleh pengguna yang login dicatat secara otomatis
- Pengguna dapat memuat ulang dataset sebelumnya dari halaman profil
- Riwayat menyimpan nama file, jumlah baris, jumlah kolom, dan tanggal upload

---

## Teknologi yang Digunakan

| Kategori | Teknologi |
|---|---|
| Backend Framework | Flask >= 2.3.0 |
| Data Processing | Pandas >= 2.0.0, NumPy >= 1.24.0 |
| Statistik | SciPy >= 1.10.0 |
| Visualisasi | Plotly >= 5.18.0 |
| Export Excel | OpenPyXL >= 3.1.0 |
| Export PDF | ReportLab >= 4.0.0 |
| AI / LLM | Google Generative AI (Gemini 2.5 Flash) |
| Deteksi Encoding | Chardet >= 5.0.0 |
| Manajemen Env | Python-Dotenv >= 1.0.0 |
| Frontend | HTML5, CSS3, JavaScript (Vanilla) |

---

## Struktur Proyek

```
datamind_fixed/
│
├── app.py                          # Entry point utama Flask, semua route API
│
├── backend/
│   ├── __init__.py
│   ├── data_loader.py              # Load file CSV/XLSX/TXT, deteksi encoding & separator
│   ├── preprocessing.py            # Deteksi skala pengukuran, konversi tipe data
│   ├── descriptive_stats.py        # Hitung statistik numerik lengkap
│   ├── categorical_analysis.py     # Analisis statistik kategorikal
│   ├── visualization.py            # Generate semua chart Plotly (JSON)
│   ├── insight_generator.py        # Kirim ringkasan ke Gemini API, terima narasi insight
│   ├── export_report.py            # Export PDF (ReportLab), Excel (OpenPyXL), HTML
│   ├── export_report_final.py      # Versi export laporan (backup/alternatif)
│   └── time_series.py              # Deteksi kolom datetime, ringkasan time series
│
├── frontend/
│   ├── templates/
│   │   └── dashboard.html          # Template utama antarmuka pengguna
│   └── static/
│       ├── css/
│       │   └── style.css           # Stylesheet utama dashboard
│       ├── js/
│       │   └── script.js           # Logic frontend, fetch API, render chart
│       └── avatars/                # Foto profil pengguna (hasil upload)
│
├── data/
│   ├── users.json                  # Database pengguna (email, nama, password hash)
│   ├── raw/                        # Temporary storage file upload (dihapus setelah diproses)
│   ├── raw_user/                   # Dataset permanen per pengguna untuk reload riwayat
│   └── history/                    # Riwayat upload dataset per pengguna (JSON)
│
├── outputs/
│   ├── reports/                    # File PDF dan HTML hasil ekspor laporan
│   └── exported_files/             # File Excel dan HTML hasil ekspor interaktif
│
├── requirements.txt                # Daftar dependensi Python
├── .env                            # Variabel lingkungan (API keys) - JANGAN di-commit
└── .gitignore                      # File dan folder yang dikecualikan dari Git
```

---

## Instalasi dan Menjalankan

### Prasyarat
- Python 3.9 atau lebih baru
- pip (Python package manager)
- Git

### Langkah Instalasi

**1. Clone repositori**

```bash
git clone https://github.com/username/datamind.git
cd datamind
```

**2. Buat virtual environment (disarankan)**

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# macOS / Linux
source venv/bin/activate
```

**3. Install semua dependensi**

```bash
pip install -r requirements.txt
```

**4. Konfigurasi variabel lingkungan**

Salin file `.env.example` menjadi `.env` atau buat file `.env` baru di root proyek:

```bash
cp .env.example .env
```

Kemudian isi nilai yang diperlukan (lihat bagian [Konfigurasi Lingkungan](#konfigurasi-lingkungan)).

**5. Jalankan aplikasi**

```bash
python app.py
```

Aplikasi akan berjalan di `http://localhost:5000` secara default.

---

## Konfigurasi Lingkungan

Buat file `.env` di direktori root proyek dengan isi sebagai berikut:

```env
GEMINI_API_KEY=API_KEY_GEMINI_KAMU_DISINI
ANTHROPIC_API_KEY=API_KEY_ANTHROPIC_KAMU_DISINI
```

| Variabel | Keterangan | Wajib |
|---|---|---|
| `GEMINI_API_KEY` | API key dari Google AI Studio untuk fitur AI Chat dan Insight Generator | Sangat disarankan |
| `ANTHROPIC_API_KEY` | API key Anthropic (opsional, untuk integrasi Claude di masa depan) | Tidak wajib |

**Cara mendapatkan Gemini API Key:**
1. Kunjungi [https://aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey)
2. Login dengan akun Google
3. Klik "Create API Key"
4. Salin key dan tempelkan ke file `.env`

> **Catatan:** Jika `GEMINI_API_KEY` tidak diisi, fitur AI Chat dan Insight Generator akan menggunakan fallback berbasis aturan Python (tidak memerlukan koneksi internet, namun narasi lebih terbatas).

> **Penting:** Jangan pernah meng-commit file `.env` ke repositori publik. File ini sudah terdaftar di `.gitignore`.

---

## Panduan Penggunaan

### 1. Daftar dan Login
Buka aplikasi di browser, lalu buat akun baru atau login dengan akun yang sudah ada. Pengguna tamu (tanpa login) masih bisa menggunakan fitur analisis, namun fitur unduh laporan terkunci dan riwayat dataset tidak tersimpan.

### 2. Upload Dataset atau Gunakan Sample Data
- Klik tombol **Upload File** dan pilih file CSV, XLSX, XLS, atau TXT (maks. 50 MB)
- Atau klik **Load Sample Data** untuk mencoba dengan dataset penjualan bawaan

### 3. Jelajahi Dashboard
Setelah data berhasil dimuat, dashboard otomatis menampilkan:
- Ringkasan dataset (jumlah baris, kolom, missing values, duplikat)
- Data Quality Health Score
- Preview data (100 baris pertama)
- Informasi setiap kolom beserta skala pengukuran yang terdeteksi

### 4. Analisis Statistik
Navigasi ke tab **Statistik** untuk melihat:
- Statistik deskriptif lengkap kolom numerik
- Analisis frekuensi dan distribusi kolom kategorikal

### 5. Visualisasi
Navigasi ke tab **Visualisasi** untuk:
- Melihat semua chart yang di-generate secara otomatis
- Memilih kolom tertentu untuk chart custom
- Chart dapat diekspor langsung sebagai gambar dari antarmuka Plotly

### 6. Data Cleaning
Navigasi ke tab **Preprocessing** untuk:
- Melihat detail missing values, duplikat, dan outlier
- Melakukan penanganan data: isi missing values, hapus duplikat, tangani outlier
- Gunakan tombol **Undo** untuk membatalkan operasi terakhir

### 7. AI Chat Assistant
Klik ikon chat untuk membuka AI Chat Assistant. Contoh pertanyaan yang bisa diajukan:
- "Berapa jumlah baris dataset ini?"
- "Kolom mana yang punya missing values terbanyak?"
- "Apakah distribusi kolom Harga normal?"
- "Berikan ringkasan insight dari data ini"

### 8. Unduh Laporan (Perlu Login)
Navigasi ke tab **Ekspor** dan pilih format:
- **PDF** - Laporan profesional 4-5 halaman dengan statistik lengkap
- **Excel** - Multi-sheet dengan data bersih, statistik numerik, dan kategorik
- **HTML** - Dashboard interaktif mandiri yang bisa dibuka tanpa internet
- **CSV** - Data bersih hasil preprocessing

---

## Titik Akhir API

### Autentikasi

| Method | Endpoint | Deskripsi |
|---|---|---|
| POST | `/api/register` | Daftar akun baru dengan nama, email, dan password |
| POST | `/api/login` | Login dan mulai sesi pengguna |
| POST | `/api/logout` | Logout dan hapus sesi |

### Data

| Method | Endpoint | Deskripsi |
|---|---|---|
| POST | `/upload` | Upload file dataset (form-data, field: `file`) |
| POST | `/load_sample` | Muat dataset sampel bawaan |
| GET | `/api/overview` | Ringkasan dataset: baris, kolom, missing, duplikat |
| GET | `/api/preview` | 100 baris pertama dataset dalam format JSON |
| GET | `/api/column_info` | Informasi setiap kolom beserta skala pengukuran |

### Statistik dan Analisis

| Method | Endpoint | Deskripsi |
|---|---|---|
| GET | `/api/stats/numerical` | Statistik deskriptif lengkap kolom numerik |
| GET | `/api/stats/categorical` | Statistik frekuensi kolom kategorikal |
| GET | `/api/charts` | Semua chart otomatis dalam format JSON Plotly |
| POST | `/api/chart_col` | Chart untuk satu kolom tertentu (body: `{"col": "nama_kolom"}`) |
| GET | `/api/charts/all` | Seluruh chart dalam satu respons |
| GET | `/api/timeseries` | Ringkasan analisis time series jika ada kolom datetime |
| POST | `/api/insight` | Generate narasi insight menggunakan Gemini AI |

### Data Quality dan Preprocessing

| Method | Endpoint | Deskripsi |
|---|---|---|
| GET | `/api/dq_health_score` | Skor kesehatan data keseluruhan |
| GET | `/api/missing_overview` | Ringkasan missing values per kolom |
| GET | `/api/missing_heatmap` | Data heatmap pola missing values (Plotly JSON) |
| GET | `/api/missing_pattern_rows` | Baris dengan pola missing values tertentu |
| GET | `/api/missing_info` | Detail lengkap missing values |
| GET | `/api/missing_rows` | Daftar baris yang mengandung missing values |
| POST | `/api/handle_missing` | Tangani missing values (body: `{"strategy": "drop"/"mean"/"median"/"mode"/"constant", "columns": [...], "value": ...}`) |
| GET | `/api/duplicate_info` | Jumlah dan detail baris duplikat |
| POST | `/api/handle_duplicate` | Hapus baris duplikat |
| GET | `/api/outlier_info` | Ringkasan outlier per kolom numerik |
| GET | `/api/outlier_rows` | Daftar baris yang mengandung outlier |
| POST | `/api/handle_outlier` | Tangani outlier (body: `{"strategy": "drop"/"clip"/"median", "columns": [...]}`) |
| POST | `/api/undo` | Batalkan operasi preprocessing terakhir |

### AI Chat

| Method | Endpoint | Deskripsi |
|---|---|---|
| POST | `/api/chat` | Kirim pesan ke AI Chat Assistant (body: `{"message": "..."}`) |

### Ekspor (Perlu Login)

| Method | Endpoint | Deskripsi |
|---|---|---|
| GET | `/export/cleaned_csv` | Unduh dataset bersih dalam format CSV |
| GET | `/export/pdf` | Unduh laporan PDF standar |
| POST | `/export/pdf_custom` | Unduh laporan PDF dengan pilihan halaman dan chart tertentu |
| GET | `/export/excel` | Unduh laporan Excel multi-sheet |
| GET | `/export/html` | Unduh dashboard HTML interaktif |
| POST | `/export/html_custom` | Unduh HTML dashboard dengan pilihan konten kustom |

### Profil Pengguna

| Method | Endpoint | Deskripsi |
|---|---|---|
| GET | `/api/user/profile` | Ambil data profil pengguna yang sedang login |
| GET | `/api/user/history` | Riwayat dataset yang pernah diunggah |
| POST | `/api/user/history/<entry_id>/reload` | Muat ulang dataset dari riwayat |
| POST | `/api/user/avatar` | Upload foto profil pengguna |

---

## Laporan Ekspor

DataMind menyediakan tiga format ekspor laporan yang komprehensif, semuanya hanya tersedia untuk pengguna yang sudah login.

### PDF
Laporan PDF dihasilkan menggunakan ReportLab dan terdiri dari beberapa bagian:
- Halaman sampul dengan nama dataset dan tanggal analisis
- Daftar isi
- Bab 1: Ikhtisar Dataset (baris, kolom, tipe data, skor kualitas data)
- Bab 2: Statistik Deskriptif Numerik (tabel lengkap per kolom)
- Bab 3: Statistik Deskriptif Kategorikal (frekuensi dan distribusi)
- Bab 4: Visualisasi (chart yang dipilih dalam format gambar)

Mendukung ekspor kustom melalui endpoint `/export/pdf_custom` dengan memilih halaman dan chart yang ingin disertakan.

### Excel
File Excel multi-sheet yang dihasilkan menggunakan OpenPyXL:
- **Sheet 1 - Data Bersih:** Seluruh dataset setelah preprocessing
- **Sheet 2 - Statistik Numerik:** Tabel statistik deskriptif lengkap
- **Sheet 3 - Statistik Kategorikal:** Tabel frekuensi dan proporsi
- **Sheet 4 - Missing Values:** Ringkasan missing per kolom

### HTML Dashboard
Dashboard HTML mandiri (standalone) yang dapat dibuka di browser tanpa memerlukan server atau koneksi internet. Berisi semua visualisasi interaktif Plotly dan tabel statistik.

---

## Sistem Autentikasi

DataMind menggunakan sistem autentikasi file-based yang sederhana dan ringan:

- Data pengguna disimpan di `data/users.json`
- Password di-hash menggunakan SHA-256 dengan salt acak (`secrets.token_hex(16)`) sebelum disimpan
- Sesi dikelola oleh Flask session dengan secret key yang dikonfigurasi di `app.py`
- Pengguna tamu dapat menggunakan semua fitur analisis, namun fitur ekspor laporan dan riwayat dataset dikunci

**Skema data pengguna (`data/users.json`):**

```json
{
  "email@contoh.com": {
    "name": "Nama Pengguna",
    "email": "email@contoh.com",
    "password": "salt$sha256hash",
    "avatar_url": "/static/avatars/avatar_email.jpeg"
  }
}
```

> **Catatan untuk produksi:** Sistem autentikasi ini dirancang untuk keperluan pengembangan dan demonstrasi. Untuk deployment produksi, disarankan untuk menggunakan database relasional (PostgreSQL, MySQL) dan library autentikasi yang lebih robust seperti Flask-Login dengan Flask-SQLAlchemy.

---

## Kontributor

Proyek ini dikembangkan oleh tim mahasiswa sebagai bagian dari tugas akhir perkuliahan:

| Nama | NIM | Peran |
|---|---|---|
| Den Yuan Frasseka | 52250050 | Project Lead & Koordinator Analisis Data |
| Boma Satrio Wicaksono D. | 52250061 | Backend Development & Data Pipeline |
| Frizzy Litmensyah | 52250062 | EDA & Pembuatan Visualisasi Grafik |
| Angelica Florentina M | 52250063 | UI/UX Design Dashboard & Frontend |
| Adam Richie Wijaya | 52250064 | Statistical Analysis & Insight Generator |
| Andre | 52250065 | Data Cleaning, Preprocessing & Validasi |

**Dosen Pembimbing:** Bakti Siregar, M.Sc., CDS

---

*DataMind V2 - Intelligent Auto EDA Platform*
