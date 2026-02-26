# Minggu 15: Studi Kasus Industri & Proyek End-to-End

## Tujuan Pembelajaran

Setelah mempelajari materi minggu ini, mahasiswa diharapkan mampu:
1. Menganalisis penerapan Big Data Analytics di berbagai sektor industri
2. Merancang arsitektur pipeline data end-to-end untuk kasus nyata
3. Menerapkan metodologi CRISP-DM dalam proyek data analytics
4. Memahami pertimbangan etika dalam penggunaan Big Data
5. Mengevaluasi dan memilih teknologi yang tepat untuk berbagai use case

---

## 1. Big Data di E-Commerce

### Recommendation Systems
Platform e-commerce seperti Tokopedia, Shopee, dan Lazada menggunakan Big Data untuk sistem rekomendasi yang meningkatkan konversi penjualan.

**Teknik yang Digunakan:**
- **Collaborative Filtering**: Merekomendasikan berdasarkan pola pengguna serupa
- **Content-Based Filtering**: Merekomendasikan berdasarkan fitur produk yang disukai
- **Hybrid Systems**: Menggabungkan keduanya untuk akurasi lebih baik
- **Deep Learning**: Neural Collaborative Filtering, Sequence-based recommendations

**Impact**: Amazon mengklaim 35% pendapatannya berasal dari sistem rekomendasi.

### Fraud Detection
Setiap transaksi e-commerce dianalisis real-time untuk mendeteksi penipuan:
- Analisis pola transaksi: frekuensi, jumlah, lokasi
- Anomaly detection: transaksi yang sangat berbeda dari profil historis
- Graph analytics: deteksi jaringan akun palsu

### Dynamic Pricing
Big Data memungkinkan penetapan harga dinamis berdasarkan:
- Permintaan real-time dan stok
- Harga kompetitor (web scraping)
- Segmentasi pengguna dan willingness-to-pay
- Musim, event, dan tren

---

## 2. Big Data di Kesehatan (Healthcare)

### Medical Imaging Analysis
AI berbasis deep learning dapat mendiagnosis penyakit dari gambar medis (MRI, CT scan, X-ray) dengan akurasi menyamai atau melampaui dokter spesialis.

**Contoh:**
- Deteksi kanker payudara dari mammogram (Google AI vs radiolog)
- Segmentasi tumor otak dari MRI 3D
- Klasifikasi retinopati diabetik dari foto fundus

### Drug Discovery
Big Data mempercepat penemuan obat yang sebelumnya membutuhkan 10–15 tahun:
- Simulasi molekuler berbasis AI
- Analisis genomik untuk target therapeutik
- Repurposing obat yang sudah ada
- Clinical trial optimization

### Patient Monitoring (IoT Healthcare)
Wearable devices dan sensor IoT menghasilkan stream data kesehatan pasien secara terus-menerus untuk:
- Deteksi dini deteriorasi kondisi pasien
- Prediksi readmission rumah sakit
- Personalized treatment recommendations

---

## 3. Big Data di Keuangan (Finance)

### Risk Management
Lembaga keuangan menggunakan model kuantitatif berbasis Big Data untuk:
- **Credit Risk**: Menilai kemampuan bayar peminjam (credit scoring)
- **Market Risk**: Mengukur risiko portofolio terhadap pergerakan pasar (VaR)
- **Operational Risk**: Mendeteksi anomali dalam proses internal

### Algorithmic Trading
High-Frequency Trading (HFT) mengeksekusi ribuan transaksi per detik berdasarkan:
- Analisis sentimen berita dan social media real-time
- Pattern recognition pada data historis harga
- Arbitrase lintas bursa efek

### Credit Scoring
Model kredit modern melampaui credit bureau tradisional dengan menggunakan:
- Data alternatif: perilaku e-commerce, penggunaan telekomunikasi
- Machine learning: gradient boosting, neural networks
- Fairness constraints untuk mencegah diskriminasi

---

## 4. Big Data di Transportasi

### Route Optimization
Platform seperti Google Maps, Waze, dan Gojek menggunakan Big Data untuk:
- Real-time traffic analysis dari GPS jutaan pengguna
- Prediksi waktu tempuh berbasis machine learning
- Optimasi armada kendaraan

### Demand Prediction (Uber/Gojek)
**Surge Pricing**: Uber dan Gojek memprediksi permintaan di area tertentu berdasarkan:
- Data historis perjalanan
- Event lokal (konser, olahraga)
- Cuaca real-time
- Hari dan jam

Prediksi ini memungkinkan driver diposisikan proaktif di area demand tinggi, mengurangi waktu tunggu.

---

## 5. Big Data di Media Sosial

### Trending Topics Detection
Platform seperti Twitter/X menganalisis jutaan tweet per menit untuk mengidentifikasi topik yang sedang viral menggunakan:
- Spike detection pada keyword frequency
- Clustering topik semantik
- Graph diffusion analysis

### Sentiment Analysis at Scale
Brand monitoring dan social listening menggunakan NLP pada jutaan post untuk memahami persepsi publik terhadap produk, kebijakan, dan event.

### Graph Analytics
Social network graph dengan miliaran node (pengguna) dan edge (koneksi):
- Community detection: mengidentifikasi kelompok pengguna
- Influence analysis: siapa opinion leader?
- Misinformation tracking: bagaimana hoaks menyebar?

---

## 6. Arsitektur End-to-End Data Pipeline

```
╔══════════════════════════════════════════════════════════════════════╗
║              ARSITEKTUR DATA PIPELINE END-TO-END                     ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  DATA SOURCES          INGESTION          STORAGE                   ║
║  ┌─────────────┐      ┌──────────┐      ┌──────────────┐           ║
║  │  Databases  │─────▶│          │─────▶│  Data Lake   │           ║
║  │  APIs       │─────▶│  Kafka   │─────▶│  (S3/HDFS)   │           ║
║  │  IoT/Sensor │─────▶│  Fluentd │─────▶│              │           ║
║  │  Files/Logs │─────▶│          │      └──────┬───────┘           ║
║  └─────────────┘      └──────────┘             │                   ║
║                                                 ▼                   ║
║  PROCESSING                                ┌──────────────┐        ║
║  ┌────────────────────────────────┐        │  Data        │        ║
║  │  Batch: Apache Spark / Hive    │◀───────│  Warehouse   │        ║
║  │  Stream: Spark Streaming/Flink │        │  (Redshift/  │        ║
║  │  ML: Spark MLlib / TensorFlow  │        │   BigQuery)  │        ║
║  └──────────────┬─────────────────┘        └──────────────┘        ║
║                 │                                                   ║
║                 ▼                                                   ║
║  SERVING & ANALYTICS                                                ║
║  ┌───────────────┐  ┌───────────────┐  ┌─────────────────────┐    ║
║  │  BI Dashboard │  │   ML Model    │  │  Data API /         │    ║
║  │  (Tableau/    │  │   Serving     │  │  Microservices      │    ║
║  │   Metabase)   │  │  (MLflow)     │  │                     │    ║
║  └───────────────┘  └───────────────┘  └─────────────────────┘    ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## 7. Metodologi Proyek Data Analytics: CRISP-DM

**CRISP-DM (Cross-Industry Standard Process for Data Mining)** adalah metodologi standar industri yang terdiri dari enam fase berulang:

1. **Business Understanding**: Memahami tujuan bisnis, mendefinisikan masalah analitik, merencanakan proyek
2. **Data Understanding**: Mengumpulkan data awal, mengeksplorasi kualitas dan karakteristik data (EDA)
3. **Data Preparation**: Membersihkan data, feature engineering, transformasi, split train/test
4. **Modeling**: Memilih algoritma, melatih model, hyperparameter tuning
5. **Evaluation**: Mengevaluasi model terhadap tujuan bisnis, review proses
6. **Deployment**: Mendeploy model ke produksi, monitoring, maintenance

CRISP-DM bersifat iteratif — tim sering kembali ke fase sebelumnya saat menemukan insight baru.

---

## 8. Etika dalam Big Data Analytics

### Privasi Data
- **GDPR** (General Data Protection Regulation): Regulasi privasi data Eropa yang memberikan hak kepada individu atas data pribadinya
- **Data Minimization**: Hanya kumpulkan data yang benar-benar diperlukan
- **Anonymization & Pseudonymization**: Melindungi identitas individu dalam dataset
- Di Indonesia: **UU Perlindungan Data Pribadi (UU PDP)** No. 27 Tahun 2022

### Bias dalam Model AI
Model ML dapat mewarisi bias dari data training, menghasilkan prediksi yang diskriminatif:
- **Historical Bias**: Pola diskriminatif historis terefleksi dalam data
- **Sampling Bias**: Data tidak representatif terhadap populasi yang dilayani
- **Algorithmic Bias**: Model memperkuat ketidakadilan yang ada

**Solusi**: Fairness-aware ML, audit model, diverse training data, explainable AI.

### Transparansi & Explainability
Model yang digunakan untuk pengambilan keputusan penting (kredit, rekrutmen, medis) harus dapat dijelaskan. Tools: SHAP (SHapley Additive exPlanations), LIME.

---

## Ringkasan

Big Data Analytics telah mengubah cara berbagai industri beroperasi dan membuat keputusan. Dari rekomendasi produk e-commerce, deteksi penyakit dari gambar medis, hingga prediksi demand transportasi — semua bergantung pada kemampuan mengumpulkan, memproses, dan menganalisis data dalam skala besar. CRISP-DM menyediakan kerangka metodologis yang terbukti untuk menjalankan proyek data secara sistematis. Namun, penggunaan Big Data juga membawa tanggung jawab etis yang besar terkait privasi, bias, dan fairness. Seorang data professional yang baik harus menguasai tidak hanya teknik, tetapi juga pertimbangan etis.

---

## Referensi

1. Mayer-Schönberger, V. & Cukier, K. (2013). *Big Data: A Revolution That Will Transform How We Live, Work, and Think*. Houghton Mifflin Harcourt.
2. Provost, F. & Fawcett, T. (2013). *Data Science for Business*. O'Reilly Media.
3. Wirth, R. & Hipp, J. (2000). *CRISP-DM: Towards a Standard Process Model for Data Mining*. KDD.
4. Barocas, S., Hardt, M., & Narayanan, A. (2023). *Fairness and Machine Learning*. MIT Press.
5. O'Neil, C. (2016). *Weapons of Math Destruction*. Crown Publishers.
6. UU Perlindungan Data Pribadi: https://peraturan.bpk.go.id/
7. GDPR: https://gdpr.eu/
