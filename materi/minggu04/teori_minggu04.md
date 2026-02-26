# Teori Minggu 4: Pengumpulan Data (Data Collection)

## Tujuan Pembelajaran

Setelah mempelajari materi ini, mahasiswa diharapkan mampu:
1. Memahami berbagai metode pengumpulan data untuk keperluan Big Data
2. Mengimplementasikan web scraping menggunakan Python (BeautifulSoup, Requests)
3. Mengakses data melalui REST API dengan autentikasi dan penanganan rate limiting
4. Menjelaskan konsep data streaming dan arsitektur producer-consumer
5. Memahami pertimbangan hukum dan etika dalam pengumpulan data

---

## Metode Pengumpulan Data untuk Big Data

Pengumpulan data adalah tahap pertama dan paling krusial dalam pipeline Big Data Analytics. Kualitas dan kelengkapan data yang dikumpulkan akan sangat mempengaruhi hasil analisis. Terdapat tiga metode utama pengumpulan data dalam konteks Big Data:

1. **Web Scraping**: Ekstraksi data dari halaman web secara otomatis
2. **API (Application Programming Interface)**: Pengambilan data melalui antarmuka terprogram yang disediakan layanan
3. **Data Streaming**: Pengumpulan data secara real-time dari sumber yang terus menghasilkan data

---

## Web Scraping

### Konsep Web Scraping

Web scraping (juga disebut web harvesting atau web data extraction) adalah teknik pengambilan data dari website secara otomatis menggunakan program komputer. Proses ini melibatkan:
1. Mengirim permintaan HTTP ke server web
2. Mengunduh konten HTML halaman
3. Mem-parsing HTML untuk mengekstrak data yang diinginkan
4. Menyimpan data dalam format terstruktur (CSV, JSON, database)

### Tools Web Scraping

#### BeautifulSoup
Library Python yang ringan dan mudah digunakan untuk mem-parsing HTML dan XML. Cocok untuk scraping halaman statis.
- **Kelebihan**: Mudah dipelajari, dokumentasi lengkap, sintaks intuitif
- **Kekurangan**: Tidak dapat menjalankan JavaScript, tidak bisa menangani halaman dinamis

#### Scrapy
Framework scraping Python yang lengkap dan berperforma tinggi untuk proyek scraping berskala besar.
- **Kelebihan**: Asynchronous, built-in pipeline untuk penyimpanan data, middleware yang dapat dikustomisasi
- **Kekurangan**: Kurva pembelajaran lebih tinggi, overhead lebih besar untuk proyek kecil

#### Selenium
Tool otomatisasi browser yang dapat menjalankan JavaScript dan berinteraksi dengan elemen halaman dinamis.
- **Kelebihan**: Dapat menangani SPA (Single Page Application), mendukung interaksi pengguna
- **Kekurangan**: Lebih lambat, membutuhkan browser driver, resource-intensive

### Etika Web Scraping

Sebelum melakukan web scraping, penting untuk memperhatikan:
- **robots.txt**: Periksa file `/robots.txt` di setiap website untuk mengetahui kebijakan crawling
- **Terms of Service**: Baca dan patuhi syarat layanan website
- **Rate Limiting**: Tambahkan jeda antara permintaan untuk tidak membebani server
- **Identifikasi Diri**: Gunakan User-Agent yang jelas mengidentifikasi bot Anda
- **Data Pribadi**: Hindari mengumpulkan data pribadi tanpa izin

---

## API (Application Programming Interface)

### REST API

REST (Representational State Transfer) adalah arsitektur yang paling umum digunakan untuk membangun API web. REST API menggunakan protokol HTTP dengan metode:

| Metode HTTP | Operasi CRUD | Deskripsi |
|---|---|---|
| `GET` | Read | Mengambil data dari server |
| `POST` | Create | Mengirim data baru ke server |
| `PUT` | Update | Memperbarui data yang ada (keseluruhan) |
| `PATCH` | Update | Memperbarui data yang ada (sebagian) |
| `DELETE` | Delete | Menghapus data dari server |

### Autentikasi API

| Metode | Deskripsi | Contoh Penggunaan |
|---|---|---|
| **API Key** | Token unik yang dikirim di header atau query string | Twitter API, OpenWeatherMap |
| **OAuth 2.0** | Standar otorisasi delegasi yang aman | Google API, GitHub API |
| **Bearer Token** | Token JWT yang dikirim di header Authorization | Banyak REST API modern |
| **Basic Auth** | Username:password yang di-encode Base64 | API internal, legacy systems |

### Rate Limiting

Rate limiting adalah pembatasan jumlah permintaan API dalam periode waktu tertentu. Strategi penanganannya:
- Pantau header respons: `X-RateLimit-Remaining`, `X-RateLimit-Reset`
- Implementasikan **exponential backoff**: tunggu 1s, 2s, 4s, 8s... setelah error 429
- Gunakan **caching** untuk mengurangi permintaan berulang
- Manfaatkan **batch endpoint** jika tersedia

---

## Data Streaming

### Konsep Data Streaming

Data streaming adalah pengumpulan dan pemrosesan data secara terus-menerus dan real-time seiring data dihasilkan. Berbeda dengan batch processing yang memproses data yang sudah terkumpul, streaming memproses setiap rekaman saat tiba.

### Apache Kafka

Apache Kafka adalah platform streaming terdistribusi yang banyak digunakan untuk membangun pipeline data real-time. Kafka menggunakan model **publish-subscribe**:

```
+------------------+        +----------------+        +------------------+
|                  |        |                |        |                  |
|    PRODUCERS     +------->+   KAFKA        +------->+    CONSUMERS     |
|                  |        |   BROKER       |        |                  |
|  - Sensor IoT    |        |  +----------+  |        |  - Spark         |
|  - Log Server    |        |  |  Topic A  |  |        |    Streaming     |
|  - Aplikasi Web  |        |  |  Topic B  |  |        |  - Database      |
|  - Database CDC  |        |  |  Topic C  |  |        |  - Dashboard     |
|                  |        |  +----------+  |        |                  |
+------------------+        +----------------+        +------------------+
             Publish                                       Subscribe
```

### Konsep Producer-Consumer

- **Producer**: Komponen yang menghasilkan dan mengirim data ke Kafka topic
- **Consumer**: Komponen yang membaca dan memproses data dari Kafka topic
- **Topic**: Kategori atau feed name untuk data streaming
- **Partition**: Pembagian topic untuk paralelisme dan skalabilitas
- **Consumer Group**: Sekelompok consumer yang bekerja sama memproses pesan

---

## Pipeline Pengumpulan Data

```
+-------------+     +-------------+     +---------------+     +------------+
|             |     |             |     |               |     |            |
|  SUMBER     |     |  SCRAPER /  |     |   CLEANING &  |     |  STORAGE   |
|  DATA       +---->+  API CLIENT +---->+   VALIDATION  +---->+            |
|             |     |             |     |               |     |            |
|  - Website  |     |  - Requests |     |  - Dedup      |     |  - CSV     |
|  - REST API |     |  - BS4      |     |  - Validate   |     |  - JSON    |
|  - Kafka    |     |  - Scrapy   |     |  - Transform  |     |  - SQL DB  |
|  - IoT      |     |  - Kafka    |     |  - Normalize  |     |  - HDFS    |
|             |     |  Consumer   |     |               |     |  - S3      |
+-------------+     +-------------+     +---------------+     +------------+
```

---

## Perbandingan Metode Pengumpulan Data

| Kriteria | Web Scraping | REST API | Data Streaming |
|---|---|---|---|
| **Kesulitan** | Sedang | Mudah | Tinggi |
| **Keandalan** | Rendah (bergantung struktur HTML) | Tinggi (kontrak API) | Tinggi |
| **Legalitas** | Perlu dicek per website | Umumnya diizinkan | Diizinkan |
| **Kecepatan** | Lambat-Sedang | Sedang | Real-time |
| **Volume Data** | Terbatas | Terbatas (rate limit) | Sangat besar |
| **Format Data** | HTML → perlu parsing | JSON/XML terstruktur | Berbagai format |
| **Biaya** | Rendah | Sering gratis/berbayar | Infrastruktur lebih mahal |
| **Pembaruan** | Bergantung jadwal scraping | Polling atau Webhook | Terus-menerus |
| **Contoh Tools** | BeautifulSoup, Scrapy | Requests, httpx | Kafka, Spark Streaming |

---

## Pertimbangan Hukum dan Etika

### Aspek Hukum

Pengumpulan data web memiliki implikasi hukum yang perlu diperhatikan:

1. **GDPR (General Data Protection Regulation)**: Regulasi Uni Eropa yang mengatur perlindungan data pribadi. Melarang pengumpulan data pribadi tanpa persetujuan eksplisit.
2. **UU PDP Indonesia**: Undang-Undang Perlindungan Data Pribadi yang mengatur pengumpulan, pemrosesan, dan penyimpanan data warga negara Indonesia.
3. **Hak Cipta**: Konten website dilindungi hak cipta. Mengumpulkan dan menggunakan konten secara komersial dapat melanggar hak cipta.
4. **Computer Fraud and Abuse Act (CFAA)**: Di AS, akses tidak sah ke sistem komputer dapat dikenai sanksi hukum.

### Panduan Etis

- **Minimasi Data**: Kumpulkan hanya data yang benar-benar dibutuhkan
- **Transparansi**: Jelaskan tujuan pengumpulan data
- **Consent**: Dapatkan persetujuan jika mengumpulkan data pribadi
- **Anonimisasi**: Anonimkan data pribadi sebelum analisis
- **Penghapusan**: Hapus data saat tidak lagi diperlukan
- **Hormat Terhadap Infrastruktur**: Jangan membebani server target dengan permintaan berlebihan

---

## Ringkasan

Pengumpulan data adalah pondasi dari setiap proyek Big Data Analytics. Pemilihan metode yang tepat bergantung pada sumber data, volume yang dibutuhkan, dan persyaratan waktu nyata. Poin-poin kunci:

1. **Web Scraping** cocok untuk data publik yang tidak tersedia melalui API, namun rentan terhadap perubahan struktur HTML.
2. **REST API** adalah metode yang lebih andal dan terstruktur untuk mengakses data dari layanan yang menyediakan antarmuka API.
3. **Data Streaming** diperlukan untuk kasus penggunaan real-time seperti analisis media sosial, monitoring sistem, atau deteksi fraud.
4. Selalu perhatikan **etika dan aspek hukum** sebelum mengumpulkan data dari sumber eksternal.

---

## Referensi

1. Mitchell, R. (2018). *Web Scraping with Python* (2nd ed.). O'Reilly Media.
2. Richardson, L., Amundsen, M., & Ruby, S. (2013). *RESTful Web APIs*. O'Reilly Media.
3. Narkhede, N., Shapira, G., & Palino, T. (2017). *Kafka: The Definitive Guide*. O'Reilly Media.
4. BeautifulSoup Documentation. https://www.crummy.com/software/BeautifulSoup/bs4/doc/
5. Scrapy Documentation. https://docs.scrapy.org/en/latest/
6. Apache Kafka Documentation. https://kafka.apache.org/documentation/
7. Kemenkominfo RI. (2022). *Undang-Undang Nomor 27 Tahun 2022 tentang Perlindungan Data Pribadi*.
