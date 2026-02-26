# Minggu 1: Pengantar Big Data

## Tujuan Pembelajaran
Setelah mengikuti perkuliahan minggu ini, mahasiswa mampu:
1. Menjelaskan definisi dan karakteristik big data (5V)
2. Mengidentifikasi use case big data di berbagai industri
3. Menjelaskan tantangan dan peluang big data
4. Menyiapkan lingkungan kerja Google Colab

---

## 1.1 Definisi Big Data

**Big Data** adalah kumpulan data yang memiliki volume, kecepatan, dan variasi yang sangat besar sehingga tidak dapat diproses secara efektif menggunakan sistem manajemen database tradisional.

> *"Big data is not about the data. Big data is about the analytics."* — Tom Davenport

### Ukuran Data
| Unit | Ukuran |
|------|--------|
| Kilobyte (KB) | 10³ byte |
| Megabyte (MB) | 10⁶ byte |
| Gigabyte (GB) | 10⁹ byte |
| Terabyte (TB) | 10¹² byte |
| Petabyte (PB) | 10¹⁵ byte |
| Exabyte (EB) | 10¹⁸ byte |
| Zettabyte (ZB) | 10²¹ byte |

Pada tahun 2025, dunia menghasilkan lebih dari **120 zettabyte** data per tahun.

---

## 1.2 Karakteristik Big Data: 5V

### 1. Volume (Ukuran)
- Jumlah data yang dihasilkan sangat besar
- Contoh: Facebook menghasilkan 500+ TB data baru setiap hari
- YouTube menerima 500 jam video per menit

### 2. Velocity (Kecepatan)
- Kecepatan data dihasilkan dan diproses
- Jenis:
  - **Batch processing**: data dikumpulkan dan diproses secara periodik
  - **Stream processing**: data diproses secara real-time
- Contoh: transaksi kartu kredit real-time untuk deteksi fraud

### 3. Variety (Keragaman)
- Jenis/format data yang beragam:
  - **Structured**: database relasional, spreadsheet
  - **Semi-structured**: JSON, XML, CSV
  - **Unstructured**: teks, gambar, audio, video, media sosial

### 4. Veracity (Kebenaran/Kualitas)
- Kualitas dan akurasi data
- Data bisa noise, bias, atau tidak lengkap
- Pentingnya data cleaning dan validasi

### 5. Value (Nilai)
- Nilai bisnis yang dapat diekstrak dari data
- Tidak semua data memiliki nilai yang sama
- Proses: raw data → information → knowledge → wisdom

---

## 1.3 Sejarah Singkat Big Data

```
1970s  → Database Relasional (Edgar Codd, IBM)
1990s  → Data Warehouse, OLAP, Business Intelligence
2003   → Google File System (GFS) Paper
2004   → Google MapReduce Paper
2006   → Apache Hadoop (Doug Cutting & Mike Cafarella)
2008   → Hadoop open source, Apache Project
2009   → Amazon AWS, cloud computing mainstream
2012   → Apache Spark (UC Berkeley AMPLab)
2014   → Apache Kafka, Flink
2015+  → Deep Learning revolution, GPU computing
2017+  → Google Cloud BigQuery, AutoML
2020+  → Real-time analytics, Edge computing
2023+  → LLM & Generative AI dengan Big Data
```

---

## 1.4 Use Case Big Data di Industri

### E-Commerce & Retail
- Sistem rekomendasi produk (Amazon, Tokopedia)
- Analisis perilaku pelanggan
- Optimasi rantai pasokan
- Dynamic pricing

### Kesehatan (Healthcare)
- Analisis rekam medis elektronik
- Drug discovery & genomics
- Prediksi wabah penyakit
- Medical imaging dengan AI

### Keuangan (Finance)
- Deteksi fraud real-time
- Credit scoring
- Algorithmic trading
- Risk management

### Smart City
- Optimasi lalu lintas
- Manajemen energi
- Prediksi kejahatan
- Layanan publik digital

### Media Sosial
- Analisis sentimen
- Trending topics
- Targeted advertising
- Content recommendation

---

## 1.5 Arsitektur Sistem Big Data

```
┌─────────────────────────────────────────────────────────┐
│                    SUMBER DATA                          │
│  IoT Sensors | Social Media | Logs | Transactions       │
└─────────────────────────┬───────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│                  INGESTION LAYER                        │
│         Kafka | Flume | Sqoop | API Gateway             │
└─────────────────────────┬───────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│                   STORAGE LAYER                         │
│      HDFS | S3 | Google Cloud Storage | NoSQL DB        │
└─────────────────────────┬───────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│                 PROCESSING LAYER                        │
│       MapReduce | Spark | Flink | Hive | Presto         │
└─────────────────────────┬───────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│                  ANALYTICS LAYER                        │
│      Machine Learning | BI Tools | Visualization        │
└─────────────────────────────────────────────────────────┘
```

---

## 1.6 Tantangan Big Data

1. **Penyimpanan**: Biaya dan infrastruktur yang dibutuhkan
2. **Keamanan & Privasi**: GDPR, regulasi data
3. **Kualitas Data**: Noise, missing values, inconsistency
4. **Skalabilitas**: Sistem harus mampu scale horizontally
5. **Kompetensi SDM**: Kebutuhan data scientist, data engineer
6. **Integrasi**: Menggabungkan data dari berbagai sumber
7. **Latensi**: Trade-off antara kecepatan dan akurasi

---

## 1.7 Google Colab untuk Big Data

**Google Colaboratory (Colab)** adalah layanan notebook Jupyter gratis dari Google yang berjalan di cloud.

### Keunggulan Google Colab
- ✅ Gratis (dengan GPU/TPU gratis)
- ✅ Tidak perlu instalasi
- ✅ Terintegrasi dengan Google Drive
- ✅ Mendukung Python, R
- ✅ Mudah berbagi dan kolaborasi
- ✅ Akses ke Google Cloud Services

### Cara Akses
1. Buka browser, kunjungi: [colab.research.google.com](https://colab.research.google.com)
2. Login dengan akun Google
3. Klik "New Notebook" atau buka dari Google Drive

---

## Rangkuman

- Big data didefinisikan oleh karakteristik 5V: Volume, Velocity, Variety, Veracity, Value
- Dunia menghasilkan data dalam jumlah yang terus meningkat eksponensial
- Big data memiliki aplikasi luas di berbagai industri
- Arsitektur big data terdiri dari layer: ingestion, storage, processing, analytics
- Google Colab adalah platform yang akan kita gunakan untuk praktikum

---

## Pertanyaan Diskusi
1. Berikan contoh big data dari kehidupan sehari-hari di Indonesia!
2. Menurut Anda, industri mana yang paling diuntungkan oleh big data? Jelaskan!
3. Apa perbedaan antara data scientist, data engineer, dan data analyst?

---

## Referensi
- Marz, N., & Warren, J. (2015). *Big Data*. Manning Publications.
- Laney, D. (2001). 3-D Data Management: Controlling Data Volume, Velocity and Variety. META Group.
- [Google Colab Documentation](https://colab.research.google.com/notebooks/intro.ipynb)
