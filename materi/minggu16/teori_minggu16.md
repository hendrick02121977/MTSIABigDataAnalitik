# Minggu 16: UAS & Proyek Akhir Big Data Analytics

## Tujuan Pembelajaran

Setelah mempelajari materi minggu ini, mahasiswa diharapkan mampu:
1. Merancang dan melaksanakan proyek akhir Big Data Analytics secara mandiri
2. Mengintegrasikan berbagai teknik yang telah dipelajari selama 16 minggu
3. Menyajikan hasil analisis data secara efektif kepada audiens teknis maupun non-teknis
4. Mendokumentasikan metodologi, temuan, dan rekomendasi secara profesional
5. Memahami jalur karir di bidang Big Data Analytics

---

## 1. Panduan Proyek Akhir

### Kriteria Proyek Akhir
Proyek akhir harus memenuhi kriteria berikut:
- **Relevansi**: Menyelesaikan masalah nyata atau simulasi kasus industri yang relevan
- **Kompleksitas**: Mencakup minimal 3 dari 5 tahap pipeline data (ingestion, EDA, preprocessing, modeling, visualization/deployment)
- **Orisinalitas**: Dataset dan analisis merupakan karya sendiri (bukan sekadar mereplikasi tutorial)
- **Dokumentasi**: Laporan tertulis dan notebook yang jelas dan terstruktur
- **Presentasi**: Mampu menjelaskan metodologi dan temuan dalam 10–15 menit

### Komponen Proyek Akhir
1. **Laporan Tertulis** (30–50 halaman): Latar belakang, tinjauan pustaka, metodologi, hasil, diskusi, kesimpulan
2. **Jupyter Notebook**: Kode lengkap dan reproducible dengan narasi yang jelas
3. **Presentasi Slide** (15–20 slide): Untuk dipresentasikan kepada dosen dan rekan
4. **Repository GitHub**: Kode dan dokumentasi yang terorganisir

### Timeline Proyek

| Minggu | Aktivitas |
|---|---|
| 9–10 | Pemilihan topik & proposal (1 halaman) |
| 11–12 | Pengumpulan & eksplorasi data |
| 13–14 | Preprocessing & pemodelan |
| 15 | Evaluasi, visualisasi, deployment simulation |
| 16 | Finalisasi laporan & presentasi UAS |

---

## 2. Rubrik Penilaian UAS

| Komponen | Bobot | Sangat Baik (A: 85–100) | Baik (B: 70–84) | Cukup (C: 55–69) | Kurang (D: <55) |
|---|---|---|---|---|---|
| Business Understanding | 10% | Masalah bisnis terdefinisi jelas, hypotheses relevan | Masalah cukup jelas | Masalah kurang fokus | Tidak ada business framing |
| Data & EDA | 20% | EDA menyeluruh, insights bermakna, visualisasi informatif | EDA memadai | EDA minimal | Tidak ada EDA |
| Preprocessing | 15% | Pipeline lengkap, feature engineering kreatif | Preprocessing standar | Preprocessing minimal | Data tidak diproses |
| Modeling | 20% | Multiple models, tuning, interpretasi hasil | 2 model, evaluasi wajar | 1 model, evaluasi dasar | Tidak ada pemodelan |
| Evaluasi | 15% | Metrik tepat, analisis kesalahan, business implications | Metrik standard | Evaluasi parsial | Evaluasi tidak tepat |
| Presentasi | 10% | Komunikasi jelas, narasi compelling, menjawab pertanyaan | Presentasi baik | Presentasi cukup | Sulit dimengerti |
| Dokumentasi | 10% | Notebook bersih, kode terkomentari, laporan profesional | Dokumentasi baik | Dokumentasi minimal | Tidak terdokumentasi |

---

## 3. Rangkuman Keseluruhan Materi (Minggu 1–15)

| Minggu | Topik | Konsep Kunci |
|---|---|---|
| 1 | Pengantar Big Data | 5V, ekosistem Hadoop, use cases |
| 2 | Hadoop & HDFS | Arsitektur HDFS, MapReduce |
| 3 | Apache Spark Dasar | RDD, transformasi/action, SparkContext |
| 4 | Spark DataFrame & SQL | DataFrame API, Spark SQL, catalyst optimizer |
| 5 | Spark MLlib | Pipeline, feature engineering, ML algorithms |
| 6 | Data Warehousing | OLAP vs OLTP, star schema, Hive |
| 7 | NoSQL Databases | MongoDB, Cassandra, HBase, Redis |
| 8 | Cloud Big Data | AWS EMR, GCP BigQuery, Azure HDInsight |
| 9 | Data Visualization | Matplotlib, Seaborn, Plotly, dashboard |
| 10 | Machine Learning at Scale | Distributed ML, hyperparameter tuning |
| 11 | Graph Analytics | GraphX, PageRank, community detection |
| 12 | Stream Processing | Kafka, Spark Streaming, windowing |
| 13 | Text Analytics & NLP | TF-IDF, Word2Vec, sentiment, LDA |
| 14 | Deep Learning | CNN, RNN/LSTM, Transfer Learning |
| 15 | Studi Kasus Industri | CRISP-DM, end-to-end pipeline, etika |

---

## 4. Contoh Topik Proyek Akhir yang Disarankan

1. **Analisis Sentimen Review Produk E-Commerce Indonesia** — Scraping data Tokopedia/Shopee, preprocessing teks Bahasa Indonesia, klasifikasi sentimen, dashboard
2. **Prediksi Churn Pelanggan Telekomunikasi** — Dataset Telco, feature engineering, multiple ML models, deployment API
3. **Sistem Rekomendasi Film/Musik** — Collaborative filtering, matrix factorization, evaluasi precision/recall
4. **Deteksi Fraud Transaksi Keuangan** — Imbalanced dataset, anomaly detection, interpretable ML
5. **Prediksi Harga Rumah Jakarta/Kota Besar** — Web scraping listing properti, geospatial features, regression analysis
6. **Analisis Tren Twitter/X Indonesia** — Streaming data, topic modeling, social network analysis
7. **Segmentasi Pelanggan Ritel dengan Clustering** — RFM analysis, K-Means, interpretasi bisnis
8. **Prediksi Kualitas Udara Kota** — IoT sensor data, time series forecasting, LSTM
9. **Klasifikasi Berita Hoaks** — NLP, feature engineering, model interpretability
10. **Analisis Dataset Kesehatan Publik (COVID-19)** — Epidemiological analysis, forecasting, geospatial visualization

---

## 5. Checklist Proyek Akhir

### Fase Perencanaan
- [ ] Topik proyek terdefinisi dengan jelas
- [ ] Dataset tersedia dan legal (lisensi sesuai)
- [ ] Masalah bisnis/penelitian terdefinisi
- [ ] Hipotesis awal dirumuskan
- [ ] Tools dan environment disiapkan

### Fase Pengembangan
- [ ] EDA lengkap dengan visualisasi informatif
- [ ] Data cleaning dan preprocessing terdokumentasi
- [ ] Feature engineering dilakukan
- [ ] Minimal 2 algoritma ML dibandingkan
- [ ] Cross-validation diterapkan
- [ ] Hyperparameter tuning dilakukan

### Fase Finalisasi
- [ ] Evaluasi menggunakan metrik yang tepat
- [ ] Analisis feature importance / model interpretability
- [ ] Kesimpulan bisnis dirumuskan
- [ ] Notebook bersih dan reproducible
- [ ] Laporan tertulis selesai
- [ ] Slide presentasi siap
- [ ] Repository GitHub terorganisir

---

## 6. Panduan Penulisan Laporan

### Struktur Laporan
1. **Halaman Judul**: Judul proyek, nama, NIM, program studi, tahun
2. **Abstrak** (200–300 kata): Ringkasan masalah, metode, hasil
3. **BAB I: Pendahuluan**: Latar belakang, rumusan masalah, tujuan, manfaat
4. **BAB II: Tinjauan Pustaka**: Teori relevan, penelitian sebelumnya
5. **BAB III: Metodologi**: Dataset, tools, tahapan analisis (CRISP-DM)
6. **BAB IV: Hasil & Pembahasan**: EDA, modeling, evaluasi, visualisasi
7. **BAB V: Kesimpulan & Saran**: Temuan utama, limitasi, saran pengembangan
8. **Daftar Pustaka**: Format APA atau IEEE
9. **Lampiran**: Kode program (jika tidak terpisah)

### Tips Penulisan
- Gunakan bahasa Indonesia baku yang jelas dan terstruktur
- Setiap gambar dan tabel harus memiliki caption dan nomor
- Kutip sumber dengan benar (hindari plagiarisme)
- Pisahkan hasil (fakta) dari pembahasan (interpretasi)
- Berikan konteks bisnis pada setiap temuan teknis

---

## 7. Tips Presentasi yang Efektif

### Persiapan
- Latih presentasi minimal 3 kali sebelum hari-H
- Setiap slide sebaiknya berisi satu ide utama
- Gunakan visualisasi daripada tabel data mentah
- Persiapkan jawaban untuk pertanyaan yang mungkin diajukan

### Struktur Presentasi (15 Menit)
1. **Hook** (1 menit): Mulai dengan fakta mengejutkan atau masalah yang relatable
2. **Problem Statement** (2 menit): Apa masalahnya? Mengapa penting?
3. **Data & Metodologi** (3 menit): Data apa yang digunakan? Pendekatan apa?
4. **Hasil Utama** (5 menit): 3–5 temuan paling penting
5. **Rekomendasi Bisnis** (2 menit): Apa yang sebaiknya dilakukan?
6. **Kesimpulan & Q&A** (2 menit)

---

## 8. Karir di Big Data Analytics

```
╔══════════════════════════════════════════════════════════════╗
║              JALUR KARIR BIG DATA ANALYTICS                  ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  DATA ENGINEER          DATA SCIENTIST        ML ENGINEER   ║
║  ┌───────────────┐      ┌──────────────┐    ┌────────────┐ ║
║  │ Build pipelines│      │ Analyze data │    │ Deploy ML  │ ║
║  │ Kafka, Spark  │      │ Build models │    │ models in  │ ║
║  │ ETL, Airflow  │      │ Statistics   │    │ production │ ║
║  │ SQL/NoSQL     │      │ ML/DL        │    │ MLOps      │ ║
║  └───────────────┘      └──────────────┘    └────────────┘ ║
║                                                              ║
║  DATA ANALYST           BI DEVELOPER       AI RESEARCHER    ║
║  ┌───────────────┐      ┌──────────────┐    ┌────────────┐ ║
║  │ SQL queries   │      │ Dashboards   │    │ Novel      │ ║
║  │ Visualization │      │ Tableau      │    │ algorithms │ ║
║  │ Business KPIs │      │ Power BI     │    │ Research   │ ║
║  │ Excel/Python  │      │ Looker       │    │ Papers     │ ║
║  └───────────────┘      └──────────────┘    └────────────┘ ║
║                                                              ║
║  SKILL YANG DIBUTUHKAN:                                      ║
║  Programming: Python, Scala, SQL                             ║
║  Big Data: Spark, Kafka, Hadoop                              ║
║  Cloud: AWS/GCP/Azure                                        ║
║  ML/DL: Scikit-learn, TensorFlow/PyTorch                    ║
║  Soft Skills: Komunikasi, problem-solving, business sense    ║
╚══════════════════════════════════════════════════════════════╝
```

---

## Ringkasan

Minggu 16 menandai akhir dari perjalanan pembelajaran Big Data Analytics selama satu semester. Mahasiswa telah menempuh perjalanan dari fondasi Hadoop dan Spark, hingga teknik canggih seperti deep learning dan stream processing. Proyek akhir adalah kesempatan untuk mengintegrasikan semua pengetahuan tersebut dalam sebuah karya nyata yang dapat menjadi portofolio profesional. Kunci sukses adalah: pilih masalah yang menarik, kerjakan secara sistematis mengikuti CRISP-DM, dokumentasikan dengan baik, dan komunikasikan temuan secara jelas. Selamat atas pencapaian Anda — semoga ilmu yang dipelajari bermanfaat dalam karir di era data-driven!

---

## Referensi

1. Provost, F. & Fawcett, T. (2013). *Data Science for Business*. O'Reilly Media.
2. VanderPlas, J. (2016). *Python Data Science Handbook*. O'Reilly Media.
3. Grus, J. (2019). *Data Science from Scratch, 2nd Edition*. O'Reilly Media.
4. Géron, A. (2022). *Hands-On Machine Learning with Scikit-Learn, Keras & TensorFlow, 3rd Edition*. O'Reilly Media.
5. Wirth, R. & Hipp, J. (2000). *CRISP-DM: Towards a Standard Process Model for Data Mining*.
6. Kaggle: https://www.kaggle.com/ (dataset dan kompetisi)
7. Papers With Code: https://paperswithcode.com/ (implementasi riset terbaru)
8. Towards Data Science: https://towardsdatascience.com/
