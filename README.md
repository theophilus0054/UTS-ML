# UTS Machine Learning - Analisis & Klasifikasi Komentar YouTube

Proyek tugas / ujian tengah semester (UTS) Machine Learning untuk scraping, preprocessing teks bahasa Indonesia, anotasi data, dan klasifikasi teks (NLP) berbasis machine learning pada komentar video YouTube.

---

## 📁 Struktur Direktori

```text
UTS-ML/
├── .env.example          # Template konfigurasi environment variable
├── .env                  # Kredensial lokal (API Key YouTube & target video) [ignored by git]
├── .gitignore            # Aturan ignore standar Python & environment
├── .python-version       # Versi Python yang digunakan (Python 3.12)
├── pyproject.toml        # Konfigurasi dependensi uv & metadata proyek
├── uv.lock               # Dependency lockfile deterministik
├── README.md             # Dokumentasi proyek
├── main.ipynb            # Jupyter Notebook utama (Scraping -> Preprocessing -> Model -> Evaluasi)
├── comments_raw.csv      # [Akan dibuat] Data mentah komentar hasil scraping YouTube
└── comments_to_label.csv # [Akan dibuat] Subset data komentar siap anotasi/pelabelan
```

---

## 🚀 Persiapan & Instalasi

Proyek ini menggunakan [`uv`](https://docs.astral.sh/uv/) sebagai package dan environment manager berkinerja tinggi.

### 1. Sinkronisasi Environment

Untuk menginstall seluruh dependensi yang diperlukan:
```bash
uv sync
```

### 2. Konfigurasi Kredensial YouTube API

1. Salin file `.env.example` menjadi `.env` (jika belum ada):
   ```bash
   cp .env.example .env
   ```
2. Dapatkan API Key melalui [Google Cloud Console](https://console.cloud.google.com/) dan aktifkan **YouTube Data API v3**.
3. Isi parameter pada `.env`:
   ```env
   YOUTUBE_API_KEY=your_actual_youtube_api_key_here
   VIDEO_ID=your_target_video_id
   MAX_RESULTS=1000
   ```

---

## 📓 Alur Kerja di `main.ipynb`

Notebook `main.ipynb` dirancang secara end-to-end dengan tahapan:

1. **Scraping Komentar**: Mengambil komentar video YouTube via YouTube Data API v3 dan menyimpannya ke `comments_raw.csv`.
2. **Sampling & Labeling**: Menyiapkan sampel data komentar untuk anotasi manual ke dalam `comments_to_label.csv`.
3. **Data Preprocessing**:
   - Case folding (lowercase)
   - Cleansing (menghapus URL, mention, emoji, karakter khusus, dan angka)
   - Tokenization
   - Stopwords removal (bahasa Indonesia)
   - Stemming kata dasar (PySastrawi)
4. **Ekstraksi Fitur**: Pembobotan kata menggunakan **TF-IDF Vectorizer**.
5. **Pemodelan Machine Learning**:
   - Train/Test Split (misal: 80% train, 20% test stratified)
   - Algoritma klasifikasi: **Multinomial Naive Bayes**, **Support Vector Machine (SVM)**, atau **Logistic Regression**
6. **Evaluasi Model**:
   - Confusion Matrix
   - Classification Report (Precision, Recall, F1-Score, Accuracy)
7. **Pengujian Inferensi**: Menguji model terhadap teks komentar baru.

---

## 🛠️ Stack & Library yang Digunakan

- **Scraping**: `google-api-python-client`
- **Manipulasi Data**: `pandas`, `numpy`
- **Text & NLP**: `nltk`, `PySastrawi`, `regex`
- **Machine Learning**: `scikit-learn`
- **Visualisasi**: `matplotlib`, `seaborn`
- **Environment**: `python-dotenv`, `ipykernel`
