# Teori Minggu 3: Apache Spark

## Tujuan Pembelajaran

Setelah mempelajari materi ini, mahasiswa diharapkan mampu:
1. Memahami konsep dasar dan arsitektur Apache Spark
2. Menjelaskan perbedaan antara RDD, DataFrame, dan Dataset
3. Mengidentifikasi operasi transformasi dan aksi pada RDD
4. Memahami penggunaan Spark SQL untuk analisis data
5. Mengimplementasikan kode PySpark sederhana untuk pengolahan data terdistribusi

---

## Pengantar Apache Spark

Apache Spark adalah framework komputasi terdistribusi open-source yang dirancang untuk pemrosesan data skala besar dengan kecepatan tinggi. Spark dikembangkan pertama kali di AMPLab, University of California, Berkeley pada tahun 2009, dan kemudian menjadi proyek Apache pada tahun 2013.

Keunggulan utama Apache Spark dibandingkan framework sebelumnya (seperti Hadoop MapReduce) adalah kemampuannya melakukan komputasi **in-memory**, sehingga dapat memproses data hingga **100x lebih cepat** dibandingkan Hadoop MapReduce untuk operasi tertentu. Spark mendukung berbagai bahasa pemrograman seperti Python (PySpark), Scala, Java, dan R.

### Komponen Ekosistem Spark

Apache Spark terdiri dari beberapa komponen utama:

- **Spark Core**: Mesin eksekusi utama yang mengelola penjadwalan tugas, manajemen memori, pemulihan kesalahan, dan interaksi dengan sistem penyimpanan.
- **Spark SQL**: Modul untuk bekerja dengan data terstruktur menggunakan SQL atau DataFrame API.
- **Spark Streaming / Structured Streaming**: Komponen untuk pemrosesan data real-time.
- **MLlib**: Library machine learning terdistribusi bawaan Spark.
- **GraphX**: API untuk komputasi graf terdistribusi.

---

## Arsitektur Apache Spark

Spark menggunakan arsitektur **master-slave** (driver-worker) yang terdiri dari tiga komponen utama:

```
+--------------------------------------------------+
|                  SPARK APPLICATION               |
|                                                  |
|   +------------------+                           |
|   |   DRIVER PROGRAM |                           |
|   |  (SparkContext)  |                           |
|   +--------+---------+                           |
|            |                                     |
|            v                                     |
|   +------------------+                           |
|   |  CLUSTER MANAGER |                           |
|   | (Standalone/YARN |                           |
|   |   /Mesos/K8s)    |                           |
|   +---+----------+---+                           |
|       |          |                               |
|       v          v                               |
|  +--------+  +--------+                          |
|  | Worker |  | Worker |  ...                     |
|  | Node   |  | Node   |                          |
|  |--------|  |--------|                          |
|  |Executor|  |Executor|                          |
|  | Task1  |  | Task1  |                          |
|  | Task2  |  | Task2  |                          |
|  +--------+  +--------+                          |
+--------------------------------------------------+
```

**Penjelasan Komponen:**

- **Driver Program**: Proses utama yang menjalankan fungsi `main()` aplikasi Spark. Driver bertanggung jawab untuk membuat `SparkContext`, mendefinisikan transformasi dan aksi, serta mengoordinasikan eksekusi tugas.
- **Cluster Manager**: Layanan eksternal untuk mengalokasikan sumber daya pada kluster. Spark mendukung Standalone, Apache YARN, Apache Mesos, dan Kubernetes.
- **Worker Node**: Node dalam kluster yang menjalankan proses `Executor`. Setiap Worker Node dapat menjalankan satu atau lebih Executor.
- **Executor**: Proses JVM yang berjalan pada Worker Node. Executor bertanggung jawab menjalankan Task dan menyimpan data dalam memori atau disk.

---

## RDD (Resilient Distributed Datasets)

### Konsep RDD

RDD adalah abstraksi data fundamental dalam Apache Spark. RDD merupakan koleksi objek yang **tidak dapat diubah (immutable)** dan **terdistribusi** di seluruh kluster, sehingga dapat diproses secara paralel.

### Karakteristik RDD

| Karakteristik | Penjelasan |
|---|---|
| **Resilient** | Toleran terhadap kegagalan; dapat direkonstruksi secara otomatis jika partisi hilang menggunakan lineage graph |
| **Distributed** | Data dibagi menjadi partisi yang tersebar di beberapa node dalam kluster |
| **Dataset** | Koleksi data yang dapat berupa file teks, database, data streaming, dll |
| **Lazy Evaluation** | Transformasi tidak dieksekusi sampai ada aksi yang memicunya |
| **Immutable** | Sekali dibuat, RDD tidak dapat diubah; setiap transformasi menghasilkan RDD baru |

### Operasi pada RDD

RDD mendukung dua jenis operasi:

#### 1. Transformasi (Lazy)
Transformasi menghasilkan RDD baru dari RDD yang ada. Transformasi bersifat **lazy**, artinya tidak langsung dieksekusi.

| Transformasi | Deskripsi |
|---|---|
| `map(func)` | Terapkan fungsi ke setiap elemen, hasilkan RDD baru |
| `filter(func)` | Pilih elemen yang memenuhi kondisi |
| `flatMap(func)` | Seperti map, tapi setiap elemen bisa menghasilkan 0 atau lebih elemen |
| `distinct()` | Hapus duplikat |
| `union(rdd)` | Gabungkan dua RDD |
| `intersection(rdd)` | Elemen yang ada di kedua RDD |
| `sortBy(func)` | Urutkan berdasarkan fungsi kunci |
| `groupByKey()` | Kelompokkan nilai berdasarkan kunci |
| `reduceByKey(func)` | Gabungkan nilai berdasarkan kunci menggunakan fungsi reduksi |

#### 2. Aksi (Eager)
Aksi memicu eksekusi rencana komputasi dan mengembalikan nilai ke driver atau menyimpan ke penyimpanan eksternal.

| Aksi | Deskripsi |
|---|---|
| `collect()` | Kembalikan semua elemen sebagai list ke driver |
| `count()` | Hitung jumlah elemen |
| `first()` | Kembalikan elemen pertama |
| `take(n)` | Kembalikan n elemen pertama |
| `reduce(func)` | Gabungkan semua elemen menggunakan fungsi asosiatif |
| `saveAsTextFile(path)` | Simpan RDD ke file teks |
| `foreach(func)` | Terapkan fungsi ke setiap elemen (side effect) |

---

## DataFrame dan Dataset API

### DataFrame

DataFrame adalah abstraksi data tingkat tinggi yang diorganisasikan dalam kolom bernama, mirip dengan tabel dalam database relasional atau DataFrame dalam pandas. DataFrame diperkenalkan di Spark 1.3 dan menjadi API utama untuk analisis data terstruktur.

**Keunggulan DataFrame:**
- Optimasi otomatis melalui **Catalyst Optimizer** dan **Tungsten Execution Engine**
- Mendukung berbagai sumber data: JSON, CSV, Parquet, ORC, Hive, JDBC
- Integrasi dengan Spark SQL
- Lebih efisien dari segi memori dibandingkan RDD

### Dataset

Dataset adalah ekstensi dari DataFrame yang menyediakan keamanan tipe (type-safety) pada waktu kompilasi. Dataset hanya tersedia di Scala dan Java; di Python (PySpark), DataFrame sudah berfungsi sebagai Dataset yang diketik secara dinamis.

---

## Spark SQL

Spark SQL memungkinkan pengguna menjalankan kueri SQL pada data terstruktur. Fitur utama Spark SQL:

- **Unified Data Access**: Baca dan tulis data dari berbagai format (JSON, Parquet, ORC, JDBC)
- **Hive Integration**: Kompatibel dengan HiveQL dan metadata Hive
- **Standard SQL**: Mendukung subset besar dari SQL standar ANSI
- **Temporary Views**: Daftarkan DataFrame sebagai tabel sementara untuk dikueri

```python
# Contoh Spark SQL
df.createOrReplaceTempView("mahasiswa")
hasil = spark.sql("SELECT jurusan, AVG(ipk) as rata_ipk FROM mahasiswa GROUP BY jurusan")
hasil.show()
```

---

## Perbandingan: RDD vs DataFrame vs Dataset

| Kriteria | RDD | DataFrame | Dataset |
|---|---|---|---|
| **Abstraksi** | Low-level | High-level | High-level |
| **Type Safety** | Ya (compile-time) | Tidak (runtime) | Ya (compile-time) |
| **Optimasi** | Manual | Otomatis (Catalyst) | Otomatis (Catalyst) |
| **Serialisasi** | Java serialization | Off-heap (Tungsten) | Off-heap (Tungsten) |
| **Bahasa** | Scala, Java, Python, R | Scala, Java, Python, R | Scala, Java |
| **Kinerja** | Lebih lambat | Lebih cepat | Lebih cepat |
| **Kemudahan** | Kompleks | Mudah | Sedang |
| **Use Case** | Data tidak terstruktur | Data terstruktur | Data terstruktur + type-safe |

---

## Contoh Kode Python (PySpark)

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, avg

# Membuat SparkSession
spark = SparkSession.builder \
    .appName("ContohSpark") \
    .getOrCreate()

# ---- Contoh RDD ----
data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
rdd = spark.sparkContext.parallelize(data)

# Transformasi: filter bilangan genap, lalu kalikan 2
rdd_hasil = rdd.filter(lambda x: x % 2 == 0).map(lambda x: x * 2)
print("Hasil RDD:", rdd_hasil.collect())  # [4, 8, 12, 16, 20]

# ---- Contoh DataFrame ----
data_mhs = [
    ("Budi", "Informatika", 3.8),
    ("Ani", "Sistem Informasi", 3.5),
    ("Citra", "Informatika", 3.9),
    ("Dodi", "Sistem Informasi", 3.2),
]
kolom = ["nama", "jurusan", "ipk"]
df = spark.createDataFrame(data_mhs, kolom)

df.show()
df.printSchema()

# Rata-rata IPK per jurusan
df.groupBy("jurusan").agg(avg("ipk").alias("rata_ipk")).show()

# ---- Spark SQL ----
df.createOrReplaceTempView("mahasiswa")
spark.sql("""
    SELECT jurusan, COUNT(*) as jumlah, AVG(ipk) as rata_ipk
    FROM mahasiswa
    GROUP BY jurusan
    ORDER BY rata_ipk DESC
""").show()

spark.stop()
```

---

## Ringkasan

Apache Spark adalah platform komputasi terdistribusi yang powerful untuk Big Data Analytics. Poin-poin kunci yang perlu diingat:

1. **Spark lebih cepat dari Hadoop MapReduce** karena menggunakan pemrosesan in-memory.
2. **RDD** adalah abstraksi dasar Spark — immutable, distributed, dan fault-tolerant.
3. **Transformasi bersifat lazy** — hanya dieksekusi saat ada aksi yang memicunya.
4. **DataFrame** menawarkan performa lebih baik dari RDD berkat optimasi Catalyst dan Tungsten.
5. **Spark SQL** memungkinkan analisis data terstruktur dengan sintaks SQL yang familiar.
6. **Ekosistem Spark** lengkap: SQL, Streaming, MLlib, GraphX dalam satu platform terpadu.

---

## Referensi

1. Zaharia, M., et al. (2016). *Apache Spark: A Unified Engine for Big Data Processing*. Communications of the ACM, 59(11), 56–65.
2. Chambers, B., & Zaharia, M. (2018). *Spark: The Definitive Guide*. O'Reilly Media.
3. Apache Spark Documentation. https://spark.apache.org/docs/latest/
4. Karau, H., Konwinski, A., Wendell, P., & Zaharia, M. (2015). *Learning Spark*. O'Reilly Media.
5. Damji, J. S., et al. (2020). *Learning Spark: Lightning-Fast Data Analytics* (2nd ed.). O'Reilly Media.
