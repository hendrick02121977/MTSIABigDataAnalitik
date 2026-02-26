# Minggu 2: Ekosistem Big Data – Hadoop, HDFS, MapReduce & YARN

## Tujuan Pembelajaran
1. Memahami arsitektur Hadoop dan komponen-komponennya
2. Menjelaskan cara kerja HDFS (Hadoop Distributed File System)
3. Memahami paradigma pemrograman MapReduce
4. Mensimulasikan MapReduce dengan Python

---

## 2.1 Apache Hadoop

**Apache Hadoop** adalah framework open-source untuk penyimpanan dan pemrosesan data terdistribusi pada cluster komputer menggunakan model pemrograman sederhana.

### Sejarah
- 2003: Google mempublikasikan paper GFS (Google File System)
- 2004: Google mempublikasikan paper MapReduce
- 2006: Doug Cutting & Mike Cafarella membuat Hadoop (terinspirasi dari paper Google)
- 2008: Hadoop menjadi Apache top-level project
- Nama "Hadoop" berasal dari nama mainan gajah kuning milik putra Doug Cutting

### Komponen Utama Hadoop
```
HADOOP ECOSYSTEM
├── HDFS (Hadoop Distributed File System) - Penyimpanan
├── YARN (Yet Another Resource Negotiator) - Manajemen Resource
├── MapReduce - Framework Pemrosesan
└── Common - Utilitas yang dibutuhkan modul lain
```

---

## 2.2 HDFS (Hadoop Distributed File System)

### Arsitektur HDFS

```
┌─────────────────────────────────────────────────────────────┐
│                    HDFS ARCHITECTURE                        │
│                                                             │
│  ┌──────────────┐        ┌───────────────────────────────┐  │
│  │  NameNode    │        │        DataNodes              │  │
│  │  (Master)    │◄──────►│  ┌─────┐  ┌─────┐  ┌─────┐  │  │
│  │              │        │  │ DN1 │  │ DN2 │  │ DN3 │  │  │
│  │ - Metadata   │        │  │Blk1 │  │Blk2 │  │Blk3 │  │  │
│  │ - Namespace  │        │  │Blk2 │  │Blk3 │  │Blk1 │  │  │
│  │ - File-Block │        │  │Blk3 │  │Blk1 │  │Blk2 │  │  │
│  │   mapping    │        │  └─────┘  └─────┘  └─────┘  │  │
│  └──────────────┘        └───────────────────────────────┘  │
│                                                             │
│  ┌──────────────┐                                           │
│  │Secondary     │  (bukan backup, tapi checkpoint helper)  │
│  │NameNode      │                                           │
│  └──────────────┘                                           │
└─────────────────────────────────────────────────────────────┘
```

### Konsep Kunci HDFS

| Konsep | Keterangan |
|--------|------------|
| **Block Size** | Default 128 MB (Hadoop 2.x) |
| **Replication Factor** | Default 3 (setiap blok disimpan di 3 node berbeda) |
| **NameNode** | Menyimpan metadata dan namespace |
| **DataNode** | Menyimpan data aktual (blok) |
| **Rack Awareness** | Penempatan replika mempertimbangkan lokasi fisik |
| **Write-Once** | File HDFS tidak dapat diubah (append-only) |

### Proses Penulisan File ke HDFS
```
Client → NameNode: "Tulis file A.csv (500 MB)"
NameNode → Client: "Bagi menjadi 4 blok, simpan di DN1, DN2, DN3"
Client → DataNode1: Tulis Blok-1 (128 MB)
DataNode1 → DataNode2: Replikasi Blok-1
DataNode2 → DataNode3: Replikasi Blok-1
... (ulangi untuk blok 2, 3, 4)
Client → NameNode: "Selesai"
```

---

## 2.3 MapReduce

**MapReduce** adalah paradigma pemrograman untuk memproses data besar secara paralel dan terdistribusi.

### Dua Fase Utama

#### MAP Phase
- Input: pasangan key-value
- Output: pasangan key-value intermediate
- Setiap mapper bekerja pada satu blok data secara independen
- Dapat berjalan paralel

#### REDUCE Phase
- Input: key + list of values (dari semua mapper dengan key yang sama)
- Output: hasil akhir agregasi
- Reducer menerima semua nilai untuk setiap key unik

### Contoh: Word Count

```
Input: "big data big analytics big"

== MAP PHASE ==
Mapper 1: (big,1), (data,1), (big,1)
Mapper 2: (analytics,1), (big,1)

== SHUFFLE & SORT ==
big:       [1, 1, 1]
data:      [1]
analytics: [1]

== REDUCE PHASE ==
big:       3
data:      1
analytics: 1
```

### Alur Kerja MapReduce
```
Input Data (HDFS)
      │
      ▼
┌─────────────┐
│  InputSplit │ (pembagian data input)
└──────┬──────┘
       ▼
┌─────────────┐      ┌─────────────┐      ┌─────────────┐
│   Mapper 1  │      │   Mapper 2  │      │   Mapper 3  │
└──────┬──────┘      └──────┬──────┘      └──────┬──────┘
       │                    │                    │
       ▼                    ▼                    ▼
┌──────────────────────────────────────────────────────┐
│                  Shuffle & Sort                      │
└──────────────────────────┬───────────────────────────┘
                           ▼
            ┌──────────────────────────┐
            │       Reducer 1          │
            └──────────────┬───────────┘
                           ▼
                    Output (HDFS)
```

---

## 2.4 YARN (Yet Another Resource Negotiator)

YARN memisahkan manajemen resource dari pemrosesan data (pemisahan dari MapReduce v1).

### Komponen YARN

| Komponen | Fungsi |
|----------|--------|
| **ResourceManager** | Master, mengelola semua resource di cluster |
| **NodeManager** | Agent di setiap node, melaporkan resource |
| **ApplicationMaster** | Mengelola lifecycle satu aplikasi |
| **Container** | Unit alokasi resource (CPU + memory) |

### Keunggulan YARN
- Multi-tenancy: mendukung berbagai framework (Spark, Tez, dll.)
- Scalability: mendukung hingga 10.000+ node
- Resource sharing yang efisien

---

## 2.5 Ekosistem Hadoop (Hadoop Ecosystem)

```
┌─────────────────────────────────────────────────────────────┐
│                    HADOOP ECOSYSTEM                         │
│                                                             │
│  Query:    Hive ──── Pig ──── Impala ──── Drill            │
│  NoSQL:    HBase ──── Cassandra ──── MongoDB               │
│  Ingest:   Sqoop ──── Flume ──── Kafka                     │
│  Process:  MapReduce ──── Spark ──── Flink ──── Storm      │
│  Coord:    ZooKeeper ──── Oozie                             │
│  Storage:  HDFS ──── S3 ──── Alluxio                       │
│  Manage:   YARN ──── Mesos                                 │
│  Security: Kerberos ──── Ranger ──── Atlas                 │
│  Monitor:  Ambari ──── Ganglia ──── Nagios                 │
└─────────────────────────────────────────────────────────────┘
```

---

## 2.6 Perbandingan: Hadoop vs Spark

| Aspek | Hadoop MapReduce | Apache Spark |
|-------|-----------------|--------------|
| Penyimpanan | Disk (HDFS) | In-memory (RAM) |
| Kecepatan | Lambat (disk I/O) | 100x lebih cepat |
| Ease of Use | Kompleks (Java) | Lebih mudah (Python, Scala) |
| Real-time | Tidak | Ya (Spark Streaming) |
| ML Support | Terbatas | MLlib built-in |
| Fault Tolerance | Ya | Ya |
| Cost | Lebih hemat | Butuh lebih banyak RAM |

---

## Rangkuman
- Hadoop terdiri dari HDFS (storage), MapReduce (processing), dan YARN (resource management)
- HDFS menyimpan data dalam blok terdistribusi dengan replikasi untuk fault tolerance
- MapReduce memproses data dalam dua fase: Map (transformasi) dan Reduce (agregasi)
- YARN memisahkan manajemen resource dari pemrosesan, memungkinkan multi-framework
- Ekosistem Hadoop sangat luas dengan berbagai tools untuk berbagai kebutuhan

---

## Referensi
- White, T. (2015). *Hadoop: The Definitive Guide* (4th ed.). O'Reilly.
- [Apache Hadoop Documentation](https://hadoop.apache.org/docs/)
- Dean, J., & Ghemawat, S. (2008). MapReduce: Simplified Data Processing on Large Clusters. *OSDI*.
