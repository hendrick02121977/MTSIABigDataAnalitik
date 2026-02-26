# Minggu 12: Stream Processing & Real-time Analytics

## Tujuan Pembelajaran

Setelah mempelajari materi minggu ini, mahasiswa diharapkan mampu:
1. Memahami konsep dan perbedaan antara Batch Processing dan Stream Processing
2. Menjelaskan arsitektur Apache Kafka beserta komponen-komponennya
3. Memahami cara kerja Spark Streaming dan Structured Streaming
4. Menerapkan pola analitik real-time seperti windowing dan watermarking
5. Mengidentifikasi use case industri yang cocok menggunakan stream processing

---

## 1. Batch Processing vs Stream Processing

### Batch Processing
Batch Processing adalah pendekatan pemrosesan data di mana data dikumpulkan dalam periode tertentu, kemudian diproses sekaligus dalam satu batch (kumpulan besar). Contohnya adalah laporan harian penjualan yang dihitung pada tengah malam.

**Karakteristik Batch Processing:**
- Data diproses secara periodik (jam, harian, mingguan)
- Latency tinggi — hasil tidak tersedia secara instan
- Throughput tinggi — cocok untuk volume data sangat besar
- Contoh tools: Apache Hadoop MapReduce, Apache Hive, Apache Spark (batch mode)

### Stream Processing
Stream Processing adalah pendekatan pemrosesan data secara terus-menerus (kontinu) saat data tiba, tanpa menunggu akumulasi dalam batch besar.

**Karakteristik Stream Processing:**
- Data diproses segera setelah tiba (low latency, milidetik hingga detik)
- Cocok untuk data yang bergerak cepat dan tidak terbatas (unbounded data)
- Contoh tools: Apache Kafka Streams, Apache Flink, Spark Structured Streaming

| Aspek | Batch Processing | Stream Processing |
|---|---|---|
| Latensi | Tinggi (menit–jam) | Rendah (ms–detik) |
| Throughput | Sangat tinggi | Sedang–tinggi |
| Kompleksitas | Rendah | Lebih tinggi |
| Use case | Laporan, ETL harian | Fraud detection, IoT |
| Contoh tools | Hadoop, Hive | Kafka, Flink, Spark Streaming |

---

## 2. Apache Kafka

### Konsep Dasar
Apache Kafka adalah platform distributed event streaming yang dirancang untuk menangani aliran data real-time dengan throughput tinggi, fault tolerance, dan skalabilitas horizontal. Kafka dikembangkan oleh LinkedIn pada 2011 dan kini menjadi standar industri untuk message streaming.

### Arsitektur Kafka

```
╔══════════════════════════════════════════════════════════════════╗
║                    ARSITEKTUR APACHE KAFKA                       ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  ┌──────────┐    ┌─────────────────────────────────────────┐    ║
║  │ PRODUCER │───▶│              KAFKA BROKER               │    ║
║  │  App A   │    │  ┌──────────────┐  ┌──────────────┐    │    ║
║  └──────────┘    │  │  Topic: logs │  │ Topic: sales │    │    ║
║                  │  │  Partition 0 │  │  Partition 0 │    │    ║
║  ┌──────────┐    │  │  Partition 1 │  │  Partition 1 │    │    ║
║  │ PRODUCER │───▶│  │  Partition 2 │  │  Partition 2 │    │    ║
║  │  App B   │    │  └──────────────┘  └──────────────┘    │    ║
║  └──────────┘    └─────────────────────────────────────────┘    ║
║                                │            │                    ║
║                                ▼            ▼                    ║
║                  ┌──────────────────────────────────────┐       ║
║                  │           CONSUMER GROUP             │       ║
║                  │  ┌─────────────┐  ┌─────────────┐   │       ║
║                  │  │ Consumer 1  │  │ Consumer 2  │   │       ║
║                  │  │ (Analytics) │  │ (Storage)   │   │       ║
║                  │  └─────────────┘  └─────────────┘   │       ║
║                  └──────────────────────────────────────┘       ║
║                                                                  ║
║  ZooKeeper: Manages Broker Metadata & Leader Election           ║
╚══════════════════════════════════════════════════════════════════╝
```

### Komponen Utama Kafka

**1. Producer**
Producer adalah aplikasi atau layanan yang mengirimkan (mempublikasikan) pesan ke Kafka topic. Producer menentukan ke topic dan partition mana pesan dikirimkan.

**2. Broker**
Broker adalah server Kafka yang menyimpan dan mengelola data. Dalam deployment produksi, Kafka biasanya dijalankan sebagai cluster yang terdiri dari beberapa broker untuk high availability dan fault tolerance.

**3. Consumer**
Consumer adalah aplikasi atau layanan yang membaca (mengonsumsi) pesan dari Kafka topic. Consumer dapat bergabung dalam Consumer Group untuk memproses pesan secara paralel.

**4. Topic**
Topic adalah kategori atau saluran logis tempat pesan disimpan. Producer menulis ke topic, consumer membaca dari topic.

**5. Partition**
Setiap topic dibagi menjadi beberapa partition untuk memungkinkan parallelisme. Setiap partition adalah urutan log yang terurut dan tidak berubah (immutable). Offset adalah posisi setiap pesan dalam partition.

**6. ZooKeeper / KRaft**
Mengelola metadata cluster, leader election, dan konfigurasi broker. Versi Kafka modern menggunakan KRaft (tanpa ZooKeeper).

### Use Cases Apache Kafka
- **Event Sourcing**: Merekam setiap perubahan state sebagai event
- **Log Aggregation**: Mengumpulkan log dari banyak layanan
- **Metrics & Monitoring**: Aliran data telemetri real-time
- **Stream Processing Pipeline**: Sebagai sumber data untuk Flink/Spark
- **Data Integration**: Menghubungkan berbagai sistem (CDC — Change Data Capture)

---

## 3. Spark Streaming & Structured Streaming

### DStream API (Spark Streaming — Legacy)
DStream (Discretized Stream) adalah API lama Spark Streaming yang memotong stream menjadi micro-batch RDD pada interval tertentu. Meskipun fungsional, API ini dianggap lebih rendah levelnya.

```python
# Contoh DStream (legacy)
from pyspark.streaming import StreamingContext
ssc = StreamingContext(sc, batchDuration=1)  # 1-second micro-batch
lines = ssc.socketTextStream("localhost", 9999)
words = lines.flatMap(lambda line: line.split(" "))
```

### Structured Streaming (Modern)
Structured Streaming memandang stream sebagai tabel yang terus bertambah (append-only unbounded table). Developer menggunakan DataFrame/SQL API yang sama seperti batch processing — hanya menambahkan `readStream` dan `writeStream`.

```python
# Contoh Structured Streaming
df = spark.readStream \
    .format("kafka") \
    .option("kafka.bootstrap.servers", "localhost:9092") \
    .option("subscribe", "sensor-data") \
    .load()

# Proses seperti DataFrame biasa
result = df.groupBy("sensor_id").agg(avg("temperature"))

# Output
result.writeStream.outputMode("complete").format("console").start()
```

**Keunggulan Structured Streaming:**
- API lebih intuitif (DataFrame/SQL)
- End-to-end exactly-once semantics
- Native integration dengan Kafka, Delta Lake
- Support event time processing dan watermarking

---

## 4. Real-time Analytics Patterns

### Event Time vs Processing Time
- **Event Time**: Waktu saat event benar-benar terjadi di sumbernya (sensor, aplikasi)
- **Processing Time**: Waktu saat event tiba dan diproses oleh sistem streaming
- **Ingestion Time**: Waktu saat event masuk ke dalam pipeline

Perbedaan ini penting karena data dapat tiba terlambat (late data) akibat jaringan, retry, atau masalah sistem.

### Watermarking
Watermark adalah mekanisme untuk menentukan seberapa lama sistem harus menunggu late data sebelum menutup sebuah window.

```python
df.withWatermark("event_time", "10 minutes") \
  .groupBy(window("event_time", "5 minutes"), "sensor_id") \
  .agg(avg("temperature"))
```

### Windowing
Windowing membagi stream menjadi segmen-segmen waktu untuk agregasi:

| Tipe Window | Deskripsi | Contoh |
|---|---|---|
| Tumbling Window | Jendela tetap, tidak overlap | Setiap 5 menit (0–5, 5–10, …) |
| Sliding Window | Jendela bergerak, bisa overlap | 5 menit setiap 1 menit |
| Session Window | Berdasarkan gap inaktivitas | Sesi pengguna (gap > 30 menit) |

---

## 5. Perbandingan Streaming Frameworks

| Framework | Latensi | Throughput | Exactly-Once | Bahasa | State Management |
|---|---|---|---|---|---|
| Apache Kafka Streams | Sangat rendah | Tinggi | Ya | Java/Scala | Ya (RocksDB) |
| Apache Flink | Sangat rendah | Sangat tinggi | Ya | Java/Scala/Python | Ya |
| Spark Structured Streaming | Rendah (micro-batch) | Tinggi | Ya | Python/Scala/R | Ya |
| Apache Storm | Rendah | Sedang | At-least-once | Java/Python | Terbatas |
| AWS Kinesis | Rendah | Tinggi | At-least-once | Multi-bahasa | Terbatas |

---

## 6. Use Cases Industri

### Fraud Detection
Bank dan fintech menggunakan stream processing untuk mendeteksi transaksi mencurigakan secara real-time. Setiap transaksi yang masuk dianalisis terhadap pola historis dalam milidetik. Jika terdeteksi anomali, transaksi dapat diblokir sebelum selesai diproses.

**Alur**: Transaksi → Kafka Topic → Flink/Spark → ML Model → Alert/Block

### IoT Monitoring
Perangkat IoT (sensor pabrik, kendaraan, smart home) menghasilkan data terus-menerus. Stream processing memungkinkan monitoring kondisi mesin, deteksi kerusakan dini (predictive maintenance), dan respons otomatis terhadap kondisi kritis.

**Alur**: Sensor → MQTT → Kafka → Spark Streaming → Dashboard + Alert

### Log Analysis
Platform digital besar (e-commerce, SaaS) memproses jutaan log per menit untuk mendeteksi error, melacak performa, dan memantau keamanan. ELK Stack (Elasticsearch, Logstash, Kibana) sering dipadukan dengan Kafka.

**Alur**: Aplikasi → Logstash/Fluentd → Kafka → Elasticsearch → Kibana

---

## Ringkasan

Minggu ini kita mempelajari fondasi stream processing — teknologi inti untuk memproses data yang bergerak cepat di era Big Data. Apache Kafka menjadi tulang punggung (backbone) sebagian besar arsitektur data modern karena kemampuannya menangani jutaan event per detik dengan fault tolerance. Spark Structured Streaming menyederhanakan pengembangan aplikasi streaming dengan API DataFrame yang familiar. Pola seperti windowing dan watermarking sangat penting untuk menghasilkan hasil yang akurat meskipun ada late data. Stream processing kini menjadi kebutuhan wajib di industri, terutama untuk fraud detection, IoT, dan analitik real-time.

---

## Referensi

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly Media.
2. Narkhede, N., Shapira, G., & Palino, T. (2017). *Kafka: The Definitive Guide*. O'Reilly Media.
3. Damji, J. S., et al. (2020). *Learning Spark, 2nd Edition*. O'Reilly Media.
4. Apache Kafka Documentation: https://kafka.apache.org/documentation/
5. Apache Spark Structured Streaming: https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html
6. Akidau, T., et al. (2015). *The Dataflow Model*. VLDB Endowment.
