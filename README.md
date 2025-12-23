# BookMatch: Book Recommendation System 

![Python](https://img.shields.io/badge/Python-3.11%2B-blue)
![Scikit-Learn](https://img.shields.io/badge/Library-Scikit--Learn-orange)
![Pandas](https://img.shields.io/badge/Library-Pandas-150458)
![Status](https://img.shields.io/badge/Status-Completed-green)

**BookMatch** adalah sistem rekomendasi buku berbasis konten (*Content-Based Filtering*) yang dirancang untuk memberikan saran bacaan yang dipersonalisasi. Sistem ini menganalisis metadata buku (judul, penulis, dan penerbit) menggunakan teknik Natural Language Processing (NLP) untuk menemukan kesamaan kontekstual antar buku.

## Latar Belakang Masalah
Dalam industri penerbitan dan e-commerce buku, pengguna sering mengalami *information overload* akibat banyaknya pilihan judul. Proyek ini bertujuan untuk:
- Membantu pengguna menemukan buku yang relevan dengan minat mereka.
- Meningkatkan *user engagement* melalui rekomendasi yang akurat.
- Mengimplementasikan algoritma Machine Learning untuk kurasi konten otomatis.

##  Metodologi & Fitur Utama
Sistem ini dibangun menggunakan pendekatan **Content-Based Filtering** dengan alur kerja sebagai berikut:

1.  **Data Preprocessing**:
    - Membersihkan *missing values* pada data penulis dan penerbit.
    - Menangani anomali pada tahun publikasi (misal: tahun 0 atau >2024).
    - Reduksi dimensi data menjadi 15,000 sampel untuk efisiensi komputasi.
2.  **Feature Engineering**:
    - Menggabungkan fitur `Book-Title`, `Book-Author`, dan `Publisher` menjadi satu `Combined-Text`.
    - Pembersihan teks (lowercase, penghapusan *stopwords* dan karakter non-alfanumerik).
3.  **Modeling**:
    - **TF-IDF Vectorization**: Mengubah teks menjadi representasi vektor numerik.
    - **Cosine Similarity**: Menghitung skor kemiripan antar vektor buku.
4.  **Recommendation Engine**:
    - Fungsi `get_recommendations()` yang menerima input judul buku dan mengembalikan Top-10 buku termirip.

## 📂 Struktur Proyek
```text
BookMatch-ML-Recommender/
├── data/                  # Dataset (books, ratings, users)
├── notebooks/             # Jupyter Notebook untuk eksperimen & modeling
│   └── books_recommendation.ipynb
├── reports/               # Laporan analisis lengkap & visualisasi
│   └── reports_recommendation.md
├── requirements.txt       # Daftar dependensi library
└── README.md              # Dokumentasi proyek ini
```

## 🚀 Cara Instalasi (Installation)

Ikuti langkah-langkah berikut untuk menjalankan proyek ini di mesin lokal Anda:

1.  **Clone Repository**
    Salin repository ini ke direktori lokal Anda:
    ```bash
    git clone [https://github.com/UsernameAnda/BookMatch-ML-Recommender.git](https://github.com/UsernameAnda/BookMatch-ML-Recommender.git)
    cd BookMatch-ML-Recommender
    ```

2.  **Persiapkan Environment (Disarankan)**
    Buat dan aktifkan virtual environment agar dependensi project terisolasi dan tidak bentrok:
    ```bash
    # Untuk Windows
    python -m venv venv
    .\venv\Scripts\activate

    # Untuk macOS/Linux
    python3 -m venv venv
    source venv/bin/activate
    ```

3.  **Install Dependensi**
    Install semua library yang dibutuhkan menggunakan file `requirements.txt`:
    ```bash
    pip install -r requirements.txt
    ```
    *Library utama meliputi: pandas, numpy, scikit-learn, nltk, matplotlib, dan seaborn*.

4.  **Jalankan Notebook**
    Buka Jupyter Notebook untuk menjalankan kode analisis dan rekomendasi:
    ```bash
    jupyter notebook notebooks/books_recommendation.ipynb
    ```

## 📊 Evaluasi Model (Model Evaluation)

Performa sistem rekomendasi dievaluasi menggunakan metrik relevansi untuk mengukur seberapa akurat dan relevan saran buku yang diberikan kepada pengguna. Berdasarkan hasil simulasi pengujian, model mencapai performa sebagai berikut:

| Metrik Evaluasi | Skor | Deskripsi |
| :--- | :--- | :--- |
| **Precision** | **0.80** | Mengukur seberapa banyak rekomendasi yang benar-benar relevan dari total rekomendasi yang diberikan. Skor 0.80 berarti 80% rekomendasi tepat sasaran. |
| **Recall** | **0.80** | Mengukur kemampuan sistem mengenali seluruh item relevan dalam dataset. Sistem berhasil menemukan 80% dari total buku yang relevan yang tersedia. |
| **F1-Score** | **0.80** | Nilai harmonis antara Precision dan Recall. Skor ini menunjukkan keseimbangan yang baik antara kualitas (ketepatan) dan kuantitas (cakupan) rekomendasi. |
| **MAP** | **0.90** | *(Mean Average Precision)* Mengukur kualitas urutan rekomendasi. Skor tinggi 0.90 menunjukkan bahwa sistem sangat baik dalam menempatkan buku yang paling relevan di posisi teratas daftar rekomendasi. |

## Tech Stack
Proyek ini dibuat menggunakan library Python berikut:
- **Pandas & NumPy**: Manipulasi dan analisis data.
- **Scikit-learn**: TF-IDF Vectorizer dan perhitungan metrik (Cosine Similarity, Precision, Recall).
- **NLTK**: Preprocessing teks (Stopwords removal).
- **Matplotlib & Seaborn**: Visualisasi data (EDA).

## Kontribusi & Credits
Project ini dibuat oleh **Bintang Akalla Junjunan** sebagai bagian dari *Expert Class Data Science*.
Dataset bersumber dari [Kaggle Book Recommendation Dataset](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset).

## 📄 Lisensi
Didistribusikan di bawah lisensi MIT. Silakan gunakan dan modifikasi sesuai kebutuhan pembelajaran.

---
*Jangan lupa berikan bintang ⭐ pada repository ini jika Anda merasa terbantu!*
