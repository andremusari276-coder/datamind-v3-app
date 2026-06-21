# 🧠 DataMind — Intelligent Auto EDA Platform

> Platform Exploratory Data Analysis (EDA) berbasis web yang cerdas, dibangun dengan Flask & Python. Upload dataset CSV/Excel/TXT, lalu DataMind secara otomatis memvisualisasikan, menganalisis, dan memberikan insight berbasis AI — tanpa coding.

---

## 📋 Daftar Isi

- [Tentang Proyek](#tentang-proyek)
- [Fitur Utama](#fitur-utama)
- [Teknologi yang Digunakan](#teknologi-yang-digunakan)
- [Struktur Proyek](#struktur-proyek)
- [Instalasi & Menjalankan](#instalasi--menjalankan)
- [Konfigurasi Environment](#konfigurasi-environment)
- [Panduan Penggunaan](#panduan-penggunaan)
- [API Endpoints](#api-endpoints)
- [Ekspor Laporan](#ekspor-laporan)
- [Sistem Autentikasi](#sistem-autentikasi)
- [Kontributor](#kontributor)

---

## 📌 Tentang Proyek

**DataMind** adalah aplikasi web EDA otomatis yang dirancang untuk memudahkan analisis data tanpa perlu menulis kode. Dikembangkan sebagai proyek mata kuliah **Data Science Programming** di **Institut Teknologi Sains Bandung (ITSB)** — Fakultas Sains Data, Semester 2.

Dengan DataMind, pengguna cukup mengupload dataset, dan sistem akan secara otomatis:
- Mendeteksi tipe dan skala pengukuran setiap kolom
- Menghitung statistik deskriptif lengkap
- Menghasilkan visualisasi interaktif
- Memberikan insight berbasis AI (Gemini)
- Menyediakan tools preprocessing data (missing values, outlier, duplikat)

---

## ✨ Fitur Utama

### 📊 Analisis Data Otomatis
- **Overview dataset** — jumlah baris, kolom, tipe data, health score
- **Statistik deskriptif numerik** — mean, median, std, skewness, kurtosis, normalitas
- **Analisis kategorik** — frekuensi, modus, distribusi nilai unik
- **Deteksi time series** — otomatis mengenali kolom datetime dan merender tren

### 📈 Visualisasi Interaktif (Plotly)
- Histogram, Boxplot, Density Plot, Q-Q Plot, Violin Plot
- Bar Chart, Pie Chart, Count Plot, Pareto Chart
- Scatter Plot, Regression Plot, Bubble Chart
- Boxplot by Category, Violin by Category, Grouped Bar, Strip Plot
- Time Series: Line, Moving Average, Rolling Mean, Trend Line
- Correlation Heatmap
- **Custom chart** — pilih jenis chart dan kolom secara dinamis

### 🧹 Data Preprocessing
- **Missing Values** — deteksi, visualisasi heatmap pola, dan penanganan per kolom (drop, mean, median, mode, zero, custom value)
- **Outlier Detection** — IQR method, tampil baris outlier + severity, penanganan (drop, cap, replace median)
- **Duplicate Handling** — deteksi duplikat, hapus berdasarkan subset kolom pilihan
- **Data Quality Health Score** — skor 0-100 berbasis bobot missing, duplikat, dan outlier
- **Undo** — kembalikan data ke kondisi sebelum aksi preprocessing terakhir

### 🤖 AI Chatbot
- Terintegrasi dengan **Google Gemini 2.5 Flash**
- Memahami konteks dataset yang sedang dianalisis
- Menjawab pertanyaan umum maupun pertanyaan spesifik tentang data
- Fallback rule-based jika API key tidak tersedia

### 📤 Ekspor Laporan
- **PDF** — laporan lengkap multi-bab (cover, BAB 1-4) dengan statistik dan visualisasi
- **Excel** — export data bersih + statistik ke file `.xlsx`
- **HTML** — dashboard interaktif offline dalam satu file `.html`
- **Cleaned CSV** — dataset setelah preprocessing
- Semua fitur ekspor membutuhkan login

### 👤 Autentikasi & Profil Pengguna
- Register & Login berbasis email + password (hash SHA-256 + salt)
- Upload avatar profil
- Riwayat dataset yang pernah dianalisis (maks 20 entri)
- Reload dataset dari riwayat langsung ke dashboard

---

## 🛠️ Teknologi yang Digunakan

| Layer | Teknologi |
|---|---|
| Backend | Python 3.x, Flask |
| Data Processing | Pandas, NumPy, SciPy |
| Visualisasi | Plotly |
| AI/LLM | Google Gemini 2.5 Flash (google-genai SDK) |
| Export PDF | ReportLab |
| Export Excel | OpenPyXL |
| Frontend | HTML, CSS, JavaScript (Vanilla) |
| Autentikasi | Flask Session, SHA-256 + Salt (file-based JSON) |
| Environment | python-dotenv |

---

## 📁 Struktur Proyek

```
datamind_fixed/
│
├── app.py                          # Entry point utama Flask
├── requirements.txt                # Dependensi Python
├── .env                            # Konfigurasi environment (tidak di-commit)
├── .gitignore
│
├── backend/                        # Modul analisis data
│   ├── __init__.py
│   ├── data_loader.py              # Load CSV, Excel, TXT + sample dataset
│   ├── preprocessing.py            # Deteksi skala pengukuran, type casting
│   ├── descriptive_stats.py        # Statistik numerik (mean, std, skew, dll)
│   ├── categorical_analysis.py     # Analisis kolom kategorik
│   ├── visualization.py            # Semua fungsi chart Plotly
│   ├── time_series.py              # Deteksi & analisis time series
│   ├── insight_generator.py        # Generasi insight + summary untuk AI
│   ├── export_report.py            # Export PDF, Excel, HTML
│   ├── export_report_new.py        # Versi alternatif export
│   └── export_report_final.py      # Versi final export
│
├── frontend/
│   ├── templates/
│   │   └── dashboard.html          # Template utama (Single Page App)
│   └── static/
│       ├── css/
│       │   └── style.css           # Stylesheet utama
│       ├── js/
│       │   └── script.js           # Logic frontend (fetch API, chart render)
│       └── avatars/                # Foto profil pengguna (auto-generated)
│
├── data/
│   ├── users.json                  # Database pengguna (tidak di-commit)
│   ├── history/                    # Riwayat upload per pengguna (tidak di-commit)
│   ├── raw/                        # File upload sementara (tidak di-commit)
│   └── raw_user/                   # File dataset permanen per user (tidak di-commit)
│
└── outputs/
    ├── reports/                    # Hasil export laporan
    └── exported_files/             # Hasil export dashboard HTML
```

---

## 🚀 Instalasi & Menjalankan

### 1. Clone Repository

```bash
git clone https://github.com/username/datamind.git
cd datamind
```

### 2. Buat Virtual Environment

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# macOS / Linux
source venv/bin/activate
```

### 3. Install Dependensi

```bash
pip install -r requirements.txt
```

### 4. Konfigurasi Environment

Buat file `.env` di root proyek (lihat bagian [Konfigurasi Environment](#konfigurasi-environment)).

### 5. Jalankan Aplikasi

```bash
python app.py
```

Buka browser dan akses: **http://localhost:5000**

---

## ⚙️ Konfigurasi Environment

Buat file `.env` di folder root dengan isi berikut:

```env
GEMINI_API_KEY=your_google_gemini_api_key_here
```

Dapatkan API key Gemini secara gratis di: https://aistudio.google.com/app/apikey

> **Catatan:** Jika `GEMINI_API_KEY` tidak diset, AI Chatbot akan menggunakan mode fallback rule-based berbasis statistik dataset.

---

## 📖 Panduan Penggunaan

### Upload Dataset
1. Klik tombol **Upload File** di dashboard
2. Pilih file CSV, Excel (`.xlsx`/`.xls`), atau TXT
3. Ukuran maksimal: **50MB**
4. Minimal: **2 baris** dan **1 kolom**
5. Nama kolom harus unik (tidak boleh duplikat)

Atau klik **Load Sample** untuk mencoba dengan dataset contoh penjualan.

### Navigasi Dashboard
Dashboard terdiri dari beberapa panel:
- **Overview** — ringkasan dataset dan health badge
- **Data Preview** — tabel preview data (bisa load semua baris)
- **Column Info** — tipe data dan skala pengukuran per kolom
- **Numerical Stats** — statistik deskriptif kolom numerik
- **Categorical Stats** — frekuensi dan distribusi kolom kategorik
- **Visualisasi** — chart otomatis + custom chart builder
- **Missing Values** — panel analisis dan penanganan nilai kosong
- **Outlier** — deteksi dan penanganan outlier per kolom
- **Duplikat** — deteksi dan hapus baris duplikat
- **AI Insight** — ringkasan insight otomatis + chatbot
- **Export** — unduh laporan (butuh login)

### Preprocessing Data
Semua aksi preprocessing mendukung **Undo** (kembali ke kondisi sebelumnya).

| Aksi | Deskripsi |
|---|---|
| Drop baris | Hapus baris dengan nilai kosong/outlier |
| Fill mean/median/mode | Isi nilai kosong dengan statistik kolom |
| Fill custom | Isi dengan nilai yang ditentukan sendiri |
| Cap outlier | Batas outlier ke nilai IQR lower/upper |
| Replace median | Ganti outlier dengan nilai median |

---

## 🔌 API Endpoints

### Data & Analisis

| Method | Endpoint | Deskripsi |
|---|---|---|
| `GET` | `/api/overview` | Ringkasan dataset |
| `GET` | `/api/preview` | Preview data (param: `?limit=10` atau `?limit=all`) |
| `GET` | `/api/column_info` | Info tipe & skala per kolom |
| `GET` | `/api/stats/numerical` | Statistik numerik |
| `GET` | `/api/stats/categorical` | Statistik kategorik |
| `GET` | `/api/charts` | Semua chart otomatis (Plotly JSON) |
| `POST` | `/api/chart_col` | Chart custom per kolom |
| `GET` | `/api/timeseries` | Analisis time series |
| `POST` | `/api/insight` | Generate AI insight |

### Preprocessing

| Method | Endpoint | Deskripsi |
|---|---|---|
| `GET` | `/api/missing_info` | Info missing values per kolom |
| `GET` | `/api/missing_heatmap` | Heatmap pola missing values |
| `GET` | `/api/missing_overview` | Overview kolom dengan missing |
| `GET` | `/api/missing_pattern_rows` | Baris dengan missing terbanyak |
| `GET` | `/api/missing_rows` | Baris missing untuk kolom tertentu |
| `POST` | `/api/handle_missing` | Terapkan aksi penanganan missing |
| `GET` | `/api/outlier_info` | Info outlier per kolom numerik |
| `GET` | `/api/outlier_rows` | Baris outlier untuk kolom tertentu |
| `POST` | `/api/handle_outlier` | Terapkan aksi penanganan outlier |
| `GET` | `/api/duplicate_info` | Info baris duplikat |
| `POST` | `/api/handle_duplicate` | Hapus baris duplikat |
| `GET` | `/api/dq_health_score` | Data Quality Health Score |
| `POST` | `/api/undo` | Undo aksi preprocessing terakhir |

### Upload & Sample

| Method | Endpoint | Deskripsi |
|---|---|---|
| `POST` | `/upload` | Upload dataset |
| `POST` | `/load_sample` | Load sample dataset |

### Autentikasi

| Method | Endpoint | Deskripsi |
|---|---|---|
| `POST` | `/api/register` | Daftar akun baru |
| `POST` | `/api/login` | Login |
| `POST` | `/api/logout` | Logout |

### Profil & Riwayat

| Method | Endpoint | Deskripsi |
|---|---|---|
| `GET` | `/api/user/profile` | Info profil + statistik pengguna |
| `GET` | `/api/user/history` | Riwayat dataset |
| `POST` | `/api/user/history/<id>/reload` | Reload dataset dari riwayat |
| `POST` | `/api/user/avatar` | Upload foto profil |

### AI Chat

| Method | Endpoint | Deskripsi |
|---|---|---|
| `POST` | `/api/chat` | Kirim pesan ke AI chatbot |

---

## 📤 Ekspor Laporan

Semua fitur ekspor **membutuhkan login**. Guest akan mendapat respons `403 login_required`.

| Endpoint | Format | Deskripsi |
|---|---|---|
| `GET /export/pdf` | PDF | Laporan lengkap multi-bab |
| `POST /export/pdf_custom` | PDF | Laporan PDF dengan halaman & chart pilihan |
| `GET /export/excel` | XLSX | Data + statistik dalam format Excel |
| `GET /export/html` | HTML | Dashboard interaktif offline |
| `POST /export/html_custom` | HTML | Dashboard HTML dengan konten pilihan |
| `GET /export/cleaned_csv` | CSV | Dataset bersih setelah preprocessing |

Semua endpoint ekspor mendukung parameter `?lang=id` (default) atau `?lang=en` untuk memilih bahasa laporan.

---

## 🔐 Sistem Autentikasi

DataMind menggunakan autentikasi berbasis file JSON (`data/users.json`) dengan enkripsi password:

- **Algoritma:** SHA-256 dengan random salt (16-byte hex)
- **Format penyimpanan:** `{salt}${hash}`
- **Sesi:** Flask server-side session

> ⚠️ Sistem autentikasi ini dirancang untuk keperluan akademik/demo. Untuk deployment produksi, disarankan menggunakan database (PostgreSQL/MySQL) dan library autentikasi yang lebih mature seperti Flask-Login + Werkzeug.

---

## 👨‍💻 Kontributor

| Nama | NIM | Program Studi |
|---|---|---|
| Andre Musari | — | S1 Sains Data, ITSB |

**Dosen Pengampu:** Bakti Siregar, M.Sc., CDS  
**Mata Kuliah:** Data Science Programming  
**Institusi:** Institut Teknologi Sains Bandung (ITSB) — Fakultas Sains Data  
**Semester:** 2 (Genap)

---

## 📄 Lisensi

Proyek ini dibuat untuk keperluan akademik. Silakan digunakan dan dikembangkan lebih lanjut dengan menyertakan atribusi.

---

<p align="center">Made with ❤️ by Andre Musari · ITSB Sains Data 2024</p>
