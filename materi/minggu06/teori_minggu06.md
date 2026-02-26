# Teori Minggu 6: Exploratory Data Analysis (EDA)

## Tujuan Pembelajaran

Setelah mempelajari materi minggu ini, mahasiswa diharapkan mampu:
1. Memahami konsep dan tujuan Exploratory Data Analysis (EDA)
2. Menghitung dan menginterpretasikan statistik deskriptif
3. Mengidentifikasi distribusi data dan anomali
4. Melakukan analisis korelasi antar variabel
5. Membuat visualisasi yang efektif untuk eksplorasi data
6. Mendeteksi outlier menggunakan metode statistik

---

## 1. Apa itu EDA dan Mengapa Penting?

**Exploratory Data Analysis (EDA)** adalah pendekatan analisis data yang bertujuan untuk merangkum karakteristik utama data, sering kali menggunakan metode visual. EDA dikembangkan oleh statistikawan John Tukey pada tahun 1970-an sebagai alternatif dari pendekatan konfirmatori tradisional.

### Tujuan EDA

- **Memahami struktur data**: mengetahui tipe, ukuran, dan distribusi data
- **Menemukan pola**: mengidentifikasi tren, hubungan, dan anomali
- **Menguji asumsi**: memverifikasi asumsi yang diperlukan untuk pemodelan
- **Memilih fitur**: menentukan variabel mana yang penting untuk analisis lanjutan
- **Mendeteksi kesalahan**: menemukan nilai yang hilang, duplikat, atau tidak masuk akal

### Alur Kerja EDA

```
+------------------+
|   Load Data      |
+--------+---------+
         |
         v
+------------------+
| Inspect & Clean  |  <-- dtypes, missing values, duplicates
+--------+---------+
         |
         v
+------------------+
| Univariate       |  <-- distribusi satu variabel
| Analysis         |
+--------+---------+
         |
         v
+------------------+
| Bivariate /      |  <-- korelasi, scatter plot
| Multivariate     |
+--------+---------+
         |
         v
+------------------+
| Outlier          |  <-- IQR, Z-score
| Detection        |
+--------+---------+
         |
         v
+------------------+
| Insights &       |  <-- kesimpulan, hipotesis
| Conclusions      |
+------------------+
```

---

## 2. Statistik Deskriptif

Statistik deskriptif merangkum karakteristik utama suatu kumpulan data. Ini adalah langkah pertama dan paling fundamental dalam EDA.

### 2.1 Ukuran Tendensi Sentral

| Statistik | Definisi | Rumus | Kelebihan | Kekurangan |
|-----------|----------|-------|-----------|------------|
| **Mean** (Rata-rata) | Jumlah semua nilai dibagi banyak data | x̄ = Σxᵢ / n | Memperhitungkan semua nilai | Sensitif terhadap outlier |
| **Median** | Nilai tengah setelah data diurutkan | Nilai ke-(n+1)/2 | Robust terhadap outlier | Tidak memperhitungkan semua nilai |
| **Mode** | Nilai yang paling sering muncul | Nilai dengan frekuensi tertinggi | Berlaku untuk data kategoris | Bisa tidak ada atau lebih dari satu |

### 2.2 Ukuran Dispersi (Sebaran)

| Statistik | Definisi | Interpretasi |
|-----------|----------|--------------|
| **Range** | Max - Min | Rentang keseluruhan data |
| **Variance (σ²)** | Rata-rata kuadrat simpangan dari mean | Makin besar, makin tersebar |
| **Std Dev (σ)** | Akar kuadrat dari variance | Interpretasi lebih intuitif (satuan sama) |
| **IQR** | Q3 - Q1 | Sebaran 50% data tengah |
| **Coefficient of Variation** | (σ / x̄) × 100% | Perbandingan relatif antar kelompok |

### 2.3 Ukuran Bentuk Distribusi

**Skewness (Kemiringan)**
- Mengukur asimetri distribusi data
- **Skewness = 0**: distribusi simetris (normal)
- **Skewness > 0** (positif): ekor panjang ke kanan, sebagian besar data di kiri
- **Skewness < 0** (negatif): ekor panjang ke kiri, sebagian besar data di kanan

**Kurtosis (Keruncingan)**
- Mengukur ketebalan ekor distribusi dibandingkan distribusi normal
- **Kurtosis = 3** (mesokurtik): sama dengan distribusi normal
- **Kurtosis > 3** (leptokurtik): ekor lebih tebal, puncak lebih tinggi
- **Kurtosis < 3** (platikurtik): ekor lebih tipis, distribusi lebih datar

### Tabel Interpretasi Statistik Deskriptif

| Statistik | Nilai | Interpretasi |
|-----------|-------|--------------|
| Mean ≈ Median ≈ Mode | Hampir sama | Distribusi mendekati simetris |
| Mean >> Median | Mean jauh lebih besar | Skewed positif, ada outlier atas |
| Mean << Median | Mean jauh lebih kecil | Skewed negatif, ada outlier bawah |
| CV < 15% | Rendah | Homogen, data konsisten |
| CV 15-35% | Sedang | Variabilitas moderat |
| CV > 35% | Tinggi | Heterogen, data sangat bervariasi |
| Skewness ∈ (-0.5, 0.5) | Mendekati nol | Hampir simetris |
| Skewness > 1 atau < -1 | Besar | Sangat miring |
| Excess Kurtosis > 1 | Positif | Leptokurtik, ekor berat |

---

## 3. Distribusi Data

### 3.1 Distribusi Normal (Gaussian)
- Berbentuk lonceng simetris
- Mean = Median = Mode
- **Aturan 68-95-99.7**: 68% data dalam ±1σ, 95% dalam ±2σ, 99.7% dalam ±3σ
- Banyak metode statistik mengasumsikan normalitas

### 3.2 Distribusi Skewed
- **Right-skewed (positif)**: penghasilan, harga rumah, ukuran file
- **Left-skewed (negatif)**: nilai ujian yang mudah, usia kematian

### 3.3 Distribusi Bimodal
- Memiliki dua puncak (dua modus)
- Sering mengindikasikan dua subpopulasi yang berbeda dalam data

### 3.4 Uji Normalitas
- **Shapiro-Wilk**: cocok untuk sampel kecil (n < 50)
- **Kolmogorov-Smirnov**: untuk sampel besar
- **Q-Q Plot**: visualisasi untuk mengecek normalitas

---

## 4. Analisis Korelasi

Korelasi mengukur kekuatan dan arah hubungan linear antara dua variabel.

### 4.1 Koefisien Korelasi Pearson (r)
- Mengukur hubungan **linear** antara dua variabel kontinu
- Asumsi: data berdistribusi normal, tidak ada outlier signifikan
- Range: -1 ≤ r ≤ 1

| Nilai r | Interpretasi |
|---------|--------------|
| 0.9 - 1.0 | Sangat kuat positif |
| 0.7 - 0.9 | Kuat positif |
| 0.5 - 0.7 | Moderat positif |
| 0.3 - 0.5 | Lemah positif |
| 0.0 - 0.3 | Sangat lemah / tidak ada |
| Nilai negatif | Berlawanan arah |

### 4.2 Koefisien Korelasi Spearman (ρ)
- Versi non-parametrik dari Pearson, berbasis ranking
- Tidak mengasumsikan normalitas
- Robust terhadap outlier
- Cocok untuk data ordinal atau data yang tidak berdistribusi normal

### 4.3 Koefisien Korelasi Kendall (τ)
- Alternatif non-parametrik lainnya, berbasis pasangan konkordans/diskordans
- Lebih robust untuk sampel kecil
- Interpretasi lebih langsung secara probabilistik

> **Penting**: Korelasi ≠ Kausalitas! Dua variabel yang berkorelasi tidak berarti yang satu menyebabkan yang lain.

---

## 5. Visualisasi untuk EDA

### 5.1 Histogram
- Menampilkan distribusi frekuensi satu variabel numerik
- Membantu mengidentifikasi distribusi, skewness, dan outlier
- Pilih jumlah bin yang tepat (Sturges rule: k = 1 + log₂(n))

### 5.2 Box Plot (Diagram Kotak)
- Menampilkan ringkasan 5 angka: Min, Q1, Median, Q3, Max
- Outlier ditampilkan sebagai titik individual
- Sangat berguna untuk perbandingan distribusi antar kelompok

```
    |-----|    |-----|
----| box |----| box |----
    |-----|    |-----|
  Min  Q1   Med  Q3  Max
         * = outlier
```

### 5.3 Scatter Plot
- Menampilkan hubungan antara dua variabel numerik
- Membantu mengidentifikasi pola, kluster, dan outlier

### 5.4 Heatmap Korelasi
- Matriks warna yang menampilkan korelasi antar semua variabel numerik
- Warna hangat (merah) = korelasi positif tinggi
- Warna dingin (biru) = korelasi negatif
- Warna netral (putih/kuning) = korelasi mendekati nol

### 5.5 Pair Plot
- Menampilkan scatter plot untuk semua pasangan variabel sekaligus
- Diagonal biasanya menampilkan distribusi univariat (histogram/KDE)
- Efisien untuk melihat banyak hubungan sekaligus

---

## 6. Deteksi Anomali dan Outlier

**Outlier** adalah nilai yang sangat berbeda dari nilai-nilai lain dalam dataset. Outlier bisa berupa kesalahan data atau observasi yang valid tapi ekstrem.

### 6.1 Metode IQR (Interquartile Range)
```
IQR = Q3 - Q1
Batas bawah = Q1 - 1.5 × IQR
Batas atas  = Q3 + 1.5 × IQR
Outlier: nilai di luar [batas bawah, batas atas]
```

### 6.2 Metode Z-Score
```
Z = (x - μ) / σ
Outlier: |Z| > 3 (atau 2.5 untuk threshold lebih ketat)
```

### 6.3 Metode Modified Z-Score (Robust)
- Menggunakan median dan MAD (Median Absolute Deviation)
- Lebih robust ketika data sudah mengandung banyak outlier

### 6.4 Penanganan Outlier
| Strategi | Kapan Digunakan |
|----------|-----------------|
| **Hapus** | Outlier adalah kesalahan data yang terbukti |
| **Cap/Winsorize** | Ganti dengan batas IQR/persentil |
| **Transform** | Log transform untuk right-skewed data |
| **Biarkan** | Outlier adalah informasi penting (e.g., fraud detection) |
| **Model terpisah** | Analisis outlier sebagai kelompok tersendiri |

---

## 7. Ringkasan

EDA adalah fondasi dari setiap proyek data science yang baik. Prinsip utamanya:

1. **Selalu mulai dengan EDA** sebelum membuat model apapun
2. **Gunakan visualisasi** untuk memahami data secara intuitif
3. **Statistik deskriptif** memberikan gambaran kuantitatif awal
4. **Korelasi** membantu memilih fitur yang relevan
5. **Outlier** harus diidentifikasi dan ditangani dengan hati-hati
6. **EDA adalah proses iteratif** — setiap temuan bisa memicu eksplorasi lebih lanjut

---

## Referensi

1. Tukey, J.W. (1977). *Exploratory Data Analysis*. Addison-Wesley.
2. McKinney, W. (2022). *Python for Data Analysis*, 3rd Edition. O'Reilly Media.
3. VanderPlas, J. (2016). *Python Data Science Handbook*. O'Reilly Media.
4. Seaborn Documentation: https://seaborn.pydata.org
5. Pandas Documentation: https://pandas.pydata.org/docs/
6. NIST/SEMATECH e-Handbook of Statistical Methods: https://www.itl.nist.gov/div898/handbook/
