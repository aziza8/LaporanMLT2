# Laporan Proyek Machine Learning - Miftahus Sholihin

## Project Overview
Sistem rekomendasi menjadi salah satu teknologi penting dalam dunia digital saat ini, terutama dalam platform berbasis hiburan seperti film dan musik. Dalam konteks platform streaming film, pemahaman terhadap preferensi pengguna adalah kunci untuk menyediakan pengalaman yang lebih personal dan relevan. Banyak pengguna yang merasa kewalahan dengan banyaknya pilihan film yang tersedia, dan tanpa bantuan sistem rekomendasi yang baik, mereka mungkin kesulitan menemukan film yang sesuai dengan selera mereka. Oleh karena itu, pengembangan sistem rekomendasi yang efektif dan akurat sangat penting untuk meningkatkan pengalaman pengguna dan retensi platform.


## Business Understanding
Dalam industri hiburan digital, platform streaming film menghadapi tantangan besar dalam menarik dan mempertahankan pengguna. Salah satu cara untuk meningkatkan pengalaman pengguna dan menjaga tingkat retensi yang tinggi adalah dengan menawarkan rekomendasi film yang relevan dan personal. Dengan begitu banyak pilihan film yang tersedia, pengguna sering kali merasa kesulitan memilih film yang sesuai dengan preferensi mereka. Oleh karena itu, pengembangan sistem rekomendasi yang dapat memberikan saran yang akurat dan sesuai dengan minat pengguna menjadi sangat penting.

### Problem Statements

- Bagaimana memahami preferensi pengguna berdasarkan pola rating yang diberikan terhadap berbagai film?
- Bagaimana membangun model rekomendasi yang mampu memberikan saran personalisasi secara akurat?

### Goals

- Menggunakan data rating untuk memahami pola preferensi pengguna terhadap berbagai genre dan film, sehingga dapat digunakan sebagai dasar untuk rekomendasi personalisasi.
- Membangun dan mengimplementasikan model rekomendasi, baik berbasis collaborative filtering, content-based filtering, maupun pendekatan hibrid, untuk menghasilkan rekomendasi yang relevan dan akurat.

    ### Solution statements
    - Memahami Preferensi Pengguna Berdasarkan Pola Rating.
    - Membangun Model Rekomendasi yang Memberikan Saran Personalisasi Secara Akurat.

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
3.  Analisis Kualitas Data
    a. Missing Values
        - Dataset `movies.csv` tidak memiliki missing values berdasarkan hasil `movies_df.isna().sum()`.
        - Dataset `ratings.csv` juga tidak ditemukan missing values (`ratings_df.isna().sum()`).
 

## Data Preparation
Proses data preparation ini melibatkan pembersihan data, transformasi, dan penggabungan informasi yang relevan untuk memastikan data siap digunakan dalam analisis dan model rekomendasi.
- Ekstraksi tahun dengan pola fleksibel
  
  Proses pertama adalah mengekstraksi informasi tahun dari judul film menggunakan metode str.extract() dengan pola regex (\d{4}). Pola ini mencari angka dengan format 4 digit, seperti tahun 1994 atau 2020, yang kemudian disimpan ke dalam kolom baru bernama year. Jika judul tidak mengandung format tahun, nilai di kolom year akan menjadi NaN. Selanjutnya, kode membersihkan kolom title dengan menghapus informasi tahun yang ada dalam tanda kurung, seperti (1994), menggunakan str.replace() dengan pola regex \(\d{4}\). Setelah penghapusan, metode str.strip() digunakan untuk menghilangkan spasi tambahan di awal atau akhir judul. Hasil akhir dari proses ini adalah kolom title yang hanya berisi nama film tanpa tahun, sementara informasi tahun dipindahkan ke kolom year. Transformasi ini membuat data lebih terstruktur dan memudahkan analisis, seperti distribusi film berdasarkan tahun atau manipulasi data judul tanpa gangguan informasi tambahan.
- Konversi fitur timestamp menjadi waktu berbasis detik.
- Menggabungkan dataframe movie_df dan ratings_df.
- Konversi movieId, title dan genres menjadi list.
- Membuat dataframe baru movie_dict yang terdiri dari id, movie_name dan genre.
- Mengubah userId menjadi unique list dan encoding userId.
- Mengubah movieId menjadi unique list dan encoding movieId.
- Mapping userId ke dataframe user dan movieId ke dataframe movie.
- Data split.
- TF-IDF.


## Modeling
Pada proyek ini dua pendekatan digunakan untuk membuat sistem rekomendasi yaitu:
1. conten based
2. colaborative filtering

Berikut ini adalah code yang digunakan untuk melatih model yang sudah dibaut
''' pyhton history = model.fit(
    x = x_train,
    y = y_train,
    batch_size = 32,
    epochs = 100,
    validation_data = (x_val, y_val)
)
'''

## Evaluation
Dalam proyek ini, evaluasi dilakukan untuk memastikan bahwa rekomendasi yang dihasilkan relevan, akurat, dan memenuhi kebutuhan pengguna. Berikut adalah rincian pendekatan evaluasi yang digunakan:
1. Root Mean Squared Error (RMSE): Mengukur perbedaan antara rating yang diprediksi dengan rating aktual yang diberikan pengguna.

 Kemudian dilakukan proses pengujian, hasil dari pengujian ini ditampilkan 10 judul film yang sesuai.
