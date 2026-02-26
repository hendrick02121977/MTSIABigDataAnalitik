# Teori Minggu 5: Praproses Data (Data Preprocessing)

## Tujuan Pembelajaran

Setelah mempelajari materi ini, mahasiswa diharapkan mampu:
1. Menjelaskan pentingnya praproses data dalam pipeline Big Data Analytics
2. Mengidentifikasi berbagai masalah kualitas data (missing values, duplikat, outlier, inkonsistensi)
3. Menerapkan teknik cleaning data menggunakan Python (pandas, scikit-learn)
4. Mengimplementasikan teknik transformasi data (normalisasi, encoding)
5. Merancang feature engineering untuk meningkatkan kualitas fitur model

---

## Pentingnya Praproses Data

Praproses data (data preprocessing) adalah serangkaian teknik yang digunakan untuk mengubah data mentah menjadi format yang bersih, konsisten, dan siap dianalisis atau digunakan untuk pemodelan machine learning. Tahap ini sering disebut sebagai **data wrangling** atau **data munging**.

Dalam industri, ditemukan bahwa **data scientist menghabiskan 60–80% waktunya** untuk membersihkan dan mempersiapkan data, bukan untuk analisis atau pemodelan. Ini menunjukkan betapa krusialnya tahap praproses dalam seluruh alur kerja data science.

### Mengapa Praproses Data Penting?

- **"Garbage In, Garbage Out"**: Model machine learning hanya sebaik data yang diberikan kepadanya
- **Konsistensi**: Memastikan data dari berbagai sumber memiliki format yang seragam
- **Akurasi**: Data yang bersih menghasilkan analisis dan prediksi yang lebih akurat
- **Efisiensi**: Data yang dioptimalkan mempercepat pelatihan model
- **Kepatuhan**: Memastikan data memenuhi standar kualitas yang dipersyaratkan

---

## Masalah Kualitas Data

### 1. Missing Values (Nilai Kosong)

Missing values terjadi ketika tidak ada nilai yang tersimpan untuk variabel dalam observasi tertentu. Penyebabnya bisa berupa:
- Kesalahan pengumpulan data (sensor rusak, form tidak diisi)
- Data tidak tersedia pada waktu pengumpulan
- Privasi (responden menolak menjawab)
- Kesalahan penggabungan dataset

**Jenis Missing Values:**
- **MCAR (Missing Completely at Random)**: Kemunculan missing value tidak bergantung pada data yang ada
- **MAR (Missing at Random)**: Kemunculan missing value bergantung pada variabel lain yang teramati
- **MNAR (Missing Not at Random)**: Kemunculan missing value bergantung pada nilai yang hilang itu sendiri

### 2. Duplikat

Data duplikat adalah baris atau rekaman yang muncul lebih dari satu kali dalam dataset. Penyebabnya:
- Penggabungan dataset dari berbagai sumber
- Kesalahan proses ETL (Extract, Transform, Load)
- Entri data manual yang dilakukan berulang kali

### 3. Outlier

Outlier adalah titik data yang menyimpang jauh dari pola umum dataset. Outlier bisa berupa:
- **Error Outlier**: Disebabkan kesalahan pengukuran atau entri (harus dihapus)
- **Natural Outlier**: Nilai ekstrim yang valid secara alami (perlu penanganan hati-hati)

### 4. Inkonsistensi Data

Inkonsistensi muncul ketika data yang sama direpresentasikan dengan cara berbeda:
- Format tanggal berbeda: "2024-01-15" vs "15/01/2024" vs "January 15, 2024"
- Penulisan nama berbeda: "Jakarta" vs "jakarta" vs "JAKARTA"
- Satuan berbeda: nilai dalam km vs miles
- Kategori tidak konsisten: "Laki-laki" vs "L" vs "M" vs "Male"

---

## Teknik Cleaning Data

### Penanganan Missing Values

| Teknik | Kapan Digunakan | Kelebihan | Kekurangan |
|---|---|---|---|
| **Listwise Deletion** | Data MCAR, dataset besar | Mudah, tidak bias jika MCAR | Kehilangan data, bias jika MAR/MNAR |
| **Mean Imputation** | Data numerik, distribusi normal | Sederhana, mempertahankan mean | Mengurangi varians |
| **Median Imputation** | Data numerik, ada outlier | Robust terhadap outlier | Mengurangi varians |
| **Mode Imputation** | Data kategorik | Sederhana | Bisa meningkatkan bias |
| **Forward Fill (ffill)** | Data time series | Mempertahankan tren temporal | Tidak cocok untuk data non-serial |
| **KNN Imputation** | Data kompleks, MAR | Akurat, mempertimbangkan konteks | Komputasi mahal |
| **MICE/Multiple Imputation** | MNAR, data kompleks | Teoritis paling tepat | Kompleks, lambat |

### Teknik Koreksi Inkonsistensi

```python
# Contoh koreksi inkonsistensi
import pandas as pd

df['kota'] = df['kota'].str.strip().str.title()  # Normalisasi kapitalisasi
df['tanggal'] = pd.to_datetime(df['tanggal'], dayfirst=True)  # Standardisasi tanggal
df['gender'] = df['gender'].map({'L': 'Laki-laki', 'P': 'Perempuan', 'M': 'Laki-laki'})
```

---

## Transformasi Data

### Normalisasi

Normalisasi mengubah skala nilai numerik ke rentang tertentu tanpa mengubah distribusi relatif nilai.

#### Min-Max Normalization (Feature Scaling)

Formula: **X_norm = (X - X_min) / (X_max - X_min)**

- Menghasilkan nilai dalam rentang [0, 1]
- Cocok untuk algoritma berbasis jarak (KNN, K-Means)
- Sensitif terhadap outlier

#### Z-Score Standardization

Formula: **X_std = (X - μ) / σ**

dimana μ adalah mean dan σ adalah standar deviasi.

- Menghasilkan distribusi dengan mean=0 dan std=1
- Cocok untuk algoritma yang mengasumsikan distribusi normal (regresi linear, SVM)
- Lebih robust terhadap outlier dibanding Min-Max

### Encoding Kategorik

| Teknik | Deskripsi | Kapan Digunakan |
|---|---|---|
| **Label Encoding** | Mengganti kategori dengan angka (0, 1, 2...) | Variabel ordinal |
| **One-Hot Encoding** | Membuat kolom biner untuk setiap kategori | Variabel nominal tanpa urutan |
| **Target Encoding** | Ganti kategori dengan rata-rata target | Kardinalitas tinggi |
| **Binary Encoding** | Representasi biner dari label | Kardinalitas sedang-tinggi |
| **Frequency Encoding** | Ganti kategori dengan frekuensinya | Kardinalitas tinggi |

---

## Feature Engineering

Feature engineering adalah proses membuat fitur baru dari data yang ada untuk meningkatkan performa model. Teknik umum:

- **Binning/Discretization**: Mengubah variabel kontinu menjadi kategori (misalnya usia → kelompok umur)
- **Polynomial Features**: Membuat fitur polinomial dari fitur yang ada (X², X·Y)
- **Date/Time Decomposition**: Mengekstrak tahun, bulan, hari, jam dari timestamp
- **Text Features**: TF-IDF, word embeddings dari data teks
- **Agregasi**: Membuat fitur statistik (mean, std, min, max) dari data yang dikelompokkan
- **Interaksi Fitur**: Perkalian atau kombinasi dua fitur yang bermakna

---

## Pipeline Praproses Data

```
+------------+    +-----------+    +----------------+    +---------------+    +------------+
|            |    |           |    |                |    |               |    |            |
|  RAW DATA  +--->+  CLEANING +--->+ TRANSFORMATION +--->+   FEATURE     +--->+ CLEAN DATA |
|            |    |           |    |                |    |  ENGINEERING  |    |            |
| - Missing  |    | - Hapus   |    | - Normalisasi  |    | - Binning     |    | - Complete |
| - Duplikat |    |   duplikat|    | - Standardisasi|    | - Polynomial  |    | - Consistent|
| - Outlier  |    | - Isi     |    | - Encoding     |    | - Datetime    |    | - Scaled   |
| - Inkonsist|    |   missing |    | - Transformasi |    |   Extract     |    | - Encoded  |
|            |    | - Fix     |    |   log/sqrt     |    | - Interaction |    |            |
|            |    |   outlier |    |                |    |               |    |            |
+------------+    +-----------+    +----------------+    +---------------+    +------------+
```

---

## Contoh Kode Python

```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import MinMaxScaler, StandardScaler, LabelEncoder
from sklearn.impute import SimpleImputer

# --- Membuat data contoh dengan masalah ---
np.random.seed(42)
data = {
    'usia': [25, np.nan, 30, 200, 28, 25, np.nan, 35, 29, 30],  # ada missing & outlier
    'gaji': [5000, 6000, np.nan, 7000, 5500, 5000, 6500, 7500, np.nan, 6000],
    'kota': ['Jakarta', 'jakarta', 'BANDUNG', 'Bandung', 'Surabaya',
             'Jakarta', 'Jakarta', 'Bandung', 'Surabaya', 'Bandung'],
    'pendidikan': ['S1', 'S2', 'SMA', 'S1', 'S2', 'S1', 'D3', 'S2', 'SMA', 'S1']
}
df = pd.DataFrame(data)

# --- Cleaning ---
# 1. Hapus duplikat
df = df.drop_duplicates()

# 2. Perbaiki inkonsistensi
df['kota'] = df['kota'].str.strip().str.title()

# 3. Tangani outlier (IQR method)
Q1 = df['usia'].quantile(0.25)
Q3 = df['usia'].quantile(0.75)
IQR = Q3 - Q1
df = df[~((df['usia'] < (Q1 - 1.5 * IQR)) | (df['usia'] > (Q3 + 1.5 * IQR)))]

# 4. Imputasi missing values
imputer = SimpleImputer(strategy='median')
df[['usia', 'gaji']] = imputer.fit_transform(df[['usia', 'gaji']])

# --- Transformasi ---
# Normalisasi kolom numerik
scaler = MinMaxScaler()
df[['usia_norm', 'gaji_norm']] = scaler.fit_transform(df[['usia', 'gaji']])

# Encoding kolom kategorik
le = LabelEncoder()
df['pendidikan_encoded'] = le.fit_transform(df['pendidikan'])

# One-Hot Encoding untuk kota
df = pd.get_dummies(df, columns=['kota'], prefix='kota')

print(df.head())
print(df.dtypes)
```

---

## Ringkasan

Praproses data adalah tahap yang tidak dapat diabaikan dalam setiap proyek analitik data. Kualitas data yang baik adalah syarat mutlak untuk menghasilkan analisis dan model yang akurat dan dapat diandalkan. Poin-poin kunci:

1. **Identifikasi masalah data** sejak awal dengan eksplorasi data (EDA) yang menyeluruh.
2. **Penanganan missing values** harus disesuaikan dengan jenis data dan pola ketidakhadirannya (MCAR/MAR/MNAR).
3. **Outlier** perlu ditangani dengan hati-hati — tidak semua outlier harus dihapus.
4. **Normalisasi dan standardisasi** penting untuk algoritma yang sensitif terhadap skala fitur.
5. **Feature engineering** yang kreatif dapat meningkatkan performa model secara signifikan.
6. Gunakan **scikit-learn Pipeline** untuk membuat alur praproses yang dapat direproduksi dan mencegah data leakage.

---

## Referensi

1. Géron, A. (2022). *Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow* (3rd ed.). O'Reilly Media.
2. McKinney, W. (2022). *Python for Data Analysis* (3rd ed.). O'Reilly Media.
3. VanderPlas, J. (2016). *Python Data Science Handbook*. O'Reilly Media.
4. García, S., Luengo, J., & Herrera, F. (2015). *Data Preprocessing in Data Mining*. Springer.
5. scikit-learn Documentation - Preprocessing. https://scikit-learn.org/stable/modules/preprocessing.html
6. pandas Documentation - User Guide. https://pandas.pydata.org/docs/user_guide/index.html
