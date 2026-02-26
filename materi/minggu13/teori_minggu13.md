# Minggu 13: Text Analytics & Natural Language Processing (NLP)

## Tujuan Pembelajaran

Setelah mempelajari materi minggu ini, mahasiswa diharapkan mampu:
1. Membedakan Text Analytics dan Natural Language Processing (NLP)
2. Menjelaskan dan mengimplementasikan pipeline NLP secara lengkap
3. Memahami konsep matematis TF-IDF dan penerapannya
4. Memahami berbagai metode Word Embedding (Word2Vec, GloVe, FastText)
5. Membangun sistem analisis sentimen sederhana
6. Menerapkan Topic Modeling menggunakan LDA
7. Mengidentifikasi aplikasi NLP dalam industri

---

## 1. Text Analytics vs NLP

### Text Analytics
Text Analytics adalah proses mengubah teks tidak terstruktur menjadi data terstruktur yang dapat dianalisis secara kuantitatif. Fokusnya pada ekstraksi informasi, pola, dan statistik dari teks.

**Contoh**: Menghitung frekuensi kata, mengkategorikan dokumen, mengekstrak entitas.

### Natural Language Processing (NLP)
NLP adalah cabang kecerdasan buatan yang memungkinkan komputer memahami, menginterpretasi, dan menghasilkan bahasa manusia (alami). NLP lebih dalam dari sekedar analisis teks karena berusaha memahami makna, konteks, dan struktur bahasa.

**Perbedaan Utama:**

| Aspek | Text Analytics | NLP |
|---|---|---|
| Fokus | Statistik & pola teks | Pemahaman makna bahasa |
| Kompleksitas | Relatif lebih sederhana | Lebih kompleks |
| Output | Angka, kategori, frekuensi | Pemahaman semantik |
| Contoh | Analisis frekuensi kata | Mesin penerjemah, chatbot |

---

## 2. Pipeline NLP

```
╔════════════════════════════════════════════════════════════════════╗
║                      PIPELINE NLP LENGKAP                         ║
╠════════════════════════════════════════════════════════════════════╣
║                                                                    ║
║  INPUT TEKS                                                        ║
║  "The quick brown foxes are JUMPING over the lazy dogs!"           ║
║       │                                                            ║
║       ▼                                                            ║
║  ┌────────────────┐                                                ║
║  │  TOKENIZATION  │ → ["The","quick","brown","foxes","are",...]    ║
║  └────────────────┘                                                ║
║       │                                                            ║
║       ▼                                                            ║
║  ┌────────────────┐                                                ║
║  │   LOWERCASING  │ → ["the","quick","brown","foxes","are",...]    ║
║  └────────────────┘                                                ║
║       │                                                            ║
║       ▼                                                            ║
║  ┌────────────────────┐                                            ║
║  │ STOPWORD REMOVAL   │ → ["quick","brown","foxes","jumping",...]  ║
║  └────────────────────┘                                            ║
║       │                                                            ║
║       ▼                                                            ║
║  ┌──────────────────────────┐                                      ║
║  │ STEMMING / LEMMATIZATION │ → ["quick","brown","fox","jump",..] ║
║  └──────────────────────────┘                                      ║
║       │                                                            ║
║       ▼                                                            ║
║  ┌────────────────┐                                                ║
║  │   POS TAGGING  │ → [("quick",JJ),("brown",JJ),("fox",NN),...] ║
║  └────────────────┘                                                ║
║       │                                                            ║
║       ▼                                                            ║
║  ┌──────────────────────────┐                                      ║
║  │  NAMED ENTITY RECOGNITION│ → {PERSON: [...], ORG: [...]}      ║
║  └──────────────────────────┘                                      ║
║       │                                                            ║
║       ▼                                                            ║
║  FITUR / REPRESENTASI VEKTOR (TF-IDF, Word2Vec, dll.)             ║
╚════════════════════════════════════════════════════════════════════╝
```

### 2.1 Tokenization
Proses memecah teks menjadi unit-unit terkecil yang disebut token. Token dapat berupa kata, sub-kata, karakter, atau kalimat.

```
"Saya suka belajar NLP" → ["Saya", "suka", "belajar", "NLP"]
```

### 2.2 Stopword Removal
Stopword adalah kata-kata umum yang tidak membawa makna signifikan seperti "the", "is", "at", "yang", "dan", "di". Menghapus stopword mengurangi noise dan dimensionalitas data.

### 2.3 Stemming & Lemmatization
- **Stemming**: Memotong akhiran kata secara kasar → "running" → "run", "studies" → "studi"
- **Lemmatization**: Mengubah kata ke bentuk dasar yang valid secara linguistik → "running" → "run", "better" → "good"

Lemmatization lebih akurat tetapi lebih lambat karena memerlukan kamus bahasa.

### 2.4 POS Tagging
Part-of-Speech Tagging menandai setiap token dengan label grammatikal: Noun (NN), Verb (VB), Adjective (JJ), Adverb (RB), dll.

### 2.5 Named Entity Recognition (NER)
NER mengidentifikasi dan mengklasifikasikan entitas bernama dalam teks seperti nama orang (PERSON), organisasi (ORG), lokasi (LOC), tanggal (DATE), dan mata uang (MONEY).

---

## 3. TF-IDF (Term Frequency – Inverse Document Frequency)

### Konsep Matematis
TF-IDF adalah metode untuk mengukur seberapa penting sebuah kata dalam sebuah dokumen relatif terhadap kumpulan dokumen (corpus).

**Term Frequency (TF):**
```
TF(t, d) = jumlah kemunculan term t dalam dokumen d
           ─────────────────────────────────────────
           total jumlah term dalam dokumen d
```

**Inverse Document Frequency (IDF):**
```
IDF(t, D) = log( N / df(t) )

  N     = jumlah total dokumen
  df(t) = jumlah dokumen yang mengandung term t
```

**TF-IDF:**
```
TF-IDF(t, d, D) = TF(t, d) × IDF(t, D)
```

**Interpretasi**: Nilai TF-IDF tinggi berarti kata tersebut sering muncul dalam dokumen tertentu tetapi jarang di dokumen lain — artinya kata itu bersifat diskriminatif (penting untuk membedakan dokumen).

### Implementasi
TF-IDF digunakan dalam:
- Information Retrieval (mesin pencari)
- Klasifikasi dokumen
- Ekstraksi kata kunci
- Rekomendasi konten

---

## 4. Word Embeddings

Word Embedding adalah representasi kata sebagai vektor numerik berdimensi rendah dalam ruang kontinu, di mana kata-kata dengan makna serupa memiliki posisi berdekatan.

### 4.1 Word2Vec
Word2Vec (Google, 2013) menggunakan neural network shallow untuk mempelajari representasi kata dari konteksnya.

**Dua Arsitektur:**
- **CBOW (Continuous Bag of Words)**: Memprediksi kata target dari konteks sekitarnya. Lebih cepat, cocok untuk kata frequent.
- **Skip-gram**: Memprediksi konteks dari kata target. Lebih baik untuk kata rare.

```
CBOW:   [quick, brown, ____, lazy] → "fox"
Skip-gram: "fox" → [quick, brown, over, lazy]
```

### 4.2 GloVe (Global Vectors for Word Representation)
GloVe (Stanford, 2014) mempelajari embedding dengan memanfaatkan statistik ko-okurrens global dari seluruh corpus. Menggabungkan kekuatan matrix factorization dan context window methods.

### 4.3 FastText
FastText (Facebook, 2016) memperluas Word2Vec dengan mempelajari embedding pada level sub-kata (character n-grams). Keunggulannya: dapat menangani kata out-of-vocabulary (OOV) dan kata morphologis kompleks.

### Perbandingan Word Embedding Methods

| Metode | Tahun | Granularitas | OOV Handling | Kecepatan Training | Kualitas |
|---|---|---|---|---|---|
| Word2Vec (CBOW) | 2013 | Kata | ❌ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |
| Word2Vec (Skip-gram) | 2013 | Kata | ❌ | ⭐⭐⭐ | ⭐⭐⭐⭐ |
| GloVe | 2014 | Kata | ❌ | ⭐⭐⭐ | ⭐⭐⭐⭐ |
| FastText | 2016 | Sub-kata | ✅ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| BERT Embeddings | 2018 | Kontekstual | ✅ | ⭐ | ⭐⭐⭐⭐⭐ |

---

## 5. Sentiment Analysis

### Pendekatan Lexicon-Based
Menggunakan kamus sentimen (sentiment lexicon) yang berisi daftar kata positif dan negatif beserta skornya. VADER (Valence Aware Dictionary and sEntiment Reasoner) adalah salah satu lexicon yang populer untuk bahasa Inggris.

**Kelebihan**: Tidak perlu training data, cepat, transparan.
**Kekurangan**: Tidak menangkap konteks, sarkasme, atau bahasa domain-spesifik.

### Pendekatan Machine Learning
Melatih model klasifikasi pada data berlabel (positif/negatif/netral). Pipeline umum: TF-IDF features → Logistic Regression / SVM / Naive Bayes.

**Kelebihan**: Adaptif terhadap domain, akurasi lebih tinggi dengan data cukup.
**Kekurangan**: Membutuhkan labeled training data.

### Pendekatan Deep Learning
Menggunakan arsitektur LSTM, BERT, atau Transformer yang mampu memahami konteks dan nuansa bahasa. Memberikan akurasi tertinggi namun membutuhkan sumber daya komputasi besar.

---

## 6. Topic Modeling dengan LDA

**Latent Dirichlet Allocation (LDA)** adalah model probabilistik yang mengasumsikan setiap dokumen merupakan campuran dari beberapa topik, dan setiap topik merupakan distribusi probabilitas atas kata-kata.

**Algoritma LDA:**
1. Tentukan jumlah topik K
2. Untuk setiap dokumen d, ambil distribusi topik θ_d ~ Dirichlet(α)
3. Untuk setiap topik k, ambil distribusi kata φ_k ~ Dirichlet(β)
4. Untuk setiap kata dalam dokumen, pilih topik z ~ Multinomial(θ_d), lalu pilih kata w ~ Multinomial(φ_z)
5. Inferensi menggunakan Variational Bayes atau Gibbs Sampling

**Output LDA**: Setiap topik direpresentasikan oleh kata-kata dengan probabilitas tertinggi.

---

## 7. Aplikasi NLP dalam Industri

### Chatbot & Virtual Assistant
Chatbot menggunakan NLP untuk memahami intent pengguna (intent recognition), mengekstrak entitas (NER), mengelola konteks percakapan, dan menghasilkan respons yang tepat. Contoh: Siri, Google Assistant, customer service bot.

### Search Engine
Mesin pencari menggunakan NLP untuk query understanding, document ranking (TF-IDF, BM25), question answering, dan semantic search. Google menggunakan BERT untuk memahami query secara kontekstual.

### Recommendation System
Platform seperti Netflix, Spotify, dan e-commerce menggunakan NLP untuk menganalisis deskripsi konten, review pengguna, dan preferensi teks untuk memberikan rekomendasi yang relevan.

---

## Ringkasan

Minggu ini kita mempelajari fondasi Text Analytics dan NLP — mulai dari pipeline dasar (tokenization hingga NER), representasi teks (TF-IDF, Word Embeddings), hingga aplikasi praktis seperti sentiment analysis dan topic modeling. Word embeddings merevolusi NLP dengan mengubah kata menjadi vektor numerik yang mampu menangkap hubungan semantik. LDA memungkinkan kita menemukan topik tersembunyi dalam kumpulan dokumen besar tanpa pelabelan manual. Kemampuan NLP kini menjadi fondasi dari hampir semua produk AI modern.

---

## Referensi

1. Jurafsky, D. & Martin, J. H. (2023). *Speech and Language Processing, 3rd Edition*. Stanford.
2. Manning, C. D. & Schütze, H. (1999). *Foundations of Statistical Natural Language Processing*. MIT Press.
3. Mikolov, T., et al. (2013). *Efficient Estimation of Word Representations in Vector Space*. arXiv:1301.3781.
4. Blei, D. M., Ng, A. Y., & Jordan, M. I. (2003). *Latent Dirichlet Allocation*. JMLR, 3, 993–1022.
5. NLTK Documentation: https://www.nltk.org/
6. Gensim Documentation: https://radimrehurek.com/gensim/
7. Scikit-learn Text Feature Extraction: https://scikit-learn.org/stable/modules/feature_extraction.html
