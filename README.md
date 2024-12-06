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
  
  Proses pertama adalah mengekstraksi informasi tahun dari judul film menggunakan metode str.extract() dengan pola regex (\d{4}). Pola ini mencari angka dengan format 4 digit, seperti tahun 1994 atau 2020, yang kemudian disimpan ke dalam kolom baru bernama year. Jika judul tidak mengandung format tahun, nilai di kolom year akan menjadi NaN. Selanjutnya, kode membersihkan kolom title dengan menghapus informasi tahun yang ada dalam tanda kurung, seperti (1994), menggunakan str.replace() dengan pola regex \(\d{4}\). Setelah penghapusan, metode str.strip() digunakan untuk menghilangkan spasi tambahan di awal atau akhir judul. Hasil akhir dari proses ini adalah kolom title yang hanya berisi nama film tanpa tahun, sementara informasi tahun dipindahkan ke kolom year. Transformasi ini membuat data lebih terstruktur dan memudahkan analisis, seperti distribusi film berdasarkan tahun atau manipulasi data judul tanpa gangguan informasi tambahan. Berikut adalah contoh hasil yang akan dihasilkan oleh kode tersebut setelah dijalankan pada DataFrame movies_df:

Data Sebelum Proses.

|movieId	| title	                | genres      |
|---------- |------                 | --------    |
|	1	    |Toy Story (1995)	    | Adventure, Animation, Children, Comedy, Fantasy
|	2	    | Jumanji (1995)	    | Adventure,Children, Fantasy|
|	3	    | Grumpier Old Men (1995)|	Comedy, Romance
|	4	    | Waiting to Exhale (1995)|	Comedy, Drama, Romance
|	5	    | Father of the Bride Part II (1995)|	Comedy

Data Setelah Proses.

|movieId	| title	                | genres                                          | year    | 
|---------- |------                 | --------                                        | ----    | 
|	1	    |Toy Story 	    | Adventure, Animation, Children, Comedy, Fantasy         | 1995
|	2	    | Jumanji 	    | Adventure,Children, Fantasy                     | 1995
|	3	    | Grumpier Old Men |	Comedy, Romance                               | 1995
|	4	    | Waiting to Exhale |	Comedy, Drama, Romance                        | 1995
|	5	    | Father of the Bride Part II |	Comedy                            | 1995


  
- Konversi fitur timestamp menjadi waktu berbasis detik.

  Proses ini bertujuan untuk mengonversi data pada kolom timestamp dalam DataFrame ratings_df menjadi format datetime yang dapat dimengerti oleh manusia. Kolom timestamp pada awalnya berisi data waktu dalam bentuk Unix timestamp, yaitu jumlah detik yang telah berlalu sejak 1 Januari 1970 (epoch time). Metode pd.to_datetime() digunakan untuk melakukan konversi ini, dengan argumen unit='s' yang menunjukkan bahwa nilai dalam kolom timestamp adalah dalam satuan detik. Setelah konversi, kolom timestamp akan berisi nilai dalam format tanggal dan waktu standar, seperti 2024-12-06 12:00:00. Proses ini penting untuk mempermudah analisis data terkait waktu, seperti memfilter data berdasarkan tanggal tertentu, menghitung selisih waktu, atau melakukan agregasi berdasarkan hari, bulan, atau tahun.
  
- Menggabungkan dataframe movie_df dan ratings_df.

    Tujuan dari proses ini adalah untuk menggabungkan dua DataFrame, yaitu movies_df dan ratings_df, berdasarkan kolom kunci movieId yang dimiliki keduanya. Fungsi merge() dari pandas digunakan untuk menggabungkan informasi dari kedua tabel menjadi sebuah DataFrame baru bernama merged_df. Penggabungan ini secara default menggunakan metode inner join, sehingga hanya baris dengan nilai movieId yang cocok di kedua DataFrame yang akan dimasukkan ke dalam hasil akhir. Dalam proses ini, data dari movies_df, seperti judul film (title) dan tahun rilis (year), akan digabungkan dengan data dari ratings_df, seperti pengguna (userId), nilai rating (rating), dan waktu rating (timestamp). Hasilnya adalah DataFrame terintegrasi yang mencakup informasi lengkap dari kedua sumber, mempermudah analisis lebih lanjut seperti memahami hubungan antara film dan rating yang diberikan oleh pengguna.

  Contoh:
Misalkan data awalnya adalah sebagai berikut:

movies_df:

|movieId |	title  | year |
|------- |-------- |------ |
|1	     | Toy Story |	1995
|2	    | Jumanji    |	1995
|3	    |Grumpier Old Men |	1995

ratings_df:

| movieId    |	userId    |	rating    | timestamp
|----------|----------|---------|----------|
|1	        |101        |4.0        |1625832000
|2	        |102	    |3.5	    |1625835600
|1	        |103	    |5.0	    |1625839200

Hasil merged_df:

|movieId    |	title    |	year    |	userId    |	rating    |	timestamp    |
|----------|-------------|----------|-------------|-----------|--------------|
|1	        |Toy Story	    |1995	    |101	    |4.0	    |1625832000
|1	        |Toy Story	    |1995	    |103	    |5.0	    |1625839200
|2	        |Jumanji	    |1995	    |102	    |3.5	    |1625835600
  
- Konversi movieId, title dan genres menjadi list.
    Proses ini melakukan konversi data dari kolom dalam DataFrame prep_movies ke dalam bentuk list Python untuk mempermudah penggunaan dan manipulasi data tersebut. DataFrame pada dasarnya adalah struktur data tabular, yang mirip dengan tabel, di mana setiap kolom adalah sebuah pandas Series. Pandas Series adalah struktur data sejenis array satu dimensi yang dapat berisi berbagai jenis data, seperti angka, string, atau objek lainnya.
  1. prep_movies['movieId']:
     
        Kolom movieId berisi ID unik untuk setiap film dalam DataFrame. Dalam konteks ini, movieId mungkin berisi angka atau ID lainnya yang digunakan untuk mengidentifikasi film secara unik. Dengan menggunakan tolist(), kita mengonversi kolom movieId, yang pada awalnya berupa pandas Series, menjadi list Python yang lebih fleksibel. List adalah struktur data yang lebih umum digunakan dalam Python dan memungkinkan kita untuk melakukan berbagai operasi, seperti pencarian atau pemrosesan elemen berdasarkan indeks.

    2. prep_movies['title']:
       
        Kolom title berisi nama atau judul dari masing-masing film. Setelah mengonversinya menjadi list menggunakan tolist(), setiap elemen dalam title akan menjadi sebuah string dalam list movie_name. Dengan cara ini, kita dapat dengan mudah mengakses nama film berdasarkan urutan atau indeks tertentu, misalnya untuk mencocokkan film dengan rating atau genre tertentu dalam analisis berikutnya.

    3. prep_movies['genres']:
       
        Kolom genres berisi genre atau kategori dari film, yang bisa terdiri dari satu atau beberapa genre (misalnya "Drama", "Action", "Comedy", dll.). Ketika kolom ini diubah menjadi list menggunakan tolist(), setiap elemen dalam genres menjadi sebuah string yang berisi genre-genre terkait untuk setiap film. Genre ini bisa terdiri dari beberapa nilai, yang biasanya dipisahkan oleh tanda koma atau spasi dalam data mentah. Setelah dikonversi menjadi list, kita bisa lebih mudah melakukan analisis atau pemrosesan data berdasarkan genre, misalnya dengan mengelompokkan film berdasarkan kategori genre tertentu.

    Dengan demikian, tujuan dari konversi ini adalah untuk mengubah kolom dalam DataFrame, yang pada awalnya terstruktur sebagai Series pandas, menjadi format list Python yang lebih umum dan fleksibel. List Python lebih mudah digunakan dalam banyak kasus, terutama saat kita perlu melakukan iterasi, filter, atau operasi manipulasi data lain yang lebih kompleks. Misalnya, jika kita ingin mencari film tertentu dengan genre "Comedy", kita dapat dengan mudah memeriksa apakah genre tersebut ada dalam list movie_genre.
  
- Membuat dataframe baru movie_dict yang terdiri dari id, movie_name dan genre.

  Proses ini bertujuan untuk membuat sebuah DataFrame baru bernama movie_dict yang menyimpan informasi tentang film, termasuk ID film, nama film, dan genre. Proses ini dilakukan dengan menggunakan fungsi pd.DataFrame() dari pustaka pandas. Berikut penjelasan lebih rinci mengenai setiap bagian kode:

    1. pd.DataFrame():

        Fungsi pd.DataFrame() digunakan untuk membuat sebuah objek DataFrame baru. DataFrame adalah struktur data dua dimensi dalam pandas yang mirip dengan tabel dalam basis data atau spreadsheet, di mana data disusun dalam baris dan kolom. Di dalam pd.DataFrame(), kita dapat memasukkan berbagai bentuk data, seperti dictionary, list, atau array, yang nantinya akan disusun menjadi baris dan kolom dalam DataFrame.

    2. movie_id, movie_name, movie_genre:
        - movie_id adalah sebuah list yang berisi ID unik untuk setiap film.
        - movie_name adalah sebuah list yang berisi nama atau judul film.
        - movie_genre adalah sebuah list yang berisi genre atau kategori yang sesuai dengan setiap film.

        Ketiga list ini sebelumnya sudah dibuat dari kolom yang ada di DataFrame prep_movies. Masing-masing list ini berisi informasi yang relevan untuk setiap film.

    3. movie_dict = pd.DataFrame({...}):

        Di dalam pd.DataFrame(), kita membuat sebuah dictionary yang berisi pasangan key-value. Setiap key adalah nama kolom yang diinginkan di DataFrame baru, dan value adalah list yang akan mengisi kolom tersebut.
        - 'id' adalah nama kolom yang akan berisi movie_id (list ID film).
        - 'movie_name' adalah nama kolom yang akan berisi movie_name (list nama film).
        - 'genre' adalah nama kolom yang akan berisi movie_genre (list genre film).
 
- Mengubah userId menjadi unique list dan encoding userId.

   Proses yang terjadi dalam kode tersebut dimulai dengan mengonversi kolom userId dalam DataFrame collaborative_df menjadi list yang berisi ID pengguna unik, kemudian melakukan encoding pada ID pengguna menjadi angka, dan akhirnya mengembalikannya lagi ke ID pengguna asli. Berikut adalah penjelasan rinci untuk setiap langkah:

    1. Mengekstraksi ID Pengguna Unik: Pada langkah pertama, kode user_ids = collaborative_df['userId'].unique().tolist() mengambil kolom userId dari DataFrame collaborative_df, yang berisi ID pengguna. Dengan menggunakan fungsi unique(), kode ini memastikan hanya ID pengguna yang unik yang diambil, menghilangkan nilai duplikat. Fungsi tolist() kemudian mengonversi hasil tersebut menjadi list Python yang berisi ID pengguna tanpa duplikat. Hasilnya adalah user_ids, sebuah list yang hanya berisi ID pengguna yang berbeda-beda.

    2. Melakukan Encoding pada ID Pengguna: Selanjutnya, dengan kode user_to_user_encoded = {x: i for i, x in enumerate(user_ids)}, dilakukan proses encoding terhadap userId. Fungsi enumerate(user_ids) memberikan pasangan indeks dan nilai dari list user_ids. Dalam hal ini, setiap userId akan dipetakan ke angka yang mewakili posisi atau indeksnya dalam list. Sebagai contoh, jika user_ids berisi [1, 2, 3], maka user_to_user_encoded akan menghasilkan dictionary {1: 0, 2: 1, 3: 2}, yang berarti ID pengguna 1 dipetakan ke angka 0, ID pengguna 2 dipetakan ke angka 1, dan seterusnya. Encoding ini sering digunakan dalam pemrosesan data agar ID yang bersifat kategorikal bisa diproses dalam model machine learning yang membutuhkan data numerik.

    3. Melakukan Reverse Encoding: Pada langkah terakhir, kode user_encoded_to_user = {i: x for i, x in enumerate(user_ids)} membalikkan proses encoding dengan reverse encoding. Kali ini, setiap angka indeks dipetakan kembali ke ID pengguna yang asli. Fungsi enumerate(user_ids) digunakan lagi untuk mendapatkan pasangan indeks dan nilai, tetapi kali ini, indeks (angka) akan menjadi key dan userId yang asli menjadi value. Sebagai contoh, jika user_ids berisi [1, 2, 3], maka user_encoded_to_user akan menghasilkan dictionary {0: 1, 1: 2, 2: 3}, yang berarti angka 0 dipetakan kembali ke ID pengguna 1, angka 1 ke ID pengguna 2, dan seterusnya.

    Secara keseluruhan, proses ini mengonversi userId yang bersifat kategorikal menjadi angka yang lebih mudah diproses dalam analisis data atau model machine learning, dan kemudian mengembalikannya lagi ke format userId yang lebih mudah dipahami. Ini adalah teknik yang umum digunakan untuk mempersiapkan data kategorikal dalam analisis dan pembelajaran mesin.
  
- Mengubah movieId menjadi unique list dan encoding movieId.

  proses yang mirip dengan kode sebelumnya, tetapi kali ini diterapkan pada movieId alih-alih userId. Proses ini bertujuan untuk mengonversi movieId dalam DataFrame collaborative_df menjadi angka, yang kemudian dapat digunakan dalam analisis atau model machine learning. Berikut penjelasan lebih lanjut tentang proses yang terjadi:

    1. Mengekstraksi movieId Unik: Pertama, kode movie_ids = collaborative_df['movieId'].unique().tolist() mengambil kolom movieId dari DataFrame collaborative_df yang berisi ID film. Fungsi unique() digunakan untuk memastikan bahwa hanya ID film yang unik (tanpa duplikat) yang diambil dari kolom tersebut. Kemudian, fungsi tolist() mengonversi hasil tersebut menjadi list Python. Hasilnya adalah list movie_ids yang hanya berisi ID film yang berbeda.

    2. Melakukan Encoding pada movieId: Setelah itu, kode movie_to_movie_encoded = {x: i for i, x in enumerate(movie_ids)} melakukan encoding pada movieId. Fungsi enumerate(movie_ids) menghasilkan pasangan indeks dan nilai dari list movie_ids, yang di mana setiap movieId akan dipetakan ke angka indeksnya dalam list. Sebagai contoh, jika movie_ids berisi [1, 2, 3], maka movie_to_movie_encoded akan menghasilkan dictionary {1: 0, 2: 1, 3: 2}. Ini berarti ID film 1 dipetakan ke angka 0, ID film 2 dipetakan ke angka 1, dan seterusnya. Encoding ini mempermudah pemrosesan data dalam model machine learning yang membutuhkan angka sebagai input.

    3. Melakukan Reverse Encoding pada movieId: Pada langkah terakhir, kode movie_encoded_to_movie = {i: x for i, x in enumerate(movie_ids)} membalikkan proses encoding, yakni melakukan reverse encoding. Di sini, setiap angka indeks yang sebelumnya digunakan sebagai key dalam encoding akan dipetakan kembali ke ID film asli. Fungsi enumerate(movie_ids) digunakan untuk menghasilkan pasangan indeks dan nilai, di mana indeks (angka) menjadi key dan movieId asli menjadi value. Misalnya, jika movie_ids berisi [1, 2, 3], maka movie_encoded_to_movie menghasilkan dictionary {0: 1, 1: 2, 2: 3}, yang berarti angka 0 dipetakan kembali ke ID film 1, angka 1 ke ID film 2, dan seterusnya.

    Secara keseluruhan, proses ini mirip dengan proses encoding userId, di mana movieId yang awalnya berupa data kategorikal diubah menjadi angka untuk mempermudah analisis atau model machine learning, dan kemudian dapat dikembalikan ke bentuk aslinya. Teknik encoding dan reverse encoding ini sering digunakan dalam pengolahan data untuk keperluan analisis dan pelatihan model.
  
- Mapping userId ke dataframe user dan movieId ke dataframe movie.

    Proses yang terjadi dalam kode ini bertujuan untuk memetakan userId dan movieId yang bersifat kategorikal dalam DataFrame collaborative_df menjadi representasi numerik yang sudah di-encode sebelumnya. Berikut penjelasan rinci tentang apa yang dilakukan setiap langkah:

    1. Mapping userId ke Representasi Numerik: Pada baris pertama, collaborative_df['user'] = collaborative_df['userId'].map(user_to_user_encoded), dilakukan pemetaan atau mapping kolom userId ke dalam kolom baru yang bernama user. Fungsi map() digunakan untuk mengganti setiap nilai dalam kolom userId dengan nilai yang sesuai dari dictionary user_to_user_encoded, yang sebelumnya berisi pemetaan antara userId dan angka encoding. Dengan demikian, setiap userId yang ada di dalam kolom userId akan digantikan dengan angka yang mewakili ID pengguna tersebut dalam bentuk numerik. Misalnya, jika sebelumnya userId 1 dipetakan ke angka 0, maka kolom user untuk userId 1 akan berisi angka 0.

    2. Mapping movieId ke Representasi Numerik: Baris kedua, collaborative_df['movie'] = collaborative_df['movieId'].map(movie_to_movie_encoded), melakukan hal yang serupa, tetapi untuk kolom movieId. Fungsi map() digunakan untuk mengganti setiap movieId yang ada dengan nilai yang sesuai dari dictionary movie_to_movie_encoded, yang berisi pemetaan antara movieId dan angka encoding yang sudah dibuat sebelumnya. Hasilnya, setiap movieId yang ada dalam DataFrame collaborative_df akan digantikan dengan angka yang mewakili film tersebut dalam bentuk numerik.

    Secara keseluruhan, kedua langkah ini mengonversi kolom userId dan movieId dalam DataFrame collaborative_df yang awalnya berisi ID pengguna dan ID film yang berbentuk kategorikal menjadi representasi numerik berdasarkan encoding yang telah dibuat sebelumnya. Proses ini penting untuk keperluan analisis data atau pembuatan model machine learning, yang biasanya memerlukan data dalam format numerik untuk pemrosesan lebih lanjut.
  
- Data split.

    Proses yang dilakukan pada tahap ini dimulai dengan membuat variabel x, yang berisi kombinasi data pengguna (user) dan film (movie) dari dataframe bernama collaborative_df. Data ini diambil menggunakan pemilihan kolom [['user', 'movie']], kemudian diubah menjadi array numerik menggunakan .values. Hasilnya, variabel x menjadi array dua dimensi yang setiap barisnya merupakan pasangan nilai user dan movie, misalnya [1, 10] untuk menunjukkan bahwa pengguna dengan ID 1 berinteraksi dengan film dengan ID 10.

    Selanjutnya, dibuat variabel y untuk menyimpan data nilai rating (rating) yang telah dinormalisasi. Normalisasi dilakukan dengan rumus (x - min_rating) / (max_rating - min_rating) untuk mengubah nilai rating menjadi skala antara 0 hingga 1. Fungsi .apply() digunakan untuk menerapkan rumus ini pada setiap nilai dalam kolom rating, dan hasil akhirnya diubah menjadi array menggunakan .values.

    Setelah itu, data dibagi menjadi data pelatihan (80%) dan data validasi (20%). Indeks pembagi dihitung menggunakan int(0.8 * collaborative_df.shape[0]), yaitu 80% dari total jumlah data. Data x dan y kemudian dipisahkan berdasarkan indeks ini, di mana 80% data pertama digunakan untuk pelatihan (x_train dan y_train), sedangkan sisanya 20% digunakan untuk validasi (x_val dan y_val). Proses ini memastikan bahwa model dapat dilatih dan diuji pada data yang berbeda untuk menghindari overfitting.
  
- TF-IDF.

  Proses ini dimulai dengan inisialisasi objek TfidfVectorizer dari library scikit-learn. TfidfVectorizer adalah alat yang digunakan untuk mengonversi teks menjadi representasi vektor numerik berdasarkan frekuensi term (TF) dan inverse document frequency (IDF). Dalam konteks ini, vektor ini digunakan untuk memproses data teks pada kolom genre dari dataset movie_dict.

    Langkah berikutnya adalah menghitung nilai IDF (Inverse Document Frequency) pada data genre dengan metode fit(movie_dict['genre']). Metode ini mempelajari fitur-fitur unik (kata-kata atau istilah) dari teks dalam kolom genre dan menghitung nilai IDF untuk setiap fitur. IDF memberikan bobot lebih tinggi pada istilah yang jarang muncul di seluruh dataset, sehingga fitur yang lebih relevan dengan dokumen tertentu akan lebih menonjol.

    Setelah proses fitting selesai, metode get_feature_names_out() dipanggil untuk memetakan indeks integer dari setiap fitur ke nama fitur aslinya. Hasilnya adalah daftar istilah unik yang telah diidentifikasi dari data genre. Dengan demikian, setiap istilah memiliki indeks yang dapat digunakan dalam representasi vektor numerik. Proses ini penting untuk mengubah data teks menjadi format yang dapat diproses oleh algoritme pembelajaran mesin.

  


## Modeling
Pada proyek ini dua pendekatan digunakan untuk membuat sistem rekomendasi yaitu:
1. Content-based filtering

    Pada projek ini menggunakan pendekatan Cosine similarity. Proses dimulai dengan menghitung kesamaan antar film menggunakan fungsi cosine_similarity() dari library scikit-learn. Fungsi ini mengukur kesamaan berbasis kosinus antara vektor-vektor dalam matriks tfidf_matrix. Matriks ini sebelumnya dibuat melalui proses transformasi teks (dalam hal ini data genre) menjadi representasi vektor numerik berbasis TF-IDF.

    cosine_similarity(tfidf_matrix) menghasilkan matriks kesamaan kosinus, di mana setiap elemen [i, j] mewakili nilai kesamaan antara film ke-i dan film ke-j berdasarkan genre mereka. Nilai kesamaan kosinus berkisar antara 0 hingga 1, di mana nilai lebih mendekati 1 menunjukkan bahwa dua film memiliki genre yang sangat mirip, sementara nilai mendekati 0 menunjukkan genre yang sangat berbeda. Matriks ini sangat berguna dalam rekomendasi film, karena memungkinkan pencarian film dengan genre yang serupa dengan film tertentu.
   
    Pada tahap ini, dibuat fungsi bernama `movie_recommendations` yang digunakan untuk memberikan rekomendasi film berdasarkan tingkat kesamaan genre. Berikut penjelasan prosesnya dalam bentuk paragraf:

    - Fungsi movie_recommendations menerima beberapa parameter, yaitu movie_name (nama film yang menjadi referensi), similarity (matriks kesamaan antar film, default adalah movie_cosine_df), items (dataframe yang berisi informasi tentang film, seperti movie_name dan genre), serta k (jumlah rekomendasi yang diinginkan, default 5). Proses dimulai dengan menemukan indeks film dengan nilai kesamaan tertinggi terhadap film yang diberikan. Hal ini dilakukan dengan memanfaatkan similarity.loc[:, movie_name], yaitu kolom kesamaan untuk film tersebut, dan kemudian menggunakan fungsi .to_numpy().argpartition(range(-1, -k, -1)) untuk mendapatkan indeks film dengan kesamaan terbesar.
    - Setelah itu, film dengan tingkat kesamaan tertinggi diambil berdasarkan indeks ini. Variabel closest menyimpan nama-nama film dengan kesamaan tertinggi, tetapi nama film yang menjadi referensi (movie_name) dihapus dari daftar rekomendasi menggunakan metode .drop(movie_name, errors='ignore'). Langkah terakhir, daftar film terdekat ini digabungkan dengan informasi tambahan dari dataframe items menggunakan metode .merge(items) untuk menampilkan nama film dan genre-nya. Fungsi mengembalikan hasil berupa dataframe berisi hingga k rekomendasi film yang paling mirip dengan film referensi.

   Hasil dari proses ini ditunjukan pada Gambar berikut.

2. Colaborative filtering

    Implementasi model rekomendasi berbasis neural network yang dinamakan RecommenderNet, menggunakan framework TensorFlow dan Keras. Model ini dirancang untuk mempelajari hubungan antara pengguna dan film berdasarkan data masukan. Pada tahap inisialisasi (__init__), model mendefinisikan beberapa lapisan embedding, yaitu representasi vektor untuk pengguna dan film. Setiap pengguna dan film direpresentasikan dalam ruang vektor berdimensi embedding_size. Lapisan embedding pengguna (user_embedding) dan embedding film (movie_embedding) dirancang untuk mempelajari representasi fitur yang relevan dengan inisialisasi bobot menggunakan metode he_normal dan regularisasi L2 untuk mencegah overfitting. Selain itu, bias untuk pengguna dan film juga dimodelkan dengan lapisan embedding terpisah (user_bias dan movie_bias) untuk menangkap preferensi spesifik.

    Metode call digunakan untuk mendefinisikan bagaimana data diproses oleh model. Ketika data masukan diterima, yang berupa pasangan [user_id, movie_id], embedding pengguna diakses melalui self.user_embedding(inputs[:, 0]), dan embedding film diakses melalui self.movie_embedding(inputs[:, 1]). Bias pengguna dan film juga diambil menggunakan cara yang serupa. Model kemudian menghitung nilai dot product antara embedding pengguna dan embedding film, yang mewakili tingkat kesesuaian atau relevansi antara pengguna dan film. Nilai ini ditambahkan dengan bias pengguna dan bias film untuk mendapatkan prediksi akhir. Hasilnya diteruskan melalui fungsi aktivasi sigmoid (tf.nn.sigmoid) untuk memastikan bahwa nilai prediksi berada dalam rentang 0 hingga 1, yang cocok untuk tugas prediksi seperti rating. Model ini memberikan pendekatan fleksibel untuk menangkap hubungan kompleks antara pengguna dan item yang direkomendasikan.

   Gambar xx adalah hasil dari rekomendasi dari sistem yang dibangun.

## Evaluation
Dalam proyek ini, evaluasi dilakukan untuk memastikan bahwa rekomendasi yang dihasilkan relevan, akurat, dan memenuhi kebutuhan pengguna. Berikut adalah rincian pendekatan evaluasi yang digunakan:
1. Content-based filtering

   Evaluasi yang dilakukan pada model ini menggunakan menggunakan tiga metrik utama: Precision, Recall, dan F1-Score, yang bertujuan untuk mengukur kinerja model secara menyeluruh.
   - **Precision** mengukur proporsi item relevan yang direkomendasikan oleh model terhadap total item yang direkomendasikan.
   - **Recall** mengukur proporsi item relevan yang direkomendasikan oleh model terhadap total item relevan yang seharusnya direkomendasikan.
   - **F1-Score** merupakan gabungan dari Precision dan Recall, yang memberikan satu nilai untuk menilai keseimbangan antara keduanya.
  
    Beberapa langkah untuk menghitung nilai precision, recall, dan f1-score sebagai berikut.
   - Langkah pertama adalah menentukan threshold pada nilai cosine similarity, yang digunakan untuk mengkategorikan nilai kesamaan menjadi 1 (mirip) atau 0 (tidak mirip). Threshold yang dipilih adalah 0.5, yang berarti jika nilai cosine similarity antara dua film lebih besar atau sama dengan 0.5, maka dianggap sebagai film yang mirip (1), dan jika lebih kecil dari 0.5, dianggap tidak mirip (0).
   - Selanjutnya, ground truth atau data kebenaran ditentukan dengan membandingkan nilai cosine similarity dengan threshold ini. Dalam hal ini, nilai cosine similarity yang lebih besar atau sama dengan 0.5 diberi label 1, sedangkan yang lebih kecil diberi label 0. Kemudian, untuk mempermudah perbandingan, kode mengambil sebagian kecil dari matriks cosine similarity dan matriks ground truth, masing-masing berukuran 1000 elemen. Matriks-matriks ini kemudian diratakan menjadi array satu dimensi menggunakan fungsi .flatten() agar dapat dibandingkan.
   - Langkah berikutnya adalah membuat prediksi berdasarkan threshold yang telah ditentukan, yaitu dengan menganggap bahwa nilai cosine similarity yang lebih besar atau sama dengan 0.5 adalah prediksi benar (1), dan lainnya adalah prediksi salah (0). Terakhir, fungsi precision_recall_fscore_support digunakan untuk menghitung tiga metrik evaluasi utama: precision, recall, dan F1-score. Precision mengukur akurasi prediksi positif, recall mengukur seberapa baik model menemukan semua prediksi positif yang sebenarnya, dan F1-score adalah rata-rata harmonis antara precision dan recall. Dengan ini, sistem dapat dievaluasi untuk melihat seberapa baik kemampuannya dalam memberikan rekomendasi yang relevan.
  
    **Hasil Evaluasi Metrik:**

    - **Precision: 1.0**
    - **Recall: 1.0**
    - **F1-Score: 1.0**

    **Interpretasi Hasil Evaluasi:**

    - Nilai **Precision** sebesar 1.0 menunjukkan bahwa semua item yang direkomendasikan oleh model benar-benar relevan, tanpa ada kesalahan (false positive).
    - Nilai **Recall** sebesar 1.0 menunjukkan bahwa model berhasil mengidentifikasi 100% dari semua item yang relevan, tanpa ada item yang terlewat (false negative).
    - Nilai **F1-Score** sebesar 1.0 mengindikasikan keseimbangan sempurna antara precision dan recall, yang berarti model berkinerja sangat baik dalam menangani kedua kelas (positif dan negatif).

2. Root Mean Squared Error (RMSE): Mengukur perbedaan antara rating yang diprediksi dengan rating aktual yang diberikan pengguna.


**Dampak Evaluasi Model Terhadap Business Understanding:**

1. **Menjawab Problem Statement:**
   - Evaluasi model menunjukkan bahwa sistem rekomendasi yang dikembangkan mampu memberikan rekomendasi yang dipersonalisasi dan relevan bagi pengguna berdasarkan preferensi mereka. Ini sesuai dengan problem statement yang diajukan, yakni bagaimana membuat sistem rekomendasi film yang efektif dan personalisasi.

2. **Mencapai Goals yang Diharapkan:**
   - Model berhasil mencapai tujuan yang diharapkan, yaitu menghasilkan rekomendasi film yang relevan untuk setiap pengguna. Nilai precision, recall, dan F1-Score yang sempurna menunjukkan bahwa model dapat mengidentifikasi dengan baik film yang sesuai dengan preferensi pengguna.
3. **Dampak Solusi Statement:**
   - Solusi yang direncanakan, yaitu penerapan teknik content-based filtering, memberikan dampak positif. Model mampu memberikan rekomendasi yang sesuai dengan minat dan preferensi pengguna tanpa perlu data dari pengguna lain (collaborative filtering). Hal ini menunjukkan bahwa teknik yang dipilih berhasil mendukung tujuan bisnis dalam meningkatkan kepuasan pengguna dan potensi peningkatan penjualan atau penyewaan film.

Dengan demikian, evaluasi model ini menunjukkan bahwa sistem rekomendasi yang dikembangkan tidak hanya memenuhi kebutuhan teknis tetapi juga berdampak positif terhadap tujuan bisnis. Hal ini diharapkan dapat meningkatkan engagement pengguna dan, pada akhirnya, mendukung pertumbuhan industri perfileman secara keseluruhan.
