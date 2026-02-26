# Minggu 14: Deep Learning untuk Big Data

## Tujuan Pembelajaran

Setelah mempelajari materi minggu ini, mahasiswa diharapkan mampu:
1. Memahami konsep dasar Neural Network: neuron, layer, dan fungsi aktivasi
2. Menjelaskan mekanisme backpropagation dan berbagai optimizer
3. Memahami arsitektur CNN dan aplikasinya pada data gambar
4. Memahami arsitektur RNN, permasalahan vanishing gradient, serta solusinya (LSTM, GRU)
5. Menerapkan konsep Transfer Learning menggunakan model pre-trained
6. Memahami teknik regularisasi untuk mencegah overfitting

---

## 1. Neural Network Dasar

### Neuron Biologis vs Artificial Neuron
Artificial Neural Network (ANN) terinspirasi dari neuron biologis di otak manusia. Setiap neuron artifisial menerima input, melakukan transformasi linear, menerapkan fungsi aktivasi, dan menghasilkan output.

```
INPUT → [ Σ(w·x + b) ] → Activation Function → OUTPUT
```

Di mana:
- **w** = bobot (weights) — seberapa penting setiap input
- **x** = nilai input
- **b** = bias — nilai offset
- **Activation Function** = fungsi nonlinear yang memungkinkan jaringan mempelajari pola kompleks

### Arsitektur Neural Network

```
╔═══════════════════════════════════════════════════════════════════╗
║              ARSITEKTUR NEURAL NETWORK (MLP)                      ║
╠═══════════════════════════════════════════════════════════════════╣
║                                                                   ║
║  INPUT LAYER    HIDDEN LAYER 1   HIDDEN LAYER 2   OUTPUT LAYER   ║
║                                                                   ║
║    ●───────────▶●──────────────▶●──────────────▶●  Class A       ║
║    ●           ●               ●               ●  Class B       ║
║    ●───────────▶●──────────────▶●──────────────▶●  Class C       ║
║    ●           ●               ●                                 ║
║    ●───────────▶●──────────────▶●                                ║
║                ●               ●                                 ║
║                ●               ●                                 ║
║                                                                   ║
║  [x1,x2,...xn]  [ReLU neurons]  [ReLU neurons]  [Softmax]        ║
║                                                                   ║
║  Forward Pass: → → → → → → → → → → → → → →                      ║
║  Backward Pass (Backprop): ← ← ← ← ← ← ← ← ←                  ║
╚═══════════════════════════════════════════════════════════════════╝
```

### Fungsi Aktivasi

**1. ReLU (Rectified Linear Unit)**
```
f(x) = max(0, x)
```
Paling populer untuk hidden layers. Sederhana, efisien, dan mengatasi vanishing gradient. Masalah: "dying ReLU" bila banyak neuron bernilai 0.

**2. Sigmoid**
```
f(x) = 1 / (1 + e^(-x))   output ∈ (0, 1)
```
Digunakan untuk output binary classification. Masalah: saturasi di nilai ekstrem menyebabkan vanishing gradient.

**3. Tanh (Hyperbolic Tangent)**
```
f(x) = (e^x - e^(-x)) / (e^x + e^(-x))   output ∈ (-1, 1)
```
Versi centered dari sigmoid. Lebih baik dari sigmoid untuk hidden layers.

**4. Softmax**
```
f(x_i) = e^(x_i) / Σ e^(x_j)   output ∈ (0,1), Σoutput = 1
```
Digunakan untuk multi-class classification output layer. Mengubah logit menjadi distribusi probabilitas.

---

## 2. Backpropagation & Gradient Descent

### Backpropagation
Backpropagation adalah algoritma untuk menghitung gradient loss function terhadap setiap parameter (bobot) jaringan menggunakan chain rule kalkulus. Gradient ini digunakan untuk memperbarui bobot agar loss berkurang.

**Proses:**
1. **Forward Pass**: Hitung prediksi dan loss
2. **Backward Pass**: Hitung gradient ∂Loss/∂w untuk setiap bobot
3. **Weight Update**: w = w - η × ∂Loss/∂w (di mana η = learning rate)

### Optimizer

**SGD (Stochastic Gradient Descent)**
```
w = w - η × ∇L(w)
```
Sederhana namun tidak stabil, bisa terjebak di local minima. Dapat ditambahkan momentum untuk mempercepat konvergensi.

**Adam (Adaptive Moment Estimation)**
```
m_t = β1 × m_{t-1} + (1 - β1) × g_t        (first moment)
v_t = β2 × v_{t-1} + (1 - β2) × g_t²       (second moment)
w = w - η × m̂_t / (√v̂_t + ε)
```
Menggabungkan Momentum dan RMSprop. Paling populer saat ini.

### Perbandingan Optimizer

| Optimizer | Adaptif | Konvergensi | Memory | Rekomendasi |
|---|---|---|---|---|
| SGD | ❌ | Lambat | Rendah | Ketika ada scheduled LR |
| SGD + Momentum | ❌ | Lebih cepat | Rendah | CV tasks dengan fine-tuning |
| RMSprop | ✅ | Baik | Sedang | RNN, time series |
| Adam | ✅ | Cepat | Sedang | Default untuk sebagian besar kasus |
| AdamW | ✅ | Cepat | Sedang | NLP tasks, Transformer |

---

## 3. Convolutional Neural Network (CNN)

CNN dirancang khusus untuk data grid (gambar, sinyal). CNN menggunakan operasi konvolusi untuk mengekstrak fitur spasial secara hierarkis.

### Komponen CNN

**1. Convolutional Layer**
Menerapkan filter (kernel) yang bergeser melintasi input untuk menghasilkan feature map. Filter mempelajari fitur seperti tepi, tekstur, dan pola.

```
Input 5×5 × Filter 3×3 → Feature Map 3×3 (stride=1, no padding)
```

**2. Activation (ReLU)**
Diterapkan setelah setiap conv layer untuk nonlinearitas.

**3. Pooling Layer**
Mengurangi dimensi spasial. Max Pooling mengambil nilai maksimum di setiap region → mempertahankan fitur paling menonjol.

**4. Flatten**
Mengubah feature map 3D menjadi vektor 1D untuk disambungkan ke fully-connected layers.

**5. Fully Connected Layers**
Melakukan klasifikasi berdasarkan fitur yang telah diekstrak.

### Arsitektur CNN Populer
- **LeNet-5** (1998): Pioneer CNN untuk digit recognition
- **AlexNet** (2012): Memenangkan ImageNet, deep CNN dengan GPU
- **VGGNet** (2014): Sangat dalam (16–19 layer) dengan filter 3×3
- **ResNet** (2015): Memperkenalkan skip connections untuk melatih jaringan sangat dalam (50–152+ layer)

---

## 4. Recurrent Neural Network (RNN)

RNN dirancang untuk data sekuensial (teks, time series, audio) dengan mempertahankan "memori" dari input sebelumnya melalui hidden state yang diumpankan kembali.

### Masalah Vanishing Gradient
Dalam RNN standar, gradient yang dipropagasi mundur melalui banyak langkah waktu menjadi sangat kecil (vanishing) atau sangat besar (exploding). Akibatnya, RNN standar sulit mempelajari dependensi jangka panjang.

### LSTM (Long Short-Term Memory)
LSTM (Hochreiter & Schmidhuber, 1997) mengatasi vanishing gradient dengan menggunakan mekanisme gating:
- **Forget Gate**: Memutuskan informasi lama apa yang dihapus dari cell state
- **Input Gate**: Memutuskan informasi baru apa yang ditambahkan ke cell state
- **Output Gate**: Memutuskan bagian cell state apa yang dioutputkan

### GRU (Gated Recurrent Unit)
GRU (Cho et al., 2014) adalah versi LSTM yang lebih sederhana dengan dua gate (reset dan update). Performa serupa dengan LSTM tetapi lebih efisien secara komputasi.

---

## 5. Transfer Learning

Transfer Learning adalah teknik memanfaatkan pengetahuan yang telah dipelajari model pada satu tugas (source task) untuk menyelesaikan tugas berbeda namun terkait (target task).

### Konsep
Melatih deep neural network dari nol membutuhkan data berlabel sangat banyak dan waktu komputasi lama. Transfer learning mengatasi ini dengan:
1. Menggunakan model yang sudah pre-trained pada dataset besar (e.g., ImageNet, Wikipedia)
2. Membekukan (freeze) layer bawah yang mengandung fitur general
3. Melatih ulang hanya beberapa layer atas (fine-tuning) dengan data target

### Model Pre-trained Populer

**Computer Vision:**
- **VGG16/VGG19**: Arsitektur sederhana dan dalam
- **ResNet50/101**: Residual connections, sangat populer
- **MobileNetV2**: Ringan untuk mobile/edge deployment
- **EfficientNet**: Efisien dengan akurasi tinggi

**NLP:**
- **BERT**: Bidirectional Transformer dari Google
- **GPT series**: Generative model dari OpenAI
- **RoBERTa**: Versi BERT yang dioptimasi

---

## 6. Regularization Techniques

### Dropout
Secara acak menonaktifkan sejumlah neuron selama training. Mencegah neuron co-adapting terlalu kuat dan mendorong feature learning yang lebih robust.

```python
model.add(Dense(256, activation='relu'))
model.add(Dropout(0.5))  # 50% neuron dinonaktifkan saat training
```

### Batch Normalization
Menormalisasi output setiap layer agar memiliki mean ≈ 0 dan variance ≈ 1 pada setiap mini-batch. Manfaat: mempercepat training, memungkinkan learning rate lebih tinggi, sedikit efek regularisasi.

### Early Stopping
Menghentikan training ketika performa pada validation set tidak meningkat lagi selama N epoch (patience). Mencegah overfitting dan menghemat waktu komputasi.

### L1 & L2 Regularization
- **L2 (Ridge/Weight Decay)**: Menambahkan λ × Σw² ke loss function, mendorong bobot kecil
- **L1 (Lasso)**: Menambahkan λ × Σ|w| ke loss, mendorong sparsity

---

## Ringkasan

Deep learning telah merevolusi kemampuan machine learning dalam berbagai domain — dari computer vision hingga NLP dan time series. CNN mengekstrak fitur spasial hierarkis dari gambar, sedangkan LSTM/GRU menangani dependensi jangka panjang dalam data sekuensial. Transfer learning memungkinkan praktisi membangun model berkinerja tinggi tanpa sumber daya komputasi besar dengan memanfaatkan model pre-trained. Teknik regularisasi seperti dropout dan batch normalization sangat penting untuk melatih model yang generalisasi dengan baik. Penguasaan deep learning adalah kompetensi krusial bagi data scientist modern di era Big Data.

---

## Referensi

1. Goodfellow, I., Bengio, Y., & Courville, A. (2016). *Deep Learning*. MIT Press. (deeplearningbook.org)
2. LeCun, Y., Bengio, Y., & Hinton, G. (2015). Deep learning. *Nature*, 521, 436–444.
3. He, K., et al. (2016). *Deep Residual Learning for Image Recognition*. CVPR.
4. Hochreiter, S. & Schmidhuber, J. (1997). Long Short-Term Memory. *Neural Computation*, 9(8), 1735–1780.
5. Devlin, J., et al. (2019). *BERT: Pre-training of Deep Bidirectional Transformers*. NAACL.
6. TensorFlow/Keras Documentation: https://www.tensorflow.org/api_docs
7. PyTorch Documentation: https://pytorch.org/docs/
