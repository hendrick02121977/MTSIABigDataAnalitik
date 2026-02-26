# Teori Minggu 8: Review UTS – Komprehensif Big Data Analytics

## Tujuan Pembelajaran

Setelah mempelajari materi minggu ini, mahasiswa diharapkan mampu:
1. Mengingat kembali dan mengintegrasikan konsep dari minggu 1 hingga 7
2. Mengidentifikasi topik-topik kunci yang akan diujikan dalam UTS
3. Menjawab contoh soal UTS dengan tepat dan terstruktur
4. Menyusun strategi belajar yang efektif untuk menghadapi UTS

---

## Mind Map Topik Big Data Analytics (Minggu 1-7)

```
                    BIG DATA ANALYTICS
                          |
      +-------------------+-------------------+
      |                   |                   |
  FONDASI             TEKNIK               INFRASTRUKTUR
      |                   |                   |
  +---+---+           +---+---+           +---+---+
  |       |           |       |           |       |
 5V's  Hadoop       Spark   Pandas      NoSQL  Cloud
  |       |           |       |           |       |
 Vol   HDFS+MapR    RDD    Cleaning    Mongo   S3/GCS
 Vel   YARN+Hive   DF+SQL   EDA        Redis   BigQuery
 Var   Spark Eco  Stream  Visualize  Cassandra DataLake
 Ver                                   CAP    Lakehouse
 Val
```

---

## 1. Ringkasan Minggu 1-2: Pengantar Big Data

### Definisi Big Data
Big Data adalah kumpulan data yang memiliki karakteristik **5V**:

| V | Nama | Deskripsi | Contoh |
|---|------|-----------|--------|
| **V1** | **Volume** | Ukuran data yang sangat besar | Petabyte, Exabyte |
| **V2** | **Velocity** | Kecepatan data dihasilkan dan diproses | Data real-time, streaming |
| **V3** | **Variety** | Keberagaman format/jenis data | Teks, gambar, video, IoT |
| **V4** | **Veracity** | Kualitas dan kepercayaan data | Data tidak bersih, noise |
| **V5** | **Value** | Nilai bisnis yang dapat diekstrak | Insight, prediksi, keputusan |

### Ekosistem Hadoop
- **HDFS (Hadoop Distributed File System)**: penyimpanan terdistribusi, toleran terhadap kesalahan
- **YARN (Yet Another Resource Negotiator)**: manajemen resource dan penjadwalan job
- **MapReduce**: model pemrograman untuk pemrosesan terdistribusi
- **Ekosistem**: Hive (SQL di Hadoop), HBase (NoSQL), Pig (scripting), Sqoop (transfer RDBMS)

**Arsitektur HDFS**: 1 NameNode (metadata) + banyak DataNode (data actual)
- Faktor replikasi default: 3 (data disimpan di 3 node berbeda)
- Block size default: 128 MB

---

## 2. Ringkasan Minggu 3: Apache Spark

### Mengapa Spark Lebih Cepat dari MapReduce?
- **In-memory processing**: data diproses di RAM, bukan ditulis ke disk setiap tahap
- **DAG execution engine**: optimasi alur eksekusi secara otomatis
- **Lazy evaluation**: transformasi tidak dieksekusi sampai ada action

### Komponen Utama Spark
```
+-----------------------------------------------------+
|              Apache Spark Ecosystem                 |
|  +----------+  +---------+  +-------+  +--------+  |
|  | Spark SQL|  |Spark ML |  |GraphX |  |Streaming|  |
|  +----------+  +---------+  +-------+  +--------+  |
|              Spark Core (RDD, DAG, Scheduler)       |
+-----------------------------------------------------+
|    Standalone | YARN | Mesos | Kubernetes           |
+-----------------------------------------------------+
```

### RDD vs DataFrame vs Dataset

| Aspek | RDD | DataFrame | Dataset |
|-------|-----|-----------|---------|
| **Tipe** | Tidak terstruktur | Terstruktur | Terstruktur + Type-safe |
| **Optimasi** | Manual | Catalyst optimizer | Catalyst optimizer |
| **API** | Fungsi tingkat rendah | SQL-like, kolom | Typed API |
| **Bahasa** | Java, Scala, Python | Semua | Java, Scala |
| **Kapan** | Kontrol penuh | Analitik umum | Production code |

### Operasi RDD Penting
- **Transformations** (lazy): `map()`, `filter()`, `flatMap()`, `groupByKey()`, `reduceByKey()`
- **Actions** (eager): `collect()`, `count()`, `take(n)`, `reduce()`, `saveAsTextFile()`

---

## 3. Ringkasan Minggu 4: Pengumpulan Data

### Web Scraping
- **BeautifulSoup**: parsing HTML/XML statis
- **Scrapy**: framework scraping skala besar
- **Selenium/Playwright**: scraping halaman dinamis (JavaScript)

### API (Application Programming Interface)
- **REST API**: HTTP methods (GET, POST, PUT, DELETE), format JSON/XML
- **Rate limiting**: pembatasan jumlah request per waktu
- **Authentication**: API Key, OAuth 2.0, JWT Token

### Etika Pengumpulan Data
- Periksa `robots.txt` sebelum scraping
- Hormati Terms of Service website
- Hindari overloading server (gunakan delay)
- Perhatikan hak cipta dan privasi data

---

## 4. Ringkasan Minggu 5: Praproses Data

### Pipeline Praproses Data

```
Raw Data → [Missing Values] → [Outliers] → [Encoding] → [Scaling] → Clean Data
```

### Penanganan Missing Values

| Metode | Kapan Digunakan |
|--------|-----------------|
| **Drop rows** | Missing < 5%, data cukup banyak |
| **Mean imputation** | Data numerik, distribusi normal |
| **Median imputation** | Data numerik, ada outlier |
| **Mode imputation** | Data kategoris |
| **KNN imputation** | Missing kompleks, hubungan antar fitur |
| **Model-based** | Missing > 30%, butuh akurasi tinggi |

### Encoding Kategoris
- **Label Encoding**: ordinal → angka berurutan (Rendah=0, Sedang=1, Tinggi=2)
- **One-Hot Encoding**: nominal → kolom biner (Kota_Jakarta, Kota_Bandung, ...)
- **Target Encoding**: rata-rata target per kategori (hati-hati overfitting)

### Feature Scaling
- **Min-Max Normalization**: x' = (x - min) / (max - min) → range [0, 1]
- **Standardization (Z-score)**: x' = (x - μ) / σ → mean=0, std=1
- **Robust Scaler**: x' = (x - median) / IQR → robust terhadap outlier

---

## 5. Ringkasan Minggu 6: EDA

### Alur EDA
1. **Inspeksi awal**: `shape`, `dtypes`, `head()`, `info()`
2. **Statistik deskriptif**: `describe()`, `skew()`, `kurtosis()`
3. **Distribusi univariat**: histogram, KDE, box plot
4. **Analisis bivariat**: scatter plot, korelasi
5. **Analisis multivariat**: pair plot, heatmap korelasi
6. **Deteksi outlier**: IQR method, Z-score

### Metode Korelasi
- **Pearson**: data numerik, hubungan linear, asumsi normalitas
- **Spearman**: data ordinal atau non-linear, robust terhadap outlier
- **Kendall**: sampel kecil, lebih robust dari Spearman

### Deteksi Outlier
```
IQR = Q3 - Q1
Lower fence = Q1 - 1.5 × IQR
Upper fence = Q3 + 1.5 × IQR
```

---

## 6. Ringkasan Minggu 7: Penyimpanan Big Data

### NoSQL Database Types
| Tipe | Contoh | Use Case |
|------|--------|----------|
| **Document** | MongoDB | CMS, katalog, profil |
| **Key-Value** | Redis | Cache, session, queue |
| **Column-Family** | Cassandra | IoT, time-series |
| **Graph** | Neo4j | Social network, rekomendasi |

### CAP Theorem
- **C**onsistency: semua node lihat data sama
- **A**vailability: selalu dapat menjawab request
- **P**artition tolerance: tetap berfungsi saat partisi jaringan
- **Hanya 2 dari 3** yang bisa dijamin sekaligus

### Data Lake Zones
- **Bronze (Raw)**: data mentah as-is
- **Silver (Processed)**: dibersihkan dan divalidasi
- **Gold (Curated)**: diagregasi, siap untuk BI/ML

---

## 7. Kisi-Kisi UTS

### Topik yang Sangat Mungkin Keluar (High Priority)
1. **Definisi dan karakteristik 5V Big Data** — hafalkan dengan contoh nyata
2. **Perbedaan RDD vs DataFrame di Spark** — tabel perbandingan
3. **Metode penanganan missing values** — beserta kondisi penggunaannya
4. **Statistik deskriptif**: mean, median, std, skewness — interpretasi nilai
5. **IQR method** untuk deteksi outlier — bisa menghitung manual
6. **Perbedaan SQL vs NoSQL** — aspek skema, skalabilitas, konsistensi
7. **CAP Theorem** — pilihan CP vs AP dengan contoh

### Topik Prioritas Menengah
8. Arsitektur HDFS dan cara kerja replikasi
9. Lazy evaluation di Spark
10. Encoding kategoris (Label vs One-Hot)
11. Data Lake vs Data Warehouse
12. Metode korelasi Pearson vs Spearman

### Topik Tambahan
13. REST API dan format data (JSON)
14. Web scraping dengan BeautifulSoup
15. Format file Big Data (Parquet vs CSV)

---

## 8. Contoh Soal UTS dan Jawaban

### Soal 1 (Multiple Choice)
**Sebuah perusahaan e-commerce memiliki data transaksi sebesar 50 TB yang terus bertambah 1 TB per hari. Data ini mencakup log klik pengguna, data produk, dan foto. Karakteristik Big Data mana yang PALING dominan pada kasus ini?**

a) Hanya Volume  
b) Volume dan Velocity  
c) Volume, Velocity, dan Variety ✅  
d) Hanya Velocity

**Jawaban: C** — Volume (50TB besar), Velocity (1TB/hari, terus bertambah), Variety (log, data produk, foto = terstruktur + tidak terstruktur)

---

### Soal 2 (Short Answer)
**Jelaskan perbedaan antara Transformation dan Action pada RDD Apache Spark. Berikan 2 contoh masing-masing.**

**Jawaban:**
- **Transformation** adalah operasi yang menghasilkan RDD baru dan bersifat *lazy* (tidak langsung dieksekusi). Contoh: `map()` (mengubah setiap elemen), `filter()` (menyaring elemen sesuai kondisi)
- **Action** adalah operasi yang memicu eksekusi dan menghasilkan nilai atau efek samping. Contoh: `collect()` (mengambil semua data ke driver), `count()` (menghitung jumlah elemen)

Spark menunda eksekusi sampai ada Action dipanggil, sehingga dapat mengoptimasi keseluruhan pipeline melalui DAG Scheduler.

---

### Soal 3 (Calculation)
**Data berikut adalah nilai ujian 7 mahasiswa: [72, 68, 85, 90, 73, 78, 95]. Hitung Q1, Q3, IQR, dan tentukan apakah nilai 95 adalah outlier!**

**Jawaban:**
Data diurutkan: [68, 72, 73, 78, 85, 90, 95]  
- Q1 = 72 (nilai ke-2, median bagian bawah)
- Q3 = 90 (nilai ke-6, median bagian atas)
- IQR = 90 - 72 = **18**
- Batas bawah = 72 - 1.5×18 = 72 - 27 = **45**
- Batas atas = 90 + 1.5×18 = 90 + 27 = **117**
- 95 berada dalam rentang [45, 117] → **BUKAN outlier**

---

### Soal 4 (Essay)
**Kapan sebaiknya menggunakan database NoSQL dibandingkan RDBMS tradisional? Jelaskan dengan contoh kasus nyata.**

**Jawaban:**
NoSQL lebih cocok digunakan ketika:
1. **Skema tidak tetap atau sering berubah**: misalnya, setiap produk di marketplace bisa memiliki atribut berbeda (buku punya ISBN, elektronik punya wattage) → cocok untuk Document Store (MongoDB)
2. **Kebutuhan skalabilitas horizontal sangat tinggi**: aplikasi media sosial dengan jutaan pengguna aktif perlu menambah server dengan mudah → Cassandra mendukung ini
3. **Performa baca/tulis sangat tinggi dengan latensi rendah**: sistem caching untuk e-commerce agar halaman produk cepat diakses → Redis
4. **Data berbentuk jaringan/relasi kompleks**: sistem rekomendasi "teman dari teman" di platform profesional → Neo4j

Sebaliknya, RDBMS tetap lebih baik untuk transaksi keuangan yang membutuhkan konsistensi ACID (transfer bank, sistem akunting).

---

### Soal 5 (Analisis)
**Sebuah dataset memiliki distribusi dengan mean=850.000, median=600.000, dan skewness=2.3. Apa yang dapat Anda simpulkan tentang data ini? Tindakan praproses apa yang disarankan?**

**Jawaban:**
- **Mean >> Median**: mengindikasikan distribusi right-skewed (miring ke kanan)
- **Skewness = 2.3 > 1**: skewness sangat tinggi, distribusi sangat tidak simetris
- Data kemungkinan adalah data penghasilan atau harga aset — beberapa nilai ekstrem sangat besar menarik mean ke atas

**Tindakan yang disarankan:**
1. **Log transformation**: mengubah x → log(x) untuk mengurangi skewness, sering efektif untuk data penghasilan/harga
2. **Identifikasi outlier**: cek apakah ada kesalahan input atau nilai valid yang sangat ekstrem (misal miliarder vs rata-rata orang)
3. **Pilih median sebagai representasi sentral** (bukan mean) dalam pelaporan karena lebih robust
4. **Pertimbangkan normalisasi robust** (RobustScaler yang menggunakan IQR) jika data akan digunakan untuk ML

---

## 9. Tips Belajar untuk UTS

### Strategi Belajar Efektif
1. **Active Recall**: coba jawab soal tanpa melihat catatan terlebih dahulu
2. **Spaced Repetition**: review materi setiap hari dengan interval yang meningkat
3. **Feynman Technique**: jelaskan konsep seolah mengajari orang lain
4. **Mind Mapping**: buat peta konsep untuk menghubungkan semua topik
5. **Practice Problems**: kerjakan soal-soal latihan sebanyak mungkin

### Checklist Persiapan UTS

- [ ] Hafal dan pahami 5V Big Data dengan contoh
- [ ] Bisa menjelaskan arsitektur HDFS dan peran NameNode/DataNode
- [ ] Pahami perbedaan Transformation vs Action di Spark
- [ ] Bisa menghitung statistik deskriptif secara manual
- [ ] Memahami IQR method dan bisa menerapkannya
- [ ] Hafal perbedaan SQL vs NoSQL (minimal 5 aspek)
- [ ] Pahami CAP Theorem dengan contoh sistem nyata
- [ ] Mengerti kapan menggunakan encoding apa
- [ ] Bisa membedakan Data Lake vs Data Warehouse

---

## Referensi

1. White, T. (2015). *Hadoop: The Definitive Guide*, 4th Edition. O'Reilly.
2. Karau, H., et al. (2015). *Learning Spark*. O'Reilly Media.
3. McKinney, W. (2022). *Python for Data Analysis*, 3rd Ed. O'Reilly.
4. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly.
5. VanderPlas, J. (2016). *Python Data Science Handbook*. O'Reilly.
6. Grus, J. (2019). *Data Science from Scratch*, 2nd Edition. O'Reilly.
