# Analisis Data Sektor Pariwisata dan Akomodasi Indonesia

# 📊 Pipeline Pengumpulan & Pengolahan Data Pariwisata & Akomodasi Indonesia
**Mata Kuliah:** Data Acquisition & Management (DAM)  
**Institusi:** INSTIKI  

---

## 🧭 Gambaran Umum Alur (Pipeline Overview)
Proyek ini mengimplementasikan siklus hidup data (*data lifecycle*) secara end-to-end, mulai dari perencanaan penyimpanan, audit etika, pengambilan data multi-sumber (File, API, dan Web Scraping), hingga integrasi dan validasi data akhir.

Berikut adalah diagram alur kerja (*pipeline*) yang dieksekusi di dalam notebook:
```text
[Sumber 1: CSV Kaggle] ──┐
                         ├──> [Validasi & Cleaning] ──> [Merge (validate='many_to_one')] ──> [Hasil Akhir & Metadata JSON]
[Sumber 2: API Photon]  ──┤
[Sumber 3: Scraping Web] ─┘
```
⚙️ Detail Alur Pengumpulan & Pengolahan Data (Soal 1 - 5)
# 1. Soal 1: Identifikasi Sumber & Estimasi Storage
Sumber Data 1: Dataset perjalanan pariwisata super prioritas Indonesia (indonesia_pariwisata_super_prioritas_enriched.csv).

Proses: Memuat dataset awal (~10.000 baris), menghitung dimensi data, serta melakukan estimasi proyeksi kebutuhan ruang penyimpanan (storage growth) untuk 1 tahun ke depan berdasarkan tingkat pertumbuhan harian/bulanan.

# 2. Soal 2: Tata Kelola & Pemeriksaan Etika Data
Proses: Menyusun dokumen checklist etika (data_ethics_checklist.md) yang mencakup:

Verifikasi ketiadaan Informasi Identifikasi Pribadi (PII / Personally Identifiable Information).

Pencatatan lisensi data terbuka (Open Data License).

Penerapan identitas etis melalui User-Agent kampus pada setiap permintaan HTTP.

# 3. Soal 3: Akuisisi Data via API (OpenStreetMap Photon)
Sumber Data 2: Inventaris akomodasi nyata (hotel, resort, villa) di 8 klaster destinasi super prioritas.

Proses:

Melakukan request otomatis ke Photon OpenStreetMap API menggunakan parameter koordinat/nama wilayah.

Menerapkan jeda waktu sopan (crawl delay / rate limiting) dan mekanisme penanganan error try-except.

Menyimpan hasil respons mentah secara utuh ke folder data/raw/photon_osm_akomodasi_indonesia_raw.json tanpa modifikasi.

# 4. Soal 4: Web Scraping Halaman Statis & Kepatuhan robots.txt
Sumber Data 3: Statistik makro pariwisata Indonesia dari Wikipedia (Tourism in Indonesia).

Proses:

Menggunakan pustaka urllib.robotparser untuk membaca dan memverifikasi https://en.wikipedia.org/robots.txt.

Memastikan status kepatuhan bernilai DIIZINKAN (True) sebelum mengambil halaman.

Menyimpan file HTML mentah ke data/raw/wikipedia_tourism_in_indonesia_raw.html.

Mengekstrak tabel statistik menggunakan BeautifulSoup dan pd.read_html.

# 5. Soal 5: Penggabungan, Validasi, & Penyimpanan Metadata
Proses Integrasi:

Menggabungkan Sumber 1 (data transaksi per klaster) dengan ringkasan Sumber 2 (jumlah akomodasi API per klaster) menggunakan pd.merge pada kunci destination_cluster.

Menerapkan parameter pengaman validate='many_to_one' untuk menjamin konsistensi relasi data.

Melakukan audit duplikasi otomatis menggunakan fungsi duplicated() pada primary key (tourist_trip_id) untuk memastikan tidak terjadi duplikasi baris (0 duplikat).

Mengekspor hasil akhir ke folder data/processed/ beserta Metadata JSON dan Catatan Provenance.

📂 Struktur Direktori Penyimpanan Data
Sesuai prinsip reproducible research, direktori data dibagi secara ketat menjadi dua bagian:

```
UTS-DAM/
├── data/
│   ├── raw/                  # Data mentah original (immutable)
│   │   ├── indonesia_pariwisata_super_prioritas_enriched.csv
│   │   ├── photon_osm_akomodasi_indonesia_raw.json
│   │   └── wikipedia_tourism_in_indonesia_raw.html
│   └── processed/            # Data hasil olahan, metadata, dan audit
│       ├── hasil_gabungan_pariwisata_akomodasi_indonesia.csv
│       ├── metadata_hasil_gabungan.json
│       ├── catatan_provenance.csv
│       └── data_ethics_checklist.md
└── UTS_DAM_Project.ipynb     # File notebook utama pengerjaan UTS
