# Minggu 11: Visualisasi Data untuk Big Data

## Tujuan Pembelajaran

Setelah mempelajari materi minggu ini, mahasiswa diharapkan mampu:
1. Memahami prinsip-prinsip visualisasi data yang efektif dan komunikatif
2. Membuat berbagai tipe visualisasi menggunakan Matplotlib
3. Menghasilkan visualisasi statistik yang kaya menggunakan Seaborn
4. Membangun visualisasi interaktif menggunakan Plotly
5. Memilih tipe visualisasi yang tepat berdasarkan tipe dan konteks data
6. Menerapkan teknik visualisasi khusus untuk dataset berukuran besar (Big Data)

---

## 1. Prinsip Visualisasi Data yang Baik

Visualisasi data bukan sekadar membuat grafik yang indah — ini adalah seni mengkomunikasikan informasi secara akurat, efisien, dan efektif.

### 1.1 Prinsip Utama (Edward Tufte)

1. **Data-Ink Ratio:** Maksimalkan rasio tinta yang merepresentasikan data. Hilangkan elemen dekoratif yang tidak menambah informasi (chartjunk).
2. **Graphical Integrity:** Representasi visual harus proporsional dengan angka yang diwakili. Sumbu yang dipotong atau skala yang menyesatkan harus dihindari.
3. **Clarity:** Pesan utama harus jelas dalam 5 detik pertama.
4. **Context:** Sertakan konteks yang cukup — label, judul, unit, sumber data.

### 1.2 Prinsip Desain Visual

- **Proximity:** Elemen terkait harus berdekatan
- **Color:** Gunakan warna secara bermakna; pertimbangkan aksesibilitas (color-blind friendly)
- **Hierarchy:** Tentukan informasi mana yang paling penting; beri penekanan visual
- **Simplicity:** "Less is more" — hindari kompleksitas yang tidak perlu

### 1.3 Kesalahan Umum yang Harus Dihindari

- Pie chart dengan terlalu banyak slice (>5)
- 3D chart yang mendistorsi persepsi
- Dual axis yang tidak konsisten
- Warna yang terlalu mencolok atau tidak bermakna
- Hilangnya label, judul, atau unit

---

## 2. Matplotlib

Matplotlib adalah library visualisasi dasar Python yang menjadi fondasi banyak library lainnya. Diinspirasi oleh MATLAB.

### 2.1 Arsitektur Matplotlib

```
Figure  ─── Container utama (seluruh gambar)
  └── Axes  ─── Area plot (bisa ada banyak per Figure)
        ├── Axis  ─── Sumbu X dan Y
        ├── Title, Labels
        ├── Ticks & Tick Labels
        └── Artists (lines, patches, text, dll.)
```

### 2.2 Dua Interface Matplotlib

**Pyplot Interface (Implicit):** Mudah digunakan, cocok untuk eksplorasi cepat:
```python
plt.plot(x, y)
plt.title("Judul")
plt.show()
```

**Object-Oriented Interface (Explicit):** Lebih kontrol, cocok untuk produksi:
```python
fig, ax = plt.subplots()
ax.plot(x, y)
ax.set_title("Judul")
plt.show()
```

### 2.3 Tipe Plot Matplotlib

| Plot | Fungsi | Kegunaan |
|------|--------|---------|
| Line Plot | `ax.plot()` | Tren data kontinu, time series |
| Bar Chart | `ax.bar()` | Perbandingan kategori |
| Scatter Plot | `ax.scatter()` | Hubungan dua variabel numerik |
| Histogram | `ax.hist()` | Distribusi satu variabel |
| Box Plot | `ax.boxplot()` | Distribusi & outlier per kategori |
| Pie Chart | `ax.pie()` | Proporsi bagian dari keseluruhan |
| Heatmap | `ax.imshow()` | Matriks data (gunakan seaborn) |
| Area Chart | `ax.fill_between()` | Tren dengan penekanan volume |

---

## 3. Seaborn

Seaborn dibangun di atas Matplotlib dan menyediakan API yang lebih tinggi untuk membuat visualisasi statistik yang indah dengan kode yang lebih sedikit.

### 3.1 Tema dan Estetika

```python
sns.set_theme(style="whitegrid")  # Gaya: darkgrid, whitegrid, dark, white, ticks
sns.set_palette("husl")           # Palet warna
```

### 3.2 Kategori Visualisasi Seaborn

**Distributional Plots:**
- `sns.histplot()` — Histogram dengan estimasi KDE
- `sns.kdeplot()` — Kernel Density Estimation
- `sns.rugplot()` — Distribusi marginal

**Categorical Plots:**
- `sns.boxplot()` — Box dan whisker
- `sns.violinplot()` — Distribusi + KDE
- `sns.stripplot()` / `sns.swarmplot()` — Titik individual
- `sns.barplot()` / `sns.countplot()` — Bar dengan estimasi statistik

**Relational Plots:**
- `sns.scatterplot()` — Scatter dengan encoding semantik
- `sns.lineplot()` — Line dengan CI

**Matrix Plots:**
- `sns.heatmap()` — Visualisasi matriks korelasi/confusion matrix
- `sns.clustermap()` — Hierarchical clustering dengan heatmap

**Multi-plot Grids:**
- `sns.pairplot()` — Matriks scatter semua pasang variabel
- `sns.FacetGrid()` — Grid plot kondisional
- `sns.jointplot()` — Joint distribution + marginal

---

## 4. Plotly

Plotly adalah library untuk membuat visualisasi interaktif yang dapat dieksplorasi pengguna secara langsung (zoom, pan, hover, filter).

### 4.1 Plotly Express vs Graph Objects

**Plotly Express (`plotly.express`):**
- API tingkat tinggi yang sederhana
- Cocok untuk eksplorasi cepat
- Kurang fleksibel untuk kustomisasi kompleks

```python
import plotly.express as px
fig = px.scatter(df, x="gdpPercap", y="lifeExp", color="continent", size="pop")
fig.show()
```

**Graph Objects (`plotly.graph_objects`):**
- API tingkat rendah yang sangat fleksibel
- Cocok untuk visualisasi dan dashboard yang dikustomisasi
- Verbose namun powerful

### 4.2 Fitur Interaktivitas Plotly

- **Hover:** Tampilkan informasi saat kursor melayang
- **Zoom & Pan:** Eksplorasi area tertentu
- **Click to filter:** Klik legenda untuk show/hide trace
- **Rangeslider:** Navigasi time series
- **Dropdown & Sliders:** Animasi dan filter interaktif

### 4.3 Plotly Dash

Untuk membangun aplikasi analitik web interaktif penuh menggunakan Python, Plotly Dash adalah solusinya. Memungkinkan pembuatan dashboard analitik tanpa menulis HTML/CSS/JavaScript.

---

## 5. Panduan Pemilihan Visualisasi

```
PANDUAN PEMILIHAN TIPE VISUALISASI
════════════════════════════════════════════════════════
  Pertanyaan: APA yang ingin Anda tampilkan?
  │
  ├── KOMPOSISI (Bagian dari keseluruhan?)
  │     ├── Satu titik waktu  →  Pie Chart, Donut, Treemap
  │     └── Berubah waktu     →  Stacked Bar, Stacked Area
  │
  ├── DISTRIBUSI (Bagaimana data tersebar?)
  │     ├── Satu variabel     →  Histogram, KDE, Box Plot
  │     └── Dua variabel      →  Scatter Plot, Hexbin, 2D KDE
  │
  ├── PERBANDINGAN (Membandingkan antar kategori?)
  │     ├── Beberapa item     →  Bar Chart, Column Chart
  │     └── Banyak item       →  Heatmap, Dot Plot
  │
  ├── HUBUNGAN (Relasi antar variabel?)
  │     ├── Dua variabel num. →  Scatter Plot, Line Chart
  │     ├── Tiga variabel     →  Bubble Chart
  │     └── Banyak variabel   →  Pair Plot, Parallel Coordinates
  │
  └── TREN / WAKTU (Perubahan sepanjang waktu?)
        ├── Satu seri         →  Line Chart, Area Chart
        └── Banyak seri       →  Multi-line, Small Multiples
════════════════════════════════════════════════════════
```

---

## 6. Pilihan Visualisasi Berdasarkan Tipe Data

| Tipe Data | Jumlah Variabel | Visualisasi Rekomendasi |
|-----------|----------------|------------------------|
| Numerik kontinu | 1 | Histogram, KDE, Box Plot |
| Numerik kontinu | 2 | Scatter Plot, Hexbin |
| Numerik kontinu | 3+ | Pair Plot, Heatmap korelasi |
| Kategorikal | 1 | Bar Chart, Pie Chart |
| Kategorikal | 2 | Grouped Bar, Heatmap |
| Numerik + Kategorikal | 1+1 | Box Plot, Violin Plot |
| Time Series | 1 | Line Chart, Area Chart |
| Geospasial | - | Choropleth, Scatter Map |
| Hierarkis | - | Treemap, Sunburst |
| Network | - | Network Graph, Chord Diagram |

---

## 7. Dashboard dan Storytelling dengan Data

### 7.1 Prinsip Dashboard

Sebuah dashboard yang efektif harus:
- **Fokus:** Setiap dashboard punya satu tujuan utama
- **Hierarki:** KPI utama di atas, detail di bawah
- **Konsistensi:** Gunakan skema warna dan format yang konsisten
- **Actionable:** Tampilkan informasi yang mendorong tindakan

### 7.2 Data Storytelling

Data storytelling adalah kemampuan untuk mengkomunikasikan insight dari data dalam bentuk narasi yang meyakinkan:

1. **Context:** Siapa audiensnya? Apa yang ingin mereka ketahui?
2. **Data:** Pilih data yang relevan dan akurat
3. **Narrative:** Bangun cerita — masalah, temuan, rekomendasi
4. **Visuals:** Pilih visualisasi yang mendukung narasi

---

## 8. Tools Visualisasi Lainnya

| Tool | Tipe | Keunggulan | Kekurangan |
|------|------|-----------|------------|
| **Tableau** | BI/Dashboard | Drag-drop intuitif, cepat | Mahal, kurang fleksibel untuk kustom |
| **Power BI** | BI/Dashboard | Integrasi Microsoft, murah | Kurang powerful dibanding Tableau |
| **D3.js** | JavaScript library | Kustomisasi penuh | Kurva belajar sangat tinggi |
| **Bokeh** | Python | Interaktif, web-native | Kurang populer dari Plotly |
| **Altair** | Python | Grammar of Graphics, deklaratif | Dataset kecil (default) |
| **Streamlit** | Python | Cepat membuat web app | Kurang fleksibel untuk produksi |

---

## 9. Visualisasi untuk Big Data: Tantangan dan Solusi

Memvisualisasikan dataset berukuran besar (jutaan baris) membawa tantangan tersendiri:

**Tantangan:**
- Overplotting: Titik-titik saling menutupi
- Rendering lambat: Jutaan elemen visual
- Memory: Tidak semua data muat di RAM

**Solusi:**
- **Sampling:** Ambil sampel representatif (stratified sampling)
- **Aggregasi:** Agregasi data sebelum plotting (binning, groupby)
- **Density plots:** Hexbin, 2D histogram, KDE
- **Datashader:** Library untuk rendering dataset sangat besar
- **WebGL rendering:** Plotly dengan WebGL untuk performa lebih baik
- **Progressive loading:** Load dan render data secara bertahap

---

## 10. Ringkasan

Visualisasi data adalah jembatan antara data mentah dan pengambilan keputusan. Minggu ini kita telah membahas:

- **Prinsip visualisasi:** Data-ink ratio, integritas grafis, kejelasan pesan
- **Matplotlib:** Fondasi visualisasi Python dengan kontrol penuh
- **Seaborn:** Visualisasi statistik yang indah dengan kode minimal
- **Plotly:** Interaktivitas yang meningkatkan eksplorasi data
- **Panduan pemilihan:** Cara memilih visualisasi yang tepat untuk data yang tepat
- **Big Data challenges:** Sampling, agregasi, dan density plots untuk dataset besar

---

## Referensi

1. Tufte, E. R. (2001). *The Visual Display of Quantitative Information* (2nd ed.). Graphics Press.
2. Few, S. (2012). *Show Me the Numbers: Designing Tables and Graphs to Enlighten* (2nd ed.). Analytics Press.
3. Knaflic, C. N. (2015). *Storytelling with Data*. Wiley.
4. Matplotlib Documentation: https://matplotlib.org/stable/contents.html
5. Seaborn Documentation: https://seaborn.pydata.org/
6. Plotly Documentation: https://plotly.com/python/
7. VanderPlas, J. (2016). *Python Data Science Handbook*. O'Reilly Media.
