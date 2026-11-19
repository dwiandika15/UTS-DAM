# Analisis Data Sektor Pariwisata dan Akomodasi Indonesia

# ✈️ UTS: Data Acquisition & Management (BIFP-243) — Sektor Pariwisata Indonesia

## 📝 Identitas Mahasiswa
* **Nama Lengkap** : I Kadek Dwi Andika
* **NIM**          : 2501010085
* **Program Studi**: Rekayasa Sistem Komputer / Sistem Informasi
* **Mata Kuliah**   : Data Acquisition & Management
* **Kelas / Group** : Kelas E
* **Institusi**     : Institut Teknologi dan Bisnis Bali (INSTIKI)
* **Tanggal Rilis** : November 2026

---

## 📌 Deskripsi Proyek
Proyek ini dikembangkan sebagai pemenuhan Penilaian Tengah Semester (UTS) dalam mata kuliah **Data Acquisition & Management**. Fokus utama dari proyek ini adalah membangun jalur (*pipeline*) akuisisi data pariwisata yang etis, terstruktur, aman, dan dapat diaudit dari tiga jenis tipe data berbeda (terstruktur, semi-terstruktur, dan tidak terstruktur) serta mengintegrasikannya dengan validasi integritas data yang ketat.

---

## 📂 Struktur Repositori & Folder Proyek

Sesuai standar penataan data yang diatur pada petunjuk evaluasi, repositori diatur dengan struktur folder yang bersih dan terisolasi:

```text
UTS-DAM/
├── data/
│   ├── raw/                               # Data mentah asli tanpa modifikasi sedikit pun
│   │   ├── indonesia_pariwisata_super_prioritas_enriched.csv  # Sumber 1
│   │   ├── photon_osm_akomodasi_indonesia_raw.json            # Sumber 2
│   │   └── wikipedia_tourism_in_indonesia_raw.html            # Sumber 3
│   └── processed/                         # Data hasil kurasi, pembersihan & integrasi
│       ├── hasil_gabungan_pariwisata_akomodasi_indonesia.csv  # Hasil Akhir Merge
│       ├── catatan_provenance.csv         # Bukti hash MD5 dari data mentah
│       └── metadata_hasil_gabungan.json   # Berkas metadata JSON lengkap
├── docs/                                  # Dokumentasi kepatuhan etika data
│   ├── data_ethics_checklist.csv          # Tabel kepatuhan format CSV
│   └── data_ethics_checklist.md           # Visualisasi aspek kepatuhan Markdown
├── .gitignore                             # Mencegah file rahasia (.env) terunggah ke publik
├── .env                                   # Konfigurasi privat User-Agent (Lokal saja)
├── UTS_DAM.ipynb                          # Notebook utama pengerjaan UTS
└── README.md                              # Dokumentasi lengkap proyek ini (Dokumen ini)
│       └── data_ethics_checklist.md
└── UTS_DAM_Project.ipynb     # File notebook utama pengerjaan UTS

```
# 🔍 Rincian Pengerjaan Setiap Soal (Soal 1 s.d. Soal 5)

# 📑 Soal 1: Identifikasi & Estimasi Storage Data Terstruktur
Sumber Data (Sumber 1): Berkas indonesia_pariwisata_super_prioritas_enriched.csv diunggah manual ke folder data/raw/.

Metrik Dataset Utama:

Jumlah Baris: 10.000 baris data perjalanan.

Jumlah Kolom: 26 kolom (misal: tourist_trip_id, destination_cluster, total_spending_per_person_idr, dll).

Ukuran Berkas: ~14,57 MB (15.282.221 bytes).

Cakupan Wilayah: 8 Klaster Destinasi Pariwisata Super Prioritas (DPSP) Indonesia (Bali Greater Hub, Borobudur, Danau Toba, Mandalika, Labuan Bajo, Bromo, Raja Ampat, Likupang).

Asumsi & Estimasi Storage 1 Tahun:

Berdasarkan akuisisi rutin bulanan data baru berukuran serupa:

$\text{Kebutuhan Dasar} = 14,57\text{ MB} \times 12 = 174,89\text{ MB}$
$\text{Cadangan Overhead (30%)} = 52,47\text{ MB}$
$\text{Total Estimasi Storage Pertahun} = \mathbf{227,36\text{ MB}}$

# 🛡️ Soal 2: Kepatuhan Lisensi, Identifikasi PII, dan Etika Data
Pemeriksaan Lisensi:

Sumber 1 (Kaggle): CC0 (Public Domain).

Sumber 2 (Photon API): Open Database License (ODbL 1.0) milik OpenStreetMap.

Sumber 3 (Wikipedia): Creative Commons Attribution-ShareAlike 4.0 (CC BY-SA 4.0).

Analisis Privasi (PII):

Direct PII (Sensitif): 0 kolom ditemukan (Tidak ada kolom nama, email, nomor ponsel, atau NIK).

Quasi-Identifiers: Teridentifikasi 5 kolom (observation_date, destination_cluster, tourist_origin_category, travel_party_size, accommodation_type) yang telah digeneralisasi sehingga aman digunakan.

Data Ethics Checklist: 9 butir kepatuhan telah divalidasi melaui skrip otomatis dan seluruh dokumen disimpan di folder docs/.

# 🌐 Soal 3: Akuisisi & Normalisasi API Semi-Terstruktur

Sumber Data (Sumber 2): Photon OpenStreetMap Search API (https://photon.komoot.io/api/).

Metode Akuisisi: Query otomatis menggunakan koordinat geografis pusat dari masing-masing 8 klaster destinasi pariwisata.

Kepatuhan Protokol: Membaca berkas .env lokal untuk memuat email kampus yang digunakan secara transparan pada header User-Agent. Menerapkan jeda waktu (rate limit) dinamis selama 1.5 detik setiap pemanggilan API.

Hasil Normalisasi: Mengubah bentuk bersarang (nested JSON) GeoJSON menjadi tabular dataframe.

Hasil: 49 baris akomodasi pariwisata unik di Indonesia (Memenuhi batas minimal instruksi tugas: > 30 baris).

# 🕷️ Soal 4: Web Scraping Data Tidak Terstruktur & robots.txt
Sumber Data (Sumber 3): Wikipedia (Tourism in Indonesia - Halaman Statis).

Pemeriksaan robots.txt: Menggunakan modul Python urllib.robotparser.RobotFileParser untuk memverifikasi secara real-time aturan domain en.wikipedia.org. Dinyatakan DIIZINKAN (True) untuk mengambil data secara aman.

Metodologi Crawling: Request dikirimkan dengan header User-Agent mahasiswa, dilanjutkan jeda waktu 2.0 detik, dan diparsing menggunakan pustaka BeautifulSoup dengan parser lxml.

Hasil Ekstraksi: Berkas HTML mentah disimpan tanpa modifikasi di data/raw/wikipedia_tourism_in_indonesia_raw.html. Berhasil mengekstraksi tabel kunjungan berukuran 48 baris x 11 kolom.

# 🔗 Soal 5: Integrasi Data, Metadata, & Provenance (Auditability)
Prosedur Integrasi:

Mengagregasikan data Sumber 2 (API Akomodasi) agar berelasi unik dengan kolom kunci destination_cluster.
Melakukan penggabungan (merge) tipe left join dengan menerapkan parameter keamanan kardinalitas validate='many_to_one' di Pandas.

Metrik Keamanan & Bukti Bebas Duplikasi:
Baris Sebelum Merge: 10.000 baris.

Baris Setelah Merge: 10.000 baris.

Jumlah Duplikat pada Primary Key (tourist_trip_id): 0 baris.

Status Validasi: TERBUKTI 100% BEBAS DUPLIKASI BARIS (LULUS UJI ASSERT).

Data Provenance: Ketiga sumber data dicatat secara rinci di dalam file data/processed/catatan_provenance.csv lengkap dengan URL akses asli, tanggal pengunduhan, lisensi resmi, lokasi fisik berkas mentah lokal, dan MD5 Hash checksum unik untuk menjamin data tidak dimanipulasi.

Metadata Integrasi: Tersimpan rapi secara detail dalam format JSON pada berkas data/processed/metadata_hasil_gabungan.json.

🛠️ Persyaratan Pustaka (Dependencies)
Untuk dapat menjalankan notebook UTS_DAM.ipynb secara lokal atau di lingkungan Google Colab, Anda memerlukan pustaka berikut:

pip install pandas numpy requests beautifulsoup4 lxml python-dotenv tabulate
📬 Kontak & Atribusi
Proyek akademik ini bersifat open-source dan bebas digunakan untuk kebutuhan riset akademik non-komersial.

Penyusun: I Kadek Dwi Andika ([redacted link])
Surel Akademis: 2501010085@instiki.ac.id
