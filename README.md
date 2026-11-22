# Analisis Segmentasi Karakteristik Audio Lagu untuk Optimalisasi Kurasi Playlist Otomatis Menggunakan Algoritma K-Means dan Hierarchical Clustering

> **Proyek Akhir Praktikum Data Science**  
> Program Studi Informatika — Fakultas Teknik Industri  
> Universitas Pembangunan Nasional "Veteran" Yogyakarta

---

## 👥 Identitas Kelompok

| No | Nama Anggota | NIM | Peran Utama |
|:--:|:-------------|:---:|:------------|
| 1 | Yohanes Herang Aji Dharma | 123240191 | Modeling & Error Analysis |
| 2 | Muhammad Azfa Imaduddin | 123240208 | Data Gathering & Business Understanding |
| 3 | Rabel Octadian Jhonifer | 123210176 | Data Cleaning & Preprocessing |
| 4 | Diky Surya Pranata | 123240011 | Exploratory Data Analysis & Visualisasi |

---

## 📌 Ringkasan Proyek

Proyek ini bertujuan mengelompokkan lagu secara otomatis berdasarkan kemiripan 9 karakteristik audio fisik (*danceability, energy, loudness, speechiness, acousticness, instrumentalness, liveness, valence, tempo*) menggunakan paradigma **Unsupervised Learning**. Hasil segmentasi dimanfaatkan sebagai draf otomatis kurasi **playlist tematik** guna mengatasi keterbatasan kurasi manual dan bias label genre konvensional.

### Rumusan Masalah
Bagaimana mengelompokkan lagu secara otomatis berdasarkan kemiripan karakteristik audio menggunakan algoritma K-Means dan Hierarchical Clustering, sehingga terbentuk segmen yang dapat diinterpretasikan sebagai profil playlist tematik?

### Evaluasi Tanpa Target (Metrik Internal)
Karena tidak ada label ground truth (*unsupervised*), evaluasi menggunakan metrik internal:
1. **Silhouette Score:** Mengukur separasi dan kepadatan klaster (rentang -1 s.d. 1).
2. **Davies-Bouldin Index (DBI):** Mengukur rasio sebaran dalam klaster terhadap jarak antarklaster (makin kecil makin baik).
3. **Calinski-Harabasz Index (CHI):** Mengukur rasio dispersi antarklaster terhadap dalam klaster (makin besar makin baik).

---

## 📂 Data & Preprocessing

- **Sumber Data:** Spotify Tracks Dataset oleh Maharshi Pandya di [Kaggle](https://www.kaggle.com/datasets/maharshipandya/-spotify-tracks-dataset) (Lisensi CC0 Public Domain).
- **Subset Analisis:** 10 genre strategis dengan kontras energi audio (*pop, hip-hop, rock, dance, classical, acoustic, metal, jazz, indie, edm*), berukuran awal 10.000 baris.
- **Cleaning:**
  - Hapus anomali fisik tempo non-positif (`tempo <= 0`).
  - Deduplikasi `track_id` (1.047 duplikat lintas genre dihapus dengan mempertahankan kemunculan pertama, seluruh genre per lagu disimpan ke `all_genres`).
  - **Dataset akhir siap model:** **8.952 baris unik** x 9 fitur audio kontinu.
- **Feature Scaling:** Seluruh 9 fitur kontinu distandarisasi menggunakan `StandardScaler` ($\mu = 0, \sigma = 1$) sebelum perhitungan jarak Euclidean.

---

## 🧠 Pemodelan & Hasil Evaluasi

### 1. Penentuan Jumlah Klaster Optimal ($k$)
Diuji pada rentang $k \in [2, 7]$:
- Titik siku (*elbow*) kurva inertia berada pada $k = 4$.
- Silhouette tertinggi untuk kandidat playlist tematik ($k \geq 3$) dicapai pada **$k = 4$ (0,2385)**.
- Didukung oleh DBI rendah (**1,424**) dan CHI yang tinggi (**2.382,4**), serta ukuran klaster terkecil **8,14%** (memenuhi batas minimal 3%).

### 2. Perbandingan K-Means vs Hierarchical Clustering (Ward)
Pada $k = 4$:

| Metrik Evaluasi | K-Means (Utama) | Hierarchical Clustering (Pembanding) | Keunggulan |
|:----------------|:---------------:|:------------------------------------:|:----------:|
| **Silhouette Score (↑)** | **0,2385** | 0,2110 | **K-Means** |
| **Davies-Bouldin Index (↓)** | **1,4236** | 1,4552 | **K-Means** |
| **Calinski-Harabasz Index (↑)** | **2.382,37** | 2.062,25 | **K-Means** |
| **Waktu Komputasi** | **0,05 detik** | 1,55 detik (30x lebih lambat) | **K-Means** |
| **Prediksi Lagu Baru (*Cold-Start*)** | **Tersedia (`predict` centroid)** | Tidak langsung (harus hitung ulang) | **K-Means** |

**Tingkat Keselarasan Model:** Adjusted Rand Index (**ARI = 0,634**) dan **87,0% lagu** berada pada klaster yang sepadan, membuktikan struktur klaster stabil dan mencerminkan pola sonik alami.

---

## 🎧 Profil 4 Playlist Tematik (Model K-Means)

| Klaster | Nama Playlist Tematik | Proporsi Lagu | Karakter Audio Utama | Genre Dominan | Mood / Rekomendasi Penggunaan |
|:-------:|:----------------------|:-------------:|:---------------------|:--------------|:------------------------------|
| **0** | **Urban Beats & Spoken Flow** | 729 (8,14%) | `speechiness` tinggi (0,256; +2,74σ), ritmis (`danceability` 0,703) | Hip-Hop (38,4%), Dance (18,9%), Pop (13,3%) | Pesta santai, nongkrong, penggemar lirik berima |
| **1** | **Chill & Acoustic Study** | 2.530 (28,26%) | `acousticness` tinggi (0,734; +0,99σ), lembut (`energy` 0,340, `loudness` -11,0 dB) | Jazz (29,4%), Acoustic (24,9%), Indie (14,2%) | Belajar, bekerja santai, relaksasi malam hari |
| **2** | **Instrumental Calm & Deep Focus** | 778 (8,69%) | `instrumentalness` sangat tinggi (0,825; +2,85σ), `energy` minimal (0,147), sangat hening (-21,8 dB) | Classical (85,7%), Jazz (7,2%) | Meditasi mendalam, fokus belajar tanpa vokal, pengantar tidur |
| **3** | **High-Energy Workout & Party** | 4.915 (54,90%) | `energy` tinggi (0,740; +0,61σ), lantang (-5,76 dB), elektrik (`acousticness` 0,133) | Metal (18,5%), Dance (15,9%), EDM (13,7%), Rock (12,8%) | Olahraga di gym, pendorong semangat kerja, pesta malam |

---

## 🔍 Error Analysis (Ambiguitas Klaster)

- Rata-rata Silhouette: **0,2385**; hanya **2,51% lagu (225 lagu)** yang memiliki $s(i) < 0$ (berada di batas wilayah dua klaster).
- Lagu ambigu tersebut tidak dibuang, melainkan dijadikan **lagu transisi (*bridge tracks*)** antartema playlist.
- Fitur paling membedakan antarklaster: `instrumentalness` ($\eta^2 = 0,773$), `acousticness` ($\eta^2 = 0,750$), dan `energy` ($\eta^2 = 0,698$).

---

## 🗂️ Struktur Direktori Proyek

```
projek_spotify_coba1/
├── projek.ipynb                   # Notebook Jupyter lengkap (semua tahap terisi hasil eksperimen)
├── requirements.txt               # Daftar pustaka Python yang dibutuhkan
├── README.md                      # Dokumentasi komprehensif proyek
├── dataset.csv                    # Dataset asli Spotify Tracks (10 genre)
├── data/
│   ├── dataset.csv                # Salinan dataset
│   └── processed/                 # Artefak model & data bersih
│       ├── kmeans_model.joblib    # Objek model KMeans terlatih
│       ├── standard_scaler.joblib # Objek StandardScaler terlatih
│       ├── spotify_clean.csv      # Data setelah pembersihan (8.952 baris)
│       └── spotify_clustered.csv  # Data lengkap beserta label klaster & nama playlist
└── figures/                       # Seluruh visualisasi analisis (disimpan beresolusi tinggi)
    ├── eda_1_distribusi_fitur.png
    ├── eda_2_boxplot_outlier.png
    ├── eda_3_heatmap_korelasi.png
    ├── eda_4_perbandingan_genre.png
    ├── model_1_elbow_silhouette.png
    ├── model_2_distribusi_klaster.png
    ├── eval_1_pca_scatter.png
    ├── eval_2_dendrogram.png
    ├── eval_3_profil_heatmap.png
    ├── eval_4_genre_per_cluster.png
    └── eval_5_error_analysis.png
```

---

## 🚀 Panduan Menjalankan

1. **Clone / Buka Repositori**
2. **Pasang dependensi Python:**
   ```bash
   pip install -r requirements.txt
   ```
3. **Buka dan jalankan Jupyter Notebook:**
   ```bash
   jupyter notebook projek.ipynb
   ```
   Pilih menu `Kernel > Restart & Run All`. Seluruh sel akan dieksekusi tanpa error.

---

## 📅 Timeline Proyek (4 Minggu)

| Minggu | Agenda Kerja | Output |
|:------:|:-------------|:-------|
| **1** | Business Understanding, Data Gathering, Audit Kualitas, EDA Visual | Perumusan rumusan masalah & metrik, visualisasi distribusi & korelasi |
| **2** | Data Cleaning, Penanganan Anomali & Duplikasi, `StandardScaler` | Dataset bersih 8.952 baris siap modeling, matriks fitur terstandarisasi |
| **3** | Eksperimen Modeling ($k=2..7$), K-Means & Hierarchical, Evaluasi Metrik | Plot Elbow & Silhouette, perbandingan performa model |
| **4** | Profiling Klaster, Error Analysis, Penamaan Playlist, Dokumentasi & README | Model tersimpan, laporan interpretasi playlist tematik, repositori final |

---

## ⚖️ Lisensi & Etika
- Dataset bersumber dari data publik Spotify API via Kaggle (CC0 1.0).
- Tidak memuat data pribadi pengguna (PII).
- Segmentasi bersifat statistik sonik untuk kurasi playlist dan bukan penilaian atas nilai artistik atau kultural karya musik.
