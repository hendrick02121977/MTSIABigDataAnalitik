# Teori Minggu 7: Penyimpanan Big Data

## Tujuan Pembelajaran

Setelah mempelajari materi minggu ini, mahasiswa diharapkan mampu:
1. Memahami evolusi teknologi penyimpanan data dari RDBMS hingga Cloud
2. Menjelaskan tipe-tipe database NoSQL dan kasus penggunaannya
3. Membedakan Data Lake, Data Warehouse, dan Data Lakehouse
4. Memahami konsep penyimpanan cloud (AWS S3, GCS, Azure Blob)
5. Menjelaskan CAP Theorem dan implikasinya pada sistem terdistribusi
6. Mengenal Google BigQuery sebagai solusi data warehousing modern

---

## 1. Evolusi Penyimpanan Data

### Era 1: File System Tradisional (1950-an – 1970-an)
- Data disimpan dalam file datar (flat files)
- Tidak ada konsistensi atau relasi antar data
- Sulit dikelola untuk data dalam jumlah besar

### Era 2: Relational Database (RDBMS) (1970-an – 2000-an)
- Edgar F. Codd memperkenalkan model relasional (1970)
- Data terstruktur dalam tabel dengan skema tetap
- SQL sebagai bahasa standar
- Contoh: Oracle, MySQL, PostgreSQL, SQL Server
- Properti **ACID**: Atomicity, Consistency, Isolation, Durability

### Era 3: NoSQL Databases (2000-an – sekarang)
- Munculnya web-scale applications (Google, Amazon, Facebook)
- Volume data tumbuh eksponensial, skema sering berubah
- Kebutuhan skalabilitas horizontal (scale-out)
- Properti **BASE**: Basically Available, Soft-state, Eventually consistent

### Era 4: Cloud Storage & Data Lakes (2010-an – sekarang)
- Penyimpanan objek murah dan skalabel (S3, GCS, Azure Blob)
- Pemisahan storage dan compute
- Data Lake: menyimpan semua data mentah dalam format asli

### Era 5: Data Lakehouse (2020-an)
- Menggabungkan fleksibilitas Data Lake dengan kemampuan ACID Data Warehouse
- Contoh: Delta Lake, Apache Iceberg, Apache Hudi

---

## 2. NoSQL Databases

### 2.1 Document Store
- Data disimpan sebagai dokumen (JSON, BSON, XML)
- Setiap dokumen dapat memiliki struktur berbeda (schema-less)
- **Contoh**: MongoDB, CouchDB, Firestore

**Use case**: katalog produk e-commerce, profil pengguna, CMS

```json
{
  "_id": "user_001",
  "nama": "Budi Santoso",
  "email": "budi@email.com",
  "alamat": {
    "kota": "Jakarta",
    "kodepos": "10110"
  },
  "pembelian": ["prod_A", "prod_C", "prod_F"]
}
```

### 2.2 Key-Value Store
- Struktur paling sederhana: kunci unik → nilai
- Sangat cepat untuk operasi baca/tulis
- **Contoh**: Redis, Amazon DynamoDB, Memcached

**Use case**: session management, caching, leaderboard real-time

```
SET user:1001:token "abc123xyz"
GET user:1001:token   → "abc123xyz"
EXPIRE user:1001:token 3600   (hapus setelah 1 jam)
```

### 2.3 Column-Family Store (Wide Column)
- Data diorganisir dalam kolom-keluarga (column families), bukan baris
- Sangat efisien untuk query analitik pada kolom tertentu
- **Contoh**: Apache Cassandra, Google Bigtable, HBase

**Use case**: time-series data, IoT sensor data, log analytics

### 2.4 Graph Database
- Data disimpan sebagai node (entitas) dan edge (relasi)
- Sangat efisien untuk query berbasis hubungan
- **Contoh**: Neo4j, Amazon Neptune, ArangoDB

**Use case**: social network, rekomendasi produk, deteksi penipuan

---

## 3. Perbandingan SQL vs NoSQL

| Aspek | SQL (RDBMS) | NoSQL |
|-------|-------------|-------|
| **Skema** | Tetap (rigid schema) | Fleksibel (schema-less) |
| **Skalabilitas** | Vertikal (scale-up) | Horizontal (scale-out) |
| **Konsistensi** | Strong (ACID) | Eventual (BASE) |
| **Query** | SQL standar | API khusus / query language sendiri |
| **Relasi** | JOIN antar tabel | Embed atau referensi manual |
| **Performa tulis** | Moderat | Sangat tinggi |
| **Performa baca** | Baik dengan indeks | Sangat baik (key-based) |
| **Struktur data** | Tabel baris-kolom | Dokumen, KV, kolom, graf |
| **Contoh** | MySQL, PostgreSQL | MongoDB, Redis, Cassandra |
| **Terbaik untuk** | Transaksi keuangan, ERP | Social media, IoT, real-time |

---

## 4. Cloud Storage

### 4.1 Amazon S3 (Simple Storage Service)
- Object storage berbasis HTTP REST
- Durabilitas 99.999999999% (11 nines)
- Kelas penyimpanan: Standard, IA (Infrequent Access), Glacier
- Mendukung versioning, lifecycle policies, event notifications

### 4.2 Google Cloud Storage (GCS)
- Terintegrasi dengan layanan Google Cloud lainnya
- Kelas: Standard, Nearline, Coldline, Archive
- Sangat terintegrasi dengan BigQuery

### 4.3 Azure Blob Storage
- Tiga tipe: Block blobs (umum), Append blobs (log), Page blobs (disk VM)
- Tier: Hot, Cool, Archive
- Terintegrasi dengan Azure Data Factory dan Synapse

### Format File untuk Big Data

| Format | Kompresi | Schema | Kelebihan |
|--------|----------|--------|-----------|
| **CSV** | Tidak | Tidak | Mudah dibaca manusia |
| **JSON** | Tidak | Tidak | Fleksibel, nested data |
| **Parquet** | Ya (columnar) | Ya | Sangat efisien untuk analitik |
| **ORC** | Ya (columnar) | Ya | Optimized untuk Hive/Spark |
| **Avro** | Ya (row) | Ya | Baik untuk streaming/Kafka |
| **Delta** | Ya | Ya + ACID | Data Lakehouse format |

---

## 5. Data Lake vs Data Warehouse vs Data Lakehouse

### Arsitektur Data Lake

```
                   DATA LAKE ARCHITECTURE
+----------------------------------------------------------+
|                                                          |
|  [Sumber Data]  -->  [Ingest Layer]  -->  [Storage]      |
|                                                          |
|  - Database          - Batch (ETL)      BRONZE / RAW     |
|  - API               - Streaming        +------------+   |
|  - IoT               - CDC             | Raw Files  |   |
|  - Logs              (Kafka, Spark)    | (as-is)    |   |
|  - Files                               +-----+------+   |
|                                              |           |
|                                        SILVER / PROCESSED|
|                                        +------------+    |
|                                        | Cleaned,   |    |
|                                        | Validated  |    |
|                                        +-----+------+    |
|                                              |           |
|                                        GOLD / CURATED    |
|                                        +------------+    |
|                                        | Aggregated,|    |
|                                        | BI-ready   |    |
|                                        +------------+    |
|                                                          |
|  [Consume Layer]: BI Tools, ML Models, SQL Analytics     |
+----------------------------------------------------------+
```

### Perbandingan Data Storage Paradigms

| Kriteria | Data Warehouse | Data Lake | Data Lakehouse |
|----------|----------------|-----------|----------------|
| **Data** | Terstruktur | Semua tipe | Semua tipe |
| **Skema** | Schema-on-write | Schema-on-read | Schema-on-write + read |
| **ACID** | Ya | Tidak | Ya (Delta/Iceberg) |
| **Performa query** | Sangat tinggi | Rendah (unoptimized) | Tinggi |
| **Biaya storage** | Mahal | Murah | Murah |
| **Fleksibilitas** | Rendah | Sangat tinggi | Tinggi |
| **Contoh** | BigQuery, Redshift | S3+Hadoop, ADLS | Databricks, Delta Lake |

---

## 6. Google BigQuery

**Google BigQuery** adalah data warehouse serverless, sangat skalabel, berbasis cloud dari Google.

### Fitur Utama
- **Serverless**: tidak perlu mengelola infrastruktur
- **Columnar storage**: data disimpan per kolom (efisien untuk analitik)
- **SQL standard**: mendukung ANSI SQL
- **Pemisahan storage-compute**: bayar sesuai pemakaian
- **ML terintegrasi**: BigQuery ML untuk membangun model langsung di SQL

### Arsitektur BigQuery
```
Project → Dataset → Table / View
```

### Contoh Query BigQuery (Public Dataset)
```sql
-- Menghitung rata-rata suhu per kota
SELECT
  station_name,
  AVG(mean_temp) AS avg_temp,
  COUNT(*) AS num_records
FROM `bigquery-public-data.noaa_gsod.gsod2023`
WHERE country = 'ID'  -- Indonesia
GROUP BY station_name
ORDER BY avg_temp DESC
LIMIT 10;
```

---

## 7. CAP Theorem

**CAP Theorem** (Brewer's Theorem, 2000): Dalam sistem terdistribusi, hanya dua dari tiga properti berikut yang dapat dijamin secara bersamaan:

```
         Consistency (C)
              /\
             /  \
            /    \
           /  CA  \
          /--------\
         / CP  | AP \
        /      |     \
       /-------|-------\
Partition    Partition
Tolerance(P)
```

| Properti | Penjelasan | Contoh Sistem |
|----------|------------|---------------|
| **C** - Consistency | Semua node melihat data yang sama pada saat bersamaan | RDBMS tradisional |
| **A** - Availability | Setiap permintaan mendapat respons (bukan error) | DNS |
| **P** - Partition Tolerance | Sistem tetap berfungsi meski ada partisi jaringan | Sistem terdistribusi |

**Pilihan desain**:
- **CP** (Consistency + Partition): MongoDB, HBase, Zookeeper
- **AP** (Availability + Partition): Cassandra, CouchDB, DynamoDB
- **CA** (Consistency + Availability): Hanya mungkin pada sistem non-terdistribusi

> **Catatan Modern (PACELC Theorem)**: Bahkan tanpa partisi, sistem terdistribusi masih harus memilih antara **Latency** dan **Consistency**.

---

## 8. Ringkasan

Perkembangan penyimpanan data mencerminkan kebutuhan yang terus berkembang:

1. **RDBMS** masih relevan untuk data terstruktur dengan kebutuhan transaksi ACID
2. **NoSQL** memberikan fleksibilitas dan skalabilitas untuk berbagai kasus penggunaan modern
3. **Cloud Storage** memungkinkan penyimpanan murah, skalabel, dan global
4. **Data Lake** cocok untuk menyimpan semua data mentah, terstruktur maupun tidak
5. **Data Lakehouse** adalah evolusi terbaru yang menggabungkan keunggulan keduanya
6. **CAP Theorem** menjelaskan trade-off fundamental dalam sistem terdistribusi

---

## Referensi

1. Brewer, E. (2000). *Towards robust distributed systems*. PODC Keynote.
2. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly Media.
3. Fowler, M. & Sadalage, P. (2012). *NoSQL Distilled*. Addison-Wesley.
4. MongoDB Documentation: https://www.mongodb.com/docs/
5. Apache Cassandra Documentation: https://cassandra.apache.org/doc/
6. Google BigQuery Documentation: https://cloud.google.com/bigquery/docs
7. AWS S3 Documentation: https://docs.aws.amazon.com/s3/
8. Delta Lake Documentation: https://delta.io/
