# Segmentasi Karakteristik Audio Lagu untuk Optimalisasi Kurasi Playlist Otomatis Menggunakan Algoritma K-Means dan Hierarchical Clustering

Proyek akhir mata kuliah **Data Science** (kelas Teori dan Praktikum), Program Studi Informatika, Fakultas Teknik Industri, Universitas Pembangunan Nasional "Veteran" Yogyakarta, Tahun Ajaran Ganjil 2026/2027.

Proyek ini membangun model **clustering** (*unsupervised learning*) untuk mengelompokkan lagu berdasarkan kemiripan karakteristik sinyal audio fisiknya guna mendukung optimalisasi kurasi *playlist* otomatis. Seluruh alur kerja mengikuti kerangka CRISP-DM dan dikerjakan secara lengkap dalam satu notebook (`main.ipynb`).

## Daftar Isi

1. [Ringkasan Proyek](#ringkasan-proyek)
2. [Anggota dan Kontribusi](#anggota-dan-kontribusi)
3. [Sumber Data](#sumber-data)
4. [Kamus Data](#kamus-data)
5. [Alur Pengerjaan](#alur-pengerjaan)
6. [Struktur Repository](#struktur-repository)
7. [Cara Menjalankan](#cara-menjalankan)
8. [Asumsi, Batasan, dan Etika](#asumsi-batasan-dan-etika)
9. [Lisensi dan Referensi](#lisensi-dan-referensi)

## Ringkasan Proyek

**Masalah.** Pengelompokan lagu secara manual berdasarkan label genre sering kali kurang akurat dalam mencerminkan suasana (*mood*) atau konteks aktivitas pendengar. Dua lagu dengan label genre yang sama (misalnya `pop` atau `indie`) dapat memiliki nuansa akustik yang bertolak belakang—satu bertempo lambat dan bernuansa tenang, sementara lainnya berenergi tinggi dan cocok untuk menari. Selain itu, kurasi *playlist* secara manual pada katalog musik digital yang terus bertumbuh tidak efisien dan rentan subjektivitas.

**Solusi.** Model *clustering* yang mengelompokkan lagu secara objektif berdasarkan **9 fitur sinyal audio fisik** (`danceability`, `energy`, `loudness`, `speechiness`, `acousticness`, `instrumentalness`, `liveness`, `valence`, dan `tempo`) tanpa bergantung pada label genre. Hasil segmentasi dipetakan menjadi profil *playlist* tematik otomatis (seperti *Chill & Acoustic Study*, *High-Energy Workout & Party*, dan *Heavy & Aggressive Rock/Metal*).

**Pemangku kepentingan.** Pengembang aplikasi atau sistem rekomendasi musik, kurator *playlist*, pendengar musik digital, musisi dan industri musik, serta analis data dan akademisi.

**Metode dan metrik.**

| Aspek | Keterangan |
|---|---|
| Jenis tugas | Unsupervised learning, clustering |
| Fitur masukan | 9 fitur akustik kontinu (terstandarisasi dengan `StandardScaler`) |
| Model yang dibandingkan | K-Means Clustering (`init='k-means++'`), Hierarchical Agglomerative Clustering (`linkage='ward'`) |
| Rentang pengujian klaster | $k = 2$ sampai $k = 6$ (dievaluasi dengan Elbow Method & Silhouette Analysis) |
| Metrik evaluasi | Silhouette Score ($\uparrow$), Davies-Bouldin Index ($\downarrow$), Calinski-Harabasz Index ($\uparrow$) |
| Reproduksibilitas | `random_state=42` |

Hasil perbandingan metrik, visualisasi klaster (PCA 2D & Dendrogram), profil akustik tiap klaster, dan kesimpulan lengkap tersedia pada bagian *Modeling*, *Evaluation*, dan *Konklusi* di `main.ipynb`.

## Anggota dan Kontribusi

**Kelompok** (Informatika, UPN "Veteran" Yogyakarta)

| No | Nama | NIM |
|---|---|---|
| 1 | Yohanes Herang Aji Dharma | 123240191 |
| 2 | Muhammad Azfa Imaduddin | 123240208 |
| 3 | Rabel Octadian Jhonifer | 123210176 |
| 4 | Diky Surya Pranata | 123240011 |

**Dosen pengampu:** Dhimas Arief Dharmawan, S.T., Ph.D.

**Pembagian pengerjaan pada `main.ipynb`**

| Tahap | Bagian | Penanggung jawab |
|---|---|---|
| Business Understanding | Pernyataan masalah, tujuan SMART, pemetaan stakeholder, penentuan metode dan metrik | Yohanes Herang Aji Dharma |
| Data Gathering | Akuisisi subset 100 data & 10 genre, deskripsi lisensi, penyusunan kamus data | Muhammad Azfa Imaduddin |
| Data Understanding | Cek missing value, cek duplikasi `track_id`, deteksi outlier menggunakan boxplot | Muhammad Azfa Imaduddin |
| Data Understanding | Exploratory Data Analysis (distribusi fitur, heatmap korelasi, profil lintas genre) | Rabel Octadian Jhonifer |
| Data Preparation | Pembersihan nilai hilang dan deduplikasi identitas trek (`track_id`) | Rabel Octadian Jhonifer |
| Data Preparation | Feature selection (pemisahan metadata vs fitur audio) dan feature scaling (`StandardScaler`) | Diky Surya Pranata |
| Modeling | Penentuan jumlah klaster optimal ($k$) dengan Elbow Method dan Silhouette Score | Diky Surya Pranata |
| Modeling | Pelatihan model K-Means Clustering dan Hierarchical Agglomerative Clustering | Diky Surya Pranata |
| Evaluation & Konklusi | Komparasi metrik evaluasi klaster, visualisasi PCA, cluster profiling playlist, dan konklusi | Yohanes Herang Aji Dharma |

## Sumber Data

| Item | Keterangan |
|---|---|
| Nama dataset | Spotify Tracks Dataset |
| Penyedia | Maharshi Pandya, melalui Kaggle |
| Tautan | https://www.kaggle.com/datasets/maharshipandya/spotify-tracks-dataset |
| Lisensi | CC0: Public Domain |
| Metode pengumpulan | Ekstraksi melalui Spotify Web API |
| Cakupan sampel | Subset 100 baris data yang mewakili 10 genre strategis (`pop`, `hip-hop`, `rock`, `dance`, `classical`, `acoustic`, `metal`, `jazz`, `indie`, `edm` — masing-masing 10 lagu) |
| File di repository | `data/spotify_100_10genres.csv` |

Dataset memuat informasi identitas lagu, metadata popularitas, kategori genre bawaan Spotify, serta karakteristik sinyal audio numerik hasil ekstraksi Spotify Audio Features.

## Kamus Data

| Feature | Description | Data Type | Example |
|---|---|---|---|
| `track_id` | ID unik trek lagu dari Spotify API (22 karakter) | Kategorikal (String) | `5SuOikwiRyPMVoIQDJUgSV` |
| `artists` | Nama artis atau penyanyi | Kategorikal (String) | `Gen Hoshino` |
| `album_name` | Nama album tempat lagu dirilis | Kategorikal (String) | `Comedy` |
| `track_name` | Judul lagu | Kategorikal (String) | `Comedy` |
| `popularity` | Tingkat kepopuleran lagu di platform Spotify (0–100) | Integer | `73` |
| `duration_ms` | Total durasi lagu dalam satuan milidetik | Integer | `230666` |
| `explicit` | Indikator keberadaan konten lirik dewasa (`True` / `False`) | Boolean | `False` |
| `danceability` | Kesesuaian lagu untuk menari berdasarkan ritme dan stabilitas ketukan (0.0–1.0) | Float | `0.676` |
| `energy` | Ukuran perseptual intensitas, kecepatan, dan aktivitas audio (0.0–1.0) | Float | `0.461` |
| `key` | Kunci nada keseluruhan menggunakan notasi Pitch Class (-1 s.d. 11) | Integer | `1` |
| `loudness` | Tingkat kenyaringan rata-rata lagu dalam satuan desibel (-60.0 s.d. 0.0 dB) | Float | `-6.746` |
| `mode` | Modalitas tangga nada lagu (`0` = Minor, `1` = Major) | Integer biner | `0` |
| `speechiness` | Deteksi keberadaan kata terucap atau lirik vokal dalam rekaman (0.0–1.0) | Float | `0.143` |
| `acousticness` | Tingkat keyakinan bahwa trek bersifat akustik tanpa instrumen elektrik (0.0–1.0) | Float | `0.0322` |
| `instrumentalness` | Prediksi apakah lagu tidak memuat vokal; nilai $>0.5$ cenderung instrumental (0.0–1.0) | Float | `0.000001` |
| `liveness` | Deteksi keberadaan audiens atau rekaman konser langsung (0.0–1.0) | Float | `0.358` |
| `valence` | Positivitas musikal; nilai tinggi = ceria/bahagia, nilai rendah = sedih/agresif (0.0–1.0) | Float | `0.715` |
| `tempo` | Kecepatan ritme lagu dalam satuan *Beats Per Minute* (BPM) | Float | `87.917` |
| `time_signature` | Perkiraan tanda birama lagu (3 s.d. 7, misal `4` mewakili ketukan 4/4) | Integer | `4` |
| `track_genre` | Kategori genre bawaan Spotify; tidak dipakai saat latih, hanya untuk evaluasi interpretasi | Kategorikal (String) | `acoustic` |

**Ringkasan kondisi data.**

- Ditemukan nilai hilang (*missing value*) minor pada atribut metadata teks (`artists`, `album_name`, `track_name`), ditangani dengan penghapusan baris (`dropna`).
- Ditemukan potensi duplikasi pada `track_id` karena satu lagu dapat terdaftar di lebih dari satu kategori `track_genre` pada Spotify API; ditangani dengan deduplikasi berbasis `track_id`.
- Terdeteksi pencilan (*outlier*) pada `speechiness`, `liveness`, dan `duration_ms` yang merupakan variasi karakteristik alami antar genre (bukan kesalahan input data), sehingga dipertahankan dan ditangani menggunakan standarisasi skala.
- Terdapat korelasi positif kuat antara `energy` dan `loudness` ($r \approx 0.81$) serta korelasi negatif kuat antara `energy` dan `acousticness` ($r \approx -0.73$).

**Pemilihan fitur.** Model hanya menggunakan 9 fitur sinyal audio kontinu (`danceability`, `energy`, `loudness`, `speechiness`, `acousticness`, `instrumentalness`, `liveness`, `valence`, `tempo`). Kolom identitas teks (`track_id`, `artists`, `album_name`, `track_name`), metadata komersial (`popularity`, `duration_ms`), atribut struktural diskrit (`key`, `mode`, `time_signature`, `explicit`), serta label `track_genre` dikeluarkan dari matriks latih agar algoritma mengelompokkan lagu murni berdasarkan kemiripan karakteristik suara.

## Alur Pengerjaan

Seluruh proses analisis dan pemodelan berada di dalam satu notebook, `main.ipynb`, dengan urutan sebagai berikut:

1. **Business Understanding**: latar belakang masalah kurasi *playlist*, rumusan pertanyaan bisnis, tujuan SMART, pemetaan stakeholder, serta pemilihan metode dan metrik evaluasi.
2. **Data Gathering**: pemuatan subset dataset 100 lagu (
