# Analisis Sentimen Ulasan Aplikasi Android (Data Mining Project)

Repositori ini berisi proyek akhir mata kuliah **Data Mining** yang melakukan **analisis sentimen** pada ulasan pengguna aplikasi Android di Google Play Store (ID aplikasi: `com.bd.nproject`).  
Pendekatan yang digunakan mengombinasikan **metode leksikon bahasa Indonesia** untuk pelabelan awal sentimen dan **Support Vector Machine (SVM)** dengan fitur **TF-IDF** untuk klasifikasi otomatis.

## 🎯 Tujuan Proyek

- Mengumpulkan ulasan pengguna aplikasi Android dari Google Play Store.
- Melakukan *preprocessing* teks berbahasa Indonesia secara bertahap.
- Melabeli sentimen secara otomatis menggunakan kamus kata positif dan negatif (lexicon-based).
- Membangun model klasifikasi sentimen menggunakan SVM.
- Membandingkan performa model dengan representasi fitur **unigram** dan **bigram** (N-gram).

## 🧾 Sumber Data

Data ulasan diambil menggunakan pustaka **`google-play-scraper`**:

- Sumber: Google Play Store
- Bahasa ulasan: Indonesia (`lang='id'`)
- Negara: Indonesia (`country='id'`)
- Jumlah ulasan: hingga 1000 ulasan paling relevan

Kolom utama yang digunakan:
- `content` → teks ulasan
- Kolom turunan hasil preprocessing dan pelabelan seperti `text_akhir`, `polarity`, dll.

## 🔧 Tahapan Pengolahan Data

1. **Pengambilan Data (Scraping)**  
   - Menggunakan `reviews_all()` dari `google_play_scraper` untuk mengambil ulasan aplikasi.
   - Menyimpan hasilnya ke dalam `DataFrame` dan file `.csv`.

2. **Preprocessing Teks Bahasa Indonesia**  
   Beberapa tahap yang digunakan pada kolom `content`:
   - *Cleaning* teks (menghapus simbol, angka, dan karakter tak relevan).
   - *Case folding* menjadi huruf kecil.
   - Normalisasi **slang words** memakai kamus kata gaul → kata baku.
   - *Tokenizing* (memecah teks menjadi kata-kata).
   - Penghapusan **stopwords** (kata umum yang tidak informatif).
   - **Stemming** menggunakan Sastrawi untuk mengubah kata ke bentuk dasar.
   - Penyusunan ulang ke bentuk kalimat akhir pada kolom `text_akhir`.

3. **Pelabelan Sentimen (Lexicon-Based)**  
   - Memuat kamus **lexicon positif** dan **lexicon negatif** bahasa Indonesia dari file `.csv`.
   - Menghitung skor sentimen tiap ulasan berdasarkan kemunculan kata-kata dalam kamus.
   - Menentukan label:
     - `positive` jika skor ≥ 0  
     - `negative` jika skor < 0  
   - Menyimpan hasilnya di kolom `polarity`.

4. **Eksplorasi & Visualisasi**  
   - Menghitung distribusi ulasan positif vs negatif.
   - Membuat **wordcloud** untuk kata-kata dominan pada ulasan positif dan negatif.

5. **Ekstraksi Fitur dengan TF-IDF + N-gram**  
   - Menggunakan `TfidfVectorizer` pada kolom `text_akhir`.
   - Dua konfigurasi fitur:
     - **Unigram**: `ngram_range=(1,1)`
     - **Bigram**: `ngram_range=(1,2)` (unigram + bigram)
   - Hasil vektor disimpan sebagai `X`, label sentimen sebagai `y` (`polarity`).

6. **Pembangunan & Evaluasi Model SVM**  
   - Membagi data menjadi **train** dan **test** dengan `train_test_split`.
   - Menggunakan `SVC(kernel='linear')` sebagai model utama.
   - Menghitung metrik:
     - Confusion matrix
     - Classification report: akurasi, presisi, recall, F1-score
   - Membuat tabel perbandingan performa antara:
     - Unigram (N=1)
     - Bigram (N=2)
