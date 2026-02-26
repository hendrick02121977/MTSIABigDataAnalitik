# Minggu 9: Machine Learning untuk Big Data

## Tujuan Pembelajaran

Setelah mempelajari materi minggu ini, mahasiswa diharapkan mampu:
1. Memahami konsep dasar Machine Learning dan kategorinya
2. Mengimplementasikan algoritma regresi untuk prediksi nilai kontinu
3. Mengimplementasikan algoritma klasifikasi dan mengevaluasi performanya
4. Menerapkan algoritma clustering untuk menemukan pola tersembunyi dalam data
5. Memahami konsep overfitting, underfitting, dan cara mengatasinya
6. Menggunakan cross-validation untuk evaluasi model yang lebih andal

---

## 1. Pengantar Machine Learning

Machine Learning (ML) adalah cabang dari kecerdasan buatan (Artificial Intelligence) yang memungkinkan komputer untuk belajar dari data tanpa diprogram secara eksplisit. Dalam konteks Big Data, ML sangat penting karena mampu mengekstrak pengetahuan dari volume data yang sangat besar secara otomatis.

### 1.1 Kategori Machine Learning

#### Supervised Learning (Pembelajaran Terarah)
Dalam supervised learning, model dilatih menggunakan data berlabel — setiap sampel memiliki input (fitur) dan output (label) yang diketahui. Model belajar memetakan input ke output sehingga dapat membuat prediksi pada data baru yang belum pernah dilihat sebelumnya.

- **Contoh tugas:** Prediksi harga rumah, deteksi spam email, klasifikasi gambar
- **Algoritma populer:** Linear Regression, Decision Tree, Random Forest, SVM, Neural Network

#### Unsupervised Learning (Pembelajaran Tidak Terarah)
Dalam unsupervised learning, model bekerja dengan data yang tidak memiliki label. Tujuannya adalah menemukan struktur atau pola tersembunyi dalam data.

- **Contoh tugas:** Segmentasi pelanggan, deteksi anomali, kompresi data
- **Algoritma populer:** K-Means, DBSCAN, PCA, Autoencoders

#### Reinforcement Learning (Pembelajaran Penguatan)
Agen belajar berinteraksi dengan lingkungan, mengambil tindakan, dan menerima reward atau penalty. Tujuannya adalah memaksimalkan reward kumulatif jangka panjang.

- **Contoh tugas:** Permainan catur, kontrol robot, optimasi iklan
- **Algoritma populer:** Q-Learning, Deep Q-Network (DQN), PPO

---

## 2. Regresi

Regresi digunakan ketika variabel target bersifat kontinu (numerik). Tujuannya adalah memprediksi nilai numerik berdasarkan fitur-fitur input.

### 2.1 Regresi Linear

Regresi linear memodelkan hubungan antara variabel independen (X) dan variabel dependen (y) sebagai fungsi linear:

```
ŷ = β₀ + β₁x₁ + β₂x₂ + ... + βₙxₙ
```

- **β₀** adalah intercept (bias)
- **β₁...βₙ** adalah koefisien (weights) untuk setiap fitur
- Model dilatih dengan meminimalkan **Sum of Squared Errors (SSE)**

### 2.2 Regresi Polinomial

Ketika hubungan antara X dan y bersifat non-linear, regresi polinomial dapat digunakan dengan menambahkan pangkat dari fitur:

```
ŷ = β₀ + β₁x + β₂x² + β₃x³ + ...
```

### 2.3 Metrik Evaluasi Regresi

| Metrik | Formula | Interpretasi |
|--------|---------|-------------|
| **MSE** (Mean Squared Error) | MSE = (1/n) Σ(yᵢ - ŷᵢ)² | Rata-rata kuadrat error; sensitif terhadap outlier |
| **RMSE** (Root MSE) | RMSE = √MSE | Satuan sama dengan target; lebih interpretatif |
| **MAE** (Mean Absolute Error) | MAE = (1/n) Σ\|yᵢ - ŷᵢ\| | Lebih robust terhadap outlier |
| **R²** (R-squared) | R² = 1 - SS_res/SS_tot | Proporsi variansi yang dijelaskan model (0-1) |

---

## 3. Klasifikasi

Klasifikasi digunakan ketika variabel target bersifat kategorikal (kelas). Tujuannya adalah memprediksi kelas dari sampel input.

### 3.1 Logistic Regression

Meskipun namanya "regression", logistic regression digunakan untuk klasifikasi biner. Fungsi sigmoid mengubah output linear menjadi probabilitas:

```
P(y=1|X) = 1 / (1 + e^(-z))   di mana z = β₀ + β₁x₁ + ... + βₙxₙ
```

### 3.2 Decision Tree

Decision tree membangun model berbentuk pohon keputusan dengan membagi data berdasarkan fitur yang paling informatif. Kriteria pembagian yang umum digunakan:
- **Gini Impurity:** Mengukur kemurnian node
- **Information Gain (Entropy):** Mengukur penurunan ketidakpastian

### 3.3 Random Forest

Random Forest adalah ensemble method yang membangun banyak decision tree (forest) dan menggabungkan prediksinya melalui voting. Keunggulan:
- Mengurangi overfitting dibanding single decision tree
- Robust terhadap noise dan outlier
- Dapat mengestimasi feature importance

### 3.4 Support Vector Machine (SVM)

SVM mencari hyperplane optimal yang memisahkan kelas-kelas dengan margin maksimum. Dengan kernel trick (linear, RBF, polynomial), SVM dapat menangani data non-linear.

### 3.5 Metrik Evaluasi Klasifikasi

**Confusion Matrix:**
```
                  Predicted Positive    Predicted Negative
Actual Positive        TP                    FN
Actual Negative        FP                    TN
```

| Metrik | Formula | Interpretasi |
|--------|---------|-------------|
| **Accuracy** | (TP+TN)/(TP+TN+FP+FN) | Proporsi prediksi yang benar |
| **Precision** | TP/(TP+FP) | Dari yang diprediksi positif, berapa yang benar? |
| **Recall** | TP/(TP+FN) | Dari yang benar positif, berapa yang terdeteksi? |
| **F1-Score** | 2×(P×R)/(P+R) | Harmonic mean of Precision & Recall |
| **ROC-AUC** | Area under ROC curve | Kemampuan diskriminasi model (0.5-1.0) |

---

## 4. Clustering

Clustering adalah teknik unsupervised learning untuk mengelompokkan data berdasarkan kemiripan tanpa menggunakan label.

### 4.1 K-Means Clustering

K-Means membagi data menjadi K cluster dengan meminimalkan Within-Cluster Sum of Squares (WCSS):

**Algoritma:**
1. Inisialisasi K centroid secara acak
2. Assign setiap titik ke centroid terdekat
3. Update centroid sebagai rata-rata anggota cluster
4. Ulangi langkah 2-3 hingga konvergen

**Pemilihan K:** Gunakan **Elbow Method** atau **Silhouette Score**

### 4.2 DBSCAN (Density-Based Spatial Clustering)

DBSCAN mengelompokkan titik-titik yang berada di area dengan kepadatan tinggi dan menandai titik di area kepadatan rendah sebagai outlier.

- **ε (epsilon):** Radius neighborhood
- **MinPts:** Jumlah minimum titik dalam radius ε untuk membentuk dense region
- **Keunggulan:** Dapat menemukan cluster berbentuk arbitrer; tidak perlu menentukan K

### 4.3 Hierarchical Clustering

Membangun hierarki cluster dalam bentuk dendrogram:
- **Agglomerative (bottom-up):** Mulai dari setiap titik sebagai cluster tunggal, gabungkan secara iteratif
- **Divisive (top-down):** Mulai dari satu cluster besar, pecah secara iteratif

**Linkage criteria:** single, complete, average, ward

### 4.4 Evaluasi Clustering

**Silhouette Score:**
```
s(i) = (b(i) - a(i)) / max(a(i), b(i))
```
- **a(i):** Rata-rata jarak ke anggota cluster yang sama
- **b(i):** Rata-rata jarak ke anggota cluster terdekat lainnya
- Nilai: -1 (buruk) hingga +1 (sempurna)

---

## 5. Overfitting, Underfitting & Cross-Validation

### 5.1 Bias-Variance Tradeoff

```
Total Error = Bias² + Variance + Irreducible Noise
```

| Kondisi | Bias | Variance | Solusi |
|---------|------|----------|--------|
| **Underfitting** | Tinggi | Rendah | Tambah kompleksitas model, lebih banyak fitur |
| **Overfitting** | Rendah | Tinggi | Regularisasi, lebih banyak data, early stopping |
| **Good Fit** | Seimbang | Seimbang | - |

### 5.2 Cross-Validation

Cross-validation memungkinkan evaluasi model yang lebih andal dengan memanfaatkan seluruh data:

**K-Fold Cross-Validation:**
1. Bagi data menjadi K bagian (fold) yang sama besar
2. Untuk setiap iterasi: gunakan 1 fold sebagai test set, K-1 fold sebagai training set
3. Hitung metrik evaluasi rata-rata dari K iterasi

**Stratified K-Fold:** Mempertahankan proporsi kelas di setiap fold — penting untuk dataset tidak seimbang.

---

## 6. Alur Kerja Machine Learning

```
┌─────────────────────────────────────────────────────────────────┐
│                   ML WORKFLOW DIAGRAM                           │
│                                                                 │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐                  │
│  │   DATA   │───▶│FEATURES  │───▶│  MODEL   │                  │
│  │          │    │          │    │          │                  │
│  │ Raw data │    │ Extract  │    │ Train &  │                  │
│  │ Collect  │    │ Select   │    │ Optimize │                  │
│  │ Clean    │    │ Engineer │    │          │                  │
│  └──────────┘    └──────────┘    └────┬─────┘                  │
│                                       │                         │
│                                       ▼                         │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐                  │
│  │ DEPLOY   │◀───│EVALUATION│◀───│VALIDATION│                  │
│  │          │    │          │    │          │                  │
│  │ Serve    │    │ Metrics  │    │ Test Set │                  │
│  │ Monitor  │    │ Compare  │    │ Cross-Val│                  │
│  │ Retrain  │    │ Select   │    │          │                  │
│  └──────────┘    └──────────┘    └──────────┘                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 7. Perbandingan Algoritma Machine Learning

| Algoritma | Tipe | Kelebihan | Kekurangan | Cocok Untuk |
|-----------|------|-----------|------------|-------------|
| Linear Regression | Regresi | Sederhana, interpretatif | Asumsi linearitas | Data linear, baseline |
| Logistic Regression | Klasifikasi | Probabilistik, cepat | Hubungan linear | Klasifikasi biner sederhana |
| Decision Tree | Klasifikasi/Regresi | Interpretatif, non-linear | Mudah overfit | Data tabular, kebutuhan interpretasi |
| Random Forest | Klasifikasi/Regresi | Akurat, robust | Kurang interpretatif | Data tabular umum |
| SVM | Klasifikasi/Regresi | Efektif dimensi tinggi | Lambat untuk data besar | Data dimensi tinggi |
| K-Means | Clustering | Sederhana, skalabel | Perlu menentukan K | Segmentasi, eksplorasi |
| DBSCAN | Clustering | Tidak perlu K, deteksi outlier | Sensitif parameter | Data dengan noise, bentuk bebas |

---

## 8. Ringkasan

Machine Learning adalah alat yang sangat kuat untuk menganalisis Big Data. Minggu ini kita telah membahas:

- **Supervised Learning** (regresi & klasifikasi) untuk prediksi dengan data berlabel
- **Unsupervised Learning** (clustering) untuk menemukan pola tersembunyi
- **Metrik evaluasi** yang tepat untuk setiap jenis tugas
- **Overfitting/underfitting** dan cara mengatasinya dengan cross-validation
- **Alur kerja ML** dari data mentah hingga deployment

Pemilihan algoritma yang tepat bergantung pada karakteristik data, ukuran dataset, kebutuhan interpretabilitas, dan batasan komputasi.

---

## Referensi

1. Géron, A. (2022). *Hands-On Machine Learning with Scikit-Learn, Keras & TensorFlow* (3rd ed.). O'Reilly Media.
2. Bishop, C. M. (2006). *Pattern Recognition and Machine Learning*. Springer.
3. Scikit-learn Documentation: https://scikit-learn.org/stable/documentation.html
4. Hastie, T., Tibshirani, R., & Friedman, J. (2009). *The Elements of Statistical Learning* (2nd ed.). Springer.
5. Raschka, S., & Mirjalili, V. (2022). *Python Machine Learning* (3rd ed.). Packt Publishing.
