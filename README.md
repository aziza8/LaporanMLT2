# Laporan Proyek Machine Learning - Miftahus Sholihin

## Project Overview

Pada bagian ini, Kamu perlu menuliskan latar belakang yang relevan dengan proyek yang diangkat.

**Rubrik/Kriteria Tambahan (Opsional)**:
- Jelaskan mengapa proyek ini penting untuk diselesaikan.
- Menyertakan hasil riset terkait atau referensi. Referensi yang diberikan harus berasal dari sumber yang kredibel dan author yang jelas.
  
  Format Referensi: [Judul Referensi](https://scholar.google.com/) 

## Business Understanding

Pada bagian ini, Anda perlu menjelaskan proses klarifikasi masalah.

Bagian laporan ini mencakup:

### Problem Statements

- Bagaimana memahami preferensi pengguna berdasarkan pola rating yang diberikan terhadap berbagai film?
- Bagaimana membangun model rekomendasi yang mampu memberikan saran personalisasi secara akurat?- 

### Goals

- Menggunakan data rating untuk memahami pola preferensi pengguna terhadap berbagai genre dan film, sehingga dapat digunakan sebagai dasar untuk rekomendasi personalisasi.
- Membangun dan mengimplementasikan model rekomendasi, baik berbasis collaborative filtering, content-based filtering, maupun pendekatan hibrid, untuk menghasilkan rekomendasi yang relevan dan akurat.

**Rubrik/Kriteria Tambahan (Opsional)**:
- Menambahkan bagian “Solution Approach” yang menguraikan cara untuk meraih goals. Bagian ini dibuat dengan ketentuan sebagai berikut: 

    ### Solution statements
    - Mengajukan 2 atau lebih solution approach (algoritma atau pendekatan sistem rekomendasi).

## Data Understanding
Untuk membangun sistem rekomendasi yang efektif, penting untuk memahami karakteristik dataset yang akan digunakan. Proyek ini memanfaatkan dua dataset utama: movies.csv dan ratings.csv. Berikut adalah analisis terhadap kedua dataset:
1. Dataset movies.csv
Dataset ini berisi informasi tentang film, mencakup:
- Jumlah Entitas: 9,742 film.
- Kolom Utama:
  - movieId: ID unik untuk setiap film.
  - title: Judul film beserta tahun rilis (contoh: "Toy Story (1995)").
  - genres: Genre film, dengan kemungkinan beberapa genre dipisahkan oleh tanda | (contoh: "Animation|Children|Comedy").
 
2. Dataset ratings.csv
Dataset ini mencatat interaksi pengguna dengan film melalui penilaian.
- Jumlah Entitas: 100,836 interaksi pengguna.
- Kolom Utama:
  - userId: ID unik pengguna yang memberikan rating.
  - movieId: ID film yang dirating (menghubungkan dengan dataset movies.csv).
  - rating: Rating yang diberikan oleh pengguna (skala 0.5 hingga 5.0, kemungkinan increment 0.5).
  - timestamp: Waktu pemberian rating dalam format UNIX timestamp.

Paragraf awal bagian ini menjelaskan informasi mengenai jumlah data, kondisi data, dan informasi mengenai data yang digunakan. Sertakan juga sumber atau tautan untuk mengunduh dataset. Contoh: [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Restaurant+%26+consumer+data).

Selanjutnya, uraikanlah seluruh variabel atau fitur pada data. Sebagai contoh:  

Variabel-variabel pada Restaurant UCI dataset adalah sebagai berikut:
- accepts : merupakan jenis pembayaran yang diterima pada restoran tertentu.
- cuisine : merupakan jenis masakan yang disajikan pada restoran.
- dst

**Rubrik/Kriteria Tambahan (Opsional)**:
- Melakukan beberapa tahapan yang diperlukan untuk memahami data, contohnya teknik visualisasi data beserta insight atau exploratory data analysis.

## Data Preparation
Pada bagian ini Anda menerapkan dan menyebutkan teknik data preparation yang dilakukan. Teknik yang digunakan pada notebook dan laporan harus berurutan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan proses data preparation yang dilakukan
- Menjelaskan alasan mengapa diperlukan tahapan data preparation tersebut.

## Modeling
Tahapan ini membahas mengenai model sisten rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan top-N recommendation sebagai output.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menyajikan dua solusi rekomendasi dengan algoritma yang berbeda.
- Menjelaskan kelebihan dan kekurangan dari solusi/pendekatan yang dipilih.

## Evaluation
Pada bagian ini Anda perlu menyebutkan metrik evaluasi yang digunakan. Kemudian, jelaskan hasil proyek berdasarkan metrik evaluasi tersebut.

Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.

**---Ini adalah bagian akhir laporan---**

_Catatan:_
- _Anda dapat menambahkan gambar, kode, atau tabel ke dalam laporan jika diperlukan. Temukan caranya pada contoh dokumen markdown di situs editor [Dillinger](https://dillinger.io/), [Github Guides: Mastering markdown](https://guides.github.com/features/mastering-markdown/), atau sumber lain di internet. Semangat!_
- Jika terdapat penjelasan yang harus menyertakan code snippet, tuliskan dengan sewajarnya. Tidak perlu menuliskan keseluruhan kode project, cukup bagian yang ingin dijelaskan saja.
