# Analisis Sentimen IndiHome

Aplikasi web berbasis Streamlit untuk mengelola dataset ulasan IndiHome, melakukan preprocessing dan pelabelan sentimen, melatih serta mengevaluasi model machine learning, dan mencoba prediksi sentimen secara interaktif.

## Fitur

- **Dashboard**: ringkasan jumlah data dan distribusi sentimen positif, negatif, dan netral.
- **Manajemen dataset**: melihat dan mencari data, menambah/mengubah/menghapus data, serta mengunggah CSV ke MySQL.
- **Preprocessing teks**: pilihan tahap emoji-to-text, cleansing, case folding, tokenisasi, normalisasi kata tidak baku, stopword removal, dan stemming Bahasa Indonesia dengan Sastrawi.
- **Labeling otomatis**: memberi label menggunakan kamus positif/negatif Excel dan kamus istilah jaringan yang dapat dikelola di aplikasi.
- **Word Cloud**: visualisasi kata dari teks asli atau teks hasil preprocessing, dengan filter sentimen.
- **Ekstraksi fitur TF-IDF**: melihat sampel matriks fitur dan kata dengan rata-rata skor TF-IDF tertinggi.
- **Modeling dan evaluasi**: melatih SVM, Naive Bayes, Decision Tree, dan Random Forest menggunakan Stratified K-Fold Cross Validation; SMOTE dapat diaktifkan untuk data latih.
- **Riwayat dan perbandingan model**: melihat metrik evaluasi, grafik perbandingan, serta confusion matrix.
- **Prediksi dan ekspor**: membandingkan prediksi model yang tersedia (dengan atau tanpa SMOTE), menyimpan riwayat prediksi, dan mengunduh laporan PDF.
- **Login**: autentikasi pengguna tersimpan di database MySQL.

## Teknologi

- Python dan Streamlit
- MySQL/MariaDB
- pandas, scikit-learn, imbalanced-learn
- Sastrawi dan emoji
- Matplotlib, Seaborn, dan WordCloud

## Struktur proyek

```text
.
├── app.py
├── app_backup.py
├── requirements.txt
├── modules/
│   ├── database.py
│   ├── db_connection.py
│   ├── machine_learning.py
│   └── text_processing.py
├── dataset/
│   └── dataset_preprocessed.csv
├── database sql/
│   └── sentiment_indihome.sql
├── kamus_positive.xlsx
├── kamus_negative.xlsx
├── logo_indihome.png
├── tfidf_vectorizer.pkl
└── model_*.pkl
```

`model_*.pkl` dan `tfidf_vectorizer.pkl` adalah artefak model/vectorizer yang digunakan oleh fitur prediksi. Artefak tersebut juga dapat dibuat atau diperbarui melalui menu **Modeling & Evaluation**.

## Persiapan

1. Pasang Python yang kompatibel dengan paket di `requirements.txt`, serta MySQL atau MariaDB (misalnya melalui XAMPP).
2. Buat database bernama `sentiment_indihome`.
3. Sesuaikan koneksi database di [`modules/database.py`](modules/database.py) dengan konfigurasi MySQL lokal:

   ```python
   host="localhost",
   user="root",
   password="",
   database="sentiment_indihome"
   ```

4. Pilih database `sentiment_indihome` di phpMyAdmin lalu impor `database sql/sentiment_indihome.sql` untuk menyiapkan struktur tabel dan data contoh. Aplikasi membuat tabel `users`, `custom_lexicon`, dan `history_prediksi` saat diperlukan, tetapi tabel dataset dan evaluasi perlu disiapkan dari dump SQL.
5. Buat lingkungan virtual dan pasang dependensi. Contoh di Windows PowerShell:

   ```powershell
   py -m venv .venv
   .\.venv\Scripts\Activate.ps1
   python -m pip install -r requirements.txt
   ```

## Menjalankan aplikasi

Jalankan dari folder utama proyek:

```powershell
streamlit run app.py
```

Streamlit akan menampilkan alamat lokal aplikasi di terminal (umumnya `http://localhost:8501`). Pastikan server MySQL aktif dan database sudah tersedia.

## Alur penggunaan

1. Masuk ke aplikasi dan siapkan dataset lewat menu **Dataset**. CSV perlu memiliki kolom teks; kolom label dan teks bersih bersifat opsional dan dapat dipilih saat upload.
2. Jalankan **Preprocessing** untuk menghasilkan `clean_tweet`.
3. Jika dataset belum memiliki label, gunakan **Labeling** untuk membuat label otomatis, atau siapkan label pada CSV.
4. Tinjau distribusi melalui **Dashboard**, **Word Cloud**, atau **TF-IDF**.
5. Pilih algoritma dan jumlah fold di **Modeling & Evaluation**. Aktifkan SMOTE bila diperlukan. Evaluasi disimpan ke MySQL dan model terpilih diekspor sebagai file `.pkl`.
6. Gunakan **History & Comparison** untuk membandingkan eksperimen dan **Prediksi Model** untuk menguji teks baru.

## Catatan

- Aplikasi mengharapkan file kamus `kamus_positive.xlsx` dan `kamus_negative.xlsx` berada di folder utama proyek.
- Menu labeling membutuhkan kolom teks hasil preprocessing (`clean_tweet`).
- Akun awal `admin` dengan kata sandi `admin` dibuat otomatis jika tabel `users` kosong. Dump SQL contoh juga memuat akun tersebut. Ganti kata sandi/kredensial awal dan jangan publikasikan aplikasi dengan konfigurasi default.
- File model Pickle harus berasal dari proyek tepercaya dan kompatibel dengan versi pustaka Python yang digunakan.
