# Analisis Segmentasi Karakteristik Audio Lagu untuk Optimalisasi Kurasi Playlist Otomatis

## 1. Deskripsi Proyek

Proyek ini bertujuan untuk menganalisis dan mengelompokkan lagu berdasarkan karakteristik audio menggunakan algoritma **K-Means Clustering** dan **Hierarchical Clustering**.

Pengelompokan dilakukan berdasarkan fitur audio seperti danceability, energy, loudness, speechiness, acousticness, instrumentalness, liveness, valence, dan tempo.

Hasil clustering diharapkan dapat membantu proses kurasi playlist secara otomatis berdasarkan kemiripan karakteristik audio lagu.

## 2. Identitas Kelompok

**Judul:** Analisis Segmentasi Karakteristik Audio Lagu untuk Optimalisasi Kurasi Playlist Otomatis Menggunakan Algoritma K-Means dan Hierarchical Clustering

**Anggota Kelompok:**

| No. | Nama | NIM |
|---|---|---|
| 1 | Yohanes Herang Aji Dharma | 123240191 |
| 2 | Muhammad Azfa Imaduddin | 123240208 |
| 3 | Rabel Octadian Jhonifer | 123210176 |
| 4 | Diky Surya Pranata | 123240011 |

**Dosen Pengampu:**  
Dhimas Arief Dharmawan, S.T., Ph.D.

**Program Studi Informatika**  
Universitas Pembangunan Nasional "Veteran" Yogyakarta  
2026

## 3. Tujuan Proyek

Tujuan dari proyek ini adalah:

1. Mengidentifikasi karakteristik audio dari kumpulan lagu.
2. Melakukan eksplorasi dan analisis terhadap fitur-fitur audio.
3. Mengelompokkan lagu berdasarkan kemiripan karakteristik audio.
4. Membandingkan hasil algoritma K-Means dan Hierarchical Clustering.
5. Menentukan algoritma yang memberikan hasil pengelompokan terbaik.
6. Menginterpretasikan hasil clustering untuk mendukung kurasi playlist otomatis.

## 4. Dataset

Dataset yang digunakan dalam proyek ini adalah **Spotify Tracks Dataset** yang diperoleh dari Kaggle.

Sumber dataset:  
https://www.kaggle.com/datasets/maharshipandya/-spotify-tracks-dataset

Dataset berisi informasi lagu beserta sejumlah karakteristik audionya.

### Fitur yang Digunakan

| No. | Fitur | Keterangan |
|---|---|---|
| 1 | Danceability | Tingkat kecocokan lagu untuk aktivitas menari |
| 2 | Energy | Tingkat intensitas dan aktivitas lagu |
| 3 | Loudness | Tingkat kekerasan suara lagu |
| 4 | Speechiness | Tingkat keberadaan unsur ucapan dalam lagu |
| 5 | Acousticness | Tingkat karakteristik akustik lagu |
| 6 | Instrumentalness | Tingkat kemungkinan lagu tidak memiliki vokal |
| 7 | Liveness | Indikasi keberadaan penampilan langsung |
| 8 | Valence | Tingkat suasana positif atau ceria dalam lagu |
| 9 | Tempo | Kecepatan atau ketukan lagu |

## 5. Metode yang Digunakan

### A. Data Preprocessing

Tahapan preprocessing meliputi:
- Memeriksa data kosong dan data duplikat.
- Memilih fitur audio yang digunakan.
- Menyiapkan data untuk proses clustering.
- Melakukan standardisasi menggunakan StandardScaler.

### B. K-Means Clustering

K-Means digunakan untuk mengelompokkan lagu ke dalam tiga cluster berdasarkan kemiripan karakteristik audio.

### C. Hierarchical Clustering

Hierarchical Clustering digunakan sebagai metode pembanding untuk melihat hubungan dan pengelompokan antarlagu berdasarkan kemiripan fitur audio.

Metode Ward digunakan dalam proses pengelompokan hierarkis.

### D. Evaluasi

Evaluasi dilakukan menggunakan tiga metrik:
- Silhouette Score
- Davies-Bouldin Index
- Calinski-Harabasz Index

## 6. Tahapan Pengerjaan

Urutan pengerjaan proyek:

1. Business Understanding
2. Data Gathering dan Data Understanding
3. Exploratory Data Analysis (EDA)
4. Data Cleaning
5. Data Preprocessing
6. Implementasi K-Means Clustering
7. Implementasi Hierarchical Clustering
8. Evaluasi Hasil Clustering
9. Visualisasi dan Interpretasi Cluster
10. Kesimpulan

## 7. Hasil Evaluasi

Berdasarkan hasil pengujian yang tercantum dalam laporan UTS kelompok, diperoleh hasil berikut:

| Metrik | K-Means | Hierarchical Ward |
|---|---:|---:|
| Silhouette Score | 0.382 | 0.354 |
| Davies-Bouldin Index | 1.214 | 1.348 |
| Calinski-Harabasz Index | 3421.50 | 3105.12 |

Berdasarkan hasil tersebut, K-Means memperoleh Silhouette Score dan Calinski-Harabasz Index yang lebih tinggi serta Davies-Bouldin Index yang lebih rendah. Dengan demikian, K-Means memberikan hasil clustering yang lebih baik pada pengujian dalam laporan kelompok.

## 8. Interpretasi Cluster

Berdasarkan interpretasi hasil pada laporan kelompok, karakteristik cluster dapat dijelaskan sebagai berikut:

- **Cluster 0 – Chill & Acoustic Study:** Kelompok lagu dengan karakteristik yang cenderung santai dan akustik, cocok untuk belajar atau bersantai.
- **Cluster 1 – High-Energy Workout & Party:** Kelompok lagu dengan energi tinggi yang cocok untuk olahraga, aktivitas, dan suasana pesta.
- **Cluster 2 – Heavy & Aggressive Rock/Metal:** Kelompok lagu dengan karakteristik musik yang lebih kuat dan agresif, seperti rock dan metal.

Catatan: Nomor cluster merupakan label hasil algoritma dan dapat berubah ketika proses clustering dijalankan ulang.

## 9. Struktur Repository

```text
Spotify_Music_Clustering_Project/
│
├── data/
│   └── README.txt
│
├── main.ipynb
├── README.md
├── requirements.txt
└── .gitignore
```

## 10. Cara Menjalankan Proyek

### A. Clone Repository

```bash
git clone https://github.com/username/repository.git
```

### B. Masuk ke Folder Proyek

```bash
cd Spotify_Music_Clustering_Project
```

### C. Install Library

```bash
pip install -r requirements.txt
```

### D. Siapkan Dataset

Unduh dataset dari Kaggle, kemudian letakkan file CSV di dalam folder `data` dengan nama:

```text
data/dataset.csv
```

### E. Jalankan Notebook

Buka file `main.ipynb` menggunakan Jupyter Notebook atau Visual Studio Code.

Jalankan setiap cell secara berurutan dari awal hingga akhir.

## 11. Teknologi dan Library

Proyek ini menggunakan beberapa teknologi dan library Python:

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Jupyter Notebook

## 12. Kesimpulan

Berdasarkan proses analisis dan pengujian yang dilakukan, algoritma K-Means dan Hierarchical Clustering dapat digunakan untuk mengelompokkan lagu berdasarkan karakteristik audio.

Hasil evaluasi menunjukkan bahwa K-Means memberikan performa lebih baik dibandingkan Hierarchical Clustering pada pengujian yang tercantum dalam laporan kelompok.

Hasil pengelompokan dapat dimanfaatkan sebagai dasar pengembangan sistem kurasi playlist otomatis berdasarkan kemiripan karakteristik audio lagu.

## 13. Catatan

- Dataset tidak disertakan langsung dalam repository karena ukuran dan ketentuan sumber dataset.
- Pengguna perlu mengunduh dataset secara mandiri dari sumber yang dicantumkan.
- Hasil notebook perlu dijalankan ulang menggunakan dataset yang sesuai untuk memperoleh hasil eksperimen.
- Interpretasi cluster dapat berubah bergantung pada data dan hasil proses clustering.

---

**Kelompok Proyek Data Science**  
Program Studi Informatika  
Universitas Pembangunan Nasional "Veteran" Yogyakarta  
2026
