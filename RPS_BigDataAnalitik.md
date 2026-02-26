# RENCANA PEMBELAJARAN SEMESTER (RPS)
## Program Magister Teknologi Informasi

---

| **Mata Kuliah**        | Big Data Analitik                              |
|------------------------|------------------------------------------------|
| **Kode Mata Kuliah**   | MK-BDA-501                                     |
| **Bobot SKS**          | 3 SKS (2 SKS Teori + 1 SKS Praktikum)         |
| **Semester**           | Ganjil/Genap                                   |
| **Program Studi**      | Magister Teknologi Sistem Informasi & Analitik |
| **Prasyarat**          | Statistik Dasar, Pemrograman Python Dasar      |
| **Platform Praktikum** | Google Colab (colab.research.google.com)       |
| **Dosen Pengampu**     |                                                |
| **Tahun Akademik**     | 2024/2025                                      |

---

## A. DESKRIPSI MATA KULIAH

Mata kuliah Big Data Analitik membekali mahasiswa dengan pemahaman teori dan kemampuan praktis dalam mengelola, memproses, dan menganalisis data berukuran besar (big data). Mahasiswa akan mempelajari ekosistem big data, teknik pengolahan data, machine learning skala besar, serta visualisasi data menggunakan tools modern berbasis cloud, khususnya Google Colab. Mahasiswa juga akan mengerjakan proyek akhir berbasis studi kasus nyata.

---

## B. CAPAIAN PEMBELAJARAN MATA KULIAH (CPMK)

Setelah menyelesaikan mata kuliah ini, mahasiswa diharapkan mampu:

1. **CPMK-1**: Memahami konsep, karakteristik, dan tantangan big data (5V: Volume, Velocity, Variety, Veracity, Value)
2. **CPMK-2**: Mengoperasikan ekosistem big data (Hadoop, Spark, Google Cloud)
3. **CPMK-3**: Mengumpulkan, membersihkan, dan melakukan eksplorasi data besar
4. **CPMK-4**: Mengimplementasikan algoritma machine learning dan deep learning pada big data
5. **CPMK-5**: Membangun pipeline analitik big data end-to-end menggunakan Google Colab
6. **CPMK-6**: Mengkomunikasikan hasil analisis data melalui visualisasi dan laporan ilmiah

---

## C. METODE PEMBELAJARAN

- **Teori**: Kuliah tatap muka / daring, diskusi, presentasi
- **Praktikum**: Hands-on coding menggunakan Google Colab
- **Tugas**: Kuis mingguan, tugas besar, proyek akhir
- **Referensi**: Jurnal ilmiah, textbook, dokumentasi resmi

---

## D. PENILAIAN

| Komponen              | Bobot |
|-----------------------|-------|
| Kehadiran & Partisipasi | 10% |
| Tugas Mingguan / Kuis | 20%   |
| Ujian Tengah Semester (UTS) | 25% |
| Ujian Akhir Semester (UAS)  | 25% |
| Proyek Akhir          | 20%   |
| **Total**             | **100%** |

---

## E. REFERENSI UTAMA

1. Marz, N., & Warren, J. (2015). *Big Data: Principles and Best Practices of Scalable Realtime Data Systems*. Manning.
2. Karau, H., Konwinski, A., Wendell, P., & Zaharia, M. (2015). *Learning Spark*. O'Reilly.
3. Géron, A. (2022). *Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow* (3rd ed.). O'Reilly.
4. White, T. (2015). *Hadoop: The Definitive Guide* (4th ed.). O'Reilly.
5. Provost, F., & Fawcett, T. (2013). *Data Science for Business*. O'Reilly.
6. Dokumentasi resmi: Apache Spark, Google Colab, TensorFlow, scikit-learn

---

## F. JADWAL PERKULIAHAN 16 MINGGU

| Minggu | Topik Teori | Topik Praktikum (Google Colab) | CPMK | Referensi |
|--------|-------------|-------------------------------|------|-----------|
| 1 | Pengantar Big Data: Definisi, 5V, Sejarah, Use Case | Setup Google Colab, Python Review, Manipulasi Data Dasar | CPMK-1 | Marz (2015) Ch.1 |
| 2 | Ekosistem Big Data: Hadoop, HDFS, MapReduce, YARN | Simulasi MapReduce dengan Python di Colab | CPMK-2 | White (2015) Ch.1-3 |
| 3 | Apache Spark: Arsitektur, RDD, DataFrame, Spark SQL | Instalasi PySpark di Colab, Operasi RDD & DataFrame | CPMK-2 | Karau (2015) Ch.1-3 |
| 4 | Pengumpulan Data: Web Scraping, API, Data Streaming | Web Scraping dengan BeautifulSoup & Requests, Akses API | CPMK-3 | Dokumentasi resmi |
| 5 | Praproses Data: Cleaning, Transformasi, Normalisasi | Data Cleaning dengan Pandas & PySpark | CPMK-3 | Géron (2022) Ch.2 |
| 6 | Analisis Data Eksplorasi (EDA): Statistik Deskriptif, Korelasi | EDA Lengkap pada Dataset Big Data | CPMK-3 | Provost (2013) Ch.3 |
| 7 | Penyimpanan Big Data: NoSQL, Cloud Storage, Data Lake | Koneksi ke Google Cloud Storage & BigQuery dari Colab | CPMK-2 | Dokumentasi GCP |
| 8 | **UJIAN TENGAH SEMESTER (UTS)** | Review & Persiapan UTS | CPMK 1-3 | - |
| 9 | Machine Learning untuk Big Data: Regresi, Klasifikasi, Clustering | Implementasi ML dengan PySpark MLlib & Scikit-learn | CPMK-4 | Géron (2022) Ch.3-6 |
| 10 | Machine Learning Lanjut: Ensemble, Feature Engineering, Pipeline | ML Pipeline & Hyperparameter Tuning di Colab | CPMK-4 | Géron (2022) Ch.7 |
| 11 | Visualisasi Data: Matplotlib, Seaborn, Plotly, Google Data Studio | Dashboard Interaktif dengan Plotly & ipywidgets | CPMK-6 | Dokumentasi resmi |
| 12 | Pemrosesan Stream: Kafka, Spark Streaming, Real-time Analytics | Simulasi Streaming Data dengan PySpark Structured Streaming | CPMK-2,5 | Karau (2015) Ch.10 |
| 13 | Text Analytics & NLP: TF-IDF, Word2Vec, Sentiment Analysis | NLP Pipeline: Tokenisasi, TF-IDF, Sentiment Analysis | CPMK-4 | Dokumentasi NLTK/spaCy |
| 14 | Deep Learning untuk Big Data: CNN, RNN, Transfer Learning | Implementasi CNN & RNN dengan TensorFlow/Keras di Colab | CPMK-4 | Géron (2022) Ch.14-16 |
| 15 | Studi Kasus Industri: E-commerce, Healthcare, Finance, Smart City | Analitik End-to-End: Proyek Kelompok | CPMK 1-6 | Jurnal terkini |
| 16 | **UJIAN AKHIR SEMESTER (UAS) & PRESENTASI PROYEK AKHIR** | Demo Proyek Akhir & Evaluasi | CPMK 1-6 | - |

---

## G. TUGAS & PROYEK AKHIR

### Tugas Mingguan
- Setiap minggu mahasiswa mengumpulkan notebook Google Colab yang berisi kode dan analisis dari praktikum.

### Proyek Akhir
- Dikerjakan secara berkelompok (2-3 orang)
- Menggunakan dataset nyata berukuran besar (minimal 1 GB atau 1 juta baris)
- Pipeline lengkap: ingestion → preprocessing → analysis → ML/DL → visualization
- Dipresentasikan pada minggu ke-16
- Laporan dalam format IEEE

---

*Dokumen RPS ini dapat diperbarui sesuai perkembangan teknologi dan kebutuhan pembelajaran.*
