# 📊 Benchmarking Algoritma Machine Learning dan Arsitektur Deep Learning untuk Klasifikasi Sentimen Ulasan Aplikasi Donghub

## 📺 Project Overview
Proyek ini bertujuan untuk melakukan **benchmarking algoritma Machine Learning (ML)** dan **arsitektur Deep Learning (DL)** pada tugas klasifikasi sentimen ulasan aplikasi **Donghub** yang diambil dari Google Play Store.  
Analisis dilakukan untuk memahami pola persepsi pengguna, sekaligus membandingkan kinerja berbagai pendekatan klasifikasi.

---

## 💼 Business Understanding
Aplikasi **Donghub** merupakan platform streaming animasi (donghua) yang populer di kalangan pengguna Indonesia. Rating dan ulasan pengguna di Google Play mencerminkan pengalaman mereka terhadap kualitas layanan, performa aplikasi, hingga pengalaman menonton.

Insight dari klasifikasi sentimen dapat digunakan untuk:
- Mengidentifikasi masalah utama aplikasi  
- Mengukur persepsi positif/negatif pengguna  
- Memberikan masukan berbasis data untuk pengembangan produk  

---

## 📁 Dataset
- **Sumber:** Scraping Google Play Store menggunakan `google_play_scraper`  
- **Spesifikasi:**
  - Aplikasi: `com.anichin.donghub`
  - Bahasa: Indonesia (`lang='id'`)
  - Negara: Indonesia (`country='id'`)
  - Jumlah ulasan: 50.000 ulasan
  - Penyortiran: `Sort.MOST_RELEVANT`

---

## 📊 Kondisi Data
- **Jumlah data akhir:** 19.318 ulasan  
- **Variabel:**
  - `reviewId`, `userName`, `userImage`, `content`, `score`, `thumbsUpCount`
  - `reviewCreatedVersion`, `at`, `replyContent`, `repliedAt`, `appVersion`

- **Distribusi rating:**
  - ⭐ 1: 25.40%  
  - ⭐ 2: 6.72%  
  - ⭐ 3: 7.48%  
  - ⭐ 4: 7.95%  
  - ⭐ 5: 52.45%  

---

## 🧹 Data Preprocessing
1. **Pembersihan Data Awal**
   - Hapus duplikat pada kolom konten  
   - Hapus ulasan dengan panjang <3 kata  
   - Filter ulasan berbahasa Indonesia (`langdetect`)  

2. **Normalisasi Teks**
   - Konversi ke huruf kecil  
   - Hapus URL, angka, dan karakter khusus  
   - Normalisasi slang Indonesia (contoh: `gk` → `tidak`, `bgt` → `banget`)  

3. **Pemrosesan NLP**
   - Hapus stopwords bahasa Indonesia (NLTK)  
   - Fitur ML: TF-IDF (`max_features=5000`, `ngram_range=(1,2)`)  
   - Fitur DL: Tokenizer (`max_words=10000`, `max_len=100`)  

4. **Penanganan Imbalance Data**
   - ML: **SMOTE**  
   - DL: **Random Oversampling**  

5. **Labeling Sentimen**
   - Negatif: Rating 1–2  
   - Positif: Rating 4–5  
   - Netral (Rating 3) dihapus  

6. **Split Data**
   - Train: 70%  
   - Validation: 15%  
   - Test: 15%  

---

## 🤖 Modeling

### Machine Learning Models
- **Logistic Regression**
  - `max_iter=500`, `solver='lbfgs'`, `class_weight='balanced'`
- **Support Vector Machine (SVM)**
  - `kernel='linear'`, `class_weight='balanced'`
- **Naïve Bayes**
  - `MultinomialNB`
- **Random Forest**
  - `n_estimators=300`, `max_depth=None`, `class_weight='balanced'`

### Deep Learning Models
- **Common Parameters:**
  - Embedding dimension: 128  
  - Optimizer: Adam  
  - Loss: `binary_crossentropy`  
  - Early Stopping: `patience=3`  

- **Arsitektur:**
  - **LSTM:** Embedding → BiLSTM(128) → Dropout(0.5) → Dense(64) → Dropout(0.3) → Output  
  - **GRU:** Embedding → BiGRU(128) → Dropout(0.5) → Dense(64) → Dropout(0.3) → Output  
  - **CNN:** Embedding → Conv1D(128, kernel=5) → GlobalMaxPooling → Dense(64) → Dropout(0.3) → Output  
  - **DNN:** Embedding → Flatten → Dense(256) → Dropout(0.5) → Dense(64) → Dropout(0.3) → Output  
  - **Transformer:** Embedding → MultiHeadAttention(4) → GlobalAvgPooling → Dense(64) → Dropout(0.3) → Output  
  - **TextCNN:** Embedding → Conv1D(3,4,5) → Concatenate → GlobalMaxPooling → Dense(64) → Dropout(0.3) → Output  

---

## 📈 Evaluation

### Metrik Evaluasi
- Accuracy  
- Precision, Recall, F1-Score  
- Confusion Matrix  
- ROC-AUC  

### Hasil Evaluasi Machine Learning
| Model               | Val Acc | Test Acc | Test AUC |
|----------------------|---------|----------|----------|
| Logistic Regression  | 85.00%  | 85.78%   | 92.99%   |
| SVM                  | 84.01%  | 86.12%   | 92.72%   |
| Naïve Bayes          | 83.89%  | 84.87%   | 91.92%   |
| Random Forest        | 82.30%  | 82.99%   | 90.96%   |

### Hasil Evaluasi Deep Learning
| Model       | Val Acc | Test Acc | Test AUC |
|-------------|---------|----------|----------|
| LSTM        | 84.46%  | 84.30%   | 92.18%   |
| GRU         | 83.89%  | 83.90%   | 92.14%   |
| CNN         | 83.72%  | 84.70%   | 92.02%   |
| DNN         | 83.89%  | 84.19%   | 92.58%   |
| Transformer | 84.46%  | 85.27%   | 92.78%   |
| TextCNN     | 83.44%  | 84.47%   | 92.22%   |

---

## 🔎 Analisis Komparatif
- Model ML klasik kompetitif dengan DL  
- Logistic Regression & SVM → performa terbaik di ML  
- Transformer → performa terbaik di DL  
- Konsistensi tinggi antara validation dan test set  
- Semua model memiliki **ROC-AUC >90%**  

---

## 📋 Conclusion
- **Model Terbaik:** Logistic Regression (ML) & Transformer (DL)  
- **Efisiensi Komputasi:** ML lebih cepat untuk training & inference  
- **Pola Data:**  
  - Ulasan negatif → masalah teknis (error, login, update)  
  - Ulasan positif → pengalaman menonton & kualitas konten  

### Rekomendasi:
- **Production:** Gunakan Logistic Regression (sederhana & cepat)  
- **Akurasi tertinggi:** Gunakan Transformer dengan tuning hyperparameter  
- **Data:** Tambahkan ulasan netral (rating 3) untuk analisis lebih komprehensif  
