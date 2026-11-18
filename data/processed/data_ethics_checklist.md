# Data Ethics Checklist - UTS Data Acquisition & Management
1. Kepatuhan Lisensi & ToS: Lulus (Sumber terbuka Kaggle, Open-Meteo CC BY 4.0, Wikipedia CC BY-SA 4.0; bebas dari situs OTA/medsos terlarang).
2. Kepatuhan robots.txt & Rate Limiting: Lulus (Dicek via RobotFileParser, timeout=15s, jeda time.sleep 1.5-2s).
3. Identitas User-Agent Transparan: Lulus (Menyertakan nama proyek dan email kampus @instiki.ac.id).
4. Perlindungan Privasi & PII: Lulus (0 kolom Direct PII; hanya ID anonim dan atribut agregat/kategori).
5. Keamanan Kredensial: Lulus (Menggunakan file .env, tidak ada API key/token di dalam notebook).
6. Keaslian Data Mentah: Lulus (Tersimpan utuh di folder data/raw/ tanpa modifikasi).
7. Pencatatan Provenance: Lulus (Tercatat URL, tanggal akses, lisensi, dan hash MD5 setiap file mentah).
8. Minimisasi Data: Lulus (Hanya mengambil parameter cuaca dan wilayah untuk 8 klaster destinasi).
9. Transparansi Bias & Keterbatasan: Lulus (Koordinat API mewakili titik pusat klaster wisata).
