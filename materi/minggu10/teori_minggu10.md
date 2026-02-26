# Minggu 10: Advanced Machine Learning

## Tujuan Pembelajaran

Setelah mempelajari materi minggu ini, mahasiswa diharapkan mampu:
1. Memahami dan menerapkan berbagai metode ensemble untuk meningkatkan performa model
2. Melakukan feature engineering dan feature selection secara sistematis
3. Membangun ML pipeline yang efisien dan reproducible menggunakan scikit-learn
4. Melakukan hyperparameter tuning menggunakan berbagai strategi pencarian
5. Menangani masalah class imbalance dalam dataset nyata
6. Memahami konsep stacking sebagai teknik ensemble tingkat lanjut

---

## 1. Ensemble Methods (Metode Ansambel)

Ensemble methods menggabungkan beberapa model (weak learners) untuk menghasilkan prediksi yang lebih baik daripada model tunggal. Prinsip dasarnya: "wisdom of the crowd" — banyak model yang lemah namun beragam dapat bersama-sama membentuk model yang kuat.

### 1.1 Bagging (Bootstrap Aggregating)

Bagging melatih beberapa model secara **paralel** menggunakan subset data yang berbeda (diambil dengan penggantian — bootstrap sampling):

1. Buat B subset data dengan bootstrap sampling
2. Latih satu model pada setiap subset
3. Gabungkan prediksi dengan **voting** (klasifikasi) atau **rata-rata** (regresi)

**Contoh:** Random Forest adalah implementasi bagging yang menggunakan decision tree dengan tambahan pemilihan fitur acak di setiap split.

**Keunggulan Bagging:**
- Mengurangi variance (mengatasi overfitting)
- Paralelisasi mudah
- Estimasi out-of-bag (OOB) untuk evaluasi tanpa test set terpisah

### 1.2 Boosting

Boosting melatih model secara **sekuensial**, di mana setiap model baru berfokus pada kesalahan model sebelumnya:

#### AdaBoost (Adaptive Boosting)
- Assign bobot yang sama ke semua sampel di awal
- Setiap iterasi: naikkan bobot sampel yang salah diklasifikasi
- Model lemah yang lebih akurat mendapat bobot lebih besar saat voting

#### Gradient Boosting
- Setiap model baru mempelajari residual (gradient loss function) dari model sebelumnya
- Lebih fleksibel karena mendukung berbagai loss function
- Implementasi populer: scikit-learn GradientBoostingClassifier

#### XGBoost (Extreme Gradient Boosting)
- Implementasi gradient boosting yang dioptimalkan secara komputasional
- Mendukung regularisasi L1 (Lasso) dan L2 (Ridge) secara native
- Menangani missing values secara otomatis
- Mendukung komputasi paralel dan GPU
- Sering memenangkan kompetisi ML (Kaggle, dll.)

### 1.3 Stacking (Stacked Generalization)

Stacking menggunakan output dari beberapa **base learners** sebagai fitur untuk **meta-learner**:

```
Layer 1 (Base Learners):  Model A ─┐
                          Model B ─┼─▶  Meta-Learner ─▶ Final Prediction
                          Model C ─┘
```

Base learners bisa berupa model yang sangat berbeda (decision tree, SVM, neural network), dan meta-learner belajar cara terbaik menggabungkan prediksinya.

---

## 2. Feature Engineering

Feature Engineering adalah proses mengubah data mentah menjadi representasi fitur yang lebih informatif untuk model ML.

### 2.1 Feature Selection (Seleksi Fitur)

**Filter Methods:**
- Berdasarkan statistik univariat (korelasi, chi-square, ANOVA F-test)
- Cepat dan independen dari model
- Contoh scikit-learn: `SelectKBest`, `VarianceThreshold`

**Wrapper Methods:**
- Menggunakan model sebagai black-box untuk mengevaluasi subset fitur
- Contoh: Recursive Feature Elimination (RFE)
- Lebih akurat tapi komputasional mahal

**Embedded Methods:**
- Seleksi fitur dilakukan selama proses pelatihan model
- Contoh: L1 regularization (Lasso) yang mendorong koefisien menjadi nol
- `SelectFromModel` dengan estimator berbasis tree atau Lasso

### 2.2 Feature Extraction (Ekstraksi Fitur)

**Principal Component Analysis (PCA):**
- Teknik reduksi dimensi linear yang mempertahankan variansi maksimal
- Mengubah fitur yang berkorelasi menjadi komponen yang saling ortogonal
- Berguna untuk visualisasi data berdimensi tinggi dan mengurangi curse of dimensionality

```
Langkah PCA:
1. Standardisasi data (zero mean, unit variance)
2. Hitung covariance matrix
3. Hitung eigenvectors dan eigenvalues
4. Pilih N komponen utama dengan eigenvalue terbesar
5. Proyeksikan data ke ruang berdimensi lebih rendah
```

---

## 3. ML Pipeline

### 3.1 Konsep Pipeline

ML Pipeline adalah rangkaian langkah preprocessing dan modeling yang dieksekusi secara berurutan. Manfaat utama:
- **Reproducibility:** Transformasi yang konsisten antara training dan inference
- **No data leakage:** Fitting transformer hanya pada training data
- **Kemudahan deployment:** Satu objek yang berisi seluruh alur
- **Grid search yang benar:** Hyperparameter tuning pada seluruh pipeline

### 3.2 Pipeline dengan scikit-learn

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
from sklearn.ensemble import RandomForestClassifier

pipe = Pipeline([
    ('scaler', StandardScaler()),
    ('pca', PCA(n_components=10)),
    ('clf', RandomForestClassifier())
])
```

### 3.3 ColumnTransformer

Untuk dataset dengan tipe kolom campuran (numerik + kategorikal):

```python
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder

preprocessor = ColumnTransformer([
    ('num', StandardScaler(), numerical_features),
    ('cat', OneHotEncoder(), categorical_features)
])
```

---

## 4. Diagram Pipeline ML

```
┌──────────────────────────────────────────────────────────────────┐
│                    ML PIPELINE DIAGRAM                           │
│                                                                  │
│  Raw Data                                                        │
│     │                                                            │
│     ▼                                                            │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  PIPELINE                                                 │   │
│  │                                                           │   │
│  │  ┌─────────────┐   ┌─────────────┐   ┌───────────────┐  │   │
│  │  │  Step 1     │   │  Step 2     │   │  Step 3       │  │   │
│  │  │  Imputer    │──▶│  Scaler/    │──▶│  Feature      │  │   │
│  │  │  (Missing   │   │  Encoder    │   │  Selection/   │  │   │
│  │  │   Values)   │   │  (Numeric+  │   │  PCA          │  │   │
│  │  │             │   │  Categoric) │   │               │  │   │
│  │  └─────────────┘   └─────────────┘   └───────┬───────┘  │   │
│  │                                               │          │   │
│  │                                               ▼          │   │
│  │                                    ┌─────────────────┐   │   │
│  │                                    │  Step 4         │   │   │
│  │                                    │  Estimator      │   │   │
│  │                                    │  (Classifier /  │   │   │
│  │                                    │   Regressor)    │   │   │
│  │                                    └────────┬────────┘   │   │
│  └─────────────────────────────────────────────│────────────┘   │
│                                                │                 │
│                                                ▼                 │
│                                          Predictions             │
└──────────────────────────────────────────────────────────────────┘
```

---

## 5. Hyperparameter Tuning

Hyperparameter adalah parameter yang ditentukan sebelum proses training (bukan dipelajari dari data). Contoh: kedalaman maksimum decision tree, learning rate, jumlah estimator.

### 5.1 Grid Search

Mencoba semua kombinasi hyperparameter dalam grid yang telah ditentukan:

```
param_grid = {
    'n_estimators': [100, 200, 300],
    'max_depth': [5, 10, None],
    'min_samples_split': [2, 5, 10]
}
Total kombinasi = 3 × 3 × 3 = 27
```

- **Kelebihan:** Menyeluruh, menemukan kombinasi optimal dalam grid
- **Kekurangan:** Eksponensial — sangat lambat dengan banyak hyperparameter

### 5.2 Random Search

Mencoba kombinasi hyperparameter secara acak dari distribusi yang ditentukan:

- **Kelebihan:** Lebih efisien — seringkali menemukan solusi baik dengan iterasi lebih sedikit
- **Kekurangan:** Tidak menjamin menemukan kombinasi optimal
- Bergstra & Bengio (2012) membuktikan random search lebih efisien dari grid search

### 5.3 Bayesian Optimization

Menggunakan model probabilistik (Gaussian Process) untuk memodelkan fungsi objektif dan memilih hyperparameter berikutnya secara cerdas berdasarkan hasil sebelumnya:

- Lebih efisien dari random search
- Implementasi: `optuna`, `hyperopt`, `scikit-optimize`

---

## 6. Menangani Class Imbalance

Class imbalance terjadi ketika distribusi kelas dalam dataset sangat tidak merata (misalnya: 95% negatif, 5% positif). Ini umum dalam deteksi fraud, diagnosis penyakit, deteksi anomali.

### 6.1 Masalah yang Ditimbulkan

Model cenderung memprediksi kelas mayoritas saja dan mencapai akurasi tinggi, tetapi gagal mendeteksi kelas minoritas yang seringkali lebih penting.

### 6.2 Teknik Penanganan

**Resampling:**
- **Oversampling:** Memperbanyak sampel kelas minoritas
  - *Random Oversampling:* Duplikasi sampel secara acak
  - *SMOTE (Synthetic Minority Over-sampling Technique):* Membuat sampel sintetis baru berdasarkan interpolasi antara sampel nyata kelas minoritas
- **Undersampling:** Mengurangi sampel kelas mayoritas
  - *Random Undersampling:* Hapus sampel secara acak
  - *Tomek Links:* Hapus sampel mayoritas yang berada di perbatasan kelas

**Algorithmic Approaches:**
- **class_weight parameter:** Berikan bobot lebih tinggi pada kelas minoritas saat training (`class_weight='balanced'`)
- **Threshold adjustment:** Ubah ambang keputusan dari 0.5 ke nilai yang lebih rendah
- **Cost-sensitive learning:** Gunakan cost matrix yang memberi penalti lebih besar pada false negative

---

## 7. Perbandingan Ensemble Methods

| Metode | Cara Kerja | Bias | Variance | Kecepatan Training | Interpretabilitas |
|--------|-----------|------|----------|-------------------|------------------|
| **Bagging (RF)** | Paralel, bootstrap | Mirip base | Lebih rendah | Cepat (paralel) | Sedang |
| **AdaBoost** | Sekuensial, bobot sampel | Lebih rendah | Sedikit lebih tinggi | Sedang | Rendah |
| **Gradient Boosting** | Sekuensial, residual | Rendah | Bisa overfit | Lambat | Rendah |
| **XGBoost** | Sekuensial, optimasi | Rendah | Terkontrol | Cepat (optimasi) | Rendah |
| **Stacking** | Hirarkis, meta-learner | Rendah | Rendah | Lambat | Sangat rendah |

---

## 8. Ringkasan

Minggu ini kita telah mempelajari teknik-teknik Machine Learning tingkat lanjut:

- **Ensemble Methods:** Bagging mengurangi variance; boosting mengurangi bias; stacking menggabungkan kelebihan keduanya
- **Feature Engineering:** Seleksi dan ekstraksi fitur yang tepat sering kali lebih berpengaruh daripada pemilihan algoritma
- **ML Pipeline:** Struktur kode yang bersih, reproducible, dan aman dari data leakage
- **Hyperparameter Tuning:** Grid search untuk ruang kecil, random search untuk ruang besar, Bayesian optimization untuk optimasi lanjutan
- **Class Imbalance:** SMOTE dan class_weight adalah senjata utama untuk menangani distribusi kelas yang tidak seimbang

---

## Referensi

1. Chen, T., & Guestrin, C. (2016). XGBoost: A Scalable Tree Boosting System. *KDD 2016*.
2. Freund, Y., & Schapire, R. E. (1997). A Decision-Theoretic Generalization of On-Line Learning. *JCSS*.
3. Chawla, N. V., et al. (2002). SMOTE: Synthetic Minority Over-sampling Technique. *JAIR*, 16, 321-357.
4. Bergstra, J., & Bengio, Y. (2012). Random Search for Hyper-Parameter Optimization. *JMLR*, 13, 281-305.
5. Géron, A. (2022). *Hands-On Machine Learning* (3rd ed.). O'Reilly Media.
6. Imbalanced-learn Documentation: https://imbalanced-learn.org/stable/
